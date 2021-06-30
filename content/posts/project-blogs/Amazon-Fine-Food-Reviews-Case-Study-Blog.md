---
title: "Amazon Fine Food Reviews Case Study Blog"
date: 2019-07-07T20:59:09+05:30
description: blog describing what the running case study is about
menu:
  sidebar:
    name: 01 Amazon Fine Food Review
    parent: pblogs
    identifier: amznfinefood
    weight: 10
---
**What is the dataset about?** The [dataset](https://www.kaggle.com/snap/amazon-fine-food-reviews) contains reviews of Amazon Food Products and their ratings over a perid of 10 years. There are ~500k product reviews. 

**What is the objective?** The objective is to classify a review either as "positive" or as "negative". 

**How to do it?** 
1. We only want to get the global sentiment of the recommendations (positive or negative). Purposefully ignore all Scores equal to 3. If the score is above 3, then the recommendation wil be set to "positive". Otherwise, it will be set to "negative".
2. For the predictive modelling purpose only Reviews, Ratings and Summary columns of the dataset are considered. We cannot directly use the text to perform classification. To leverage the power of linear algebra and modelling algorithms, we convert the text into their vector form. 

## Implementation Flow
1. ### Importing Data
The dataset is present in a csv file and also in sqlite format. We use the sqlite file to import the data into our work environment. As the dataset was present in the sqlite format, it becomes quite easy to pull only those records which do not have a neutral review. 
 

2. ### Exploratory Data Analysis:
Some basic EDA was performed such as distribution of target variable class and other analysis such as count of duplicates etc. A detailled EDA can be found [here](https://nycdatascience.com/blog/student-works/amazon-fine-foods-visualization/)

3. ### Data Cleaning and Pre-Processing Steps:
Performed following cleaning and preprocessing activities:

    * Deduplication: It is observed that the reviews data had many duplicate entries. Hence it was necessary to remove duplicates in order to get unbiased results for the analysis of the data. 
    * Remove Stopwords: Stopword removal has to be performed always before vectorizing the data because stopword removal is performed on the raw text data, but not the vectorized data. In case if you are willing to exclude the words 'not' and 'very', then it is recommended to go for bigrams, tri-grams rather than uni-grams because using these two words as features in uni-grams do not add much value.
    * Removed the html tags
    * Remove any punctuations or limited set of special characters like , or . or # etc.
    * Check if the word is made up of english letters and is not alpha-numeric
    * Check to see if the length of the word is greater than 2 (as it was researched that there is no adjective in 2-letters)
    * Convert the word to lowercase


4. ### Data Partioning:
The data was split was into train and test data in the ration 70:30 before vectorization to avoid the issue of data leakage.

5. ### Vectorization of text: 
    Types of vectorization techniques:

    1. Bag of Words
    2. Term Frequency- Inverse Document Frequency (Tf-Idf)
    3. Average word2vec
    4. Tf-Idf-weighted word2vec

Let me describe each one of these techniques in brief:

1. **Bag of Words:** BoW creates a vocabulary of all the unique words and their counts occuring in all the documents. The disadavantage of BoW is it doesnt take into account the semantic relationship of words that occur together. The obvious advantage is that it is very simple to calcaulate bag of words for a corpus.

2. **Term Frequency- Inverse Document Frequency**: These terms are borrowed from Information Retrival theory. **Term Frequency** is the ratio of times a word appears in a document to the total number of words in that document. **Inverse Document Frequency** of a word is the ratio of the total number of documents to the number of documents that the word appears in. Combining these two terms gives us the weightage of a rare term.
**Example:** Let us consider Wikipedia. The words such as 'the', 'for' etc appear a lot but words such 'civilization' appears once in 1000 documents. This means that the document is mostly about civilization. So tfidf score of this word would be much higher than the words such as 'the', 'for'.

3. **Average word2vec:** Word2Vec is a recent study where words are converted into their vector form. Word2vec learns the relationship without being explicitly programmed. Similar words are near each other while dissimilar words are apart in  the d-dimensional space. There are lot of pre-trained vectors made on wikipedia articles, google news. Word2Vec can help us determine relationship between words such Paris is the captial of France and so is Barcelona the capital of Spain. In this technique, semantic relationship of words is considered. In this case study, I have trained word2vec on the reviews feature from scratch. Take the words in the review and convert it to its vector form and divide by the number of words in the review. **[w2v(w1) + w2v(w2) + ,,, + w2v(wn) ]/n**

4. **Tf-Idf-weighted word2vec:** Suppose a review has these words w1,w2,w3,w4,w5. We calculate tfidf score for each word. We then convert the word in its vector form using w2v. The vector form is then multiplied by the tfidf score for all the terms and is then divided by sum of tfidf scores

5. **Modelling:**

The Supervised Learning Algorithms used are:

* kNN
* Navie Bayes
* Logistic Regression
* Decision Trees
* Random Forest, Gradient Boosting Decsion Trees, lightGBM
* Support Vector Machine


The Unsupervised Algorithms used are:

* k-Means
* Hierarchical Clustering
* DBSCAN
* Truncated SVD
* Dimensionality reduction technique: t-sne

I will describe in brief what I did for each algorithm apart from modelling:

* **KNN** 
1. Standardized data before performing classification on text reviews from the Amazon Fine Food Reviews dataset.  
2. Implemented k-fold cross-validation technique by writing simple for loops to find the best value of k i.e. number of neighbors  
3. The classification results were summarized using heatmap confusion matrix. 

* **Naive Bayes** 
1. Implemented Naive Bayes classifier on text reviews using the vector form: Bag of words, TFIDF, Word2Vec and TFIDF- weighted word2vec 
2. Implemented k-fold cross-validation technique by writing simple for loops. 
3. Listed out the important features of the model for the positive and the negative class

* **Logistic Regression** 
1. Performed Logistic Regression with L1 and L2 regularization on BoW, TFIDF, Avg-W2V, TFIDF-WW2V. 
2. Listed out important features from the model for the positive and the negative class. 
3. Used hyperparameter technique to determine the value of C (C = 1/λ), variable that retains strength modification of regularization

* **Decision Trees** 
1. Apart from modelling, printed out top 20 important features for BoW and TFIDF vectorizer. 
2. Performed feature engineering by combining summary column text with review text. 
3. Heatmap of 2 hyperparameters; min_sample_split and max_depth. 
4. Visualizing a decision tree of certain depth.

* **Random Forest and Gradient Boosting** 
1. For the GBDT part, experimented with sklearn's RandomForestClassifier and Microsoft's lightgbm. 
2. Made use of GridSearchCV to find the best value of max_depth and n_esitmators. 
3. Visualized the top 20 important features using the wordcloud for BoW and TF-IDF. 
4. For the TFIDF- weighted W2V vectorizer, calculated max_depth, n_estimators using GridSearchCV for the lightgbm. 

* **Support Vector Machines** 
1. Performed Support Vector Machine Classification on the Amazon fine food dataset using SGDClasssifier for the linear kernel and SVC for RBF kernel.
2. Made use of GridSearchCV to find the best value of C , the hyperparameter. 
3. Also used CalibratedSearchCV when working with SGD classifier. 
4. Printed out feature importance for BoW vectorizer

* **PCA and tSNE** 
1. Dimensionality reduction technique like PCA on text reviews with polarity based color coding from the Amazon Fine Food Reviews dataset. 
2. The text reviews were preprocessed and vectorized in Bag of Words, TFIDF, Word2Vec and TFIDF- weighted word2vec. 
3. PCA and tsne were applied on the vectorizers for visualization of high dimensional data. 

* **k-means, hierarchical clustering, dbscan**
1. Determined the best value of k for k-means, eps for dbscan using the elbow method. 
2. Plotted loss on y axis and k values on x. 
3. Created word cloud of reviews for each number of k. 
4. For agglomerative clustering, took arbitrary number and plotted word clouds along with their description. 

* **Truncated SVD** 
1. Took top 3000 features from tf-idf vectorizers using idf_ score. 
2. Calculated the co-occurrence matrix with the selected features. 
3. Selected n_components in truncated svd, with maximum explained variance. 
4. Applied K-Means clustering and choose the best number of clusters based on elbow method. 
5. Print out wordclouds for each cluster. 
6. Wrote a function that takes a word and returns the most similar words using cosine similarity between the vectors. 


**Hyperparameter tuning:** Almost all algorithms require hyperparameters. These implementations determined the hyperparameters using for loop, Grid Search and Random Search. 

**k-fold Cross-Validation:** The models were trained using cross validation to avoid overfitting. The k values considered for cv were 3, 5, 10.

**Performance Metrics:** The target class was imbalanced so the performance metrics used were the confusion matrix and AUC- ROC curve.


**Libraries used**
* Everything was done in python
* For majority of modelling part, sklearn's standard algorithms were used.
* Used gensim to train word2vev on the Reviews data.
* For plotting charts, used matplotlib.



After each plot, written down the observation below it and at the end summarized the results using table.
