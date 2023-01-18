# Case Study - FairKM

## Introduction
In this lesson, you will learn about __FairKM__ and how a group of researchers recognized the impact of bias on __human-centric applications__ (such as resource allocation) and devised a solution to mitigate that bias to reduce __disparate outcomes__ that arise from bias in machine learning.

## Learning Objectives
* Describe FairKM (AKA Fair-Lloyd) and discuss the reasons why the researches created it
* Desscribe how FairKM (AKA Fair-Lloyd) differs from Lloyd's heuristic

## Case Summary
The publication "Socially Fair k-Means Clustering" (Ghadiri et al., 2018) asserts that the __Lloyd's heuristic__, a popular k-means clustering algorithm can result to outcomes that are unfavorable to subgroups of the data, especially in __human-centric applications__. The authors claim that when clusters are __biased__ there can be a disparate impact for important decisions, like resource allocation.

As a response, the authors present an alternative algorithm known as __Fair-Lloyd__ as a modification of a Lloyd's heuristic that is designed to be fair and objective. It achieves this by choosing cluster centers that provide __equitable costs__ for different groups. On benchmark datasets, Fair-Lloyd exhibits unbiased performance (compared to Lloyd's heuristic) by ensuring that all groups have equal costs in the output k-clustering. Since the algorithm does not negatively impact the performance, it is easy to substitute wherever Lloyd's heuristic is used. 

What distinguishes FairKM from other attempts at developing fair clustering algorithms, is its emphasis on __socially fair__ features versus __proportionally fair__ features. The researchers assert that a socially fair clustering is one that minimizes the maximum average clustering cost over different demographic groups. 

## Dissecting FairKM
On page 8 of the paper, The authors discuss their experimental evaluation and how they tweaked Lloyd's heuristic to minimize bias. Let's review some of the most important steps in that process.

### FairKM Experimental Evaluation
1. First, the researchers __defined socially fair clusters__ as a clustering that has equal clustering costs across groups.
* When assigning points to a cluster, FairKM will not only consider similarities to the centroid but also which cluster it will skew the least in terms of sensitive attributes.
* This allows the clusters formed to try and form proportionally respect the demographic characteristics of the overall dataset. 


2. Next, the researchers __selected three datasets__ and __divided them into populations__:
* __Adult dataset__ 
    * consists of records of 48,842 individuals collected from census data, with 103 features
    * divided five subpopulations by race: “Amer-Indian-Eskim”, “AsianPac-Islander”, “Black”, “White”, and “Other”
* __Labeled faces in the wild (LFW) dataset__
    * consists of 13,232 images of celebrities
    * divided into two subpopulations: "Male" and "Female"
* __Credit dataset__
    * consists of records of 30000 individuals with 21 features
    * divded into two subpopulations: "High Education" and "Low Education"
    
    
3. Then, the researched __preprocessed the data__:
    * __continous features were normalized__ to have a mean of 0 and a variance of 1
    * __catagorical features were encoded__
    
    
4. For both Lloyd’s and Fair-Lloyd researchers tried 200 different center initialization, each with 200 iterations. The initial centers were __random__ in both cases.


5. The researchers managed dimensionality with Principal Component Analysis (PCA) reducing the dimension to k. 
    * Since PCA itself could induce __representational bias__ towards one of the (demographic) groups and Fair-PCA has been shown to be an unbiased alternative (see [Samadi et al. 2018](https://arxiv.org/abs/1811.00103)), it was used as a third option.
    * In total, three techniques were applied: without PCA, with PCA, and with Fair-PCA.
    

### Results of Experimental Evaluation
Upon applying Lloyd's heuristic and Fair-Lloyd to the three datasets, the following results were observed:

1. The standard Lloyd’s algorithm results in a significant gap between the clustering cost of individuals in different groups. For example:
* __Females__ - The average clustering cost of a female is up to 15% (11%) higher than a male in the Adult (LFW) dataset when using standard Lloyd’s.
* __Lower Educated__ - Lloyd’s leads up to 12% higher average cost for a lower-educated individual compared to a higher-educated individual.

2.  Fair-Lloyd algorithm effectively eliminates this bias by outputting a clustering with equal clustering costs for individuals in different subgroups. For example:
* The Credit and Adult datasets the average costs of two demographic groups are identical.

* For the LFW dataset, the authors observed a very small difference in the average clustering cost over the two groups in
the fair clustering (0.4%, 1% and 0.6% difference for without PCA, with PCA, and with Fair-PCA respectively). 

* Fair-Lloyd mitigates the bias of the output clustering __independent__ of which preprocessing technique was applied (without PCA, with PCA, and with Fair-PCA)

* Fair-Lloyd acheived these results with only a 4% increase to runtime and a 4.1% (no PCA), 2.2% (PCA) and 0.3% (FairPCA) increase in the standard k-means cost to Fair-Lloyd's solutions.


### Conclusions from Results
Based on the results, the researchers came to the following conclusions:

* __The cost of fairness__ - Fair-Lloyd is worth the small increase in runtime and k-means cost.  
* __Socially fair is more effective than proportionally fair__ - Previous methods for achieving fairness in k-means clustering considered the proportionality of the sensitive attributes in each cluster. The researchers found that improving the proportionality is actually at odds with improving the maximum average cost of the groups. Instead FairKM reduced bias by selecting features that are socially fair.


## Summary
Fairness has been a topic of serious concern in the machine learning community. In the past, efforts to make clustering algorithms more fair focused on proportion of sensitive features. On the other hand, FairKM devised a statistical method to determine which features created the most skew and to choose a centroid that takes this into consideration. The result is a low-cost and effective way to reduce machine learning bias in human-centric applications of clustering algorithms.
