    library("dplyr") ## for filtering and selecting rows
    library("plyr")  ## for renmaing factors
    library("reshape2") ##  for melting dataframe

Disclaimer
----------

Hello! If you are viewing this page then hopefully you want to access
some really large data files containing raw RNA transcript counts and
estimates of transcripts per million.

To obtain the data analyzed in this markdown file, I ran the Kallisto
program the Stampede Cluster at the Texas Advacned Computing Facility.
It runs really fast! The data are exported as abunance files in a
subdirectory for every sample. On my local computer, they appear in
`../data/GSE99765_Dissociation` and
`../data/GSE100225_IntegrativeWT2015`.

These files and this analysis will take up considerable space and time.
If you want to run the analysis, first download the above directorys
from this GitHub repository:

Kallisto Gather
---------------

The kallisto output gives you read counts for sample in an abundance
file for every single sample. This portion of the code goes through and
finds each samples' abundance.tsv file, extracts the data, and combines
it all into a dataframe. The `counts` file is unnormalized, but the
`tpm` is the data after being normalized by transcripts per million.
This script was developed with assistance from Anna Batthenhouse and
Dennis Whylie.

Rather than examine unique transcripts, my analyses will focus on
gene-level exprrssion. I use some string splitting to take the very long
transcript identifying and create a `geneids` file that has all the
database identifiers for each transcript. Then, I'll save the dount
data.

First, here's the analysis for the Dissociation Test data. To repeat
this analysis, first unzip `Kallisto_Dissociation.tar.gz` and
`Kallisto_StressCognition.tar.gz`.

    setwd("../data/Kallisto_Dissociation/")
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

    ## this for loop uses the reduce function to make two data frame with counts or tpm from all the samples
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

    # tpm to tpmbygene
    tpmbygene <-  full_join(geneids, tpm) # merge tpm and genids

    ## Joining, by = "id"

    tpmbygene <- tpmbygene[-c(1:6,8:12)]   ## keep gene name and tpm for samples)
    tpmbygene <- melt(tpmbygene, id=c("gene")) ## lenghten 
    tpmbygene <- dcast(tpmbygene, gene ~ variable, value.var= "value", fun.aggregate=mean) #then widen by sum
    row.names(tpmbygene) <- tpmbygene$gene ## make gene the row name
    tpmbygene[1] <- NULL ## make gene the row name
    tpmbygene <- round(tpmbygene) #round all value to nearest 1s place


    # count to countbygene
    countbygene <- full_join(geneids, count) # merge count and genids

    ## Joining, by = "id"

    countbygene <- countbygene[-c(1:6,8:12)]   ## rkeep gene name and counts for samples)
    countbygene <- melt(countbygene, id=c("gene")) ## lenghten 
    countbygene  <- dcast(countbygene, gene ~ variable, value.var= "value", fun.aggregate=sum) #then widen by sum
    row.names(countbygene) <- countbygene$gene ## make gene the row name
    countbygene[1] <- NULL ## make gene the row name
    countbygene <- round(countbygene) #round all value to nearest 1s place

    write.csv(countbygene, "../../data/GSE99765_DissociationCountData.csv", row.names=T)

Now, here's the analysis for the stressful experiences and the cognitive
training.

    setwd("../data/Kallisto_StressCognition//")
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

    ## this for loop uses the reduce function to make two data frame with counts or tpm from all the samples
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


    # tpm to tpmbygene
    tpmbygene <-  full_join(geneids, tpm) # merge tpm and genids

    ## Joining, by = "id"

    tpmbygene <- tpmbygene[-c(1:6,8:12)]   ## keep gene name and tpm for samples)
    tpmbygene <- melt(tpmbygene, id=c("gene")) ## lenghten 
    tpmbygene <- dcast(tpmbygene, gene ~ variable, value.var= "value", fun.aggregate=mean) #then widen by sum
    row.names(tpmbygene) <- tpmbygene$gene ## make gene the row name
    tpmbygene[1] <- NULL ## make gene the row name
    tpmbygene <- round(tpmbygene) #round all value to nearest 1s place


    # count to countbygene
    countbygene <- full_join(geneids, count) # merge count and genids

    ## Joining, by = "id"

    countbygene <- countbygene[-c(1:6,8:12)]   ## rkeep gene name and counts for samples)
    countbygene <- melt(countbygene, id=c("gene")) ## lenghten 
    countbygene  <- dcast(countbygene, gene ~ variable, value.var= "value", fun.aggregate=sum) #then widen by sum
    row.names(countbygene) <- countbygene$gene ## make gene the row name
    countbygene[1] <- NULL ## make gene the row name
    countbygene <- round(countbygene) #round all value to nearest 1s place

    write.csv(countbygene, "../../data/GSE100225_IntegrativeWT2015CountData.csv", row.names=T)

Session Info
------------

    sessionInfo()

    ## R version 3.3.1 (2016-06-21)
    ## Platform: x86_64-apple-darwin13.4.0 (64-bit)
    ## Running under: OS X 10.10.5 (Yosemite)
    ## 
    ## locale:
    ## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ## [1] reshape2_1.4.2 plyr_1.8.4     dplyr_0.5.0   
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] Rcpp_0.12.9     digest_0.6.12   rprojroot_1.2   assertthat_0.1 
    ##  [5] R6_2.2.0        DBI_0.6         backports_1.0.5 magrittr_1.5   
    ##  [9] evaluate_0.10   stringi_1.1.2   rmarkdown_1.3   tools_3.3.1    
    ## [13] stringr_1.2.0   yaml_2.1.14     htmltools_0.3.5 knitr_1.15.1   
    ## [17] tibble_1.2