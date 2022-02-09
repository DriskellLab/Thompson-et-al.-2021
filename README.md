# Thompson-et-al.-2021
P0 scATAC-seq and scRNA-seq paper, open access in the Journal of Investigative Dermatology via: https://doi.org/10.1016/j.jid.2021.11.032

##Packages & Package Versions##
 [1] tidyr_1.1.3                        patchwork_1.1.1                   
 [3] tictoc_1.0.1                       future_1.21.0                     
 [5] stringr_1.4.0                      metap_1.4                         
 [7] multtest_2.47.0                    SummarizedExperiment_1.21.3       
 [9] MatrixGenerics_1.3.1               matrixStats_0.58.0                
[11] BSgenome.Mmusculus.UCSC.mm10_1.4.0 BSgenome_1.59.4                   
[13] rtracklayer_1.51.5                 Biostrings_2.59.4                 
[15] XVector_0.31.1                     EnsDb.Mmusculus.v79_2.99.0        
[17] ensembldb_2.15.3                   AnnotationFilter_1.15.0           
[19] GenomicFeatures_1.43.8             AnnotationDbi_1.53.2              
[21] Biobase_2.51.0                     GenomicRanges_1.43.4              
[23] viridisLite_0.4.0                  GenomeInfoDb_1.27.13              
[25] IRanges_2.25.11                    S4Vectors_0.29.19                 
[27] BiocGenerics_0.37.6                Signac_1.2.1                      
[29] readr_1.4.0                        dplyr_1.0.6                       
[31] RColorBrewer_1.1-2                 limma_3.47.15                     
[33] reticulate_1.20                    sctransform_0.3.2                 
[35] ggplot2_3.3.3                      SeuratObject_4.0.1                
[37] Seurat_4.0.1                       hdf5r_1.3.3 


### Loading data from processed files ###
To create the fragments file object, the fragments.tsv.gz and fragments.tsv.tbi files must be in the same directory!


### Possible bug with loading EnsDb.Mmusculus.v79 & a fix ###
Users attempting to replicate these results using our exact code may encounter a bug related to loading in the UCSC mm10 genome annotation depending upon their package versions. We have included a bug fix that can be easily merged into our pipeline, expanding upon comments made on the bioconductor forum (https://github.com/Bioconductor/GenomeInfoDb/issues/27). At least in the package versions used in our analysis, the following bug fix will allow for the creation of a Seurat object for scATAC data that retains full functionality of CoveragePlot() and other scATAC-specific Seurat/Signac functions that pull genomic coordinates from the annotation:

Anywhere that genes(EnsDb.Mmusculus.v79) is called, this bug fix must be applied to avoid functions erroring.

Obtain Genome Annotation
#create granges object with TSS positions
gene.ranges <- genes(EnsDb.Mmusculus.v79)
ucsc.levels <- str_replace(string = paste("chr", seqlevels(gene.ranges), sep = ''),
pattern = 'chrMT', replacement = 'chrM')
seqlevels(gene.ranges) <- ucsc.levels
genome(gene.ranges) <- 'mm10'
#seqlevelsStyle(gene.ranges)#view the style of the gene.ranges annotation
#seqlevelsStyle(gene.ranges) <- 'UCSC'
gene.ranges <- gene.ranges[gene.ranges$gene_biotype == 'protein_coding', ]
gene.ranges <- keepStandardChromosomes(gene.ranges, pruning.mode = 'coarse')
