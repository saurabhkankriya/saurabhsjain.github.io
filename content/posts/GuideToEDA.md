---
title: "Begineer's Guide to EDA"
date: 2019-06-02T10:55:43+05:30
description: guide on how for getting started with EDA for complete beginners
menu:
  sidebar:
    name: Begineer's Guide to EDA
    identifier: guideforEDA
    weight: 10
---
**Exploratory Data Analysis (EDA)** is about unraveling the story the data has in it.

Each EDA is unique in its own way. Although there is no fix set of rules that you must follow for doing EDA, there are some steps that are common in almost any analysis.  
If you are a beginner and are just starting out with EDA and probably have no idea what to do, this basic guide will give you some starting pointers. Not complete in its entirety, but you have something presentable to your manager. 

* Set the objective of the EDA. State what is the dataset about. Enlist column info. 
* Jot down questions that you are looking to answer. Remember, questions should come first and then the answers to them. It's never the other way round.

* Import libraries and the dataset.

* Simple Analysis:
   1. View the first few rows of the dataset. Also recommeded is to select 20 records at random. This will show if there any abnormality in the data.
   2. View the datatype of the columns. How many rows are integer, object, boolean. Identify what columns are categorical, numerical, nominal.
   3. View the shape of the dataset. This will tell you the number of rows and columns in the dataset.
   4. Look at the descriptive statistics (mean, median, standard deviation, percentiles etc) of the dataset. This can reveal a lotof things statistically. 
   5. Find out which columns are predictor and which one is response.
   6. Take a random look at the response variable (if it is present). Response variable is the target column. 
   
      
* Find outliers if any. Do your analysis with and without them. See what is the difference in results you observe. 

* See if there is any missing data. If yes, impute it using domain knowledge. There are libraries available but contact someone who has knowledge about the data. If not, look up Google.

* There is no limit to the number of things you can do in the following sections:

    A. **Univariate Analysis:**
	
       a. Take a look at the class distribution of the response variable. You can also do this for other categorical features. Visualize/ write your observations.
	   
       b. Draw subplots to take a look at data spread of predictor variables.
	   
    B. **Bivariate Analysis:**
	
       a. Plot charts between one predictor and the response variable.
	   
    C. **Multivariate Analysis:**
	
       a. Plot charts that help you look at the affect of 2 or more predictor variables on the response variable.

Some common plots/ charts that you can use in your EDA. The list is much broader:

   1. Bar Charts
   2. Scatter plot
   3. Histogram
   4. Co-relations/ Chi-square Test
   5. Box plot/ Violin plot
   6. PDF
   7. CDF 
   8. Area plot
   9. Facet charts
   10. Pair-plot etc

* Leave note/observations after you encounter that something might be useful. Conclusions should be added after each sections. 

* Look for anything that deviates from the normal. You have pandas/ R/ SQL at your disposal; bend them in ways you want. 


* For next level EDA consider doing:

	* Hypothesis testing
	* Application of Centrl Limit Theorem
	* Confidence Intevals etc 

Notes: 

1. We do have tendency to add our personal opinion which may not be observed in the dataset. Please do the analysis from neutral point of view. Do not get anxious if the results what you expect are not observed after doing EDA. Check your EDA again and validate your data from the source. 
2. Do not ever delete the data. Save it in some form. 
3. The purpose of EDA is to get a good understanding of the dataset and answer the questions that are posed. Sometimes, you may have questions that you have no answers to and can seek the answers from others. For example, you may want to know what is the reason for missing data? So at the end of your analysis, you can ask questions you have.

Always remember its not the number of lines of code in your analysis but the quality of your analysis that will be useful.

~~Double~~ Triple check your analysis before explaining your understanding to anyone else.

Useful Resources:

Python https://www.machinelearningplus.com/plots/top-50-matplotlib-visualizations-the-master-plots-python/

R https://www.computerworld.com/article/2935394/my-ggplot2-cheat-sheet-search-by-task.html 

All the best!