# CovFuzze Plot
A python script to plot gene coverage and IP peaks with standard deviations calculated from multiple replicates. [covfʌzeɪ]

Note: up until v0.1.3, coverage has been calculated using bedtools coverage -d. This considers gaps across positions as +1 reads. 
Future versions will migrate to other (preferably faster) methods that don't count gaps.  

usage: covfuzze [-h] [-g GENE] [-o OUT] --bams BAMS [BAMS ...] --bed 
                         BED [--gtf GTF] [-p PEAKS] -l LABELS [LABELS ...]
                         [-n NSUBPLOTS] [--normalize] [--scale]

## dependencies:
- numpy, tested with 1.11.1
- pysam, tested with 0.9.1
- pandas, tested with 0.18.1
- seaborn, tested with 0.7.1 
- matplotlib, tested with 2.0.2 - requires macosx backend to run on mac and may have issues with pip install - try conda 
- pybedtools, tested with 0.7.10 - requires bedtools available in path
It requires quite a bit of memory to run with whole genome bam files. If running on a cluster, recommend allocating 200GB. 

## installation
Easiest way:
```
pip install covfuzze
```
If you have issues with dependencies, try installing them separately with conda or as appropriate for your OS 

## required arguments:
```
  -o OUT, --out OUT     output prefix (incl. dir) 
  --bams BAMS [BAMS ...] 
                        bam files (separated by spaces) 
  --bed BED             bed file for region(s) of interest  
  -l LABELS [LABELS ...], --labels LABELS [LABELS ...] 
                        labels associated with bams - if replicates, use same 
                        labels
```
## optional arguments:
```
  -h, --help            show this help message and exit 
  -g GENE, --gene GENE  gene name (default = GeneDoe) 
  --gtf GTF             gtf file for gene to plot cds as shaded regions separated by exon
  -p PEAKS, --peaks PEAKS 
                        bed file with peaks
  -n NSUBPLOTS, --nsubplots NSUBPLOTS 
                        number of subplots- bams will be split evenly based on 
                        the order given (default = 1) 
  --normalize           normalize by gene length/summed coverage (default = False)
  --scale               scale y axis separately for each subplot (default = False)

```

## Example:
```
covfuzze -o $outdir/$prefix --bams 
    alignments/sample_1_Input.sample.star.bam alignments/sample_1_IP.sample.star.bam 
    alignments/sample_2_Input.sample.star.bam alignments/sample_2_IP.sample.star.bam 
    alignments/sample_3_Input.sample.star.bam alignments/sample_3_IP.sample.star.bam 
    --bed ${gene}_exons.bed 
    -l sample_Input sample_IP sample_Input sample_IP sample_Input sample_IP 
    --gene ${gene} -n 1 --scale
```
