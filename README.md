README
================
yutian
3/28/2023

# Introduction

The Immune Variation (ImmVar) project assayed gene expression in CD14
+CD16 − monocytes and CD4 + T-cells on the Affymetrix Human Gene 1.0 ST
Array platform in order to characterize the role of cell type in genetic
regulation of gene expression. Here, I am going to use `variancePartition` to analyze data from this project. Details about [variancePartition](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-016-1323-z#MOESM1)

## EXERCISE

### 1. Read variancePartition paper

### 2. Apply it to ImmVar dataset and describe what you learned about biology of gene expression

See the results of applying variancePartition to ImmVar dataset at [here](https://github.com/Margery0011/variancePartition/tree/main/Interviewtest/variancePartition)
 
Gene expression is the process by which the information encoded in a gene is turned into a function, `variancePartition` can be used to 
interpret drivers of variation in complex gene expression studies. The variancePartition software and workflow have been developed to make it easier to analyze complex gene expression datasets. In this study, we have data gene expression  data in CD14
+CD16 − monocytes and CD4 + T-cells, `VariancePartition` uses the power of the linear mixed model to jointly quantify the contribution of multiple sources of variation in high-throughput genomics studies.

We analyzed 984 objects from both cell types and found that multiple variables contribute to expression variation in this dataset. After removing the batch effect, it became apparent that the variation across cell types is the strongest biological driver of variation (22.1%), followed by variation across individuals (8.2%)  Variation across batches is negligible after correcting for variation due to other variables. Additionally, the effects of age and sex are also negligible after correcting for other variables.

Details can be found [here](https://github.com/Margery0011/variancePartition/tree/main/Interviewtest/variancePartition)

### 3. How would you apply this framework to a biological question you are interested in?

First, prepare for the inputs, we should have three components to for the analysis. They should be `Gene expression data`, `Meta-data about each experiment` and `Formula indicating which meta-data variables to consider`

 `Gene expression data` should be a matrix of normalized gene
expression values with genes as rows and experiments as columns. It can be Count-based quantification,  Isoform quantification or  Microarray data.

`Meta data` should be A data.frame with information
about each experiment we are insterested in. 

`Formula ` is An R
formula such as
∼ Age + (1|Individual) + (1|Tissue) + (1|Batch) indicating which
meta-data variables should be used in the analysis.
variancePartition will assess the contribution of each meta-data variable to vari-
ation in gene expression and can report the intra-class correlation for each vari-
able.

Before using variancePartition to run the analysis, we should ensure that the expression data has already been normalized. Then, we can load the library and specify the variables we are interested in. 

As for choosing the varaibles,  we could first include all variables in the first analysis and then drop variables that have minimal effect, also, including variables in the model that do not vary within an individual will reduce the apparent contribution of the individual as estimated by variancePartition. Categorical variables should (almost) always be modeled as a random effect. We should also decide to model the variables as fixed or random effect. Categorical variables should (almost) always be modeled as a random effect. 

Batch removal is also a neceessary part since we want to focus on biological variables In RNA-seq analysis, it is common for the the impact of this technical variability to be removed before downstream analysis.


After that, we can fit the model and extract the results. A linear mixed model is fitted on gene expression. If categorical variables are specified, a linear mixed model is used. If all variables are modeled as fixed effects, a linear model is used. Each entry in the results is a regression model fit on a single gene.

Variance fractions are extracted from each model fit. For each gene, the fraction of variation attributable to each variable is returned. Lastly we should interpretate these results provided by the analysis, it gives us insight into the expression data at multiple levels.


 # variancePartition
https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-016-1323-z

# Data from ImmVar are described here and can be downloaded from GEO: 
    doi:10.1126/science.1249547
    doi:10.1016/j.smim.2015.03.003
