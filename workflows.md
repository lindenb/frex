# Workflows

## Tools & data
* Reference
  * Nantes : we used the fasta sequence human_g1k_v37.fasta provided in the GATK bundle: https://www.broadinstitute.org/gatk/guide/article.php?id=1213 
* bwa
  * Nantes : version bwa-0.7.10
* samtools
  * Nantes: version 1.1 
* bcftools
  * Nantes: version 1.1 
* picard
  * Nantes: version 1.126
* gatk
   * Nantes: version 3.3.0
* java
   * Nantes: version 1.7.0_25

## Code

* Nantes: My complete workflow is currently hosted on our univ git repositoy: https://gitlab.univ-nantes.fr/pierre.lindenbaum/frenchexomes . The repo is currently hidden from external people.

## FASTQ Preprocessing

* Nantes: none
 
## Read Mapping Preprocessing

### Nantes

Reads where aligned with bwa and option -M. Sample and Center have been specified in the -R header. BAM was sorted on the fly with samtools A typical mapping looks like:
```bash
 
bwa-0.7.10/bwa mem   -M \
	-R '@RG\tID:p11\tLB:sample_name\tSM:sample_name\tPL:ILLUMINA\tCN:center' \
	human_g1k_v37.fasta R1.fastq.gz R2.fastq.gz  |\
  samtools view -u -  |\
  samtools sort  -@ 3 -o sorted.bam -T tmp -
```
## Merging the BAMs
 
### Nantes

for a few samples (B00GG8Z, B00FX08,B00FX09,B00FX0A) , there were more than one pair of FASTQ. sorted bam where merged with picard:
 
 ```bash
java -XX:ParallelGCThreads=4 -jar /picard.jar MergeSamFiles \
	SO=coordinate \
	AS=true \
	MAX_RECORDS_IN_RAM=1000000 \
	CREATE_INDEX=false \
	COMPRESSION_LEVEL=9 \
	VALIDATION_STRINGENCY=SILENT \
	USE_THREADING=true \
	VERBOSITY=WARNING \
	O=merged.bam \
	TMP_DIR=tmp \
	COMMENT="Merged from (...) " \
	 I=sorted1.bam   I=sorted2.bam
 ```
## Removing/Flagging the duplicates
 
### Nantes

PCR+ Optical duplicated duplicated were marked with picard MarkDuplicates: 
```
java  -jar picard.jar MarkDuplicates \
	MAX_RECORDS_IN_RAM=1000000 \
	COMPRESSION_LEVEL=9 \
	TMP_DIR=tmp \
	INPUT/merged.bam \
	O=markdup.bam \
	M=markdup.metrics \
	AS=true PG=PMD \
	VALIDATION_STRINGENCY=SILENT \
	CREATE_INDEX=true
```
## Realign Indels
 
### Nantes
indels were realigned with GATK using the data (1000G_phase1.indels.b37.vcf  and Mills_and_1000G_gold_standard ) provided by the GATK bundle in the regions of the capture+/-500pb.

```bash
java   -jar GenomeAnalysisTK.jar -T RealignerTargetCreator \
	-R human_g1k_v37.fasta \
	-L:capture,BED extended.capture.500.bed \
	-I markdup.bam \
	-o bam.intervals \
	--known:onekindels,VCF 1000G_phase1.indels.b37.vcf  \
	--known:millsindels,VCF Mills_and_1000G_gold_standard.indels.b37.vcf  \
	-S SILENT
```

```bash
java   -jar GenomeAnalysisTK.jar -T IndelRealigner \
	-R human_g1k_v37.fasta \
	-I markdup.bam \
  -L:capture,BED extended.capture.500.bed \
	-o realign.bam.tmp.bam \
	-targetIntervals  bam.intervals  \
	-known:onekindels,VCF 1000G_phase1.indels.b37.vcf  \
	-known:millsindels,VCF Mills_and_1000G_gold_standard.indels.b37.vcf  \
	-S SILENT 
```
## Base recalibration
 
### Nantes
Bam were recalibrated with GATK using the data ( dbsnp138 ) provided by the GATK bundle in the regions of the capture+/-500pb.

```bash
java   -jar GenomeAnalysisTK.jar -T  BaseRecalibrator \
  -R human_g1k_v37.fasta \
	--validation_strictness LENIENT \
	-I realign.bam \
	-l INFO \
	-o  recal.grp \
	-knownSites:dbsnp138,VCF dbsnp_138.b37.vcf \
	-L:capture,BED extended.capture.500.bed \
	-cov ReadGroupCovariate \
	-cov QualityScoreCovariate \
	-cov CycleCovariate \
	-cov ContextCovariate
```

```bash
java   -jar GenomeAnalysisTK.jar -T PrintReads \
	-R human_g1k_v37.fasta \
	-BQSR recal.grp \
	-I realign.bam \
	-L:capture,BED extended.capture.500.bed \
	-o final.bam \
	--validation_strictness LENIENT \
	-l INFO 
```
