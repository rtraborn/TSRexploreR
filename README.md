# TSRexploreR

TSRexploreR facilitates processing and exploration of data generated by global transcription start site (TSS) mapping technologies such as [CAGE](https://link.springer.com/protocol/10.1007%2F978-1-4939-0805-9_7) and [STRIPE-seq](https://genome.cshlp.org/content/30/6/910.long). TSSs can be extracted and aggregated from mapped reads in BAM/SAM format or as pre-aggregated TSSs in GRanges, bigWig, bedGraph, or CTSS format. TSSs can then be normalized and clustered into transcription start regions (TSRs), or TSRs detected with other software such as [CAGEr](https://academic.oup.com/nar/article/43/8/e51/2414172) can be imported.

A strength of TSRexploreR is the flexibility and ease with which data can be visualized. Numerous plots can be generated for TSSs and TSRs such as density plots, heatmaps, sequence logos, gene tracks, correlation heatmaps, and much more. For most plots, we also provide the ability to filter, order, quantile, and group TSSs or TSRs by various metrics such as inter-quantile range, shape index, and score. TSRexploreR also facilitates detection of differential TSR usage with [DESeq2](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-014-0550-8) and [edgeR](https://academic.oup.com/bioinformatics/article/26/1/139/182458).

We also introduce a new approach to discovering shifts in TSS distributions within merged TSRs from distinct samples. TSRexploreR calculates a signed variant of [earth mover's distance](https://en.wikipedia.org/wiki/Earth_mover%27s_distance), which we term earth mover's score (EMS), which indicates both the magnitude and direction of a shift. EMS calculation is statistically controlled by a permutation test, which generates a p-value and FDR threshold.

## Installing TSRexploreR

TSRexploreR can be installed wholly from Github (with or without a [conda](https://docs.conda.io/en/latest/) environment) or run from within a [Singularity](https://singularity.lbl.gov) container.

#### Github

```
Sys.setenv("R_REMOTES_NO_ERRORS_FROM_WARNINGS"="true")
devtools::install_github("zentnerlab/TSRexploreR")
```

#### Github and conda

Conda can be optionally used before the above Github installation step to download most of the package dependencies. This also has the benefit of creating an R environment separate from your main software environment, which helps avoid dependency version conflicts and makes removing associated sofware easy if desired. Installation instructions for miniconda3 can be found [here](https://docs.conda.io/projects/conda/en/latest/user-guide/install/).

First, create the conda environment.

```
conda create -n TSRexploreR -y -c conda-forge -c bioconda \
qpdf r-base>=4.0.0 r-tidyverse r-data.table r-devtools r-ggseqlogo r-cowplot r-rcpp \
r-assertthat r-testthat r-cairo r-ggrastr r-pkgdown \
bioconductor-apeglm bioconductor-genomicranges bioconductor-genomicfeatures \
bioconductor-genomicalignments bioconductor-biostrings bioconductor-rsamtools \
bioconductor-chipseeker bioconductor-edger bioconductor-deseq2 bioconductor-clusterProfiler \
bioconductor-complexheatmap bioconductor-cager bioconductor-tsrchitect bioconductor-gviz \
bioconductor-rtracklayer bioconductor-biocgenerics bioconductor-plyranges \
bioconductor-pcatools bioconductor-genomeinfodb bioconductor-bsgenome bioconductor-bioccheck \
bioconductor-bsgenome.scerevisiae.ucsc.saccer3 bioconductor-txdb.scerevisiae.ucsc.saccer3.sgdgene
```

After the environment has been created, it must be activated prior to use.

```
conda activate TSRexploreR
```

TSRexploreR can now be installed into this environment, where it will be available upon activation of the environment.

```
Sys.setenv("R_REMOTES_NO_ERRORS_FROM_WARNINGS"="true")
devtools::install_github("zentnerlab/TSRexploreR")
```

#### Singularity

The TSRexploreR Singularity container is akin to a box that contains TSRexploreR and all required dependencies. The advantage of containerized software is that it requires no installation (beyond the main container software), is self-contained, and provides stable and reproducible results over time.
Singularity installation instructions can be found [here](https://sylabs.io/docs/).

After Singularity is installed, the container can be pulled and run in a few commands. First, create a working directory with all required files and run the following commands.

Download the singularity container.

```
singularity pull --arch amd64 library://zentlab/default/tsrexplorer:main
```

You can then run R from within the container housing TSRexploreR.

```
singularity exec -eCB `pwd` -H `pwd` tsrexplorer_main.sif R
```

## Using TSRexploreR

We provide several vignettes to showcase the various functionalities of TSRexplorer:

- [Standard Analysis](documentation/STANDARD_ANALYSIS.pdf) - Common workflow steps for TSS and TSR analysis and visualization.
- [BAM Processing](documentation/BAM_PROCESSING.pdf) - Importing and processing aligned reads from BAM/SAM format.
- [Differential TSSs/TSRs](documentation/DIFF_FEATURES.pdf) - Using DESeq2 or edgeR to find differential TSSs or TSRs between conditions.
- [TSS Shifting](documentation/FEATURE_SHIFT.pdf) - Detecting shifts in TSS distributions within TSRs between conditions using the earth mover's score (EMS).
- [Data Conditionals](documentation/DATA_CONDITIONING.pdf) - Flexible filtering, quantiling, ordering, and grouping TSSs or TSRs during plotting.
