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

### Compare the distributions of the body heights of the two species from the 'magic_guys.csv' dataset graphically

```{r}
library(ggplot2)
data <- read.csv("magic_guys.csv", header = TRUE)
par(mfrow=c(2,2))
hist(data$length[data$species == "jedi"],
     main = "Jedi length distribution",
     xlab = "Length",
     col = "darkcyan",
     breaks = 10)
hist(data$length[data$species == "jedi"],
     main = "Jedi length distribution",
     xlab = "Length",
     col = "darkcyan",
     breaks = 15)
hist(data$length[data$species == "sith"],
     main = "Sith length distribution",
     xlab = "Length",
     col = "darkmagenta",
     breaks = 10)
hist(data$length[data$species == "sith"],
     main = "Sith length distribution",
     xlab = "Length",
     col = "darkmagenta",
     breaks = 15)

ggplot(data, aes(x = length, fill = species)) + 
  geom_histogram(bins = 10) +
  scale_fill_manual(values = c("darkcyan", "darkmagenta")) +
  theme_minimal()
ggplot(data, aes(x = length, fill = species)) + 
  geom_histogram(bins = 15) +
  scale_fill_manual(values = c("darkcyan", "darkmagenta")) +
  theme_minimal() 

par(mfrow=c(1,1))
boxplot(data$length ~ data$species,
        main = "Length distribution by species",
        xlab = "Species",
        ylab = "Length",
        col = c("darkcyan", "darkmagenta"))
ggplot(data, aes(x = species, y = length)) +
  geom_boxplot(fill = c("darkcyan", "darkmagenta")) +
  theme_minimal()
```

For demonstration purposes, I will save the last boxplot

```{r}
img <- ggplot(data, aes(x = species, y = length)) +
  geom_boxplot(fill = c("darkcyan", "darkmagenta")) +
  theme_minimal()
install.packages("svglite")
library(svglite)
ggsave("img.png", img)
ggsave("img.pdf", img)
ggsave("img.svg", img)
```

.png is good for presentations and general screen viewing, as you can manipulate the background. .pdf is good for printing and publications. .svg is good for embedding in websites.

### Load the gene expression data matrix from the ‘microarray_data.tab’ dataset provided in the shared folder, it is a big tabular separated matrix.

```{r}
data <- read.table("microarray_data.tab", header = TRUE, sep = "\t")
```

How big is the matrix in terms of rows and columns?

```{r}
dims(data)
```

Distribution of missing values

```{r}
missing <- rowSums(is.na(data))
ggplot(data.frame(missing), aes(x=missing)) +
  geom_histogram(fill="plum4", color="black", bins=30) +
  labs(title="Distribution of Missing Values per Gene",
       x="# of Missing Values", 
       y="# of Genes") +
  theme_minimal()
```

Find the genes for which there are more than X% (X=10%, 20%, 50%) missing values.

```{r}
threshold <- c(0.1, 0.2, 0.5)
for (i in threshold) {
  cutoff <- i * ncol(data)
  genes_filtered <- rownames(data)[missing > cutoff])
 
  print(dim(genes_filtered))
})

###I am not sure what the output is supposed to be here :(
```

Impute values: I am writing a function called impute that will replace all missing values with the average expression value.

```{r}
impute <- function(x) { 
  avg <- colMeans()
  for (i in 1:ncol(x)) {
    x[, is.na(x[, i])] <- avg[i]
  }
  return(x)
}
```

Visualise the data in the CO2 dataset

```{r}
data(CO2)
ggplot(CO2, aes(x=conc, y=uptake, color=Treatment)) +
  geom_point(size=2, alpha=0.9) +
  geom_smooth(method="lm", se=TRUE) +
  facet_wrap(~Type) +
  scale_color_manual(values = c("darkcyan", "darkmagenta")) +
  labs(title="CO2 Uptake",
       x="Ambient CO2 concentration (mL/L)",
       y="CO2 uptake rate (µmol/m² sec)") +
  theme_minimal()
```

CO2 uptake increases with CO2 concentration Also, Missisipi plants react differently to the treatment, compared to Quebec, where plants behave the same.

## Task 8

### Installing the package 

```{r}
devtools::install_github("hirscheylab/tidybiology")
library(tidybiology)
library(tidyverse)
data("chromosome")
data("proteins")
colnames(chromosome)
colnames(proteins)
#important to create the graphs below
```

Summary statistics for "chromosome

```{r}
chromosome %>%
  summarize(across(c(variations, protein_codinggenes, mi_rna),
                   list(mean = function(x) mean(x, na.rm = TRUE),
                        median = function(x) median(x, na.rm = TRUE),
                        max = function(x) max(x, na.rm = TRUE))))
```

Chromosome size distribution

```{r}
ggplot(chromosome, aes(x = length_mm)) +
  geom_histogram(aes(y = ..density..), bins = 15, fill = "plum4", color = "black") +
  geom_density(color = "black") +
  labs(title = "Distribution of chromosome size",
       x = "Chromosome size (mm)",
       y = "Count") +
  theme_minimal() 
```

Correlation of protein coding genes and miRNAs to chromosome length

```{r}
ggplot(chromosome, aes(x = protein_codinggenes, y = length_mm)) +
  geom_point(color = "hotpink4", size = 3) +
  geom_smooth(method = "lm", color = "grey40", se = TRUE) +
  labs(title = "Correlation of chromosome size and length",
       x = " # of protein coding genes",
       y = "Chromosome size (mm)") +
  theme_minimal() 
         
ggplot(chromosome, aes(x = mi_rna, y = length_mm)) +
  geom_point(color = "lightseagreen", size = 3) +
  geom_smooth(method = "lm", color = "grey40", se = TRUE) +
  labs(title = "Correlation of number of miRNAs and length",
       x = " # of miRNAs",
       y = "Chromosome size (mm)") +
  theme_minimal() 
```

Protein length and mass statistics and visualisation

```{r}
proteins  %>%
  summarize(across(c(length, mass),
                   list(mean = function(x) mean(x, na.rm = TRUE),
                        median = function(x) median(x, na.rm = TRUE),
                        max = function(x) max(x, na.rm = TRUE))))
            
ggplot(proteins, aes(x = length, y = mass)) +
  geom_jitter(width = 0.7, height = 0.7, color = "darkred", size = 1, alpha = 0.4) +
  geom_smooth(method = "lm", color = "grey40", se = TRUE) +
  scale_y_log10() +
  scale_x_log10() +
  labs(title = "Correlation of protein length and mass",
       x = "Protein length (aa)",
       y = "Protein mass (kDa)") +
  theme_minimal() 
```
