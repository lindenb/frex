# Compare VCF

## VCF pre-treatment & comparaison

Each VCF was

* filtered with the capture region
* variant left aligned with https://github.com/lindenb/jvarkit/wiki/VCFFixIndels
* sorted on `contigs`(chrom/pos)

Samples were compared using  https://github.com/lindenb/jvarkit/wiki/VcfCompareCallers


## Lefort vs Lindenbaum/UnifiedGenotyper

File [diff_20150313LefortAdes_20141212LindenbUnifiedGenotyper.tsv](diff_20150313LefortAdes_20141212LindenbUnifiedGenotyper.tsv)

### Fraction of Variants Uniq to Lefort

```
$ grep -v Sample diff_20150313LefortAdes_20141212LindenbUnifiedGenotyper.tsv |
awk '{print $2 / ($2+$5+$8+$9)}' | sort -n |
gnuplot -e "set terminal dumb  ; set title 'Lefort vs Lindenb/GATK-UGT: uniq Lefort'; set xlabel 'Sample' ; plot '-' with lines notitle;"
```

About 1% variants found in Lefort but not in Lindenb

```

                        Lefort vs Lindenb/GATK-UGT: uniq Lefort

    0.0171 ++-----+-----+------+-----+------+-----+------+-----+------+----++
           +      +     +      +     +      +     +      +     +      +     +
           |                                                            *   |
     0.017 ++                                                           *  ++
           |                                                            *   |
           |                                                           *    |
    0.0169 ++                                                          *   ++
           |                                                          **    |
           |                                                         **     |
    0.0168 ++                                                   ******     ++
           |                                          ***********           |
           |                         ******************                     |
    0.0167 ++          ***************                                     ++
           |   ********                                                     |
           | ***                                                            |
    0.0166 +**                                                             ++
           **                                                               |
           *      +     +      +     +      +     +      +     +      +     +
    0.0165 *+-----+-----+------+-----+------+-----+------+-----+------+----++
           0      50   100    150   200    250   300    350   400    450   500
                                        Sample

```

### Fraction of Variants Uniq to Lindenbaum

```
$ grep -v Sample diff_20150313LefortAdes_20141212LindenbUnifiedGenotyper.tsv |\
awk '{print $5 / ($2+$5+$8+$9)}' | sort -n |\
gnuplot -e "set terminal dumb  ; set title 'Lefort vs Lindenb/GATK-UGT: uniq Lindenb'; set xlabel 'Sample' ; plot '-' with lines notitle;"
```

About 3% variants found in Lindenb but not in Lefort


```

                       Lefort vs Lindenb/GATK-UGT: uniq Lindenb

    0.0396 ++-----+-----+------+-----+------+-----+------+-----+------+----++
           +      +     +      +     +      +     +      +     +      + *   +
    0.0394 ++                                                           *  ++
           |                                                           *    |
           |                                                           *    |
    0.0392 ++                                                          *   ++
           |                                                           *    |
     0.039 ++                                                          *   ++
           |                                                           *    |
    0.0388 ++                                                          *   ++
           |                                                           *    |
    0.0386 ++                                                          *   ++
           |                                                           *    |
           |                                                          **    |
    0.0384 ++                                                  ********    ++
           |                          **************************            |
    0.0382 ++   **********************                                     ++
           ****** +     +      +     +      +     +      +     +      +     +
     0.038 ++-----+-----+------+-----+------+-----+------+-----+------+----++
           0      50   100    150   200    250   300    350   400    450   500
                                        Sample
```

### Genotype Concordance

```
$ grep -v Sample diff_20150313LefortAdes_20141212LindenbUnifiedGenotyper.tsv | awk '{print $13 / ($13+$17)}' |\
 sort -n | gnuplot -e "set terminal dumb  ; set title 'Genotype Concordance'; set xlabel 'Sample' ; plot '-' with lines notitle;"
```

99% of common variants are called the same way.

```

                                 Genotype Concordance

     0.995 ++-----+-----+------+-----+------+-----+------+-----+------+----++
           +      +     +      +     +      +     +      +     +      +     +
           |                                                          ***   |
    0.9945 ++                                                  ********    ++
           |                                 *******************            |
           |              ********************                              |
     0.994 ++ *************                                                ++
           |***                                                             |
           |*                                                               |
    0.9935 +*                                                              ++
           |*                                                               |
           *                                                                |
     0.993 *+                                                              ++
           *                                                                |
           *                                                                |
    0.9925 *+                                                              ++
           *                                                                |
           +      +     +      +     +      +     +      +     +      +     +
     0.992 ++-----+-----+------+-----+------+-----+------+-----+------+----++
           0      50   100    150   200    250   300    350   400    450   500
                                        Sample

```

### A Few Variant in each category:


<table>
<tr><th>Category</th><th>Sample</th><th>Variant 1</th><th>Genotype 1</th><th>Variant 2</th><th>Genotype 2</th></tr>
<tr><td>both_missing</td><td>B00G5XG</td><td>(SNP) 1:69494 . T*/[C]</td><td/><td>(SNP) 1:69494 . T*/[C]</td><td/></tr>
<tr><td>both_missing</td><td>B00G5XG</td><td>(SNP) 1:69495 . C*/[T]</td><td/><td>(SNP) 1:69495 . C*/[T]</td><td/></tr>
<tr><td>both_missing</td><td>B00G5XG</td><td>(SNP) 1:69511 rs75062661 A*/[G]</td><td/><td>(SNP) 1:69511 rs75062661 A*/[G]</td><td/></tr>
<tr><td>both_missing</td><td>B00G5XG</td><td>(SNP) 1:69552 rs2531266 G*/[C]</td><td/><td>(SNP) 1:69552 rs55874132 G*/[C]</td><td/></tr>
<tr><td>both_missing</td><td>B00G5XG</td><td/><td/><td>(SNP) 1:1167597 . C*/[G]</td><td/></tr>
<tr><td>both_missing</td><td>B00G5XG</td><td>(SNP) 1:1167796 rs190796582 C*/[T]</td><td/><td>(SNP) 1:1167796 rs190796582 C*/[T]</td><td/></tr>
<tr><td>called_and_same</td><td>B00G5XG</td><td>(SNP) 1:14653 rs62635297 C*/[T]</td><td>(HET)C*/T DP:11</td><td>(SNP) 1:14653 rs375086259 C*/[T]</td><td>(HET)C*/T DP:20</td></tr>
<tr><td>called_and_same</td><td>B00G5XG</td><td>(SNP) 1:14671 rs201055865 G*/[C]</td><td>(HOM_REF)G*/G* DP:11</td><td>(SNP) 1:14671 rs201055865 G*/[C]</td><td>(HOM_REF)G*/G* DP:20</td></tr>
<tr><td>called_and_same</td><td>B00G5XG</td><td>(SNP) 1:14671 rs201055865 G*/[C]</td><td>(HOM_REF)G*/G* DP:11</td><td>(SNP) 1:14671 rs201055865 G*/[C]</td><td>(HOM_REF)G*/G* DP:20</td></tr>
<tr><td>called_and_same</td><td>B00G5XG</td><td>(SNP) 1:14677 rs201327123 G*/[A]</td><td>(HET)G*/A DP:14</td><td>(SNP) 1:14677 rs201327123 G*/[A]</td><td>(HET)G*/A DP:22</td></tr>
<tr><td>called_and_same</td><td>B00G5XG</td><td>(SNP) 1:14696 . C*/[A]</td><td>(HOM_REF)C*/C* DP:12</td><td>(SNP) 1:14696 . C*/[A]</td><td>(HOM_REF)C*/C* DP:20</td></tr>
<tr><td>called_and_same</td><td>B00G5XG</td><td>(SNP) 1:14696 . C*/[A]</td><td>(HOM_REF)C*/C* DP:12</td><td>(SNP) 1:14696 . C*/[A]</td><td>(HOM_REF)C*/C* DP:20</td></tr>
<tr><td>called_and_same_het</td><td>B00G5XG</td><td>(SNP) 1:14653 rs62635297 C*/[T]</td><td>(HET)C*/T DP:11</td><td>(SNP) 1:14653 rs375086259 C*/[T]</td><td>(HET)C*/T DP:20</td></tr>
<tr><td>called_and_same_het</td><td>B00G5XG</td><td>(SNP) 1:14677 rs201327123 G*/[A]</td><td>(HET)G*/A DP:14</td><td>(SNP) 1:14677 rs201327123 G*/[A]</td><td>(HET)G*/A DP:22</td></tr>
<tr><td>called_and_same_het</td><td>B00G5XG</td><td>(SNP) 1:878314 rs142558220 G*/[C]</td><td>(HET)G*/C DP:109</td><td>(SNP) 1:878314 rs142558220 G*/[C]</td><td>(HET)G*/C DP:114</td></tr>
<tr><td>called_and_same_het</td><td>B00G5XG</td><td>(SNP) 1:878565 rs548173587 C*/[T]</td><td>(HET)C*/T DP:69</td><td>(SNP) 1:878565 . C*/[T]</td><td>(HET)C*/T DP:72</td></tr>
<tr><td>called_and_same_het</td><td>B00G5XG</td><td>(SNP) 1:881627 rs2272757 G*/[A]</td><td>(HET)G*/A DP:50</td><td>(SNP) 1:881627 rs2272757 G*/[A]</td><td>(HET)G*/A DP:51</td></tr>
<tr><td>called_and_same_het</td><td>B00G5XG</td><td>(SNP) 1:900505 rs28705211 G*/[C]</td><td>(HET)G*/C DP:106</td><td>(SNP) 1:900505 rs28705211 G*/[C]</td><td>(HET)G*/C DP:119</td></tr>
<tr><td>called_and_same_hom_var</td><td>B00G5XG</td><td>(SNP) 1:762273 rs3115849 G*/[A]</td><td>(HOM_VAR)A/A DP:97</td><td>(SNP) 1:762273 rs3115849 G*/[A]</td><td>(HOM_VAR)A/A DP:112</td></tr>
<tr><td>called_and_same_hom_var</td><td>B00G5XG</td><td>(SNP) 1:876499 rs4372192 A*/[G]</td><td>(HOM_VAR)G/G DP:23</td><td>(SNP) 1:876499 rs4372192 A*/[G]</td><td>(HOM_VAR)G/G DP:23</td></tr>
<tr><td>called_and_same_hom_var</td><td>B00G5XG</td><td>(SNP) 1:877715 rs6605066 C*/[G]</td><td>(HOM_VAR)G/G DP:6</td><td>(SNP) 1:877715 rs6605066 C*/[G]</td><td>(HOM_VAR)G/G DP:6</td></tr>
<tr><td>called_and_same_hom_var</td><td>B00G5XG</td><td>(SNP) 1:877831 rs6672356 T*/[C]</td><td>(HOM_VAR)C/C DP:10</td><td>(SNP) 1:877831 rs6672356 T*/[C]</td><td>(HOM_VAR)C/C DP:12</td></tr>
<tr><td>called_and_same_hom_var</td><td>B00G5XG</td><td>(SNP) 1:880238 rs3748592 A*/[G]</td><td>(HOM_VAR)G/G DP:106</td><td>(SNP) 1:880238 rs3748592 A*/[G]</td><td>(HOM_VAR)G/G DP:107</td></tr>
<tr><td>called_and_same_hom_var</td><td>B00G5XG</td><td>(SNP) 1:883625 rs4970378 A*/[G]</td><td>(HOM_VAR)G/G DP:58</td><td>(SNP) 1:883625 rs4970378 A*/[G]</td><td>(HOM_VAR)G/G DP:60</td></tr>
<tr><td>called_but_discordant</td><td>B00G5XG</td><td>(SNP) 1:65872 . T*/[G]</td><td>(HOM_REF)T*/T* DP:8</td><td>(SNP) 1:65872 . T*/[G]</td><td>(HET)T*/G DP:217</td></tr>
<tr><td>called_but_discordant</td><td>B00G5XG</td><td>(SNP) 1:762345 rs74045218 A*/[G]</td><td>(HOM_REF)A*/A* DP:63</td><td>(SNP) 1:762345 rs74045218 A*/[G]</td><td>(HET)A*/G DP:71</td></tr>
<tr><td>called_but_discordant</td><td>B00G5XG</td><td>(SNP) 1:1247589 . C*/[G]</td><td>(HOM_REF)C*/C* DP:12</td><td>(SNP) 1:1247589 . C*/[G]</td><td>(HET)C*/G DP:18</td></tr>
<tr><td>called_but_discordant</td><td>B00G5XG</td><td>(MIXED) 1:1404665 . TAA*/[TA, T, AAA]</td><td>(HOM_REF)TAA*/TAA* DP:13</td><td>(INDEL) 1:1404665 . TAA*/[TAAA, T, TA]</td><td>(HET)TAA*/T DP:20</td></tr>
<tr><td>called_but_discordant</td><td>B00G5XG</td><td>(INDEL) 1:1405057 . GT*/[G, GTT]</td><td>(HOM_REF)GT*/GT* DP:14</td><td>(INDEL) 1:1405057 . GT*/[GTT, G]</td><td>(HET)GT*/G DP:19</td></tr>
<tr><td>called_but_discordant</td><td>B00G5XG</td><td>(SNP) 1:1594393 rs141732428 T*/[A]</td><td>(HOM_REF)T*/T* DP:152</td><td>(SNP) 1:1594393 rs141732428 T*/[A]</td><td>(HET)T*/A DP:162</td></tr>
<tr><td>called_but_discordant_het1_het2</td><td>B00G5XG</td><td>(INDEL) 1:7980364 . CTT*/[C, CT, CTTT, CTTTT]</td><td>(HET)CTT*/CT DP:21</td><td>(INDEL) 1:7980364 . CTT*/[CTTT, C, CT, CTTTT]</td><td>(HET)CTTT/CT DP:68</td></tr>
<tr><td>called_but_discordant_het1_het2</td><td>B00G5XG</td><td>(MIXED) 1:9163163 . TA*/[AA, TAA, TAAA, TAAAA, T, TAAAAA]</td><td>(HET)TAAA/T DP:41</td><td>(INDEL) 1:9163163 . TA*/[TAA, TAAAA, TTAA, TAAAAA, TAAA, T]</td><td>(HET)TA*/TAAA DP:113</td></tr>
<tr><td>called_but_discordant_het1_het2</td><td>B00G5XG</td><td>(INDEL) 1:19585758 rs372637349 CAA*/[CA, C]</td><td>(HET)CAA*/C DP:10</td><td>(INDEL) 1:19585758 . CAA*/[CAAA, C, CA]</td><td>(HET)CAA*/CA DP:15</td></tr>
<tr><td>called_but_discordant_het1_het2</td><td>B00G5XG</td><td>(MIXED) 1:25227527 . GT*/[TT, G, GTTTTTTTTT, GTTTTTTTTTT, GTT, GTTT]</td><td>(HET)GT*/TT DP:18</td><td>(INDEL) 1:25227527 . GT*/[GTT, G, GTTT]</td><td>(HET)GT*/GTT DP:28</td></tr>
<tr><td>called_but_discordant_het1_het2</td><td>B00G5XG</td><td>(INDEL) 1:31838551 . TA*/[T, TAA, TAAA]</td><td>(HET)TA*/T DP:20</td><td>(INDEL) 1:31838551 . TA*/[TAAA, TAA, T]</td><td>(HET)TA*/TAA DP:57</td></tr>
<tr><td>called_but_discordant_het1_het2</td><td>B00G5XG</td><td>(INDEL) 1:34254108 . CAA*/[C, CA, CAAA, CAAAA, CAAAAA]</td><td>(HET)CAA*/CA DP:25</td><td>(INDEL) 1:34254108 . CAA*/[CAAA, CAAAA, C, CA]</td><td>(HET)CAAA/CA DP:50</td></tr>
<tr><td>called_but_discordant_het1_hom2</td><td>B00G5XG</td><td>(MIXED) 1:3731654 rs113041467 C*/[CT, T]</td><td>(HET)C*/CT DP:102</td><td>(SNP) 1:3731654 rs113041467 C*/[T]</td><td>(HOM_REF)C*/C* DP:109</td></tr>
<tr><td>called_but_discordant_het1_hom2</td><td>B00G5XG</td><td>(SNP) 1:3816893 rs71634382 C*/[T]</td><td>(HET)C*/T DP:9</td><td>(SNP) 1:3816893 rs71634382 C*/[T]</td><td>(HOM_REF)C*/C* DP:9</td></tr>
<tr><td>called_but_discordant_het1_hom2</td><td>B00G5XG</td><td>(SNP) 1:12907507 rs12409975 C*/[T]</td><td>(HET)C*/T DP:102</td><td>(SNP) 1:12907507 rs12409975 C*/[T]</td><td>(HOM_REF)C*/C* DP:119</td></tr>
<tr><td>called_but_discordant_het1_hom2</td><td>B00G5XG</td><td>(SNP) 1:12907508 rs138145027 T*/[C]</td><td>(HET)T*/C DP:101</td><td>(SNP) 1:12907508 rs138145027 T*/[C]</td><td>(HOM_REF)T*/T* DP:118</td></tr>
<tr><td>called_but_discordant_het1_hom2</td><td>B00G5XG</td><td>(SNP) 1:16066753 . A*/[C]</td><td>(HET)A*/C DP:14</td><td>(SNP) 1:16066753 . A*/[C]</td><td>(HOM_REF)A*/A* DP:16</td></tr>
<tr><td>called_but_discordant_het1_hom2</td><td>B00G5XG</td><td>(INDEL) 1:17299006 rs36118977 G*/[GC]</td><td>(HET)G*/GC DP:14</td><td>(INDEL) 1:17299006 rs36118977 G*/[GC]</td><td>(HOM_VAR)GC/GC DP:26</td></tr>
<tr><td>called_but_discordant_hom1_het2</td><td>B00G5XG</td><td>(SNP) 1:65872 . T*/[G]</td><td>(HOM_REF)T*/T* DP:8</td><td>(SNP) 1:65872 . T*/[G]</td><td>(HET)T*/G DP:217</td></tr>
<tr><td>called_but_discordant_hom1_het2</td><td>B00G5XG</td><td>(SNP) 1:762345 rs74045218 A*/[G]</td><td>(HOM_REF)A*/A* DP:63</td><td>(SNP) 1:762345 rs74045218 A*/[G]</td><td>(HET)A*/G DP:71</td></tr>
<tr><td>called_but_discordant_hom1_het2</td><td>B00G5XG</td><td>(SNP) 1:1247589 . C*/[G]</td><td>(HOM_REF)C*/C* DP:12</td><td>(SNP) 1:1247589 . C*/[G]</td><td>(HET)C*/G DP:18</td></tr>
<tr><td>called_but_discordant_hom1_het2</td><td>B00G5XG</td><td>(MIXED) 1:1404665 . TAA*/[TA, T, AAA]</td><td>(HOM_REF)TAA*/TAA* DP:13</td><td>(INDEL) 1:1404665 . TAA*/[TAAA, T, TA]</td><td>(HET)TAA*/T DP:20</td></tr>
<tr><td>called_but_discordant_hom1_het2</td><td>B00G5XG</td><td>(INDEL) 1:1405057 . GT*/[G, GTT]</td><td>(HOM_REF)GT*/GT* DP:14</td><td>(INDEL) 1:1405057 . GT*/[GTT, G]</td><td>(HET)GT*/G DP:19</td></tr>
<tr><td>called_but_discordant_hom1_het2</td><td>B00G5XG</td><td>(SNP) 1:1594393 rs141732428 T*/[A]</td><td>(HOM_REF)T*/T* DP:152</td><td>(SNP) 1:1594393 rs141732428 T*/[A]</td><td>(HET)T*/A DP:162</td></tr>
<tr><td>called_but_discordant_hom1_hom2</td><td>B00G5XG</td><td>(MIXED) 1:6161100 rs78653623 T*/[TA, A]</td><td>(HOM_VAR)TA/TA DP:123</td><td>(SNP) 1:6161100 rs78653623 T*/[A]</td><td>(HOM_REF)T*/T* DP:128</td></tr>
<tr><td>called_but_discordant_hom1_hom2</td><td>B00G5XG</td><td>(INDEL) 1:7827903 rs34335657 C*/[CAA, CA, CAAA]</td><td>(HOM_VAR)CAA/CAA DP:8</td><td>(SNP) 1:7827903 rs201601214 C*/[A]</td><td>(HOM_REF)C*/C* DP:19</td></tr>
<tr><td>called_but_discordant_hom1_hom2</td><td>B00G5XG</td><td>(INDEL) 1:11780650 . C*/[CAAAAA]</td><td>(HOM_REF)C*/C* DP:11</td><td>(INDEL) 1:11780650 . C*/[CAAA, CA]</td><td>(HOM_VAR)CA/CA DP:12</td></tr>
<tr><td>called_but_discordant_hom1_hom2</td><td>B00G5XG</td><td>(INDEL) 1:15439083 rs140963536 A*/[AAGGTCTTTTTTTTCTTCTCCCAGCGGC, AAGGTCTTTTTTTCTTCTCCCAGCGGC]</td><td>(HOM_VAR)AAGGTCTTTTTTTTCTTCTCCCAGCGGC/AAGGTCTTTTTTTTCTTCTCCCAGCGGC DP:42</td><td>(INDEL) 1:15439083 . A*/[AAGGTCTTTTTTTCTTCTCCCA]</td><td>(HOM_VAR)AAGGTCTTTTTTTCTTCTCCCA/AAGGTCTTTTTTTCTTCTCCCA DP:60</td></tr>
<tr><td>called_but_discordant_hom1_hom2</td><td>B00G5XG</td><td>(INDEL) 1:17299013 rs201543544 CA*/[C]</td><td>(HOM_REF)CA*/CA* DP:22</td><td>(INDEL) 1:17299013 rs201543544 CA*/[C]</td><td>(HOM_VAR)C/C DP:24</td></tr>
<tr><td>called_but_discordant_hom1_hom2</td><td>B00G5XG</td><td>(INDEL) 1:44451114 rs371619921 GA*/[G]</td><td>(HOM_REF)GA*/GA* DP:8</td><td>(INDEL) 1:44451114 rs371619921 GA*/[G]</td><td>(HOM_VAR)G/G DP:11</td></tr>
<tr><td>common_context_discordant_id</td><td>B00G5XG</td><td>(SNP) 1:14653 rs62635297 C*/[T]</td><td>(HET)C*/T DP:11</td><td>(SNP) 1:14653 rs375086259 C*/[T]</td><td>(HET)C*/T DP:20</td></tr>
<tr><td>common_context_discordant_id</td><td>B00G5XG</td><td>(SNP) 1:721538 rs569523401 A*/[C]</td><td>(HOM_REF)A*/A* DP:33</td><td>(SNP) 1:721538 . A*/[C]</td><td>(HOM_REF)A*/A* DP:80</td></tr>
<tr><td>common_context_discordant_id</td><td>B00G5XG</td><td>(SNP) 1:721694 rs538190743 A*/[G]</td><td>(HOM_REF)A*/A* DP:33</td><td>(SNP) 1:721694 . A*/[G]</td><td>(HOM_REF)A*/A* DP:79</td></tr>
<tr><td>common_context_discordant_id</td><td>B00G5XG</td><td>(SNP) 1:752950 rs548488495 C*/[T]</td><td>(HOM_REF)C*/C* DP:3</td><td>(SNP) 1:752950 . C*/[T]</td><td>(HOM_REF)C*/C* DP:184</td></tr>
<tr><td>common_context_discordant_id</td><td>B00G5XG</td><td>(SNP) 1:762109 rs540936498 C*/[T]</td><td>(HOM_REF)C*/C* DP:38</td><td>(SNP) 1:762109 . C*/[T]</td><td>(HOM_REF)C*/C* DP:250</td></tr>
<tr><td>common_context_discordant_id</td><td>B00G5XG</td><td>(SNP) 1:762409 rs563839199 G*/[C]</td><td>(HOM_REF)G*/G* DP:29</td><td>(SNP) 1:762409 . G*/[C]</td><td>(HOM_REF)G*/G* DP:35</td></tr>
<tr><td>common_context_indel</td><td>B00G5XG</td><td>(INDEL) 1:721750 . TTTA*/[T]</td><td>(HOM_REF)TTTA*/TTTA* DP:26</td><td>(INDEL) 1:721750 . TTTA*/[T]</td><td>(HOM_REF)TTTA*/TTTA* DP:27</td></tr>
<tr><td>common_context_indel</td><td>B00G5XG</td><td>(INDEL) 1:777465 . G*/[GTGCC]</td><td>(HOM_REF)G*/G* DP:38</td><td>(INDEL) 1:777465 . G*/[GTGCC]</td><td>(HOM_REF)G*/G* DP:48</td></tr>
<tr><td>common_context_indel</td><td>B00G5XG</td><td>(INDEL) 1:879497 . TC*/[T]</td><td>(HOM_REF)TC*/TC* DP:39</td><td>(INDEL) 1:879497 . TC*/[T]</td><td>(HOM_REF)TC*/TC* DP:219</td></tr>
<tr><td>common_context_indel</td><td>B00G5XG</td><td>(INDEL) 1:879789 rs145942386 CCTGGAA*/[C]</td><td>(HOM_REF)CCTGGAA*/CCTGGAA* DP:22</td><td>(INDEL) 1:879789 rs145942386 CCTGGAA*/[C]</td><td>(HOM_REF)CCTGGAA*/CCTGGAA* DP:23</td></tr>
<tr><td>common_context_indel</td><td>B00G5XG</td><td>(INDEL) 1:897475 rs538369199 AGTCTTT*/[A]</td><td>(HOM_REF)AGTCTTT*/AGTCTTT* DP:76</td><td>(INDEL) 1:897475 . AGTCTTT*/[A]</td><td>(HOM_REF)AGTCTTT*/AGTCTTT* DP:112</td></tr>
<tr><td>common_context_indel</td><td>B00G5XG</td><td>(INDEL) 1:900717 rs142545439 CTTAT*/[C]</td><td>(HET)CTTAT*/C DP:36</td><td>(INDEL) 1:900717 rs374537094 CTTAT*/[C]</td><td>(HET)CTTAT*/C DP:42</td></tr>
<tr><td>common_context_snp</td><td>B00G5XG</td><td>(SNP) 1:14653 rs62635297 C*/[T]</td><td>(HET)C*/T DP:11</td><td>(SNP) 1:14653 rs375086259 C*/[T]</td><td>(HET)C*/T DP:20</td></tr>
<tr><td>common_context_snp</td><td>B00G5XG</td><td>(SNP) 1:14671 rs201055865 G*/[C]</td><td>(HOM_REF)G*/G* DP:11</td><td>(SNP) 1:14671 rs201055865 G*/[C]</td><td>(HOM_REF)G*/G* DP:20</td></tr>
<tr><td>common_context_snp</td><td>B00G5XG</td><td>(SNP) 1:14677 rs201327123 G*/[A]</td><td>(HET)G*/A DP:14</td><td>(SNP) 1:14677 rs201327123 G*/[A]</td><td>(HET)G*/A DP:22</td></tr>
<tr><td>common_context_snp</td><td>B00G5XG</td><td>(SNP) 1:14696 . C*/[A]</td><td>(HOM_REF)C*/C* DP:12</td><td>(SNP) 1:14696 . C*/[A]</td><td>(HOM_REF)C*/C* DP:20</td></tr>
<tr><td>common_context_snp</td><td>B00G5XG</td><td>(SNP) 1:65872 . T*/[G]</td><td>(HOM_REF)T*/T* DP:8</td><td>(SNP) 1:65872 . T*/[G]</td><td>(HET)T*/G DP:217</td></tr>
<tr><td>common_context_snp</td><td>B00G5XG</td><td>(SNP) 1:65892 . C*/[T]</td><td>(HOM_REF)C*/C* DP:11</td><td>(SNP) 1:65892 . C*/[T]</td><td>(HOM_REF)C*/C* DP:242</td></tr>
<tr><td>unique_to_file_1</td><td>B00G5XG</td><td>(INDEL) 1:874778 rs568340123 GCCTCCCCAGCCACGGTGAGGACCCACCCTGGCATGATCCCCCTCATCA*/[G]</td><td>(HET)GCCTCCCCAGCCACGGTGAGGACCCACCCTGGCATGATCCCCCTCATCA*/G DP:41</td><td/><td/></tr>
<tr><td>unique_to_file_1</td><td>B00G5XG</td><td>(INDEL) 1:874816 . CCCCCTCATCACCTCCCCAGCCACGGTGAGGACCCACCCTGGCATGATCT*/[C]</td><td>(HOM_REF)CCCCCTCATCACCTCCCCAGCCACGGTGAGGACCCACCCTGGCATGATCT*/CCCCCTCATCACCTCCCCAGCCACGGTGAGGACCCACCCTGGCATGATCT* DP:41</td><td/><td/></tr>
<tr><td>unique_to_file_1</td><td>B00G5XG</td><td>(INDEL) 1:879848 . ACTTCAGCAGCCCACAGCTGTGGGG*/[A]</td><td>(HOM_REF)ACTTCAGCAGCCCACAGCTGTGGGG*/ACTTCAGCAGCCCACAGCTGTGGGG* DP:27</td><td/><td/></tr>
<tr><td>unique_to_file_1</td><td>B00G5XG</td><td>(INDEL) 1:881996 . CAAAA*/[C]</td><td>(HOM_REF)CAAAA*/CAAAA* DP:36</td><td/><td/></tr>
<tr><td>unique_to_file_1</td><td>B00G5XG</td><td>(INDEL) 1:884041 . C*/[CCCTGGCTGCACCCTGGTCCCCCTGGTCCCTTTGGCCCTACA]</td><td>(HOM_REF)C*/C* DP:42</td><td/><td/></tr>
<tr><td>unique_to_file_1</td><td>B00G5XG</td><td>(SNP) 1:897465 . A*/[C]</td><td>(HOM_REF)A*/A* DP:76</td><td/><td/></tr>
<tr><td>unique_to_file_1_indel</td><td>B00G5XG</td><td>(INDEL) 1:874778 rs568340123 GCCTCCCCAGCCACGGTGAGGACCCACCCTGGCATGATCCCCCTCATCA*/[G]</td><td>(HET)GCCTCCCCAGCCACGGTGAGGACCCACCCTGGCATGATCCCCCTCATCA*/G DP:41</td><td/><td/></tr>
<tr><td>unique_to_file_1_indel</td><td>B00G5XG</td><td>(INDEL) 1:874816 . CCCCCTCATCACCTCCCCAGCCACGGTGAGGACCCACCCTGGCATGATCT*/[C]</td><td>(HOM_REF)CCCCCTCATCACCTCCCCAGCCACGGTGAGGACCCACCCTGGCATGATCT*/CCCCCTCATCACCTCCCCAGCCACGGTGAGGACCCACCCTGGCATGATCT* DP:41</td><td/><td/></tr>
<tr><td>unique_to_file_1_indel</td><td>B00G5XG</td><td>(INDEL) 1:879848 . ACTTCAGCAGCCCACAGCTGTGGGG*/[A]</td><td>(HOM_REF)ACTTCAGCAGCCCACAGCTGTGGGG*/ACTTCAGCAGCCCACAGCTGTGGGG* DP:27</td><td/><td/></tr>
<tr><td>unique_to_file_1_indel</td><td>B00G5XG</td><td>(INDEL) 1:881996 . CAAAA*/[C]</td><td>(HOM_REF)CAAAA*/CAAAA* DP:36</td><td/><td/></tr>
<tr><td>unique_to_file_1_indel</td><td>B00G5XG</td><td>(INDEL) 1:884041 . C*/[CCCTGGCTGCACCCTGGTCCCCCTGGTCCCTTTGGCCCTACA]</td><td>(HOM_REF)C*/C* DP:42</td><td/><td/></tr>
<tr><td>unique_to_file_1_indel</td><td>B00G5XG</td><td>(INDEL) 1:1026782 rs551309425 GGGGAA*/[G]</td><td>(HOM_REF)GGGGAA*/GGGGAA* DP:47</td><td/><td/></tr>
<tr><td>unique_to_file_1_snp</td><td>B00G5XG</td><td>(SNP) 1:897465 . A*/[C]</td><td>(HOM_REF)A*/A* DP:76</td><td/><td/></tr>
<tr><td>unique_to_file_1_snp</td><td>B00G5XG</td><td>(SNP) 1:897470 . A*/[C]</td><td>(HOM_REF)A*/A* DP:76</td><td/><td/></tr>
<tr><td>unique_to_file_1_snp</td><td>B00G5XG</td><td>(SNP) 1:907623 . A*/[G]</td><td>(HOM_REF)A*/A* DP:34</td><td/><td/></tr>
<tr><td>unique_to_file_1_snp</td><td>B00G5XG</td><td>(SNP) 1:1228318 . A*/[G]</td><td>(HOM_REF)A*/A* DP:38</td><td/><td/></tr>
<tr><td>unique_to_file_1_snp</td><td>B00G5XG</td><td>(SNP) 1:1323947 rs149870149 C*/[G]</td><td>(HOM_REF)C*/C* DP:2</td><td/><td/></tr>
<tr><td>unique_to_file_1_snp</td><td>B00G5XG</td><td>(SNP) 1:1323965 rs146981251 A*/[C, G]</td><td>(HOM_REF)A*/A* DP:8</td><td/><td/></tr>
<tr><td>unique_to_file_2_indel</td><td>B00G5XG</td><td/><td/><td>(INDEL) 1:861473 . A*/[ATCGAGAG]</td><td>(HOM_REF)A*/A* DP:63</td></tr>
<tr><td>unique_to_file_2_indel</td><td>B00G5XG</td><td/><td/><td>(INDEL) 1:874816 rs200996316 C*/[CT]</td><td>(HET)C*/CT DP:40</td></tr>
<tr><td>unique_to_file_2_indel</td><td>B00G5XG</td><td/><td/><td>(INDEL) 1:881996 . CAA*/[C]</td><td>(HOM_REF)CAA*/CAA* DP:39</td></tr>
<tr><td>unique_to_file_2_indel</td><td>B00G5XG</td><td/><td/><td>(INDEL) 1:894122 . GA*/[G]</td><td>(HOM_REF)GA*/GA* DP:100</td></tr>
<tr><td>unique_to_file_2_indel</td><td>B00G5XG</td><td/><td/><td>(INDEL) 1:1026784 . GGAAGGGCCCACC*/[G]</td><td>(HOM_REF)GGAAGGGCCCACC*/GGAAGGGCCCACC* DP:81</td></tr>
<tr><td>unique_to_file_2_indel</td><td>B00G5XG</td><td/><td/><td>(INDEL) 1:1225810 rs35469413 C*/[CGG]</td><td>(HOM_REF)C*/C* DP:104</td></tr>
<tr><td>unique_to_file_2_snp</td><td>B00G5XG</td><td/><td/><td>(SNP) 1:14673 rs369473859 G*/[C]</td><td>(HET)G*/C DP:21</td></tr>
<tr><td>unique_to_file_2_snp</td><td>B00G5XG</td><td/><td/><td>(SNP) 1:14699 rs372910670 C*/[G]</td><td>(HET)C*/G DP:20</td></tr>
<tr><td>unique_to_file_2_snp</td><td>B00G5XG</td><td/><td/><td>(SNP) 1:762472 rs145493205 C*/[T]</td><td>(HOM_REF)C*/C* DP:36</td></tr>
<tr><td>unique_to_file_2_snp</td><td>B00G5XG</td><td/><td/><td>(SNP) 1:874373 . C*/[T]</td><td>(HOM_REF)C*/C* DP:79</td></tr>
<tr><td>unique_to_file_2_snp</td><td>B00G5XG</td><td/><td/><td>(SNP) 1:897460 . A*/[C]</td><td>(HET)A*/C DP:116</td></tr>
<tr><td>unique_to_file_2_snp</td><td>B00G5XG</td><td/><td/><td>(SNP) 1:907631 . A*/[C]</td><td>(HET)A*/C DP:112</td></tr>
</table>

difference in the context (using Pierre L's bam):

Sample **B00G5XG** and category **both_missing** at **1:69494**

```
       69491     69501        
NNNNNNNNNNNNNNNNNNNNNNNNNNNNNN
```

Sample **B00G5XG** and category **called_and_same** at **1:14653**

```
        14651     14661       
GTCAGAGCAACGGCCCAAGTCTGGGTCTGG
..........Y...................
....   ...T...................
...............    ,,,,,,,,,,,
..........T...........  ,,,,,,
..........T................. ,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..........T...................
..............................
..........T...................
..........T...................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
..........T...................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
             .................
```

Sample **B00G5XG** and category **called_and_same_het** at **1:14653**

```
        14651     14661       
GTCAGAGCAACGGCCCAAGTCTGGGTCTGG
..........Y...................
....   ...T...................
...............    ,,,,,,,,,,,
..........T...........  ,,,,,,
..........T................. ,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..........T...................
..............................
..........T...................
..........T...................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
..........T...................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
             .................
```

Sample **B00G5XG** and category **called_and_same_hom_var** at **1:762273**

```
        762271    762281      
CTGCCAAGGAGGAGCCCGCTTCTCAGTGGG
..........A...................
,   ,,,,,,a,,,,,,,,,,,,,,,,,,,
,     ,,,,a,,,,,,,,,,,,,,,,,,,
,,    ,,,,a,,,,,,,,,,,,,,,,,,,
,,    ,,,,a,,,,,,,,,,,,,,,,,,,
,c     ,,,a,,,,,,,,,,,,,,,,,,,
,,      ..A...................
,,            ,,,,,,,,,,,,,,,,
,,             ,,,,,,,,,,,,,,,
,,,            ,,,,,,,,,,,,,,,
....            ,,,,,,,,,,,,,,
,,,,            ,,,,,,,,,,,,,,
.....             ............
,,,,,               ,,,,,,,,,,
,,,,,                 ,,,,,,,,
,,,,,,                 ,,,,,,,
,,,,,,,,                ,,,,,,
,,,,,,,,                ,,,,,,
,,,,,,,,,                  ,,,
,,,,,,,,,                  ,,,
,,,,,,,,,                   ,,
,,,,,,,,,                    ,
,,,,,,,,,                    ,
,,,,,,,,,,a                  ,
..........A.                  
,,,,,,,,,,a,,                 
..........A...                
..........A...                
,,,,,,,,,,a,,,                
,,,,,,,,,,a,,,,               
,,,,,,,,,,a,,,,               
,,,,,,,,,,a,,,,               
,,,,,,,,,,                    
,,,,,,,,,,a,,,,,              
,,,,,,,,,,a,,,,,,             
,,,,,,,,,,a,,,,,,             
,,,,,,,,,,a,,,,,,             
,,,,,,,,,,a,,,,,,             
....A.....A...G...            
,,,,,,,,,,a,,,,,,,            
,,,,,,,,,,a,,,,,,,            
,,,,,,,,,,a,,,,,,,            
,,,,,,,,,,a,,,,,,,,,          
..........A..........         
,,,,,,,,,,a,,,,,,,,,,         
..........A...........        
..........A............       
,,,,,,,,,,a,,,,,,,,,,,,       
,,,,,,,,,,a,,,,,,,,,,,,       
..........A.............      
,,,,,,,,,,a,,,,,,,,,,,,,      
,,,,,,,,,,a,,,,,,,,,,,,,      
,,,,,,,,,,a,,,,,,,,,,,,,      
,,,,,,,,,,a,,,,,,,,,,,,,      
,,,,,,,,,,a,,,,,,,,,,,,,      
..........A..............     
..........A..............     
..........A................   
,,,,,,,,,,a,,,,,,,,,,,,,,,,   
,,,,,,,,,,a,,,,,,,,,,,,,,,,   
,,,,,,,,,,a,,,,,,,,,,,,,,,,   
,,,,,,,,,,a,,,,,,,,,,,,,,,,   
,,,,,,,,,,a,,,,,,,,,,,,,,,,,  
,,,,,,,,,,a,,,,,,,,,,,,,,,,,, 
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
.C........AA..G...............
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
..........A...................
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
..........A...................
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
..........A...................
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
..........A...................
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
..........A...................
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
..........A...................
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,a,,,,,,,,,,,,,,,,,,,
```

Sample **B00G5XG** and category **called_but_discordant** at **1:65872**

```
          65871     65881     
ATA*GTCCTTTTCCCCATCTTTCCCCAGGT
... ..........................
...*..........................
...*..........................
..  a,,,,,,,,,,,,,,,,,,,,,,,,,
...*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*, ,,,,,,,,,,,,,,,,,,,,,,,,
...*..,,,,,,,,,,,,,,,,,,,,,,,,
...*...   ,,,,,,,,,,,,,,,,,,,,
...*...   ,,,,,,,,,,,,,,,,,,,,
...*...,,,,,,,,,,,,,,,,,,,,,,,
...*....   ,,,,,,,,,,,,,,,,,,,
...*....    ..................
...*....      ,,,,,,,,,,,,,,,,
...*....      ,,,,,,,,,,,,,,,,
...*....      ,,,,,,,,,,,,,,,,
...*....      ,,,,,,,,,,,,,,,,
...*.....     ,,,,,,,,,,,,,,,,
...*.....        .............
...*.....        ,,,,,,,,,,,,,
...*.....         ,,,,,,,,,,,,
...*......         ,,,,,,,,,,,
...*......         ,,,,,,,,,,,
...*.......        ,,,,,,,,,,,
...*.......          ,,,,,,,,,
...*.......          ,,,,,,,,,
...*........         ,,,,,,,,,
...*.......G..        ........
...*..........        ,,,,,,,,
...*...........        ,,,,,,,
...*............       ,,,,,,,
...*.............      ,,,,,,,
...*.................  ,,,,,,,
,,,*,,,,,,,,,,,,,,,,,   ......
...*................... ,,,,,,
...*.................... ,,,,,
...*.................... ,,,,,
,,,*,,,,,,,g,,,,,,,,,,,,, ,,,,
...*....................... ..
...*........................ ,
...*........................ ,
...*........................ ,
...*......................... 
...*.......G................. 
...*..........................
...*..........................
...*..........................
...*..........................
...*..........................
...*..........................
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
...*..........................
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.......G..................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*....................G.....
...*..........................
...*..........................
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,c,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*.........AG...............
...*..........................
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.......G..................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
...*..........................
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*.............G............
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.......G..................
...*..........................
,,t*,,,,a,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.........................G
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,g,,,,,,,,*,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
 ,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
 ,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
 ,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
 ,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
  .*..........................
    ,,,,,,,,,,,,,,,,,,,,,,,,,,
    ,,,,,,,,,,,,,,,,,,,,,,,,,,
       ,,,,,,,,,,,,,,,,,,,,,,,
                        ,,,,,,
                        ,,,,,,
                        ,,,,,,
                        ,,,,,,
                          ,,,,
                          ,,,,
                           ...
                           ,,,
                            ,,
                             ,
```

Sample **B00G5XG** and category **called_but_discordant_het1_het2** at **1:7980364**

```
       7980361                
CTAGAAAATG***C**TTTTTTTTTTTTTT
..........   .  ..............
,  ,,,,,,,***,**,,,,,,,,,c,,,,
,   ,,,,,t***,**,,,,,,,,,,,,,,
,            ,**,,,,,,,,,,,,,,
,               ,,,,,,,,,,,,,,
,,              ,,,,,,,,,,,,,,
,,              ,,,,,,,,,,,,,,
...             ,,,,,,,,,,,,,,
...             ,,,,,,,,,,,,,,
,,,             ,,,,,,,,,,,,,,
,,,,            ,,,,,,,,,,,,,,
,,,,            ,,,,,,,,,,,,,,
,,,,            ,,,,,,,,,,,,,,
,,,,,,          ,,,,,,,,,,,,,,
........        ,,,,,,,,,,,,,,
,,,,,,,,,        ,,,,,,,,,,,,,
..........***      ...........
,,,,,,,,,,***       ..........
,,,,,,,,,,***,**       ,,,,,,,
,,,,,,,,,,***,**           ,,,
..........***.**.             
..........***.**.             
,,,,,,,,,,***,**,             
,,,,,,,,,,***,**,             
,,,,,,,,,,***,**,,            
,,,,,,,,,,***,**,,,           
,,,,,,,,,,***,**,,,,          
,,,,,,,,,,***,**,,,,          
,,,,,g,,,,***,**,,,,,,,,      
,,,,,,,,,,***,**,,,,,,,,,     
,,,,,,,,,,***,**,,,,,,,,,,    
..........***.**...........   
..........***.**............  
,,,,,,,,,,***,**,,,,,,,,,,,,  
..........***.**............. 
t,,,,,,,,,***g**,,,,,,,,,,,,,,
,,,,,,,,,,***,**,,,,,,,,,,,,,,
..........***.**..............
..........***.**..............
t,,a,,,,,,***t**,,,,,,,,,,,,,,
,,,,,,,,,,***,**,,,,,,,,,,,,,,
,,,a,,,,,,***,**,,,,,,,,,,,,,,
..........***.**..............
..........***.**..............
,,,,,,,,,,***,***,,,,,,,,,,,,,
..........***.**..............
..........***.**..............
,,,a,,,,,,*******,,,,,,,,,,,,,
t,,,,,,,,,***,t*,,,,,,,,,,,,,,
,,,a,,,,,,tttt**,,,,,,,,,,,,,,
t,,a,,,,,,***t**,,,,,,,,,,,,,,
,a,a,,,,,t***t**,,,,,,,,,,,,,,
t,,a,,,,at***t**,,,,,,,,,,,,,,
t,ta,,,,,t***,**,,,,,,,,,,,,,,
,,,,,,,,,,***,***,,,,,,,,,,,,,
t,,a,,,,,****t**,,,,,,,,,,,,,,
,,,,,,,,,,***,***,,,,,,,,,,,,,
t,,,,,,,at***,t*,,,,,,,,,,,,,,
,,,,,,,,,*******,c,,,,,,,,,,,,
,,,a,,,,,,***,**,,,,,,,,,,,,,,
,,,,,,,c,,******,,,,,,,,,,,,,,
,,,,,,,,,,***,tt,,,,,,,,,,,,,,
,,,,,,,,,,***,t*,,,,,,,,,,,,,,
,,,,,,,,,,***,t*,,,,,,,,,,,,,,
..........***.***.............
,,,,,,,,,,***,***,,,,,,,,,,,,,
,,,,,,,,,,***,***,,,,,,,,,,,,,
..........***.**..............
..........***.**..............
t,,a,,,,,t***tt*,,,,,,,,,,,,,,
t,,,,,,,,,tttt**,,,,,,,,,,,,,,
,,,t,,,,,t***g**,,,,,,,,,,,,,,
..........***.**..............
t,,,,,,,at***,t*,,,,,,,,,,,,,,
..........***.T*..............
..........***.**..............
..........***.**..............
t,,,,,,,,,***,****,,,,,,,,,,,,
,,,,,,,,,t***,t*,,,,,,,,,,,,,,
..........***.**..............
..........***.**..............
..........***.**..............
,,,,,,,,,,***,**,,,,,,,,,,,,,,
,,,a,,,,,,***,**,,,,,,,,,,,,,,
..........***.***.............
,,,,,,,,,,***,**,,,,,,,,,,,,,,
```

Sample **B00G5XG** and category **called_but_discordant_het1_hom2** at **1:3731654**

```
       3731651    3731661     
GAGCGCCTCCC*GGCGGTGCTCTCTGGCTC
........... ..................
...........*..................
...........*..................
...........T..................
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
....  ,,,,,*,,,,,,,,,,,,,,,,,,
,,,,     ,,*,,,,,,,,,,,,,,,,,,
,,,,         .................
.....          ,,,,,,,,,,,,,,,
.....          ,,,,,,,,,,,,,,,
,,,,,,         ,,,,,,,,,,,,,,,
,,,,,,            ............
,,,,,,,             ,,,,,,,,,,
........             ,,,,,,,,,
,,,,,,,,              ,,,,,,,,
.........               ,,,,,,
.........                .....
.........                 ,,,,
..........                ,,,,
,,,,,,,,,,                ,,,,
...........*                ,,
...........*                 ,
,,,,,,,,,,,*                  
,,,,,,,,,,,*                  
,,,,,,,,,,,*                  
,,,,,,,,,,,*,                 
...........*..                
...........T.                 
,,,,,,,,,,,*,,,               
,,,,,,,,,,,*,,,               
...........*....              
,,,,,,,,,,,t,,,               
...........*.....             
...........*......            
...........*......            
,,,,,,,,,,,*,,,,,,            
...........*.......           
,,,,,,,,,,,*,,,,,,,           
,,,,,,,,,,,*,,,,,,,           
,,,,,,,,,,,t,,,,,,            
,,,,,,,,,,,t,,,,,,            
,,,,,,,,,,,t,,,,,,,           
,,,,,,,,,,,*,,,,,,,,          
,,,,,,,,,,,t,,,,,,,,,         
,,,,,,,,,,,*,,,,,,,,,,,,,     
,,,,,,,,,,,t,,,,,,,,,,,,      
,,,,,,,,,,,*,,,,,,,,,,,,,     
...........T.............     
...........T.............     
,,,,,,,,,,,t,,,,,,,,,,,,,     
...........*...............   
,,,,,,,,,,,t,,,,,,,,,,,,,,    
,,,,,,,,,,,*,,,,,,,,,,,,,,,,  
,,,,,,,,,,,t,,,,,,,,,,c,,,,,  
...........T..................
...........T..................
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
...........*..................
...........T..................
...........T..................
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
...........T..................
...........*..................
...........T..................
...........T..................
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
...........*..................
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,t,,,,,,,,
...........*..................
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
...........T..................
...........T..................
...........*..................
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
...........T..................
...........*..................
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
...........*..................
...........T..................
...........*..................
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
...........T..................
...........T..................
...........T..................
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
...........*..................
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
...........T..................
...........T..................
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
...........*..................
,,,,,,,,,,,*,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
...........*..................
 ,,,,,,,,,,t,,,,,,,,,,,,,,,,,,
    .......*..................
    ,,,,,,,t,,,,,,,,,,,,,,,,,,
```

Sample **B00G5XG** and category **called_but_discordant_hom1_het2** at **1:65872**

```
          65871     65881     
ATA*GTCCTTTTCCCCATCTTTCCCCAGGT
... ..........................
...*..........................
...*..........................
..  a,,,,,,,,,,,,,,,,,,,,,,,,,
...*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*, ,,,,,,,,,,,,,,,,,,,,,,,,
...*..,,,,,,,,,,,,,,,,,,,,,,,,
...*...   ,,,,,,,,,,,,,,,,,,,,
...*...   ,,,,,,,,,,,,,,,,,,,,
...*...,,,,,,,,,,,,,,,,,,,,,,,
...*....   ,,,,,,,,,,,,,,,,,,,
...*....    ..................
...*....      ,,,,,,,,,,,,,,,,
...*....      ,,,,,,,,,,,,,,,,
...*....      ,,,,,,,,,,,,,,,,
...*....      ,,,,,,,,,,,,,,,,
...*.....     ,,,,,,,,,,,,,,,,
...*.....        .............
...*.....        ,,,,,,,,,,,,,
...*.....         ,,,,,,,,,,,,
...*......         ,,,,,,,,,,,
...*......         ,,,,,,,,,,,
...*.......        ,,,,,,,,,,,
...*.......          ,,,,,,,,,
...*.......          ,,,,,,,,,
...*........         ,,,,,,,,,
...*.......G..        ........
...*..........        ,,,,,,,,
...*...........        ,,,,,,,
...*............       ,,,,,,,
...*.............      ,,,,,,,
...*.................  ,,,,,,,
,,,*,,,,,,,,,,,,,,,,,   ......
...*................... ,,,,,,
...*.................... ,,,,,
...*.................... ,,,,,
,,,*,,,,,,,g,,,,,,,,,,,,, ,,,,
...*....................... ..
...*........................ ,
...*........................ ,
...*........................ ,
...*......................... 
...*.......G................. 
...*..........................
...*..........................
...*..........................
...*..........................
...*..........................
...*..........................
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
...*..........................
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.......G..................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*....................G.....
...*..........................
...*..........................
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,c,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*.........AG...............
...*..........................
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.......G..................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
...*..........................
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*.............G............
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.......G..................
...*..........................
,,t*,,,,a,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,g,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.........................G
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,g,,,,,,,,*,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*.......G..................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
...*..........................
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
 ,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
 ,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
 ,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
 ,,*,,,,,,,,,,,,,,,,,,,,,,,,,,
  .*..........................
    ,,,,,,,,,,,,,,,,,,,,,,,,,,
    ,,,,,,,,,,,,,,,,,,,,,,,,,,
       ,,,,,,,,,,,,,,,,,,,,,,,
                        ,,,,,,
                        ,,,,,,
                        ,,,,,,
                        ,,,,,,
                          ,,,,
                          ,,,,
                           ...
                           ,,,
                            ,,
                             ,
```

Sample **B00G5XG** and category **called_but_discordant_hom1_hom2** at **1:6161100**

```
 6161091    6161101           
TCTTTTTTTTT*AAACCTACCGTGGTTCCT
........... ..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
, .........A..................
,, ........A..................
,,,, ......A..................
,,,,,  ....A..................
........  ,a,,,,,,,,,,,,,,,,,,
..........  ..................
..........  ,,,,,,,,,,,,,,,,,,
..........  ,,,,,,,,,,,,,,,,,,
...........*,,,,,,,,,,,,,,,,,,
...........*... ,,,,,,,,,,,,,,
,,,,,,,,,,,*,,, ,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,, ...........
,,,,,,,,,,,a,,,,,,,, .........
...........A........... ......
,,,,,,,,,,,a,,,,,,,,,,,  ,,,,,
,,,,,,,,,,,a,,,,,,,,,,,  ,,,,,
...........A.............  ...
...........A..............  ..
...........A..............  ..
...........A............... ,,
...........A................  
...........A................  
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
...........A..................
...........A..................
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A.............A....
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........*..................
...........A..................
...........A..................
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
...........A..................
...........A.............A....
...........A..................
...........A..................
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,g,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
...........A..................
...........A.......AA.........
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
...........A..................
...........A..................
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,a,,,,,,,,,,,,,,,,,,
...........A..................
...........A..................
...........A..................
...........A..................
   ........A..................
    .......A..................
    .......A..................
     ,,,,,,a,,,,,,,,,,,,,,,,,,
     ,,,,,,a,,,,,,,,,,,,,,,,,,
       ,,,,a,,,,,,,,,,,,,,,,,,
             ,,,,,,,,,,,,,,,,,
             ,,,,,,,,,,,,,,,,,
             ,,,,,,,,,,,,,,,,,
              ................
              ................
              ................
              ,,,,,,,,,,,,,,,,
              ,,,,,,,,,,,,,,,,
               ...............
               ...............
                  ............
                  ,,,,,,,,,,,,
                    ..........
                    ..........
                    ,,,,,,,,,,
                    ,,,,,,,,,,
                     .........
                     .........
                     ,,,,,,,,,
                      ,,,,,,,,
                      ,,,,,,,,
                      ,,,,,,,,
                       .......
                       ,,,,,,,
                            ,,
```

Sample **B00G5XG** and category **common_context** at **1:14653**

```
        14651     14661       
GTCAGAGCAACGGCCCAAGTCTGGGTCTGG
..........Y...................
....   ...T...................
...............    ,,,,,,,,,,,
..........T...........  ,,,,,,
..........T................. ,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..........T...................
..............................
..........T...................
..........T...................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
..........T...................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
             .................
```

Sample **B00G5XG** and category **common_context_discordant_id** at **1:14653**

```
        14651     14661       
GTCAGAGCAACGGCCCAAGTCTGGGTCTGG
..........Y...................
....   ...T...................
...............    ,,,,,,,,,,,
..........T...........  ,,,,,,
..........T................. ,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..........T...................
..............................
..........T...................
..........T...................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
..........T...................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
             .................
```
Sample **B00G5XG** and category **common_context_indel** at **1:721750**
```
 721741    721751             
CACTTAATTTTTTATTATTTATTTATTTTA
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
....     .....................
,,,,       ...................
,,,,,        .................
,,,,,,                     ...
,,,,,,                     ...
,,,,,,,                      .
,,,,,,,,                      
,,,,,,,,                      
,,,,,,,,                      
,,,,,,,,,,                    
,,,,,,,,,,,,                  
.............                 
.............                 
,,,,,,,,,,,,,,,,              
,,,,,,,,,,,,,,,,,             
,,,,,,,,,,,,,,,,,,            
,,,,,,,,,,,,,,,,,,,           
,,,,,,,,,,,,,,,,,,,,,         
,,,,,,,,,,,,,,,,,,,,,,,,      
,,,,,,,,,,,,,,,,,,,,,,,,      
,,,,,,,,,,,,,,,,,,,,,,,,,,    
,,,,,,,,,,,,,,,,,,,,,,,,,,,   
,,,,,,,,,,,,,,,,,,,,,,,,,,,   
,,,,,,,,,,,,,,,,,,,,,,,,,,,,  
,,,,,,,,,,,,,,,,,,,,,,,,,,,,  
,,,,,,,,,,,,,,,,,,,,,,,,,,,,, 
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
```
Sample **B00G5XG** and category **common_context_snp** at **1:14653**
```
        14651     14661       
GTCAGAGCAACGGCCCAAGTCTGGGTCTGG
..........Y...................
....   ...T...................
...............    ,,,,,,,,,,,
..........T...........  ,,,,,,
..........T................. ,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..........T...................
..............................
..........T...................
..........T...................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
..........T...................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
             .................
```
Sample **B00G5XG** and category **unique_to_file_1** at **1:874778**
```
   874771    874781           
TACCACCTGGGCCTCCCCAGCCACGGTGAG
..............................
,,  ..........................
....         ,,,,,,,,,,,,,,,,,
,,,,,,,,,,   ,,,,,,,,,,,,,,,,,
............. ,,,,,,,,,,,,,,,,
..................     ,,,,,,,
....................        ,,
,,,,,,,,,,,,,,,,,,,,,       ,,
......................       ,
,,,,,,,,,,,,,,,,,,,,,,        
.......................       
,,,,c,,,,,,,,,,,,,,,,,,,      
,,,,,,,,,,,,,,,,,,,,,,,,      
..........................    
..........................    
,,,,,,,,,,,,,,,,,,,,,,,,,,,   
,,,,,,,,,,,,,,,,,,,,,,,,,,,   
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,g,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
                ,,,,,,,,,,,,,,
                ,,,,,,,,,,,,,,
                ,,,,,,,,,,,,,,
```
Sample **B00G5XG** and category **unique_to_file_1_indel** at **1:874778**
```
   874771    874781           
TACCACCTGGGCCTCCCCAGCCACGGTGAG
..............................
,,  ..........................
....         ,,,,,,,,,,,,,,,,,
,,,,,,,,,,   ,,,,,,,,,,,,,,,,,
............. ,,,,,,,,,,,,,,,,
..................     ,,,,,,,
....................        ,,
,,,,,,,,,,,,,,,,,,,,,       ,,
......................       ,
,,,,,,,,,,,,,,,,,,,,,,        
.......................       
,,,,c,,,,,,,,,,,,,,,,,,,      
,,,,,,,,,,,,,,,,,,,,,,,,      
..........................    
..........................    
,,,,,,,,,,,,,,,,,,,,,,,,,,,   
,,,,,,,,,,,,,,,,,,,,,,,,,,,   
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,g,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
                ,,,,,,,,,,,,,,
                ,,,,,,,,,,,,,,
                ,,,,,,,,,,,,,,
```
Sample **B00G5XG** and category **unique_to_file_1_snp** at **1:897465**
```
      897461    897471        
CCCCCACCCCACCCCACCCCAGTCTTTGTC
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
...  ,,,,,,,,,,,,,,,,,,,,,,,,,
...  ,,,,,,,,,,c,,,,,,,,,,,,,,
,,,  ,,,,,,,,,,,,,,,,,,,,,,,,,
,,, ..........................
....  ........................
,,,,   .......................
.....    ,,,,,,,,,,,,,,,,,,,,,
.....C   ,,,,,,,,,,,,,,,,,,,,,
.....C    ....................
......    c,,,,,,,,,,,,,,,,,,,
,,,,,,,    ...................
,,,,,,,,    ..................
.....T...   ,,,,,,,,,,,,,,,,,,
.........     ................
..........      ..............
.....C....C     ,,,,,,,,,,,,,,
..........C      .............
..........C      .............
............     ,,,,,,,,,,,,,
,,,,,,,,,,,,,     ............
,,,,,,,,,,,,,      ...........
,,,,,,,,,,,,,      ...........
.....C........      ..........
.....C........      ..........
,,,,,,,,,,,,,,      ..........
.....C....C.....    ,,,,,,,,,,
.............        .........
G....C...........    .........
.....C............   .........
,,,,,,,,,,,,,,,,,,,   ,,,,,,,,
.....C....C...........  ......
,,,,,,,,,,,,,,,,,,,,,,  ......
,,,,,,,,,,,,,,,,,,,,,,,  ,,,,,
...............C.....  .......
.....C....C............... ...
G....C....C....C......  ......
...............C....C.....  ,,
.....C................G...   .
..........C...........G...   .
..............................
G...................    ......
A...................    ......
,,,,,,,,,,,,,,,c,,,,,,,,,,,,,,
.....C....C.........CA......G.
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
.....C........................
..............................
.....                   ,,,,,,
....................C.........
..............................
..............................
...........A........     ,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..........C...................
.....C....C....C....C.........
.....C.........C....C.........
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
.....C....C.........C.........
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,c,,,,,,,,,,,,,,
..............................
.....C........................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
.....C........................
.....C........................
.....C..............C.G.......
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
.....C........................
.....C....C.........C.........
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
.....C........................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,t,,,,c,,,,t,,,,,,,,,,,,,,
.....C....C....C....C.........
.....C..............C.........
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
......................G.......
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
...............C..............
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,t,,,,,,,,,,,,,,
.....C....C.........C.........
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
............................G.
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
...............C....C.A.......
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,c,,,,,,,,,,,,,,,,,,,,,,,,
 ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
  .........A..........A.......
  ............................
  ............................
                          ,,,,
                             .
                             ,
                             ,
```

Sample **B00G5XG** and category **unique_to_file_2_indel** at **1:861473**

```
        861471    861481      
GGTTTTCAGGATCGAGAGTCCTAACCTCAC
..............................
,  ,,,,,,,,,,,,,,,,,,,,,,,,,,,
...    ,,,,,,,,,,,,,,,,,,,,,,,
,,,             ,,,,,,,,,,,,,,
,,,,               ,,,,,,,,,,,
,,,,,               ,,,,,,,,,,
,,,,,,                  ,,,,,,
,,,,,,                     ,,,
,,,,,,,                       
,,,,,,,                       
,,,,,,,                       
,,,,,,,,,                     
,,,,,,,,,,,,,                 
,,,,,,,,,,,,,,                
...............               
,,,,,,,,,,,,,,,,,             
..................            
,,,,,,,,,,,,,,,,,,            
,,,,,,,,,,,,,,,,,,            
,,,,,,,,,,,,,,,,,,,,,         
,,,,,,,,,,,,,,,,,,,,,         
,,,,,,,,,,,,,,,,,,,,,,        
,,,,,,,,,,,,,,,,,,,,,,,       
,,,,,,,,,,,,,,,,,,,,,,,,      
,,,,,,,,,,,,,,,,,,,,,,,,,     
,,,,,,,,,,,,,,,,,,,,,,,,,     
,,,,,,,,,,,,,,,,,,,,,,,,,     
,,,,,,,,,,,,,,,,,,,,,,,,,,    
............................  
..............................
..C...........................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..A..........................G
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,c,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,a,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,g,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,t,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
```

Sample **B00G5XG** and category **unique_to_file_2_snp** at **1:14673**

```
        14671     14681       
CTGGGTCTGGGGGGGAAGGTGTCATGGAGC
..............................
..  ,,,,,,,,,,,,,,,,,,,,,,,,,,
........ ,,,,,,,,,,,,,,,,,,,,,
..........C........     ,,,,,,
,,,,,,,,,,c,,,,,,,,      ,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,c,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............A...............
..............................
..............A...............
..............................
..........C...................
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
..........C...................
..............................
..............A...............
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
..............................
...................G..........
..............................
,,,,,,,,,,,,,,a,,,,,,,,,,,,,,,
             .................
```



