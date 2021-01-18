### There are three tables that you typically need to look at when you're dealing with a Genomics Experiment.
### The Genomics Data, the Feature Data and the Phenotype Data.



```R
##---- load data

con =url("http://bowtie-bio.sourceforge.net/recount/ExpressionSets/bodymap_eset.RData")
load(file=con)

close(con)
```


```R
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("Biobase")
```

    Updating HTML index of packages in '.Library'
    Making 'packages.html' ... done
    Bioconductor version 3.10 (BiocManager 1.30.10), R 3.6.1 (2019-07-05)
    Installing package(s) 'BiocVersion', 'Biobase'
    also installing the dependency ‘BiocGenerics’
    
    Updating HTML index of packages in '.Library'
    Making 'packages.html' ... done
    Old packages: 'askpass', 'backports', 'BH', 'boot', 'broom', 'callr', 'caret',
      'class', 'cli', 'clipr', 'cluster', 'codetools', 'colorspace', 'curl',
      'data.table', 'DBI', 'dbplyr', 'digest', 'dplyr', 'ellipsis', 'evaluate',
      'fansi', 'forcats', 'foreach', 'formatR', 'fs', 'generics', 'ggplot2',
      'glmnet', 'glue', 'gower', 'haven', 'hexbin', 'hms', 'htmltools',
      'htmlwidgets', 'httpuv', 'httr', 'ipred', 'IRkernel', 'iterators',
      'jsonlite', 'KernSmooth', 'knitr', 'labeling', 'later', 'lattice', 'lava',
      'lubridate', 'magrittr', 'markdown', 'MASS', 'Matrix', 'mgcv', 'mime',
      'ModelMetrics', 'modelr', 'nlme', 'nnet', 'numDeriv', 'openssl', 'pbdZMQ',
      'pillar', 'pkgconfig', 'plyr', 'prettyunits', 'processx', 'prodlim',
      'progress', 'promises', 'ps', 'purrr', 'quantmod', 'R6', 'Rcpp', 'readr',
      'recipes', 'repr', 'reprex', 'reshape2', 'rlang', 'rmarkdown', 'rstudioapi',
      'rvest', 'scales', 'selectr', 'shiny', 'spatial', 'SQUAREM', 'stringi',
      'survival', 'sys', 'tibble', 'tidyr', 'tidyselect', 'tidyverse', 'tinytex',
      'TTR', 'uuid', 'whisker', 'withr', 'xfun', 'xml2', 'xts', 'yaml', 'zoo'



```R
##---- expression_set ---- summary about the data
bm= bodymap.eset
bm
```

    Loading required package: Biobase
    Loading required package: BiocGenerics
    Loading required package: parallel
    
    Attaching package: ‘BiocGenerics’
    
    The following objects are masked from ‘package:parallel’:
    
        clusterApply, clusterApplyLB, clusterCall, clusterEvalQ,
        clusterExport, clusterMap, parApply, parCapply, parLapply,
        parLapplyLB, parRapply, parSapply, parSapplyLB
    
    The following objects are masked from ‘package:stats’:
    
        IQR, mad, sd, var, xtabs
    
    The following objects are masked from ‘package:base’:
    
        anyDuplicated, append, as.data.frame, basename, cbind, colnames,
        dirname, do.call, duplicated, eval, evalq, Filter, Find, get, grep,
        grepl, intersect, is.unsorted, lapply, Map, mapply, match, mget,
        order, paste, pmax, pmax.int, pmin, pmin.int, Position, rank,
        rbind, Reduce, rownames, sapply, setdiff, sort, table, tapply,
        union, unique, unsplit, which, which.max, which.min
    
    Welcome to Bioconductor
    
        Vignettes contain introductory material; view with
        'browseVignettes()'. To cite Bioconductor, see
        'citation("Biobase")', and for packages 'citation("pkgname")'.
    



    ExpressionSet (storageMode: lockedEnvironment)
    assayData: 52580 features, 19 samples 
      element names: exprs 
    protocolData: none
    phenoData
      sampleNames: ERS025098 ERS025092 ... ERS025091 (19 total)
      varLabels: sample.id num.tech.reps ... race (6 total)
      varMetadata: labelDescription
    featureData
      featureNames: ENSG00000000003 ENSG00000000005 ... LRG_99 (52580
        total)
      fvarLabels: gene
      fvarMetadata: labelDescription
    experimentData: use 'experimentData(object)'
    Annotation:  



```R
## --- Informations about the data expression
exp_data = exprs(bm)
```


```R
dim(exp_data)
```


<ol class=list-inline>
	<li>52580</li>
	<li>19</li>
</ol>




```R
## --- Informations about the data phenotype
pheno_data = pData(bm)



```


```R
head(pheno_data)
```


<table>
<thead><tr><th></th><th scope=col>sample.id</th><th scope=col>num.tech.reps</th><th scope=col>tissue.type</th><th scope=col>gender</th><th scope=col>age</th><th scope=col>race</th></tr></thead>
<tbody>
	<tr><th scope=row>ERS025098</th><td>ERS025098</td><td>2        </td><td>adipose  </td><td>F        </td><td>73       </td><td>caucasian</td></tr>
	<tr><th scope=row>ERS025092</th><td>ERS025092</td><td>2        </td><td>adrenal  </td><td>M        </td><td>60       </td><td>caucasian</td></tr>
	<tr><th scope=row>ERS025085</th><td>ERS025085</td><td>2        </td><td>brain    </td><td>F        </td><td>77       </td><td>caucasian</td></tr>
	<tr><th scope=row>ERS025088</th><td>ERS025088</td><td>2        </td><td>breast   </td><td>F        </td><td>29       </td><td>caucasian</td></tr>
	<tr><th scope=row>ERS025089</th><td>ERS025089</td><td>2        </td><td>colon    </td><td>F        </td><td>68       </td><td>caucasian</td></tr>
	<tr><th scope=row>ERS025082</th><td>ERS025082</td><td>2        </td><td>heart    </td><td>M        </td><td>77       </td><td>caucasian</td></tr>
</tbody>
</table>



The number of rows of the phenotype data need to match with the number of columns data of the data expression . This informations describe the samples.


```R
## --- Informations about the feature data
feature_data = fData(bm)
head(feature_data)

## Describe the genes
```


<table>
<thead><tr><th></th><th scope=col>gene</th></tr></thead>
<tbody>
	<tr><th scope=row>ENSG00000000003</th><td>ENSG00000000003</td></tr>
	<tr><th scope=row>ENSG00000000005</th><td>ENSG00000000005</td></tr>
	<tr><th scope=row>ENSG00000000419</th><td>ENSG00000000419</td></tr>
	<tr><th scope=row>ENSG00000000457</th><td>ENSG00000000457</td></tr>
	<tr><th scope=row>ENSG00000000460</th><td>ENSG00000000460</td></tr>
	<tr><th scope=row>ENSG00000000938</th><td>ENSG00000000938</td></tr>
</tbody>
</table>


