# Genome-Wide Association Studies (GWAS) Pipeline

 ### Part I. Preprocessing of genotype and phenotype data
 #### a. Quality Control of genotype data (including array, sequenced, imputed data)
 #### b. Select covariates and combine them with phenotype, PCs (or global ancestry)

 ### Part II. Association test and downstream analalysis
 #### c. Conduct GWAS for common variants
 d. CODA: compute effective number of tests
 e. Create Manhattan plots


For the example tutorials for different pipelines, go to this wiki (https://github.com/izeunice05/test/wiki).

This is a GWAS excercise/tutorial example code using Taiwan Biobank

| Command   |    Description         |
|---------  |------------------------|
| --geno    | missingness per marker |


```
cd /project/elee4754_1222/gwas_mec/NH/merged/plink_format\
```
plink --merge-list /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_merge.txt --make-bed --out megagda_allchr

plink --bfile megagda_allchr --geno 0.05 --mac 1 --make-bed --out /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.geno
plink --bfile /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.geno --mind 0.05 --make-bed --out /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.mind
plink --bfile /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.mind --maf 0.01 --make-bed --out /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.maf
plink --bfile /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.maf --hwe 0.0001 --make-bed --out /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchrhwe
plink --bfile /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.hwe --genome --min 0.025 --make-bed --out /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.genome
plink --bfile /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.genome --snps-only --make-bed --out /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.snps 
plink --bfile /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.snps --recode A --out /project/elee4754_1222/gwas_mec/qc_imputed_data/recode_megagda_allchr.snps

# Plink logistic
cd /project/elee4754_1222/gwas_mec
plink --bfile /project/elee4754_1222/gwas_mec/qc_imputed_data/megagda_allchr.snps --logistic --pheno pheno_anyT2D_nh_all.txt --covar covariates_anyT2D_nh_all.txt --allow-no-sex --ci 0.95 --out /project/elee4754_1222/gwas_mec/gwas_T2D/gwas.t2d.snps
