---
title: "Quora Question Pairs Case Study Blog"
date: 2019-07-22T22:11:59+05:30
description: brief description of steps done in the case study
menu:
  sidebar:
    name: 02 Quora Question Pairs Case Study Blog
    parent: pblogs
    identifier: quoraquestionpair
    weight: 10
---

## Objective:
Identify which questions asked on Quora are duplicates of questions that have already been asked. 

## Solution at Quora:
At the time (around 2017) this competition was launched, Quora was using Random Forest to classify duplicates. From the latest blog on engineering.quora.com, it can be seen that Quora is now using Deep Learning techniques to identify duplicate questions. 

## Business Value:
Multiple questions with the same intent can cause seekers to spend more time finding the best answer to their question, and make writers feel they need to answer multiple versions of the same question. Quora values canonical questions because they provide
a better experience to active seekers and writers, and offer more value to both of these groups in the long term. This could be useful to instantly provide answers to questions that have already been answered.

## Real world Constraints:

1. The cost of a mis-classification can be very high.
2. You would want a probability of a pair of questions to be duplicates so that you can choose any threshold of choice.
3. No strict latency concerns.
4. Interpretability is partially important.

## Data Overview:
Data is in a file Train.csv

* Train.csv contains 5 columns : qid1, qid2, question1, question2, is_duplicate
* Size of Train.csv - 60MB
* Number of rows in Train.csv = 404,290

**Example Data point:**

"id","qid1","qid2","question1","question2","is_duplicate"

"0","1","2","What is the step by step guide to invest in share market in india?","What is the step by step guide to invest in share market?","0"

"1","3","4","What is the story of Kohinoor (Koh-i-Noor) Diamond?","What would happen if the Indian government stole the Kohinoor (Koh-i-Noor) diamond back?","0"

"7","15","16","How can I be a good geologist?","What should I do to be a great geologist?","1"

"11","23","24","How do I read and find my YouTube comments?","How can I see all my Youtube comments?","1"


## Mapping real wold problem to a ML problem: 
It is a binary classification problem, for a given pair of questions we need to predict if they are duplicate or not.

**Performance metric:**

* Log-loss
* Confusion Matrix


## EDA performed
* Number of duplicate(smilar) and non-duplicate(non similar) questions
* Stats about questions such as:
   * total number of question pairs for training 
   * question pairs are not Similar (is_duplicate = 0) or similar (is_duplicate = 1)
   * number of unique questions
   * checking for duplicates etc
   

## Basic Feature Extraction
Constructed a few features like: 

* freq_qid1 = Frequency of qid1's
* freq_qid2 = Frequency of qid2's
* q1len = Length of q1
* q2len = Length of q2
* q1_n_words = Number of words in Question 1
* q2_n_words = Number of words in Question 2
* word_Common = (Number of common unique words in Question 1 and Question 2)
* word_Total =(Total num of words in Question 1 + Total num of words in Question 2)
* word_share = (word_common)/(word_Total)
* freq_q1+freq_q2 = sum total of frequency of qid1 and qid2
* freq_q1-freq_q2 = absolute difference of frequency of qid1 and qid2

## Preprocessing of text:

* Removing html tags
* Removing Punctuations
* Performing stemming
* Removing Stopwords
* Expanding contractions etc

## Advanced Feature Extraction (NLP and Fuzzy Features)

* cwc_min : Ratio of common_word_count to min lenghth of word count of Q1 and Q2
* cwc_min = common_word_count / (min(len(q1_words), len(q2_words))
* cwc_max : Ratio of common_word_count to max lenghth of word count of Q1 and Q2
* cwc_max = common_word_count / (max(len(q1_words), len(q2_words))
* csc_min : Ratio of common_stop_count to min lenghth of stop count of Q1 and Q2
* csc_min = common_stop_count / (min(len(q1_stops), len(q2_stops))
* csc_max : Ratio of common_stop_count to max lenghth of stop count of Q1 and Q2
* csc_max = common_stop_count / (max(len(q1_stops), len(q2_stops))
* ctc_min : Ratio of common_token_count to min lenghth of token count of Q1 and Q2
* ctc_min = common_token_count / (min(len(q1_tokens), len(q2_tokens))
* ctc_max : Ratio of common_token_count to max lenghth of token count of Q1 and Q2
* ctc_max = common_token_count / (max(len(q1_tokens), len(q2_tokens))
* last_word_eq : Check if Last word of both questions is equal or not
* last_word_eq = int(q1_tokens[-1] == q2_tokens[-1])
* first_word_eq : Check if First word of both questions is equal or not
* first_word_eq = int(q1_tokens[0] == q2_tokens[0])
* abs_len_diff : Abs. length difference
* abs_len_diff = abs(len(q1_tokens) - len(q2_tokens))
* mean_len : Average Token Length of both Questions
* mean_len = (len(q1_tokens) + len(q2_tokens))/2
* fuzz_ratio : https://github.com/seatgeek/fuzzywuzzy#usage
http://chairnerd.seatgeek.com/fuzzywuzzy-fuzzy-string-matching-in-python/

* fuzz_partial_ratio : https://github.com/seatgeek/fuzzywuzzy#usage
http://chairnerd.seatgeek.com/fuzzywuzzy-fuzzy-string-matching-in-python/

* token_sort_ratio : https://github.com/seatgeek/fuzzywuzzy#usage
http://chairnerd.seatgeek.com/fuzzywuzzy-fuzzy-string-matching-in-python/

* token_set_ratio : https://github.com/seatgeek/fuzzywuzzy#usage
http://chairnerd.seatgeek.com/fuzzywuzzy-fuzzy-string-matching-in-python/

* longest_substr_ratio : Ratio of length longest common substring to min lenghth of token count of Q1 and Q2
* longest_substr_ratio = len(longest common substring) / (min(len(q1_tokens),len(q2_tokens))

   
Created Word Cloud of Duplicates and Non-Duplicates Question pairs to see the words which appear in both classes.

## Modelling
1. Data leakage was tackled using partioning data into training and test set in the ratio 70:30 before vectorization. 
2. Vectorization of questions q1 and q2 was performed using tfidf-idf weighted word2vec and tfidf. 
3. Created a simple benchmark model that gave predictions at random. 
4. Used Logistic Regression and Linear SVM with hyperparameter tuning. 
5. Also implemented Xgboost model before and after hyperparameter tuning. 




