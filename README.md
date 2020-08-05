# Parkinson Risk score calculation in FOUNDIN data

## Genetic Risk Score Calculation

- **Date Written:** August 5 2020
- **Date Last Updated:** August 5 2020
- **Workflow obtained from:** https://github.com/neurogenetics/genetic-risk-score
- **Cornelis and Mary** 

### Description
Genetic risk scores (AKA: polygenic scores, polygenic risk scores, or genome-wide scores etc) is a summary measure of a set of risk-associated genetic variants and can be easily calculated using PLINK

### Requirements 
1. **PLINK** (v1.9 is used here)
	 - The most common and easiest way to do this in PLINK ([v1.9](https://www.cog-genomics.org/plink/1.9/score))
	 - Another way to do this is using [PRSice](https://choishingwan.github.io/PRSice/)
2. **PLINK Binary Files** (`.bed`, `.bim`, `.fam`) 
3. **Score File** 
	- This is a file with a variant identifier, allele, and an associated score value
	- These scores come from GWA Studies 
	- This [repository](https://github.com/neurogenetics/genetic-risk-score) has the risk score file for Parkinson's disease based on the most recent PD GWAS (META5) [Nalls _et al._, 2019](https://pubmed.ncbi.nlm.nih.gov/31701892/)
		- `META5_GRS_RSid.txt` has variants structured as RS-IDs
		- `META5_GRS_RSid_no_GBA_no_G2019S.txt.txt` has variants structured as RS-IDs and is excluding the full GBA region and LRRK2 G2019S (4 variants differ)
	- The format of the file should match this example: 
```
rs356182	A	-0.2774
rs62053943	T	-0.27
rs117615688	A	-0.2324
rs75859381	T	-0.2207
rs34311866	T	-0.2126
```

### PLINK Commands

```bash 
module load plink #if on Biowulf, this loads v1.9
plink --bfile $yourfile --score $scorefile --out $outputfilename
```
Where: 
- `$yourfile` = standard binary file prefix (will point to `.bed`, `.bim`, and `.fam` files)
- `$outputfilename` = whatever you want it to be, the output will have the extension `.profile`
- `$scorefile` = file with variant-name, allele and score-value

actual commands:

```bash
plink --bed wgs_hg38_ppmi.july2018.bed --bim wgs_hg38_ppmi.july2018_ORIGINAL.bim \
--fam wgs_hg38_ppmi.july2018.fam --score META5_GRS_RSid.txt --out FOUNDIN_GRS
# 90 valid predictors loaded.

plink --bed wgs_hg38_ppmi.july2018.bed --bim wgs_hg38_ppmi.july2018_ORIGINAL.bim \
--fam wgs_hg38_ppmi.july2018.fam --score META5_GRS_RSid_no_GBA_no_G2019S.txt --out FOUNDIN_GRS_no_GBA_LRRK2
# 86 valid predictors loaded.
```

