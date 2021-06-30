---
title: "Personalized Cancer Diagnosis"
date: 2019-08-31T22:59:09+05:30
description: blog describing what the running case study is about
menu:
  sidebar:
    name: 04 Personalized Cancer Diagnosis
    parent: pblogs
    identifier: perscandiag
    weight: 10
---
**Problem Statement:** Classify the given genetic variations/mutations based on evidence from text-based clinical literature.

**Details:** 
* https://www.kaggle.com/c/msk-redefining-cancer-treatment/
* https://www.kaggle.com/c/msk-redefining-cancer-treatment/discussion/35336#198462


**Real-world/Business objectives and constraints**
* No low-latency requirement.
* Interpretability is important.
* Errors can be very costly.
* Probability of a data-point belonging to each class is needed.


**Data**

1. Data Overview
[Source](https://www.kaggle.com/c/msk-redefining-cancer-treatment/data)
    
    We have two data files: one conatins the information about the genetic mutations and the other contains the clinical evidence
(text) that human experts/pathologists use to classify the genetic mutations. Both these data files are have a common column called ID
    Data file's information: 
        * training_variants (ID , Gene, Variations, Class)
        * training_text (ID, Text)
        

2. Example Data Point

*training_variants*

**ID,Gene,Variation,Class**

0,FAM58A,Truncating Mutations,1

1,CBL,W802*,2

2,CBL,Q249E,2

...


*training_text*

**ID,Text**

0||Cyclin-dependent kinases (CDKs) regulate a variety of fundamental cellular processes. CDK10 stands out as one of the last
orphan CDKs for which no activating cyclin has been identified and no kinase activity revealed. Previous work has shown that CDK10
silencing increases ETS2 (v-ets erythroblastosis virus E26 oncogene homolog 2)-driven activation of the MAPK pathway, which
confers tamoxifen resistance to breast cancer cells. The precise mechanisms by which CDK10 modulates ETS2 activity, and more
generally the functions of CDK10, remain elusive. Here we demonstrate that CDK10 is a cyclin-dependent kinase by identifying cyclin
M as an activating cyclin. Cyclin M, an orphan cyclin, is the product of FAM58A, whose mutations cause STAR syndrome, a human
developmental anomaly whose features include toe syndactyly, telecanthus, and anogenital and renal malformations. We show that
STAR syndrome-associated cyclin M mutants are unable to interact with CDK10. Cyclin M silencing phenocopies CDK10 silencing in
increasing c-Raf and in conferring tamoxifen resistance to breast cancer cells. CDK10/cyclin M phosphorylates ETS2 in vitro, and in
cells it positively controls ETS2 degradation by the proteasome. ETS2 protein levels are increased in cells derived from a STAR...


**Mapping the real-world problem to an ML problem**
1. Type of Machine Learning Problem: There are nine different classes a genetic mutation can be classified into => Multi class classification problem

2. Performance Metric: [Source](https://www.kaggle.com/c/msk-redefining-cancer-treatment#evaluation)

* Multi class log-loss
* Confusion matrix


**Machine Learing Objectives and Constraints**

* Objective: Predict the probability of each data-point belonging to each of the nine classes.

* Constraints:
    * Interpretability
    * Class probabilities are needed.
    * Penalize the errors in class probabilites => Metric is Log-loss.
    * No Latency constraints.


**Preprocessing of text data:**
1. Removed stop words
2. Converted all text to lowercase
3. Removed all symbolic and numeric text
4. Merging both gene_variations and text data based on ID

**Train, CV and Test Datasets:** Splited the data into train, test and cross validation data sets, preserving the ratio of class distribution in the original data set. The splitting was done with 64%,16%, 20% of data respectively.





Temporal Nature: which changes with time







If its log loss then use random model because ll can take values from 0 to inifity
fr auc, we need the most simple model will have a auc score of 0.5 

Models that work well in:

High dimensional data: nb, log reg, linr svm, rf
Low dim data: rf, knn, dt


https://kevinzakka.github.io/2016/07/13/k-nearest-neighbor/
https://www.quora.com/profile/Chomba-Bupe
https://mathbitsnotebook.com/Algebra1/StatisticsData/STSD.html
http://scott.fortmann-roe.com/docs/BiasVariance.html
Questions: http://www.faqs.org/faqs/ai-faq/neural-nets/part2/


The KNN classifier is a non parametric and instance-based learning algorithm.
k-NN performs much better if all of the data have the same scale
k-NN works well with a small number of input variables (p), but struggles when the number of inputs is very large
k-NN makes no assumptions about the functional form of the problem being solved

Non-parametric means it makes no explicit assumptions about the functional form of h, avoiding the dangers of mismodeling the underlying distribution of the data. For example, suppose our data is highly non-Gaussian but the learning model we choose assumes a Gaussian form. In that case, our algorithm would make extremely poor predictions.
Instance-based learning means that our algorithm doesn’t explicitly learn a model. Instead, it chooses to memorize the training instances which are subsequently used as “knowledge” for the prediction phase. Concretely, this means that only when a query to our database is made (i.e. when we ask it to predict a label given an input), will the algorithm use the training instances to spit out an answer.


In instance-based learning there are normally no parameters to tune, the system is normally hard coded with priors in form of fixed weights or some algorithms like tree search based algorithms. Such a system normally does what is known as lazy learning by absorbing the training data instances and using those data instances for inference.
For example, in scale invariant feature transform (SIFT), the recognition pipeline is hard coded. All it does during learning is to index descriptors into an indexing data structure such as a kd-tree together with some geometric information. That information is then directly used during inference to recognize instances of objects.
Thus instance-based learning is just about storing the training data instances. Though the training data itself can be preprocessed in many ways and stored in memory.

In model-based learning, we are talking more about reinforcement learning (RL). In model-based RL an agent builds a model of the environment in which it exists so that it can use that model to infer which actions to take in that environment that lead to desired outcomes in order to achieve a given goal.
Model-based learning can also be seen as the opposite of instance-based learning. In model-based learning there are parameters to tune. These parameters with optimal settings are supposed to model the problem as accurately as possible thus learning is not simply about memorization but rather more about searching for those optimal parameters.




here are many other distance measures that can be used, such as Euclidean, manhattan and cosine distance. However, there is no 'best' distance, because the distance fundamentally depends on the form of data representation you choose, especially considering various structural representations.You can choose the best distance metric based on the properties of your data. If you are unsure, you can experiment with different distance metrics and different values of K together and see which mix results in the most accurate models.

Euclidean is a good distance measure to use if the input variables are similar in type (e.g. all measured widths and heights). Manhattan distance is a good measure to use if the input variables are not similar in type (such as age, gender, height, etc.). Due to the curse of dimensionality, we know that euclidean distance becomes a poor choice as the number of dimensions increases so use Cosine distance.




Training error is the error that you get when you run the trained model back on the training data. Remember that this data has already been used to train the model and this necessarily doesn't mean that the model once trained will accurately perform when applied back on the training data itself.

Test error is the error when you get when you run the trained model on a set of data that it has previously never been exposed to. This data is often used to measure the accuracy of the model before it is shipped to production.

Laplace smoothing is used to avoid overfitting

The log loss of random model is 2.5. The precision matrix is calculated by dividing a cell value with the sum of the columns. The Recall matrix is calculated by dividing a cell value with the sum of the cells in the row. We want the diagonal elements in the confusion matrix to be higher values because the represent the count of actual values and predicted values. The problem has been dealt with by class balance and without class balance. To feature the categorical variables, we used one hot encoding(logistic reg, linear svm) and response coding techniques. ONly use train data from response coding and never use cv or test data. From the LR model, we get output y but as our loss function is logloss we need to have a model that will give us probabilites. If the feature overlaps in tr, cv and test data, then the feature is stable; better ml models. if there is no overlap, models will overfit. When the number of features is less, models such as knn, dt, rf work pretty well because the curse of dimensionality hasnt kicked in yet. For higher dimension data: linear SVM, logreg, naive bayes. 

From the previous modelling that we have performed, we can safely say that Gene and Variation are important features. So we will directly focus on creating models using TFIDF vectorization and Logistic Regression modelling using CountVectorizer. 



Gene and Variation are both categorical feature. Text is simply a text feature; a sequence of words. There are two ways to convert the gene and variation categoraical features; onehot encoding and the other is response encoding. 

Onehot encoding: There are 253 unique values of gene in the training data. We create a vector of length 253. For occurance of any of the 253 gene, we put 1 under its name and keep all other values to 0. 

Responce Coding: We take the feature and the response variable. Lets say for a gene gi the response values count for 100. Out of which, 70 values are for class 2 and 30 are for class 3. We create a vector of length of values in response variable. In this case it is 0 to 9. We write the class probabilites for gi as 70/100 for class 2 and 30/100 for class 3 and put 0 as values for the remaining classes. Converting a gene gi into a vector of number of classees using probabilites (y=2|gene = gi) and (y=3 | gene=g1). There is a problem with this approach. These probabilites can be extremely small or can be zero. This is where we use Laplace smoothing. 	laplace smoothing = (Numerator + alpha) / (Denominator + (k * alpha)) where alpha is hyper parameter and k is number of distinct class lables.

For text data, we have used navie bayes approach to do response coding of text.

OHE: logreg, linear SVM
RC: NB, rf 

To determine whethere the feature is significant or not, we create a model using the feature and the reponse. If it is less than log loss of random model, we can conclude that the model is useful. This is performed in the univariate analysis of the gene feature. 

Overlapping of features is better. The more the overlap, the stable your model will be

1. Convert the text feature into TfIDFVectorizer instead of CountVectorizer


Always use CaliberatedCV when log loss is your metric and you need probabilites. Some probabilites of the models may not be calliberated.  https://machinelearningmastery.com/calibrated-classification-model-in-scikit-learn/
https://hackernoon.com/choosing-the-right-machine-learning-algorithm-68126944ce1f


Laplace smoothing is used to avoid overfitting in Naive Bayes

Performed checks in determining whether Gene and Variation are useful or not by modelling them againt the response variable using Logistic + Caliberated Classifier and hypertuning
Also performed checks on the stability of the Gene, Variation and Text features. Gene and Text are stable features while variation is not, we still keep the variation feature as it helps us in dropping the log loss error. 
We performed feature engineering on the Text feature by considering n-grams that helped us reduce the log loss


you can think like adding a new set of features, removing a subset of features, creating a new set of features from original features etc all these can be considered as feature engineerings.


**Source/Useful Links:** Some articles and reference blogs about the problem statement
* https://www.forbes.com/sites/matthewherper/2017/06/03/a-new-cancer-drug-helped-almost-everyone-who-took-it-almost-hereswhat-it-teaches-us/#2a44ee2f6b25
* https://www.youtube.com/watch?v=UwbuW7oK8rk
* https://www.youtube.com/watch?v=qxXRKVompI8 