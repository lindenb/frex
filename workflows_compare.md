# Workflows comparisons (DRAFT)

* Brest

Je ne sais pas comment les différents pipelines vont être comparés.
Pour notre équipe à B, j'avais commencé à tester quelques programmes à l'aide du site http://www.bioplanet.com/gcat.
L'intérêt de ce site est qu'il propose des jeux de données de "Genome in a Bottle".
Pour faire court, ce groupe cherche à définir les bonnes pratiques pour l'analyse des NGS chez l'Humain.
https://sites.stanford.edu/abms/giab

Ils ont défini les "vrais SNPs/INDELs" de l'individu NA12878 (CEU fem) par croisement des données de 11 génomes entiers et 3 exomes avec 5 plateformes.

Le site GCAT permet de se comparer à ces vrais résultats.
Vous pouvez voir le résultat de mes tests ici : http://www.bioplanet.com/gcat/reports/4729-bqdrqlgmnn/variant-calls/ion-torrent-225bp-se-exome-20x/soft-filter-freebayes/compare-4728-gevntnynqi-4693-sbxjhtwddx-4695-kshrgugcrf/group-read-depth

Les résultats du pipeline ADES sont très surprenant (peu de 'vrais' SNPs détectés).
Je dis "très surprenant" parce que nos tests avec GATK n'ont qu'une seule étape de différente du pipeline ADES (je m'attendais donc à des résultats quasi identique), il s'agit de 'IndelRealigner'.
Le pipeline ADES utilise l'option '-targetIntervals $TARGET', nous utilisons le résultat de RealignerTargetCreator. (Note : nous utilisons GATK 3.2, mais les notes de version n'indique pas de changement majeurs avec la 3.3)

Avant de tirer de conclusion, je précise que j'ai filtré le fastq fourni et j'ai pu constaté que le filtrage joue un rôle très important parfois. (un filtre à peine plus fort divise la sensibilité de Freebayes par 10 !) 


	* Les filtres
	__no filter__ : le fichier fastq brut.
	__soft_filter__ : le fichier fastq a été filtré avec les commandes suivantes
		(FASTX Toolkit 0.0.14)
		fastq_quality_trimmer -t 22 -i gcat_set_054_1.fastq \
				| ../fastx_trimmer -f 5 -l 230 \
			> gcat_set_054_soft_filter.fastq
		# le premier prog. rogne toutes les extrémités des séquences si leurs bases ont une qualité inférieure à 22.
		# Le second coupe les 5 premières bases et après la 230e de chaque séquence.
		# Les valeurs ont été choisi "à l'oeil" à l'aide des graphes de fastqc.

	* L'alignement - contruction des bam
	La génération du bam a été fait à l'aide d'un script maison qui reprend les recommandations de GATK.
	La référence utilisée est HG19.

	L'alignement du fastq n'a été fait qu'avec bwa mem, sauf lorsque l'alignement fait partie intégrante du pipeline.
	C'est le prog. qui semble le plus utilisé, et je n'ai pas le temps de tester toutes les combinaisons possibles.
	Les séquences dupliquées ont été marquées. (L'en-tête a aussi été modifié).
		bwa mem -M -t 20 -R $header "$REF_FASTA" $fastq_in \
				| samtools view -uS - \
				| samtools sort - "$with_dup_file"
		picard-tools MarkDuplicates INPUT=$with_dup_file.bam OUTPUT=$bam_out METRICS_FILE=$metrics
		samtools index "$bam_out"

	Nous avons fait un réalignement local
	$GATK_CALL -T RealignerTargetCreator -I $bam_in -o $tmp_file -known $MILLS_VCF
	$GATK_CALL -T IndelRealigner -I $bam_in -targetIntervals $tmp_file.intervals -o $bam_out
	
	Nous avons réajusté les scores d'alignement (toujours selon les recommandations de GATK.
	$GATK_CALL -T BaseRecalibrator -I $bam_in --knownSites $DBSNP_VCF -o $tmp_file
	$GATK_CALL -T PrintReads -I $bam_in --BQSR $tmp_file -o $bam_out

	bwa : Version: 0.7.5a-r405
	picard-tools : Copyright 2010 The Broad Institute
	samtools     : samtools 1.0, Using htslib 1.0
	gatk         : 3.2-2-gec30cee

	remarques :
		 Le réalignement local autour des indel n'apporte aucune modification (à confirmer).
		 J'ai constaté que lorsqu'une mutation est suivi d'une insertion (erreur de séquençage),
		 le bam contient toujours l'insertion suivi de la mutation. J'ai observé que celà empêche la détection de certains variants.
		 -> C'est peut-être une difficulté de tous les programmes d'alignement.

	* Les filtres post-détection
	Certains filtres ont été appliqués sur les VCF obtenus après la détection.
	*only_PASS* marque les résultats où seuls les variants satisfaisants les filtres ont été rapportés.

	* La détection des SNPs/INDELs
		Les fichiers de ref pour dbNSP, HapMap et Omni et Mills sont ceux fournis par le bundle GATK.
		dbSNP  : b138
		HapMap : hapmap_3.3
		OMNI_VCF : 1000G_omni2.5.hg19.vcf.gz
		Indel/Mills : Mills_and_1000G_gold_standard.indels.hg19
	ADES_pipeline - tout le pipeline a été fait à partir du fastq filtré.
	Freebayes - version:  v0.9.18-3-gb72a21b
		$freebayes --fasta-reference $REF_FASTA gcat_set_054_soft_filter.recalibrated.bam > fb.vcf
	Freebayes only_PASS - FILTER='QA>=20&&QR>=20', appliqué avec GATK VariantFiltration
	GotCloud  - version: 1.14.9
		Par défaut utilise l'algorithme _aln_ de bwa.
		Testé avec l'algo _mem_ (la doc ne le précise pas mais on peut le choisir _info de la développeuse).
	GATK_Haplotype Caller - script maison qui reprend l'ensemble des recommandations de GATK pour HC.
	GATK_Unified Genotyper - script maison qui reprend l'ensemble des recommandations de GATK pour UG.
	scalpel - Version: 0.3.1(beta)
		logiciel de détection des indels (validé sur données illumina)
		testé sur vcf générés par GotCloud.
		perl scalpel.pl --single --bam gcat_set_054_soft_filter.recalibrated.bam --bed gcat_set_054.bed --ref hg19.fasta --logs --numprocs 30

	* Mes conclusion provisoire
	Tous les logiciels sont mauvais pour la détection des indels.
	Freebayes est très bien et peu sensible au bruit, mais il faut trouver un bon filtre que qualité (sur le vcf).
		-> sensibilité +
	GotCloud est très bien pour les SNPs.
		-> spécificité +
	Freebayes et GotCloud sont extrèment rapides (hors alignement) sur 1 seul jeu de données (GotCloud = 3 min.)
	(Got cloud redevient "seulement correct" en multi-sample -> 3 indiv. = 1 heure)
	GATK génère des gvcf qui peuvent être précieux pour la recherche (multi-sample).

pipeline	|	filtre	|	nb SNPs	|	nb InDels	|	GiB sens.	|	GiB spec.	|	VP	|	FP	|	FN	|	VN
------------|-----------|-----------|---------------|---------------|---------------|-------|-------|-------|------
GATK-HC	|	no filter	|	1514	|	284	|	1.90%	|	99.999%	|	296	|	235	|	15253	|	37238356
GATK-HC	|	soft filter	|	41502	|	33299	|	58.98%	|	99.962%	|	8939	|	14183	|	6218	|	37224408
GATK-HC	|	soft filter PASS	|	39747	|	32319	|	58.75%	|	99.963%	|	8906	|	13799	|	6252	|	37224792
GATK-HC	|	hard filter	|	4766	|	1239	|	7.79%	|	99.998%	|	1198	|	852	|	14183	|	37237739
GATK-UG	|	no filter	|	NA	|	NA	|	NA	|	NA	|	NA	|	NA	|	NA	|	NA
GATK-UG	|	soft filter	|	51450	|	39515	|	64.23%	|	99.951%	|	9760	|	18393	|	5436	|	37220198
GATK-UG	|	soft filter PASS	|	47902	|	34969	|	63.74%	|	99.955%	|	9687	|	16597	|	5511	|	37221994
GATK-UG	|	hard filter	|	5394	|	63	|	8.62%	|	99.999%	|	1325	|	467	|	14038	|	37238124
FreeBayes	|	no filter	|	72195	|	278289	|	74.65%	|	99.927%	|	10607	|	101793	|	3602	|	37136798
FreeBayes	|	soft filter	|	66606	|	265158	|	73.39%	|	99.734%	|	10398	|	99206	|	3771	|	37139385
FreeBayes	|	soft filter PASS	|	324714	|	35800	|	31.86%	|	99.979%	|	4779	|	7685	|	10219	|	37230906
FreeBayes	|	hard filter	|	6917	|	3889	|	7.90%	|	99.994%	|	1196	|	2198	|	13939	|	37236393
ADES	|	soft filter	|	5242	|	24081	|	4.03%	|	99.984%	|	631	|	5821	|	15014	|	37232770
gotCloud	|	soft filter	|	21034	|	0	|	46.94%	|	99.995%	|	7020	|	1941	|	7936	|	37236650
gotCloud (BWA MEM) SNP + INDEL	|	soft filter	|	21033	|	35081	|	47.05%	|	99.960%	|	7034	|	148	|	7917	|	37223755
gotCloud (BWA MEM) GC_SNP + Scalpel_INDEL	|	soft filter	|	21033	|	2451	|	46.94%	|	99.992%	|	7021	|	3143	|	7935	|	37235448


	Les données de Genome in a Bottle sont dispo sur le ftp du NCBI.
	Je vais les examiner d'avantage pour casser la boite-noire qu'est le site bioplanet. (avez-vous remarqué que selon les tests, le nombre total de 'vrais' SNPs =VP++FN n'est pas le même ?)
