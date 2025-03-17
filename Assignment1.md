# Assignment 1

*Christina Tsagkogianni, PhD student in Neuroscience*

*Karolinska Institutet*

Read the research article of the hands-on working group you are assigned to (see file “Student Groups.pdf” in shared folder General course material) and answer the following questions:

## Task 1

#### What is the medically relevant insight from the article?

This study describes the transcriptional signatures of the 3 distinct areas of carotid atherosclerotic plaques: proximal, most stenotic, and distal. They find that plaque ruptures most often occur in the proximal and most stenotic regions of the the plaques. Using transcriptomics techniques, they identify MMP9 as a potential therapeutic target against plaque rapture.

#### Which genomics technology/ technologies were used?

Bulk RNA-Sequencing, to identify DEGs in the 3 plaque regions.

Spatial transcriptomics, to validate the location of the DEGs in the plaques

GWAS, to identify whether the identified DEGs were relevant to cardiovascular diseases

### Further research related questions

#### List and explain at least three questions/ hypotheses you can think of that extend the analysis presented in the paper.

1.  Predictive value of MMP9 as a biomarker for cardiovascular events: can the levels of circulating MMP9 serve as a biomarker to predict cardiovascular events early on?
2.  What is the role of epigenetic modifications in the site-specific vulnerability of atherosclerotic plaques?
3.  Correlate the transcriptomics data obtained from the study with proteomics data, using advanced proteomics strategies, such as single cell proteomics, or spatial/deep visual proteomics

#### Devise a computational analysis strategy for (some of) the listed questions under 3a.

1.  Conduct statistical analyses on large patient datasets to evaluate the association between circulating MMP9 levels and the incidence of cardiovascular events, adjusting for cofounders. For this project, longitudinal patient data will be analysed using statistical software such as R.
2.  The RNA-seq dataset will be integrated with ChIP-Seq or ATAC-Seq data to correlate gene expression with chromatin accessibility
3.  For the proteomics data, several techniques can be implemented. For single cell proteomics, isolate single cells using a FACS sorter and perform single cell mass-spec based proteomics in the relevant cell populations (macrophages, NK cells, B cells, SMCs). For spatial proteomics, the tissue will be sectioned. Then, using a laser capture microdissection microscope, the relevant plaque areas (proximal, most stenotic, distal) will be isolated and the proteome will be analysed using deep visual proteomics (Rosenberger et al, 2023, Nat methods).

## Task 4

```{r}
a <- sqrt(10)
message("The square root of 10 is ", print(a))
b <- log(32, base = 2)
message("The logarithm of 32 to the base 2 is ", print(b))
c <- sum(1:1000)
message("The sum of the numbers from 1 to 1000 is ", print(c))
d <- sum(seq(2, 1000, by = 2))
message("The sum of all even numbers from 2 to 1000 is ", print(d))
e <- choose(100, 2)
message("There are ", print(e), " pairwise comparisons for 100 genes")
```

I am not sure what the last question asks for :)

## Task 5

```{r}
data(CO2)
?CO2
```

The `CO2` data frame has 84 rows and 5 columns of data from an experiment on the cold tolerance of the grass species *Echinochloa crus-galli*.

```{r}
library(dplyr)
CO2 %>%
  group_by(Type) %>%
  summarise(
    mean_uptake = mean(uptake),
    median_uptake = median(uptake)
  )
```

## Task 6

### Question 1

```{r}
ratio <- function(x) {
  mean_val <- mean(x, na.rm = TRUE) 
  median_val <- median(x, na.rm = TRUE)
  return(mean_val / median_val)
}
```

### Question 2

```{r}
mean_func <- function(x) {
   x_trimmed <- x[x != min(x) & x != max(x)] #removing lowest and highest values
   return(mean(x_trimmed, na.rm = TRUE))
}
```

### Question 3

Pipes in R are used to make the code more readable and easy to understand, by structuring operations in a logical sequence. This can be unnecessary if the code is already simple. Also, when debugging it could be hard to tell where the error is coming from when the pipeline is long.

### Question 4

The apply family contains functions that essentially minimise the need to create loops, by applying a specific function to an object. When working with large datasets, such as single cell RNA seq data, it can help streamline the process by quickly performing repetitive tasks.

## Task 7
