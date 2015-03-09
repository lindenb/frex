# Workflow Nantes

## Create an 'extended bed +/- 500' for capture

capture.extend.size=500

```makefile
#
# Clean-up original capture
#
${capture.bed}: ${capture0.bed}
	mkdir -p $(dir $@)   &&  \
	grep -vE '(^browser|^track)' $< |\
	cut -f1,2,3 | LC_ALL=C sort -t '	' -k1,1 -k2,2n -k3,3n |\
	${bedtools.exe} merge  > $@
```


```makefile
${extended.capture.bed}: ${capture.bed} $(addsuffix .fai,${REF})
	mkdir -p $(dir $@)   &&  \
	${bedtools.exe} slop -b ${capture.extend.size} -g $(addsuffix .fai,${REF}) -i $< |\
	LC_ALL=C sort -t '	' -k1,1 -k2,2n -k3,3n |\
	${bedtools.exe} merge -d  ${capture.extend.size}   > $@

```

## Mapping reads and sort

```bash
${WORKDIR}/packages/bwa-0.7.10/bwa mem -t 19 -M \
	-R '@RG\tID:p101\tLB:B00G7JN\tSM:B00G7JN\tPL:ILLUMINA\tCN:Bordeaux' \
	${WORKDIR}/packages/broad/bundle/human_g1k_v37.fasta  /ccc/genostore/cont007/fg0019/fg0019/rawdata/projet_FRENCHEXBORDEAUX_461/B00G7JN/RunsSolexa/140514_HISEQ2_C3FVBACXX/F461_CS_B00G7JN_6_1_C3FVBACXX.IND11.fastq.gz /ccc/genostore/cont007/fg0019/fg0019/rawdata/projet_FRENCHEXBORDEAUX_461/B00G7JN/RunsSolexa/140514_HISEQ2_C3FVBACXX/F461_CS_B00G7JN_6_2_C3FVBACXX.IND11.fastq.gz  |\
${WORKDIR}/packages/samtools/samtools view -u -  |\
${WORKDIR}/packages/samtools/samtools sort  -@ 3 -o ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/B00G7JN_p101/BAM/__DELETE__20141212_FrenchEx_B00G7JN_p101_sorted.bam.tmp.bam -T  ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/B00G7JN_p101/BAM/__DELETE__20141212_FrenchEx__sort_p101 -  &&  \
mv  ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/B00G7JN_p101/BAM/__DELETE__20141212_FrenchEx_B00G7JN_p101_sorted.bam.tmp.bam ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/B00G7JN_p101/BAM/__DELETE__20141212_FrenchEx_B00G7JN_p101_sorted.bam
```

## Merge BAM when needed (more than one pair of fastq per sample)

```
mkdir -p ${SCRATCHDIR}/FrenchEx/Projects/Lille/Samples/B00FX0A/BAM/ /ccc/genostore/cont007/fg0019/lindenbp/FrenchEx/Projects/Lille/Samples/B00FX0A/BAM/   &&  \
/usr/local/jdk-1.7.0_25/bin/java -XX:ParallelGCThreads=2 -Xmx512m -Djava.io.tmpdir=${SCRATCHDIR}/FrenchEx/Projects/Lille/Samples/B00FX0A/BAM/  -jar ${WORKDIR}/packages/picard-tools-1.126/picard.jar MarkDuplicates \
	MAX_RECORDS_IN_RAM=1000000 \
	COMPRESSION_LEVEL=9 \
	TMP_DIR=${SCRATCHDIR}/FrenchEx/Projects/Lille/Samples/B00FX0A/BAM/ \
	INPUT=${SCRATCHDIR}/FrenchEx/Projects/Lille/Samples/B00FX0A/BAM/__DELETE__20141212_FrenchEx_B00FX0A_merged.bam \
	O=${SCRATCHDIR}/FrenchEx/Projects/Lille/Samples/B00FX0A/BAM/__DELETE__20141212_FrenchEx_B00FX0A_markdup.bam.tmp.bam \
	M=/ccc/genostore/cont007/fg0019/lindenbp/FrenchEx/Projects/Lille/Samples/B00FX0A/BAM/20141212_FrenchEx_B00FX0A_markdup.metrics \
	AS=true PG=PMD \
	VALIDATION_STRINGENCY=SILENT \
	CREATE_INDEX=true  &&  \
mv  ${SCRATCHDIR}/FrenchEx/Projects/Lille/Samples/B00FX0A/BAM/__DELETE__20141212_FrenchEx_B00FX0A_markdup.bam.tmp.bam ${SCRATCHDIR}/FrenchEx/Projects/Lille/Samples/B00FX0A/BAM/__DELETE__20141212_FrenchEx_B00FX0A_markdup.bam  &&  \
mv  ${SCRATCHDIR}/FrenchEx/Projects/Lille/Samples/B00FX0A/BAM/__DELETE__20141212_FrenchEx_B00FX0A_markdup.bam.tmp.bai ${SCRATCHDIR}/FrenchEx/Projects/Lille/Samples/B00FX0A/BAM/__DELETE__20141212_FrenchEx_B00FX0A_markdup.bai
```

## MarkDuplicates

```bash
mkdir -p ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/ /ccc/genostore/cont007/fg0019/lindenbp/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/   &&  \
/usr/local/jdk-1.7.0_25/bin/java -XX:ParallelGCThreads=2 -Xmx512m -Djava.io.tmpdir=${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/  -jar ${WORKDIR}/packages/picard-tools-1.126/picard.jar MarkDuplicates \
	MAX_RECORDS_IN_RAM=1000000 \
	COMPRESSION_LEVEL=9 \
	TMP_DIR=${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/ \
	INPUT=${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/B00G7JN_p101/BAM/__DELETE__20141212_FrenchEx_B00G7JN_p101_sorted.bam \
	O=${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_markdup.bam.tmp.bam \
	M=/ccc/genostore/cont007/fg0019/lindenbp/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/20141212_FrenchEx_B00G7JN_markdup.metrics \
	AS=true PG=PMD \
	VALIDATION_STRINGENCY=SILENT \
	CREATE_INDEX=true  &&  \
mv  ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_markdup.bam.tmp.bam ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_markdup.bam  &&  \
mv  ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_markdup.bam.tmp.bai ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_markdup.bai
```

## Realign

```
mkdir -p ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/  &&  \
/usr/local/jdk-1.7.0_25/bin/java  -XX:ParallelGCThreads=2  -Xmx2g  -Djava.io.tmpdir=${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/ -jar ${WORKDIR}/packages/broad/gatk/3.3.0/GenomeAnalysisTK.jar -T RealignerTargetCreator \
	-R ${WORKDIR}/packages/broad/bundle/human_g1k_v37.fasta -et NO_ET -K  ${WORKDIR}/packages/broad/gatk/gatk.no_home.key \
	-L:capture,BED ${SCRATCHDIR}/FrenchEx/BED/extended.capture.500.bed \
	-I ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_markdup.bam \
	-o ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam.intervals.gz.tmp.intervals \
	--known:onekindels,VCF ${WORKDIR}/packages/broad/bundle/1000G_phase1.indels.b37.vcf  \
	--known:millsindels,VCF ${WORKDIR}/packages/broad/bundle/Mills_and_1000G_gold_standard.indels.b37.vcf  \
	-S SILENT  &&  \
gzip --best -f  ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam.intervals.gz.tmp.intervals  &&  \
mv  ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam.intervals.gz.tmp.intervals.gz ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam.intervals.gz
```

```
mkdir -p ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/  &&  \
gunzip -c ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam.intervals.gz > ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam.intervals  &&  \
/usr/local/jdk-1.7.0_25/bin/java  -XX:ParallelGCThreads=5 -Xmx2g  -Djava.io.tmpdir=${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/ -jar ${WORKDIR}/packages/broad/gatk/3.3.0/GenomeAnalysisTK.jar -T IndelRealigner \
	-R ${WORKDIR}/packages/broad/bundle/human_g1k_v37.fasta -et NO_ET -K  ${WORKDIR}/packages/broad/gatk/gatk.no_home.key \
	-I ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_markdup.bam \
	-o ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam.tmp.bam \
	-targetIntervals  ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam.intervals  \
	-known:onekindels,VCF ${WORKDIR}/packages/broad/bundle/1000G_phase1.indels.b37.vcf  \
	-known:millsindels,VCF ${WORKDIR}/packages/broad/bundle/Mills_and_1000G_gold_standard.indels.b37.vcf  \
	-S SILENT  &&  \
mv ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam.tmp.bam ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam  &&  \
mv ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam.tmp.bai ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bai  &&  \
rm -f ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam.intervals
```

## Recalibrate

```
mkdir -p ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/  &&  \
/usr/local/jdk-1.7.0_25/bin/java  -XX:ParallelGCThreads=5 -Xmx2g  -Djava.io.tmpdir=${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/ -jar ${WORKDIR}/packages/broad/gatk/3.3.0/GenomeAnalysisTK.jar -T BaseRecalibrator \
	-R ${WORKDIR}/packages/broad/bundle/human_g1k_v37.fasta -et NO_ET -K  ${WORKDIR}/packages/broad/gatk/gatk.no_home.key \
	--validation_strictness LENIENT \
	-I ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam \
	-l INFO \
	-o  ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_recal.grp \
	-knownSites:dbsnp138,VCF ${WORKDIR}/packages/broad/bundle/dbsnp_138.b37.vcf \
	-L:capture,BED ${SCRATCHDIR}/FrenchEx/BED/extended.capture.500.bed \
	-cov ReadGroupCovariate \
	-cov QualityScoreCovariate \
	-cov CycleCovariate \
	-cov ContextCovariate  &&  \
gzip -f --best ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_recal.grp 
```


```
mkdir -p ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/  &&  \
gunzip -c ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_recal.grp.gz > ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_recal.grp  &&  \
/usr/local/jdk-1.7.0_25/bin/java  -XX:ParallelGCThreads=5 -Xmx4g  -Djava.io.tmpdir=${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/ -jar ${WORKDIR}/packages/broad/gatk/3.3.0/GenomeAnalysisTK.jar -T PrintReads \
	-R ${WORKDIR}/packages/broad/bundle/human_g1k_v37.fasta -et NO_ET -K  ${WORKDIR}/packages/broad/gatk/gatk.no_home.key \
	-BQSR ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_recal.grp \
	-I ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/__DELETE__20141212_FrenchEx_B00G7JN_realign.bam \
	-o ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/20141212_FrenchEx_Bordeaux_B00G7JN_final.bam.tmp.bam \
	--validation_strictness LENIENT \
	-l INFO  &&  \
        mv ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/20141212_FrenchEx_Bordeaux_B00G7JN_final.bam.tmp.bam ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/20141212_FrenchEx_Bordeaux_B00G7JN_final.bam  &&  \
        mv ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/20141212_FrenchEx_Bordeaux_B00G7JN_final.bam.tmp.bai ${SCRATCHDIR}/FrenchEx/Projects/Bordeaux/Samples/B00G7JN/BAM/20141212_FrenchEx_Bordeaux_B00G7JN_final.bai
```

## Calling

### with samtools

for each segment $(1):

```makefile
$(call segment_method_vcf,$(1),samtools) : ${SCRATCHDIR}/FrenchEx/BAM/${output.prefix}bams.list ${extended.capture.bed}
	mkdir -p $$(dir $$@)   &&  \
	awk -F '	' $(call segment_awk,$(1)) ${extended.capture.bed} > $$(addsuffix .tmp.bed,$$@)  &&  \
	$${samtools.exe} mpileup --uncompressed --BCF --output-tags DP,DV,DP4,SP  \
		--max-depth 97200 \
		--max-idepth 97200  \
		--redo-BAQ --adjust-MQ 50 --min-ireads 3 --gap-frac 0.002  --min-MQ 30 \
		--positions  $$(addsuffix .tmp.bed,$$@) \
		--region $(1) \
		--fasta-ref $$(REF) \
		--bam-list $$<  |\
	$${bcftools.exe} call --output-type u --variants-only --multiallelic-caller --format-fields GQ,GP  - |\
	$${bcftools.exe} filter --exclude 'MAX(DP)<10' --output-type v - > $$(addsuffix .tmp.vcf,$$@)  &&  \
	$${bgzip.exe} -f $$(addsuffix .tmp.vcf,$$@)  &&  \
	mv $$(addsuffix .tmp.vcf.gz,$$@)  $$@  &&  \
	rm -f $$(addsuffix .tmp.bed,$$@)
```

### With unified Genotyper

```makefile
	mkdir -p $$(dir $$@)  &&  \
	awk -F '	' $(call segment_awk,$(1)) ${extended.capture.bed} > $$(addsuffix .tmp.bed,$$@)  &&  \
	$${java.exe} -XX:ParallelGCThreads=8 -Xmx20g -Djava.io.tmpdir=$$(dir $$@) -jar $${gatk.jar} -T UnifiedGenotyper \
		-R $$(REF) $${gatk.no.phone.home} \
		--validation_strictness LENIENT \
		-glm BOTH -nt 16 -L $$(addsuffix .tmp.bed,$$@) \
		-I  $$< \
		--dbsnp:vcfinput,VCF $${gatk.bundle.dbsnp.vcf} \
		-o $$(addsuffix .tmp.vcf,$$@)&&  \
	$${bgzip.exe} -f $$(addsuffix .tmp.vcf,$$@)  &&  \
	mv  $$(addsuffix .tmp.vcf.gz,$$@) $$@  &&  \
	rm  -f $$(addsuffix .tmp.vcf.idx,$$@)  $$(addsuffix .tmp.bed,$$@) 

```

## Segment annotation

```makefile
$(call segment_method_annot_vcf,$(1),$(2))  : $(call segment_method_vcf,$(1),$(2))
	mkdir -p $$(dir $$@)  &&  \
	gunzip -c $$< | ${java.exe} -jar $$(call jvarkit,nozerovariationvcf) -r $(REF) |\
	${java.exe}   -Djava.io.tmpdir=$$(dir $$@) -jar $${snpeff.jar} eff  \
			-i vcf -o vcf  -nodownload  -sequenceOntology \
			-lof -csvStats  -nextProt  -oicr -csvStats \
			$$(foreach P,CD4 GM06990 GM12878 H1ESC HMEC HSMM HUVEC HeLa-S3 HepG2 IMR90 K562 K562b NH-A NHEK, -reg $${P} ) \
			-c $${snpeff.config} \
			-s $$(addsuffix .snpeff.report,$$@) \
			GRCh37.75  > $$(addsuffix .tmp1.vcf, $$@ )  &&  \
	${vep.exe} \
		--cache --dir $${vep.cache} --write_cache \
		--species homo_sapiens_merged \
		--assembly  GRCh37  --db_version 78 \
		--fasta $$(REF) \
		--offline  --symbol --format vcf --force_overwrite \
		--sift=b --polyphen=b  --xref_refseq \
		-i $$(addsuffix .tmp1.vcf, $$@ ) \
		-o $$(addsuffix .tmp3.vcf, $$@ ) \
		--quiet --vcf --no_stats \
	$$(if $$(findstring samtools,$(2)),  &&   ${java.exe}  -XX:ParallelGCThreads=2 -Xmx250m  -Djava.io.tmpdir=$$(dir $$@) -jar ${gatk.jar} -T VariantA
nnotator  -R $(REF) ${gatk.no.phone.home}  --variant $$(addsuffix .tmp3.vcf, $$@ ) -L $$(addsuffix .tmp3.vcf, $$@ ) -o $$(addsuffix .tmp1.vcf, $$@ ) --dbs
np ${gatk.bundle.dbsnp.vcf}  &&    mv  $$(addsuffix .tmp1.vcf, $$@ ) $$(addsuffix .tmp3.vcf, $$@)  )  &&  \
	grep -vwF FAKESNP $$(addsuffix .tmp3.vcf, $$@) > $$(addsuffix .tmp2.vcf, $$@)  &&  \
	$${bgzip.exe} -f $$(addsuffix .tmp2.vcf, $$@ )  &&  \
	mv $$(addsuffix .tmp2.vcf.gz, $$@ ) $$@  &&  \
	rm -f $$(addsuffix .tmp1.vcf, $$@ ) $$(addsuffix .tmp3.vcf, $$@ )
```

## Merge VCF segments


```makefile
$$(call merge_method_vcf_filename,$(1)) : $$(call segment_method_list,$(1))
	mkdir -p $$(dir $$@ )  &&  \
	perl -I ${vcftools.dir}/lib/perl5/site_perl  ${vcftools.dir}/perl/vcf-concat -f $$< > $$(addsuffix .tmp.vcf,$$(basename $$@))  &&  \
	$${java.exe}  -XX:ParallelGCThreads=10 -Xmx30g  -Djava.io.tmpdir=$$(dir $$@)  -jar $${picard.jar} SortVcf \
		INPUT=$$(addsuffix .tmp.vcf,$$(basename $$@)) \
		MAX_RECORDS_IN_RAM=10000 \
		TMP_DIR=$$(dir $$@) \
		VALIDATION_STRINGENCY=SILENT \
		CREATE_INDEX=false \
		OUTPUT=$$(basename $$@)  &&  \
	rm -f $$(addsuffix .tmp.vcf,$$(basename $$@))  &&  \
	$${bgzip.exe} -f $$(basename $$@)  &&  \
	$${tabix.exe} -f -p vcf $$@ 
```






