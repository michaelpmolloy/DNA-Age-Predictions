
# Predicting Age with DNA Methylation data
My capstone project to complete my Bachelor of Science (Data Science) degree at Curtin university.

## Overview

Analysis of different age prediction models using methylation levels present in human DNA. An elastic-net regression is created from the dataset. A neural network is trained using the first 60 principle components of Principle Component Analysis on the dataset. A second neural network is trained using the first 16 components of Uniform Manifold Approximation (UMAP) on the dataset. 


## Methylation 
DNA methylation is an epigenetic process used by the body to influence gene expression and cellular function without changing the structure or sequence of the DNA. Methylation usually occurs when a methyl group attaches to cytosine in DNA (Moore, Le, and Fan 2012). Methylation is commonly measured using an Illumina 450k array which includes a methylation level between 0 and 1 for 450 000 methylation sites around the body. Changes in DNA methylation can occur due to several external factors including age, diet and lifestyle (Kader, Ghai, and Maharaj 2018).

![methylation diagram](Images/methylationDiagram.jpg)


## Project Objective

* Create an epigenetic clock model to predict chronological age from a person’s DNA methylation profile using elastic-net regression and neural networks
* Provide insight into the relative prediction accuracy of elastic-net and neural networks when applied to high-dimensional data sets
* Compare the dimensionality reduction capabilities of principle component analysis to Uniform Manifold Approximation 

## Tools Used 
* **R Studio**: Data prep and cleaning, PCA reduction, UMAP reduction, Elastic-Net regression and visualisation
* **Python**: Tensorflow neural networks, visualisations 


## Neural Networks 

Neural networks can be applied to methylation data due to their ability to handle large complex data sets and learn non-linear relationships between variables. Dimensionality reduction is used to reduce the number of input features therefore decreasing the amount of data needed to train the network. Larger neural networks need more samples needed to fully train all the parameters in the model. DNA methylation samples are costly therefore dimensionality reduction allowed neural networks to be trained with limited samples/budget. 

### Uniform Manifold Approximation (UMAP)

The UMAP transformation is computationally expensive therefore 16 features were the maximum number of features it could produce. Through experimentation and testing a value of 20 neighbours was found to give the best results allowing both global and local features to be preserved in the reduced data. The optimal minimum distance was found to be 0.1. The resulting lower-dimensional dataset showed significant separation between gender and some separation of different ages as shown below. 

<img src="Images/UMAPplot.png" alt="UMAP Plot">


### UMAP Neural Network

The best UMAP trained neural network used all 16 components and achieved a standard error of 9.08 years and an R squared value of 0.695 and presented in table 2. The best UMAP trained neural network relied on a neural network with a 16 neural input layer, 4 hidden layers decreasing from 14 to 5, then an output layer. Three dropout layers were implemented between the first four layers. 

![UMAP Neural Network Plot](Images/UmapNnAccuracyPlot.png)


### Principal Component Analysis (PCA)

Principal component analysis is first applied producing 255 principal components. The PCA plot of the first two components below displayed a significant male outlier that had a PCA 2 component of over 30 which was significantly larger than the remaining samples therefore it is concluded it is an outlier and removed. The PCA plot showed significant seperation of gender in the PCA 1 component and slight age separation in the PCA 2 component. 

![PCA plot](Images/PCAplot.png)

### PCA Neural Network

The best PCA trained neural network relied on a neural network with a 60 neural input layer, 4 hidden layers decreasing from 40 to 5, then an output layer. Three dropout layers were implemented between the first four layers to ensure the neural network did not over-train. The optimal model achieved a standard error of 5.378 years.


![PCA Neural Network Plot](Images/PcaNnAccuracyPlot.png)




## Elastic-net regression 
Elastic net regression is a linear hybrid regression technique which uses the Lasso penalty and Ridge penalty to fit data to a linear model with the lowest Residual Sum of Squares (RSS). The Lasso and Ridge penalties are used to prevent overfitting with each penalty having a specific purpose. The elastic-net regression combines the benefits of Lasso and Ridge regression allowing the model to both shrink and remove parameters. An elastic-net regression is therefore suitable for the high-dimensional methylation data set as it can shrink or remove variables allowing for correlated or unimportant variables to be removed. The loss function used by the elastic-net regression is shown below. 

![elastic-net formula](Images/elasticNetFormula.jpg)

The elastic-net regression model is using the ‘glmnet’ package in R studio. The ‘cv.glmnet’ function uses 10 fold cross validation to train, test and optimise the model parameters while using only the training data. The optimal model achieved a standard error of 2.605 years and an R squared coefficient of 0.975. It used 102 different methylation sites to predict age. The predictions and real age values are visually presented below. The model was very accurate at predicting young people between the ages 10 to 20, however it was marginally less accurate at predicting older people between ages 35 to 60.


![elastic-net prediction accuracy plot](Images/elasticNetAccuracyPlot.png)


## Comparing models 

The data contains two major age groups in the data with a young group and an old group. The PCA neural network and elastic-net were both able to accurately predict the age of younger people as shown in figure 10. The UMAP neural network did not predict the age of the young cohort accurately with most predictions being older than their real age. The accuracy of both neural networks reduced significantly when predicting the age of the older cohort. The elastic-net model outperformed both neural network models with very accurate predictions in the older cohort, and with only a small number of predictions having any significant error.


![all models prediction accuracy plot](Images/allModelsAccuracyPlot.png)


## Conclusions

### Dimensionality reduction 

* **Outlier detection**: The PCA plot was able to identify an outlier which once removed improved the predictions of both neural network models. The UMAP plot did not visually separate the outlier and grouped it with the other data.

* **Neural net accuracy**: The PCA neural network performed accurately with a standard error of 5.38 which is significantly less than the UMAP neural networks standard error of 9.08, which indicates that the PCA reduction was able to retain more of the local and global structure of the original data.

* **Computational efficiency**: PCA was significantly more efficient at dimensionality reduction allowing the creation of up to 256 (limited to the number of samples provided to it) principle components. UMAP dimensionality reduction could produce a maximum of 16 components on the same machine indicating that UMAP dimensionality reduction takes significantly more computational power to produce components. 


### Elastic-net compared to neural networks (PCA neural network)

* **Accuracy**: The elastic-net model significantly outperformed the best neural network model (PCA neural network) especially in the older cohort which the neural network struggled to give accurate predictions. 

* **Implementation**: The elastic was significantly easier to implement as it automatically performed feature selection and training, compared to neural networks which needed dimensionality reduction then training. 

* **Model interpretability**: The elastic-net model was very interpretable as all the 102 parameters used to predict age relate to specific methylation levels at sites around the body. The elastic-net model therefore shows which methylation sites around the body are most correlated to age. The neural network was not very interpretable as the PCA input parameters are combinations of the original methylation site levels therefore inputs for the neural network cannot be traced to specific methylation site around the body like in the elastic net model.




## Details and acknowledgments 


### Dataset 

The data set used contains Illumina 450k DNA methylation array data. The dataset is from the Powell et al. (2012) study which studied the epigenetics of families and the heritable similarity between family member’s epigenetics. The data includes 257 samples of 450 000 variables from people between the ages of 10 and 75. The data is not independent as it includes the epigenetic profiles of families which may share heritable epigenetic traits. Due to the large size of the dataset 1.27Gb it is not provided in the repository 

### Data Cleaning and Preprocessing 

The data set provided from the Powell et al. (2012) study is pre-processed to avoid bias and error from the array collection method. The pre-processing included the “removal of background chip effect, removal of outliers, computation of average bead signal and calculation of detection p-values using negative controls presented on the array” Powell et al. (2012).

The dataset contained 0.24% missing values which were imputed based on gender as Hannum et. al (2013) showed that the gender of a person effects their methylation profile. 



### Optimisations 
**Data structure**: The DNA dataset was 8GB when in the from of an R data frame. I transformed the data from a 'Data Frame' to a 'Numeric matrix' which reduced the data size from 8Gb to 1Gb. The 'Numeric matrix' data structure optimised the data structure making reading and writing the data frame much more efficient.

**Min-Max Scaling**: Scaling was applied to the data as neural networks trained with features of the same scale have improved gradient descent in the training of a neural network resulting in a faster convergence (Al-Faiz et al. 2018).

**Drop-Out Regularisation**: The dropout regularization technique is used in the training of the neural networks to prevent overfitting. As shown in figure 2 dropout technique randomly removes a percentage of nodes from each hidden layer before each epoch in the training of the network. The removed nodes parameter values are not updated for the specific epochs in which they are removed. The random removal of the nodes prevents the model from over-fitting to the data and prevents the model relying mostly on specific nodes in the network for its predictions. The model trained with dropout regularization has better generalised predictive abilities (Gal et al. 2016)

![drop-out regularisation diagram](Images/dropoutRegularisation.jpg)



### Acknowledgements
* Powell JE, Henders AK, McRae AF, Caracella A, Smith S, Wright MJ, et al. (2012) The Brisbane Systems Genetics Study: Genetical Genomics Meets Complex Trait Genetics. PLoS ONE 7(4): e35430. https://doi.org/10.1371/journal.pone.0035430 (Data Source)

* Hannum, Gregory, Justin Guinney, Ling Zhao, Li Zhang, Guy Hughes, SriniVas Sadda, Brandy Klotzle, et al. 2013. “Genome-Wide Methylation Profiles Reveal Quantitative Views of Human Aging Rates.” Molecular Cell 49 (2): 359–67. https://doi.org/10.1016/j.molcel.2012.10.016. (Affect of gender on Methylation profiles)

* Al-Faiz, Mohammed Z, Ali Abdulhafidh Ibrahim, and Sarmad M Hadi. 2019. “The Effect of Z- Score Standardization (Normalization) on Binary Input Due the Speed of Learning in Back- Propagation Neural Network.” Iraqi Journal of Information and Communication Technology 1 (3): 42–48. https://doi.org/10.31987/ijict.1.3.41.
