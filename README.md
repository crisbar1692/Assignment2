# **README-Assignment 2**

**Scientific Programming-Systems Biology** 
 
This Assignment is inspired in the previous work of Wiener et al. [1](https://pubs.acs.org/doi/abs/10.1021/ja01193a005) Where the boiling point of different alkanes was predicted based in other properties. In similar way, I developed a regression model to predict boiling points of multiple alkanes based on several chemical descriptors. This assignment is made in R studio with an interactive notebook using R Markdown integrating plots, code and comments. The author of this assignment is Cristian Alberto Barrios Espinosa, i6185546, is a second-year student of the master in Systems Biology at Maastricht University. 

## **Content of Repository**
- Assignment2.rmd (R Markdown file)
- Authors
- Readme.md
- License
 
## **Used Packages**

 Several packages were used to build this model [wikidataQueryServiceR 2](https://cran.r-project.org/web/packages/WikidataQueryServiceR/index.html) to make queries of the alkanes and their boiling points from wikidata. As well, [rcdk 3](https://cran.r-project.org/web/packages/rcdk/index.html) was used to extract descriptors for each one of the alkanes to be used as dependent variables. Finally, [pls 4](https://cran.r-project.org/web/packages/pls/vignettes/pls-manual.pdf) will provide the functions required to produce the regression model. [tictoc 4](https://cran.r-project.org/web/packages/tictoc/index.html) is a package to count the time of running code in different sessions that demmand longer time. [EnvStats 5](https://cran.r-project.org/web/packages/EnvStats/index.html) has the rosner test useful to find outliers.
 
  To run the code make sure that you run these lines in the console:
 
 ```R
if (!requireNamespace("WikidataQueryServiceR", quietly = TRUE))
    install.packages("WikidataQueryServiceR")
if (!requireNamespace("rJava", quietly = TRUE))
    install.packages("rJava")
if (!requireNamespace("rcdk", quietly = TRUE))
    install.packages("rcdk")
if (!requireNamespace("pls", quietly = TRUE))
    install.packages("pls")
if (!requireNamespace("gplots", quietly = TRUE))
    install.packages("gplots")
if (!requireNamespace("tictoc", quietly = TRUE))
    install.packages("tictoc")
if (!requireNamespace("EnvStats", quietly = TRUE))
    install.packages("EnvStats")

library("EnvStats")
library("tictoc")
library("WikidataQueryServiceR")
library("rJava")
library("rcdk")
library("pls")

```
## **Query of Alkanes** 
Alkanes were extracted from wikidata together with their boiling points and the latest group was transformed to Kelvin to unify the data and to be able to have a valid regression model. 

## **Partial least squares regression model** 
The kind of model used is Partial least squares regression. It find a linear regression model by projecting the independent and dependend variables into an additional space. It is particulary suitable for cases like this where the amount of observable variables is big.

>**The underlyng equations of the model are these::**
$X=TP^{T}+E$
$Y=UQ^{T}+F$

## **Function to Calculate Root Mean Squared Error of Prediction (RMSEP)** 
The RMSEP will be calculated several times based on different descriptors, training sets, and test sets. Consequently to make the code more friendly and clear this function was made. The output is always RMSEP for three components while the inputs are the boiling points, parsed smiles of the alkanes, the "descriptors indexes" a subset of all the numbers between 1 and 50 (the total number of descriptors), "training and test indexes" a subset of all the number between 1 and 142 (the total amount of alkanes analyzed). 
## **Algorithm to select the best descriptors** 

The algorithm designed for this task makes 50 iterations wherein each one of them takes a random sample of descriptors and calculates the average of the corresponding RMSEP for 4 test sets randomly selected in each iteration. After making these calculations 50 times, the algorithm will give the smallest RMSEP together with the indexes of descriptors required to reproduce the results. 
 >***Warning:* This algorithm may require several hours to run the 50 iterations. Nonetheless all the results from this process are already in the next parts of the code. Consequently, it is not necessary to run this algorithm in order to get results from the last part of the code.** 
 
 

## **Visualization**
After getting the descriptors for the best performance with three components, the scatter plot with predicted and measured boiling points will be shown. 

## Other models
In other models as mentioned in Lie et all [7](https://pubs.acs.org/doi/abs/10.1021/ci970109z), better predictions have been achieved using similar methods of linear regression, with RMS of less than 5K. Consequently, the current model can still be improved using better algorithms to find the correct descriptors.

## **License** 
The license used is [MIT](https://choosealicense.com/licenses/mit/) it has permissions for commercial use, distribution, modification, and private use. There are limitations regarding the liability and warranty. 


## **Index of variables and functions**

| Variable                | Description                                                                          |
|-------------------------|--------------------------------------------------------------------------------------|
| results_query           | Results of the query with alkanes                                                    |
| list_alkanes            | Modification of results_query to obtain important data                               |
| celsius                 | Vector with all the boiling points in Celsius                                        |
| fahrenh                  | Vector with all the boiling points in Fahrentheit                                    |
| parsed_smiles_alkanes   | Parsed smiles of alkanes to extract rcdk                                             |
| ideal_RMSEP             | Numerical value to run in the algorithm storing the best RMSEP                       |
| ideal_vector            | Vector to store the descriptors indexes that produce the best RMSEP                  |
| descriptor_vector       | Vector with the indexes of descriptors used in each iteration                        |
| RMSEP_3comp_function    | Funtion to calculate the RMSEP for each test set and subset of descriptor            |
| descriptor_names        | Descriptor names that correspond to the descriptor indexes                            |
| regression_data_alkanes | Data frame to store independent and dependent variables used in the regression model |
| train_vector            | Vector with integers that are indexes of the training data set                       |
| train_set               | Data corresponding to the rows given by train_vector                                 |
| test_vector             | Vector with integers that are indexes of the test set                                |
| test_set                | Data corresponding to the rows given by the test vector                              |
| model_alkanes           | Regression model build with pls package and the available data                       |
| RMSEP_test_3comp        | RMSEP fot the test set with 3 components                                             |
| size_vector             | Amount of descriptors used in each iteration of the algorithm                        |
| counter1/counter2       | Counters to run each on of the while loops                                           |      
| predicted_values        | Boiling points predicted using the obstained model                                   |
| measured_values         | Measured Boiling points obtained from wikidata                                       |
| error_vector            | distance between the points of the predicted vs measured plot and the diagonal       |
| outliers                | outliers calculated over the error_vector using Rosner Test                          |


## **References**



1.	Wiener, H., Structural determination of paraffin boiling points. Journal of the American Chemical Society, 1947. 69(1): p. 17-20.
2.	Hernández, D., et al. Querying wikidata: Comparing sparql, relational and graph databases. in International Semantic Web Conference. 2016. Springer.
3.	Guha, R. and M.R. Cherto, rcdk: Integrating the CDK with R. 2017.
4.	Wehrens, R. and B.-H. Mevik, The pls package: principal component and partial least squares regression in R. 2007.
5.	Izrailev, S., tictoc: Functions for Timing R Scripts. R package version 1.0. URL: https://CRAN. R-project. org/package= tictoc, 2014.
6.	Millard, S.P., M.A. Kowarik, and M. Imports, Package ‘EnvStats’. Package for environmental statistics. version, 2018. 2(1).
7.	Liu, S., C. Cao, and Z. Li, Approach to estimation and prediction for normal boiling point (NBP) of alkanes based on a novel molecular distance-edge (MDE) vector, λ. Journal of chemical information and computer sciences, 1998. 38(3): p. 387-394.


