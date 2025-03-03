    library("dplyr") ## for filtering and selecting rows
    library("plyr")  ## for renmaing factors
    library("reshape2") ##  for melting dataframe

Disclaimer
----------

If you are viewing this page then hopefully you want to access some
really large data files containing RNA transcript counts and estimates
of transcripts per million. These files and this analysis will take up
considerable space and time.

Kallisto Gather
---------------

To obtain the data analyzed in this markdown file, I ran the [kallisto
program](../UNIXworkflow/04_04_kallisto.md) the Stampede Cluster at the
Texas Advacned Computing Facility. It runs really fast! The data are
exported as abunance files in a subdirectory for every sample.

The kallisto output gives you read counts for sample in an abundance
file for every single sample. This portion of the code goes through and
finds each samples’ abundance.tsv file, extracts the data, and combines
it all into a dataframe. The `counts` file is unnormalized, but the
`tpm` is the data after being normalized by transcripts per million.
This script was developed with assistance from Anna Batthenhouse and
Dennis Whylie.

Rather than examine unique transcripts, my analyses will focus on
gene-level exprrssion. I use some string splitting to take the very long
transcript identifying and create a `geneids` file that has all the
database identifiers for each transcript. Then, I’ll save the dount
data.

    setwd("../data/GSE99765_Dissociation/")
    ## this will create lists of all the samples
    kallistoDirs = dir(".")
    kallistoDirs = kallistoDirs[!grepl("\\.(R|py|pl|sh|xlsx?|txt|tsv|csv|org|md|obo|png|jpg|pdf)$",
    kallistoDirs, ignore.case=TRUE)]

    kallistoFiles = paste0(kallistoDirs, "/abundance.tsv")
    names(kallistoFiles) = kallistoDirs
    if(file.exists(kallistoFiles))
      kallistoData = lapply(
      kallistoFiles,
      read.table,
      sep = "\t",
      row.names = 1,
      header = TRUE
    )

    ## this for loop uses the reduce function to make two data frame with counts or tpm from all the samples. note, only counts are used

    ids = Reduce(f=union, x=lapply(kallistoData, rownames))
    if (all(sapply(kallistoData, function(x) {all(rownames(x)==ids)}))) {
      count = data.frame(
        id = ids,
        sapply(kallistoData, function(x) {x$est_counts}),
        check.names = FALSE,
        stringsAsFactors = FALSE
    )
      tpm = data.frame(
        id = ids,
        sapply(kallistoData, function(x) {x$tpm}),
        check.names = FALSE,
        stringsAsFactors = FALSE
    )
    }

    ## make a dataframe with the parts of the gene id as columns
    geneids <- count[c(1)] 
    geneids$ENSMUST <- sapply(strsplit(as.character(geneids$id),'\\|'), "[", 1)
    geneids$ENSMUSG <- sapply(strsplit(as.character(geneids$id),'\\|'), "[", 2)
    geneids$OTTMUSG <- sapply(strsplit(as.character(geneids$id),'\\|'), "[", 3)
    geneids$OTTMUST <- sapply(strsplit(as.character(geneids$id),'\\|'), "[", 4)
    geneids$transcript <- sapply(strsplit(as.character(geneids$id),'\\|'), "[", 5)
    geneids$gene <- sapply(strsplit(as.character(geneids$id),'\\|'), "[", 6)
    geneids$length <- sapply(strsplit(as.character(geneids$id),'\\|'), "[", 7)
    geneids$structure1 <- sapply(strsplit(as.character(geneids$id),'\\|'), "[", 8)
    geneids$structure2 <- sapply(strsplit(as.character(geneids$id),'\\|'), "[", 9)
    geneids$structure3 <- sapply(strsplit(as.character(geneids$id),'\\|'), "[", 10)
    geneids$transcript_lenght <- as.factor(paste(geneids$transcript, geneids$length, sep="_"))


    # tpm to tpmbytranscript - note: not used for subsequent analyses
    tpmbytranscript <-  full_join(geneids, tpm) # merge tpm and genids

    ## Joining, by = "id"

    tpmbytranscript <- tpmbytranscript[-c(1:11)]   ## keep gene name and tpm for samples)
    row.names(tpmbytranscript) <- tpmbytranscript$transcript_lenght ## make gene the row name
    tpmbytranscript[1] <- NULL ## make gene the row name
    tpmbytranscript <- round(tpmbytranscript) #round all value to nearest 1s place

    # count to countbygene
    countbygene <- full_join(geneids, count) # merge count and genids

    ## Joining, by = "id"

    countbygene <- countbygene[-c(1:6,8:12)]   ## rkeep gene name and counts for samples)
    countbygene <- melt(countbygene, id=c("gene")) ## lenghten 
    countbygene  <- dcast(countbygene, gene ~ variable, value.var= "value", fun.aggregate=sum) #then widen by sum
    row.names(countbygene) <- countbygene$gene ## make gene the row name
    countbygene[1] <- NULL ## make gene the row name
    countbygene <- round(countbygene) #round all value to nearest 1s place

    setwd("~/GitHub/DissociationTest/scripts")
    write.csv(geneids, "../results/geneids.csv", row.names=F)
    write.csv(tpmbytranscript, "../results/GSE99765_Dissociation_tpmbytranscript.csv", row.names=T)
    write.csv(countbygene, "../results/GSE99765_DissociationCountData.csv", row.names=T)

Session Info
------------

    sessionInfo()

    ## R version 3.5.2 (2018-12-20)
    ## Platform: x86_64-apple-darwin15.6.0 (64-bit)
    ## Running under: macOS Mojave 10.14.2
    ## 
    ## Matrix products: default
    ## BLAS: /Library/Frameworks/R.framework/Versions/3.5/Resources/lib/libRblas.0.dylib
    ## LAPACK: /Library/Frameworks/R.framework/Versions/3.5/Resources/lib/libRlapack.dylib
    ## 
    ## locale:
    ## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ## [1] reshape2_1.4.3 plyr_1.8.4     dplyr_0.8.0.1 
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] Rcpp_1.0.0       crayon_1.3.4     digest_0.6.18    assertthat_0.2.0
    ##  [5] R6_2.4.0         magrittr_1.5     evaluate_0.13    pillar_1.3.1    
    ##  [9] rlang_0.3.1      stringi_1.3.1    rmarkdown_1.12   tools_3.5.2     
    ## [13] stringr_1.4.0    glue_1.3.0       purrr_0.3.1      xfun_0.5        
    ## [17] yaml_2.2.0       compiler_3.5.2   pkgconfig_2.0.2  htmltools_0.3.6 
    ## [21] tidyselect_0.2.5 knitr_1.22       tibble_2.0.1
