# Thompson-et-al.-2021
P0 scATAC-seq and scRNA-seq paper

Packages & Package Versions:
<need_to_do>


### Possible bug with loading EnsDb.Mmusculus.v79 & a fix ###
Users attempting to replicate these results using our exact code may encounter a bug related to loading in the UCSC mm10 genome annotation depending upon their package versions. We have included a bug fix that can be easily merged into our pipeline, expanding upon comments made on the bioconductor forum (https://github.com/Bioconductor/GenomeInfoDb/issues/27). At least in the package versions used in our analysis, the following bug fix will allow for the creation of a Seurat object for scATAC data that retains full functionality of CoveragePlot() and other scATAC-specific Seurat/Signac functions that pull genomic coordinates from the annotation:

Anywhere that genes(EnsDb.Mmusculus.v79) is called, this bug fix must be applied to avoid functions erroring.

Obtain Genome Annotation
create granges object with TSS positions
gene.ranges <- genes(EnsDb.Mmusculus.v79)
ucsc.levels <- str_replace(string = paste("chr", seqlevels(gene.ranges), sep = ''),
pattern = 'chrMT', replacement = 'chrM')
seqlevels(gene.ranges) <- ucsc.levels
genome(gene.ranges) <- 'mm10'
#seqlevelsStyle(gene.ranges)#view the style of the gene.ranges annotation
#seqlevelsStyle(gene.ranges) <- 'UCSC'
gene.ranges <- gene.ranges[gene.ranges$gene_biotype == 'protein_coding', ]
gene.ranges <- keepStandardChromosomes(gene.ranges, pruning.mode = 'coarse')
