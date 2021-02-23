# 3q29 subset for HPRC year 1

First, we extract the region from GRCh38:

```
samtools faidx /lizardfs/erikg/HPRC/year1/assemblies/grch38.fna.gz grch38#chr3:192600001-198295559 >grch38_3q29.fa
```

```
wfmash -t 48 -s 5000 -p 98 grch38_3q29.fa /lizardfs/erikg/HPRC/year1/parts/chr3.pan+refs.fa >HPRCy1.chr3.pan+refs_vs_grch38_3q29.paf
```

Then, we make a BED file for the mappings:

```
awk '{ print $1, $3, $4 }' HPRCy1.chr3.pan+refs_vs_grch38_3q29.paf | tr ' ' '\t' | sort -V >HPRCy1.3q29.bed
```

And merge the regions in it to make a single BED record per matching contig:

```
bedtools merge -d 1000000 -i HPRCy1.3q29.bed >HPRCy1.3q29.merge.bed
```

Finally, extracting the FASTA file for the regions:

```
bedtools getfasta -fi /lizardfs/erikg/HPRC/year1/parts/chr3.pan+refs.fa -bed HPRCy1.3q29.merge.bed >HPRCy1.3q29.fa
```

Then, build a graph.