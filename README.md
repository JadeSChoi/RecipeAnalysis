# Analyzing the Relationship Between Time it Takes to Make a Recipe and its Rating

Author: Jade Choi

## Introduction

In this project I used two datasets related to food recipes. Using the data in the dataset, I deicded to explore the impact that the time it takes to make the recipe had on the ratings of it. I wanted to see if investing your time into a longer recipe was worth the investment, by looking at the ratings. Additionally I tried to predict the ratings and to see which factors were most impactful in creating high quality food so that users could maximize their returns for the lease amount of resources. The first data set was titled recipes which contained information about 83782 unique recipes (83782 rows) with 10 description (10 columns). From this data set the most important columns was 'minutes', which shows how long each recipe takes, 'nutrition' which shows the nutritional aspect of the recipes, 'n_steps' which shows the number of steps each recipe has, 'n_ingredients' which shows the number of ingredients within in each recipe. The next data set was 'interactions' which had the 'ratings' which showed the ratings for each recipe, and 'review' which included an personal review of each recipe.

## Data Cleaning and Exploratory Data Analysis

Conducted the following data cleaning steps to improve data analysis.

1. merged the recipe and interactions data set through left merge to create one data set 
1. changed all the ratings with 0 into NA becasue many of the 0 simply just meant they didn't rate it
1. include a column called average_rating into df_merge with the average rating of each recipe
1. added a column 'complexity' which shows shich recipes are shorter and longer based on the number of ingredients, boolean column
1. added a column 'calories' which gets the calories from 'nutrition' and makes it into a column
1. adds a column 'quick_Recipe' which shows shich recipes are shorter and longer based on the 'minutes' any thing less than the medium (35 minutes) is considered short
1. noticed that there were a lot of outliers for 'minutes' so added a 'outlier' column the identified if each one was a outlier or not through boolean expression.

#### Result 

### Univariate Analysis 

As shown in the histogram below, the minutes data is heavily skewed to the right as there are some values that are over one million minutes. Deeming this as signifcant outliers, using the 'outlier' column got rid of the upper outliers within the data set to create a more usefull histogram. The histogram is still heavily skewed

<iframe
  src="assets/C:\Users\<YourUsername>\Desktop\dsc80-2024-fa\RecipeAnalysis\assets\plot1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
## Assessment of Missingness 

The missing data within the merged dataset are 'review', 'rating' and 'date'.

### NMAR Analysis

The NMAR data is 'review' because the reviews are based on the people's willingness itself. If people liked the dish they are more likely inclined to write a review for that recipe. Thus the missingness comes from the nature of the 'review' data itself thus making it NMAR.

### Missingness Dependecy

We now want to explore the 'rating' data to see if its missingness is related to any other columns. 

The first column I decicded to explore was the minutes data and how it impacted the missingness of the 'rating' data 

Null Hypothesis: The missingness of ratings does not depend on the time it takes to make the recipe.

Alternate Hypothesis: The missingness of ratings does depend on the time it takes to make the recipe.

Test Statistic: The absolute difference of mean in minutes of the distribution of the group without missing ratings and the distribution of the group without missing ratings.

Significance Level: 0.05

Given the observed difference of 51.45 when simulating the abs mean differences 500 times, I ended with a p-value of 0.12 which is greater than the significance level of 0.05 meaning we failed to reject the null hypothesis and that minutes didn't really have an impact on the missingness of the 'rating'.

The next column I decided to explore was 'n_step' to see if the number of steps had an impact on the missingness of the 'rating'

Null Hypothesis: The missingness of ratings does not depend on the number of steps it takes to make the recipe.

Alternate Hypothesis: The missingness of ratings does depend on the number of steps it takes to make the recipe.

Test Statistic: The absolute difference of mean in number of steps  of the distribution of the group without missing ratings and the distribution of the group without missing ratings.

Significance Level: 0.05

With the observed absoulute difference of 1.3386 and when simulating the abs mean differences 500 times, I end with a p value of 0.00 meaning we reject the null hypothesis and that the number of steps has a an impact on the missingness of the 'rating'


<iframe
  src="assets/C:\Users\<YourUsername>\Desktop\dsc80-2024-fa\RecipeAnalysis\assets\plot2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing

This hypothesis testing takes us back to our orginal question of if the number of time a recipe takes to do impact the rating. For the testing I decided to use the 'quick_Recipe' column which I created when data cleaning, which splits the recipes into short or not short recipes. I choose to a permutation test because we didn't have any actual information on the population and we wanted to see if two distributions indicated that it came from the same population. Additionally I didn't do a directional hypothesis, because I wasn't sure if the length of the recipe would higher or lower the rating. It could higher since the recipes could be better quality and thus the food turns out better, but it could be lower since people do like simple and quick recipes. Therefore I decided to go with the absolute difference of means in this situation. 

Null Hypothesis: The preparation time for a shorter recipe doesn't affect its rating.

Alternate Hypothesis:The preparation time for a shorter recipe does affect its rating. 

Test Statistic: The absolute difference of mean in time of the distribution of short recipes and long recipes

Significance Level: 0.05

With the observed absoulute difference of 0.0348 and when simulating the abs mean differences 1000 times, I end with a p value of 0.00 meaning we reject the null hypothesis and the test suggest the number of minutes has an impact on the rating. 

<iframe
  src="assets/C:\Users\<YourUsername>\Desktop\dsc80-2024-fa\RecipeAnalysis\assets\plot3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem

I plan to predict the number of minutes it takes to prepare a recipe. This would be a regression problem since the values I am tyring the predict, 'minutes' is a numerical value. The response variables which I am planning on using are 'n_steps', number of steps which are (numerical), 'complexity', if a recipe is complex or not (boolean value created from number of ingredients), 'ratings' (categorical from 1-5), and 'calories' the amount of calories in each recipe (numerical). I will use R^2 as the metric for evaluting my model because it is used to measure variance within a regression model.

## Base Model 

In our base model we used linear regression model make predictions. The features we are using for this model is 'n_steps' and 'complexity'.
We hot encoded the boolean values in 'complexity' which turned my boolean values into 0 and 1 giving them numerical value. The R^2 valued I recieved when using this model is 0.0167 indicating a very low accuracy within the model

## Final Model 

The Features:
n_steps: This feature represents the number of steps that are in a recipe, and in way measures how sophisticated a recipe is which and naturally the more step you have the most time it would take for a recipe which is why I included it as a feature. Complexity is a boolean thus I used OneHotEncoder to give each category a binary value. I too used OneHotEncoder for the 'ratings' variable to ensure that the model could capture the categorical values of that column. Finaly calories is a numeric feature which is why I used StandardScalar to normalize the features so that it could better align with the model. 

The model is a Linear Regression model that holds polunomial features. The hyperparameters in which I control for the model were essentially the polynomial degree. I test the best polynomial degrees from 1 to 5 using 5 fold cross validation and evaluated their performance using R^2. Using the cross validation technique added another layer to my model in the way it trained. Then once it was finish training the model selected the best degree which it used to train the whole dataset once again, which was finally used on the test set. However despite all this the R^2 didn't increase at a score of 0.0135














