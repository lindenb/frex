# Workflows comparisons (DRAFT)

(je copie le mail)


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
