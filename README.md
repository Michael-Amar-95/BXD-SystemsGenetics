# BXD-SystemsGenetics

## Overview

This project investigates the relationship between genetic variation, intermediate molecular phenotypes, and behavioral traits in BXD recombinant inbred mice.

The main objective was to explore whether gene expression in different tissues can help explain the genetic basis of physiological and behavioral responses. Specifically, we focused on two biological contexts:

- **Morphine response:** Investigating the relationship between morphine-induced behavioral responses and gene expression in liver and myeloid cells.
- **Novel environment response:** Investigating whether behavioral changes following exposure to a new environment, measured by urination frequency, are associated with gene expression in these tissues.

We hypothesized that myeloid gene expression would have limited association with morphine response, while liver gene expression would have limited association with behavioral responses to a novel environment.

To investigate these hypotheses, we integrated BXD genotyping data, gene expression profiles from two tissues, and physiological and behavioral phenotypes. The analysis included QTL mapping, eQTL analysis, integration of genetic associations, and statistical causality testing.

## Datasets

### Gene expression data

Gene expression datasets were obtained from the NCBI Gene Expression Omnibus (GEO):

| Tissue | GEO accession |
|---|---|
| Myeloid cells | [GSE18067](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE18067) |
| Liver | [GSE17522](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE17522) |

### Behavioral and physiological phenotypes

We selected two phenotypes from the BXD phenotype collection:

| Phenotype ID | Description |
|---|---|
| 1002 | Novel open-field behavior: number of urinations in males during the test period |
| 1003 | Morphine response (50 mg/kg, IP): vertical activity (rears), measured 90–105 minutes after injection in an activity chamber, for males |

### Genetic data

BXD strain genotyping data were used to identify genetic loci associated with gene expression and the selected phenotypes.

## Methodology

### 1. Data preprocessing

Gene expression data were processed separately for liver and myeloid tissues.

The preprocessing steps included:

- Reorganizing the expression matrices so that rows represented genes (annotated by gene symbols) and columns represented BXD strains.
- Removing genes without valid identifiers.
- Averaging expression measurements across repeated observations of the same strain.
- Filtering genes with low maximum expression values and low variance, using the 35th percentile as the filtering threshold.
- Combining multiple probes corresponding to the same gene by averaging their expression values.

After preprocessing:

| Tissue | Genes | BXD strains |
|---|---:|---:|
| Myeloid | 9,155 | 25 |
| Liver | 6,983 | 41 |

Genotype preprocessing included retaining relevant loci and filtering neighboring loci with identical genotype patterns.

The resulting datasets contained:

| Tissue | Genomic loci |
|---|---:|
| Myeloid | 850 |
| Liver | 1,403 |

### 2. eQTL analysis

We performed genome-wide association analysis to identify genetic variants associated with gene expression.

For each gene, linear regression was used to test the association between its expression level and each available genetic locus.

Multiple testing correction was performed using the False Discovery Rate (FDR) procedure. Significant associations were retained as eQTLs.

We further classified eQTLs into:

- **Cis-eQTLs:** Genetic loci located near the associated gene.
- **Trans-eQTLs:** Genetic loci located at distant genomic positions.

Results:

| Tissue | Association tests | eQTLs | Cis-eQTLs | Trans-eQTLs |
|---|---:|---:|---:|---:|
| Myeloid | 9155 × 850 | 1,643 | 459 | 1,184 |
| Liver | 6983 × 1403 | 5,627 | 1,403 | 4,224 |

Analysis of the eQTL distributions revealed several genomic regions with elevated numbers of associated genes, particularly in liver tissue. One prominent region was identified around genomic position 1.75 × 10⁹ bp.

The liver dataset exhibited a larger concentration of trans-eQTL associations, whereas the myeloid dataset showed a relatively stronger cis-eQTL pattern.

In both tissues, trans-eQTLs exhibited a wider interquartile range (IQR) of p-values, while cis-eQTLs generally showed stronger statistical associations (lower p-values).

### 3. QTL analysis

We performed genome-wide association analysis to identify genetic loci associated with the two selected behavioral phenotypes.

Linear regression was used to test the association between each phenotype and the available genetic loci. Redundant SNPs with identical genotype patterns were consolidated before analysis.

Multiple testing correction was applied to control for false-positive associations.

Only phenotype 1003 (morphine-induced vertical activity) yielded significant QTL associations. Three significant loci were identified, all located on chromosome 10.

Phenotype 1002 (urination frequency in a novel environment) showed elevated association signals on chromosomes 1 and 8, but these did not reach the significance threshold.

Significant QTL results:

| SNP | Chromosome | -log10(p-value) |
|---|---:|---:|
| rs3721803 | 10 | 5.1516 |
| rs3695003 | 10 | 5.2110 |
| D10Mit28 | 10 | 5.1516 |

### 4. Integration of QTL and eQTL results

To investigate whether gene expression could provide additional information about the genetic basis of the behavioral phenotypes, we compared the significant QTLs with the identified eQTLs.

We focused on associations where the QTL and eQTL positions were within 2 Mb of each other.

Since significant QTLs were identified only for phenotype 1003, the integration analysis focused on morphine-induced vertical activity.

The comparison identified:

- Two of the three significant QTLs had matching nearby eQTL associations.
- Seven gene–SNP associations were identified.
- All seven associations originated from the liver expression dataset.
- No corresponding associations were identified in the myeloid dataset.

The associated genes were:

- Aig1
- BC013529
- Olfr1121

These findings suggest that liver gene expression may provide useful intermediate molecular information for investigating the genetic basis of morphine response.

The identified associations were subsequently examined using causality analysis.

### 5. Causality analysis

In the final stage, we applied a statistical causality test to investigate potential relationships between genetic loci, gene expression, and behavioral phenotypes.

The analysis included gene–phenotype pairs for which the corresponding QTL and eQTL were located within 2 Mb of each other.

Only liver-derived associations were available for this analysis.

For each gene–SNP–phenotype triplet, we calculated the log-likelihood of the competing causal models and evaluated their relative support using a likelihood ratio (LR).

A permutation test was performed on the selected triplets to assess the statistical significance of the proposed causal relationships.

The analysis included the following genes:

- Aig1
- BC013529
- Olfr1121

The results consistently selected model M3 as the best-fitting model across the tested triplets.

However, the permutation test yielded high p-values:

| SNP | Genes | Best model | Permutation p-value |
|---|---|---|---:|
| rs3721803 | Aig1, BC013529, Olfr1121 | M3 | 0.80 |
| D10Mit28 | Aig1, BC013529, Olfr1121 | M3 | 0.65 |

None of the tested causal hypotheses reached statistical significance.

Therefore, despite identifying significant genetic associations and nearby gene expression signals, the causality analysis did not provide sufficient statistical evidence to establish causal relationships between the identified genes and morphine-induced behavioral responses.

## Conclusions

This project demonstrates an integrative systems genetics approach for investigating genotype-to-phenotype relationships using multiple molecular and behavioral datasets from BXD mice.

The main findings were:

1. **Tissue-specific eQTL patterns:** Liver and myeloid expression datasets exhibited different genomic association patterns, with a larger number of identified eQTLs in liver.
2. **Morphine-associated QTLs:** Three significant QTLs associated with morphine-induced vertical activity were identified on chromosome 10.
3. **Added value of gene expression:** Integrating QTL and eQTL results identified seven nearby gene–SNP associations, exclusively in the liver dataset.
4. **Causality limitations:** Although the association analysis identified candidate genes, permutation testing did not support statistically significant causal relationships.

Overall, the results highlight the potential value of integrating genetic variation with tissue-specific gene expression to investigate complex phenotypes, while emphasizing the distinction between statistical association and evidence of causality.

## Technologies and Methods

- **Programming:** Python
- **Statistical analysis:** Linear regression, association testing, FDR correction
- **Genetics:** BXD recombinant inbred mice, QTL mapping, eQTL mapping
- **Multi-omics integration:** Genotype and gene expression association analysis
- **Causal inference:** Likelihood-based model comparison and permutation testing
- **Visualization:** Manhattan plots and genomic association distributions

## Data Availability

The original gene expression datasets are publicly available through the NCBI Gene Expression Omnibus (GEO).

Due to their file size, the original SOFT family files are not included in this GitHub repository. They can be downloaded directly from NCBI using the links below:

| Dataset | Tissue | GEO Accession | Download |
|---|---|---|---|
| BXD Mouse Liver Gene Expression | Liver | GSE17522 | [GSE17522_family.soft.gz](https://ftp.ncbi.nlm.nih.gov/geo/series/GSE17nnn/GSE17522/soft/GSE17522_family.soft.gz) |
| BXD Hematopoietic Cell Gene Expression | Myeloid cells | GSE18067 | [GSE18067_family.soft.gz](https://ftp.ncbi.nlm.nih.gov/geo/series/GSE18nnn/GSE18067/soft/GSE18067_family.soft.gz) |

The downloaded files are compressed using gzip. Extract them before running the preprocessing scripts.

For additional dataset information, sample annotations, and supplementary files, refer to the original GEO records:

- [GSE17522 – Liver gene expression](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE17522)
- [GSE18067 – Hematopoietic cell gene expression](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE18067)
