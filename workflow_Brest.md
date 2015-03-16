(avant propos)
Correction de dbSNP :

corr_dbsnp_142.b37.vcf : dbSNP b142 (All_20150102.vcf.gz), sans SNPs étrange.
Les variants exclus n'ont pas été listés, mais ce sont soit des SNPs avec des allèles alternatifs égaux à la ref, soit 2 fois le même allèle alternatif.
pour avoir une idée des pb regardez ceux-là :
rs373366119, rs34500567, rs59792241
J'ai peut-ête été brutal parce que j'ai rejeté complètement ces SNPs.

```
	bcftools view -h /PUBLIC_DATA/GENERATED/dbSNP/dbsnp_142.b37.vcf.gz > corr_dbsnp_142.b37.vcf
	# next if alt allele is present twice
	# do not print if alt is ref
	bcftools view -H /PUBLIC_DATA/GENERATED/dbSNP/dbsnp_142.b37.vcf.gz \
			| perl -lane '@a_ = split /,/, $F[4]; %h_ = map{ $_ => 1 } @a_; $n=@_; $m=keys %h_; next if( $n != $m ); print "$_" unless( $h_{$F[3]} );' \
		>> corr_dbsnp_142.b37.vcf
```

Note : dbSNP vient de sortir une nouvelle version, avec les corrections que j'ai faite.
Mais, leur version a enlevé ~475_000 variants, je les ai contacté pour avoir des infos, je leur ai transmis la liste des différences.

Génération du VCF :
scripts/FrEx__gvcf2vcf_multi.sh
--> à part dbSNP, les fichiers sont ceux du pack GATK 2.8 b37.
```
gatk="java -Xms100G -Xmx100G -jar /PROGS/EXTERN/GATK/GenomeAnalysisTK-3.3-0/GenomeAnalysisTK.jar" 
GATK_CALL="$gatk -R $REF_FASTA" 
DBSNP_VCF=../corr_dbsnp_142.b37.vcf

# build vcf
for town in ${A_TOWNS[*]}
do
    for gvcf_file in $TOWN_PATH/$town/*.gvcf.gz
    do
        gvcf_list_opt=$gvcf_list_opt" -V $gvcf_file" 
    done
done
$GATK_CALL -T GenotypeGVCFs $gvcf_list_opt --dbsnp $DBSNP_VCF -o $OUTMASK.vcf.gz -nt 8

# VQSR - SNP
local hapmap_option="-resource:hapmap,known=false,training=true,truth=true,prior=15.0 $HAPMAP_VCF" 
local   omni_option="-resource:omni,known=false,training=true,truth=true,prior=12.0 $MILLE_GENOMES_OMNI_VCF" 
local milleG_option="-resource:1000G,known=false,training=true,truth=false,prior=10.0 $MILLE_GENOMES_HIGH_VCF" 
local  dbSNP_option="-resource:dbsnp,known=true,training=false,truth=false,prior=2.0 $DBSNP_VCF" 
local tranche_option="-tranche 100.0 -tranche 99.9 -tranche 99.0 -tranche 90.0" 
local    an_options="-an DP -an QD -an FS -an SOR -an MQ -an MQRankSum -an ReadPosRankSum -an InbreedingCoeff" 

$GATK_CALL -T VariantRecalibrator -input $vcf_in \
        $hapmap_option $omni_option $milleG_option $dbSNP_option \
        $an_options -mode SNP $tranche_option \
        -recalFile $tmp_file1 -tranchesFile $tmp_file2 \
        -rscriptFile $R_mask.SNP.R

$GATK_CALL -T ApplyRecalibration -input $vcf_in \
        -recalFile $tmp_file1 -tranchesFile $tmp_file2 \
        --ts_filter_level 99.5 \
        -mode SNP -o $out_mask.SNP.vcf.gz

# VQSR - INDEL
local mills_option="-resource:mills,known=true,training=true,truth=true,prior=12.0 $MILLS_VCF" 
an_options="-an DP -an QD -an FS -an SOR -an MQRankSum -an ReadPosRankSum -an InbreedingCoeff" 

$GATK_CALL -T VariantRecalibrator -input $vcf_in \
        $mills_option \
        $an_options -mode INDEL $tranche_option \
        --maxGaussians 4 \
        -recalFile $tmp_file1 -tranchesFile $tmp_file2 \
        -rscriptFile $R_mask.INDEL.R

$GATK_CALL -T ApplyRecalibration -input $vcf_in \
        -recalFile $tmp_file1 -tranchesFile $tmp_file2 \
        --ts_filter_level 99.0 \
        -mode INDEL -o $out_mask.INDEL.vcf.gz
```

valeurs par défaut de GenotypeGVCFs (https://www.broadinstitute.org/gatk/gatkdocs/org_broadinstitute_gatk_tools_walkers_variantutils_GenotypeGVCFs.php)
--heterozygosity 0.001
--indel_heterozygosity 1.25E-4
--sample_ploidy 2
--standard_min_confidence_threshold_for_calling 30.0
--standard_min_confidence_threshold_for_emitting 30.0
--max_alternate_alleles 6

valeurs par défaut de VariantRecalibrator (https://www.broadinstitute.org/gatk/gatkdocs/org_broadinstitute_gatk_tools_walkers_variantrecalibration_VariantRecalibrator.php)
--target_titv 2.15
--badLodCutoff -5.0
--dirichlet 0.001
--maxGaussians 8 (seulement pour SNP)
--maxIterations 150
--maxNegativeGaussians 2
--maxNumTrainingData 2500000
--minNumBadVariants 1000
--numKMeans 100
--priorCounts 20.0
--shrinkage 1.0
--stdThreshold 10.0

renommage des individus de Rouen
(la commande a été faite après VQSR (le script précédent a été modifié pour le faire avant).
```
zcat $OUTMASK.vcf.gz | convert-header-rouen.awk \
        > $OUTMASK.renamed.vcf
    bgzip $OUTMASK.renamed.vcf
    tabix $OUTMASK.renamed.vcf.gz
réduire au régions sélectionnées +100b
for file in *.renamed.vcf.gz
do
{
    list_file=''
    bname=$( basename $file .vcf.gz )

    outfile=$bname.region_ext.vcf

    bcftools view -h $file > $outfile
    bcftools view -H -R /NGS/FrEx/exome_regions.bed $file \
            | sort -V -k 1,2 -T . | uniq \
        >> $outfile

    bgzip $outfile
    tabix $outfile.gz
} &
done
wait
```

filter les variants
```
bcftools view -f PASS frex_new_test.recalibrated.SNP.renamed.region_ext.vcf.gz -O z -o frex.recalibrated.SNP.renamed.region_ext.PASS.vcf.gz &
bcftools view -f PASS frex_new_test.recalibrated.INDEL.renamed.region_ext.vcf.gz -O z -o frex.recalibrated.INDEL.renamed.region_ext.PASS.vcf.gz &
wait
```
exclure les indiv
```
cat ../BORDEAUX.samples.lst ../BREST.samples.lst ../LILLE.samples.lst ../NANTES.samples.lst ../ROUEN.samples.lst ../excluded_indiv.lst \
        | sort | uniq -u \
    > kept_indiv.lst

bcftools view -S kept_indiv.lst frex.recalibrated.SNP.renamed.region_ext.PASS.vcf.gz -O z -o frex.recalibrated.SNP.renamed.region_ext.PASS.ind_rem.vcf.gz
bcftools view -S kept_indiv.lst frex.recalibrated.INDEL.renamed.region_ext.PASS.vcf.gz -O z -o frex.recalibrated.INDEL.renamed.region_ext.PASS.ind_rem.vcf.gz
```

fusionner les SNP et INDEL
-> "à la main", il me semble qu'il y a un prog dans GATK qui permet de le faire.
```
bcftools view -h frex.recalibrated.SNP.renamed.region_ext.PASS.vcf.gz > SNP.h
bcftools view -h frex.recalibrated.INDEL.renamed.region_ext.PASS.vcf.gz > indel.h

head -n 1 SNP.h > frex.recalibrated.SNP_INDEL.renamed.region_ext.PASS.vcf

( tail -n +2 SNP.h | head -n -1 ; tail -n +2 indel.h | head -n -1 ) \
        | sort | uniq \
    >> frex.recalibrated.SNP_INDEL.renamed.region_ext.PASS.vcf

tail -n 1 SNP.h >> frex.recalibrated.SNP_INDEL.renamed.region_ext.PASS.vcf

( bcftools view -H frex.recalibrated.SNP.renamed.region_ext.PASS.vcf.gz;
    bcftools view -H  frex.recalibrated.INDEL.renamed.region_ext.PASS.vcf.gz ) \
        | sort -V -k 1,3 -T . | uniq \
    >> frex.recalibrated.SNP_INDEL.renamed.region_ext.PASS.vcf

bgzip frex.recalibrated.SNP_INDEL.renamed.region_ext.PASS.vcf
tabix frex.recalibrated.SNP_INDEL.renamed.region_ext.PASS.vcf.gz
```
