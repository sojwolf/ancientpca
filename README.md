# ancientpca
A Probabilistic Approach to Conducting PCA with Sparse Genotype Data from Ancient Samples

## Install and load in R

```
library(devtools)
install_github('sojwolf/ancientpca')
library(ancientpca)
```

## Dependencies

R packages you need installed:
```
library(vcfR)
library(softImpute)
library(ggplot2)
library(ggrepel)
library(cowplot)
```

## Input data

1. **Genotypes:** vcf-file with genotypes only (i.e. "0/0", "0/1", etc.). Missing values can be coded as "." or "./."
2. **Meta Data:** csv-file that contains headers "Sample_ID" and "Population" (can contain other columns too) for at least all samples in your vcf-file.


## Example workflow

1. **Load vcf-file** into R and subsample, if need be by setting the maximum allowed missingness per sample and SNP. The default is set to 1, i.e. 100%. The vcf-file can be zipped.

```
gt <- vcfToGenotypeMatrix(vcf_file="vcf_file.vcf", max_missing_snp=0.9, max_missing_sample=0.95)
```

2. **Impute** missing genotypes using package [softImpute](https://web.stanford.edu/~hastie/swData/softImpute/vignette.html)

```
gt_imputed <- impGenotypeMatrix(genotype_matrix=gt)
```

3. **Calculate PCA** using R's prcomp function

```
pca_gt <- prcomp(gt_imputed, center = TRUE, scale. = TRUE)
```

4. **Plot** PCs 1-6 and save output to PDF file.

```
plotImpPCA(pca_obj=pca_gt, imputed_matrix=gt_imputed, original_matrix=gt, max_missing_snp=0.9, max_missing_sample=0.95,
  meta_file = "meta.csv", output_pca_pdf = "output.pdf")
```
