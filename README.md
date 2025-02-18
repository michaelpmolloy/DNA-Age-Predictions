
# Predicting Age with DNA Methylation data
I completed this analysis in my final semester at Curtin University as part of the "Industrial Project" (MATH3004) unit to complete my Bachelor of Science (Data Science) degree.
## Overview

This repository contain analysis of different predictive models which use methylation levels present in human DNA to produce an age prediction. In this analysis 3 different models are compared to provde insight into their releative predictive accuracy when applied to high-dimensional datasets. 
## Project Objective

The objective of this research is to create an epigenetic clock model to predict chronological age from a person’s DNA methylation profile. Elastic-net regression and neural networks will be used to model age, and the performance of both models compared.


This project provides insight into the relative prediction accuracy of elastic-net and neural networks on age estimation when applied to a large methylation dataset. While there is research available that applies these methods singularly, there is no extensive research on their comparative predictive abilities when applied to the same data. The best neural network will be compared against the best elastic net regression to compare the two methods. The two neural networks will also be compared to check the dimensionality reduction capabilities of PCA vs UMAP. 
## Dataset 

The data set chosen for this study includes Illumina 450k DNA methylation array data. The dataset is from the Powell et al. (2012) study which studied the epigenetics of families and the heritable similarity between family member’s epigenetics. The data includes 257 samples of 450 000 variables from people between the ages of 10 and 75. The data is not independent as it includes the epigenetic profiles of families which may share heritable epigenetic traits. The data is provided in two different files. The "BSGMethylation.csv" file contains the ID of the person and all their 450 000 methylation levels. The "BSGInfo.csv" file contains the ID, age, gender and twin status.

## Data Cleaning and Preprocessing 

The data set provided from the Powell et al. (2012) study is pre-processed to avoid bias and error from the array collection method. The pre-processing included the “removal of background chip effect, removal of outliers, computation of average bead signal and calculation of detection p-values using negative controls presented on the array” Powell et al. (2012).

The dataset cotained 0.24% missing values which were imputed based on gender as Hannum et. al (2013) showed that the gender of a person effects their methylation profile. 
## Tools Used 
* **R Studio**: Data prep and cleaning, PCA reduction, UMAP reduction, Elastic-Net regression and visualisation
* **Python**: Tensorflow neural networks, visualisations 



## Optimisations 
**Data structure**: When the BDGMethylation.csv file provided is transposed to the correct orientation it increased in size from 1.27Gb to 8GB due to the extremely feature number which is not ordinary or optimised for in most software. I transformed the data from a 'Data Frame' to a 'Numeric matrix' which reduced the data size from 8Gb to 1Gb. The 'Numeric matrix' data structure optimised the datastructure making reading and writing the dataframe wuch more efficient and less time consuming. 

**Min-Max Scaling**: Scaling was applied to the data as neural networks trained with features of the same scale have improved gradient descent in the training of a neural network resulting in a faster convergence (Al-Faiz et al. 2018).
## Data Analysis 
* PCA dimensionaity reduction was applied and plotted as shown below
 ___PCA_PLOT_PHOTO
## Findings 
## Conclusions 
## Acknowledgements
* Powell JE, Henders AK, McRae AF, Caracella A, Smith S, Wright MJ, et al. (2012) The Brisbane Systems Genetics Study: Genetical Genomics Meets Complex Trait Genetics. PLoS ONE 7(4): e35430. https://doi.org/10.1371/journal.pone.0035430 (Data Source)

* Hannum, Gregory, Justin Guinney, Ling Zhao, Li Zhang, Guy Hughes, SriniVas Sadda, Brandy Klotzle, et al. 2013. “Genome-Wide Methylation Profiles Reveal Quantitative Views of Human Aging Rates.” Molecular Cell 49 (2): 359–67. https://doi.org/10.1016/j.molcel.2012.10.016. (Affect of gender on Methylation profiles)

* Al-Faiz, Mohammed Z, Ali Abdulhafidh Ibrahim, and Sarmad M Hadi. 2019. “The Effect of Z- Score Standardization (Normalization) on Binary Input Due the Speed of Learning in Back- Propagation Neural Network.” Iraqi Journal of Information and Communication Technology 1 (3): 42–48. https://doi.org/10.31987/ijict.1.3.41.