# Recipe-Rating-Analysis
# Does Recipe Preparation Time Impact Rating?
By Indianola Scott
## Introduction
This project uses two datasets from Food.com that have information on different recipes and their respective ratings. The main question investigated in this project is: Does recipe preparation time impact average recipe ratings?

The cleaned dataset has 83,782 rows, with the most relevant columns being recipe id which is unique to each recipe, minutes which includes the total amount of minutes the recipe takes to prepare, and average rating which is a column containing the average rating given to each unique recipe. 
Understanding the relationship between preparation time and ratings can help determine whether or not the time required to make a recipe is associated with how users rate it. 
## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
The original datasets had information about the same recipes, but split across two datasets. The first one contained information about the recipes, and the second contained those recipes respective ratings. When cleaning the data I first loaded both datasets in and cleaned the rating column by replacing all ratings of 0 with NaN, since a rating of 0 represents a missing rating instead of an actual rating on the datasets 1-5 rating scale. This prevents the missing ratings from artificially lowering the average ratings of recipes.

Next I merged the recipes dataset with the interactions dataset by matching them on unique recipe id's. I then calculated the mean of the non missing ratings for each recipe and assigned them to a new column called avg_rating. The resulting dataset has one row per a recipe, and I used it for the remainder of my analysis. 

Lastly, I converted the submitted column into date time objects so that those values are stored as dates that can be used correctly in any type of time based analysis.

Below are the first five rows of the merged and cleaned dataset, and their data for the most relevant columns.

| name                                 |     id |   minutes | submitted           |   n_steps |   n_ingredients |   avg_rating |
|:-------------------------------------|-------:|----------:|:--------------------|----------:|----------------:|-------------:|
| 1 brownies in the world    best ever | 333281 |        40 | 2008-10-27 00:00:00 |        10 |               9 |            4 |
| 1 in canada chocolate chip cookies   | 453467 |        45 | 2011-04-11 00:00:00 |        12 |              11 |            5 |
| 412 broccoli casserole               | 306168 |        40 | 2008-05-30 00:00:00 |         6 |               9 |            5 |
| millionaire pound cake               | 286009 |       120 | 2008-02-12 00:00:00 |         7 |               7 |            5 |
| 2000 meatloaf                        | 475785 |        90 | 2012-03-06 00:00:00 |        17 |              13 |            5 |


### Univariate Analysis
<iframe
  src="assets/preparation-times.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The distribution of recipe preparation times is strongly skewed right, with most of the recipes only needing short preparation times. To make the distribution easier to interpret, the visualization excludes any recipes that are above the 95th percentile of preparation time as it'd stretch the scale with their extreme values. 

### Bivariate Analysis
<iframe
    src="assets/rating-by-time-group.html"
    width="800"
    height="600"
    frameborder="0"
></iframe>
The distributions of average recipe ratings are similar for recipes that take 60 minutes or less and recipes that take more than 60 minutes. Both groups have a median rating of 5, however there are differences in the spread of ratings between the two groups. This motivates a hypothesis test to be able to determine whether or not their mean rating differ significantly.

### Interesting Aggregate

| time_group           |   average_rating |   median_rating |   number_of_recipes |   average_prep_time |
|:---------------------|-----------------:|----------------:|--------------------:|--------------------:|
| 60 minutes or less   |          4.62931 |               5 |               60989 |             29.0833 |
| More than 60 minutes |          4.61345 |               5 |               20184 |            360.039  |


Recipes taking 60 minutes or less have a slightly higher average rating (4.629) than recipes taking more than 60 minutes (4.613) and both groups have a median rating of 5. Although the difference in average ratings (0.016) is small, it still motivates further testing to properly determine whether the observed difference is statistically significant. 

## Assessment of Missingness
### MNAR Analysis
I believe that the average rating column in the dataset could be MNAR (Missing Not At Random), as whether or not a user chooses to provide a rating could depend on the rating they'd give. For example, users who had neutral experiences might be more likely to not leave any rating at all, which would make it impossible to determine from the observed data alone whether or not this is the case.

If additional information about user behavior was collected, like the number of users who prepared a recipe but did not rate it, then the missingness could potentially be explained and classified as MAR instead. 
### Missingness Dependency
To figure out whether the missingness of average rating depends on any other observed recipe characteristics, I performed permutation tests comparing recipes with missing average ratings to recipes without missing average ratings. For both tests, I used the absolute difference in median values between the two different groups as the test statistic, and a significance level of 0.05.
#### Missingness of Avg. Rating and Preparation Time
Null Hypothesis: The distribution of preparation time is the same for recipes with and without missing average rating, so the missingness of average rating doesn't depend on preparation time (minutes).

Alternative Hypothesis: The distribution of preparation time is not the same for recipes with and without missing average rating, so the missingness of average rating does depend on preparation time (minutes).

The observed difference in median preparation time was 10 minutes, and after performing 1,000 permutations, none of the simulated differences were at least as extreme as the observed difference. This gave a p-value of less than 0.001, which is below the significance level and leads me to reject the null hypothesis. This provides evidence that the missingness of average rating does depend on the recipes preparation time. 
The permutation test results are shown below:
<iframe
    src="assets/missingness-minutes.html"
    width="800"
    height="600"
    frameborder="0"
></iframe>

#### Missingness of Avg. Rating and Number of Ingredients
Null Hypothesis: The distribution of number of ingredients is the same for recipes with and without missing average rating, so the missingness of average rating doesn't depend on number of ingredients.

Alternative Hypothesis: The distribution of number of ingredients is not the same for recipes with and without missing average rating, so the missingness of average rating does depend on number of ingredients.

The observed difference in median number of ingredients was 0, and after performing 1,000 permutations, I got a p-value of 1.0. Since this p-value is greater than the significance level I fail to reject the null hypothesis. This test doesn't provide evidence that the missingness of average rating depends on the recipes number of ingredients. 

## Hypothesis Testing
To determine if preparation time is associated with average recipe rating, I performed a two sided permutation test that compared recipes taking 60 minutes or less to recipes taking more than 60 minutes. I chose 60 minutes since it's the 75th percentile of preparation times in the dataset. 

Null Hypothesis: Recipes that take 60 minutes or less and recipes that take more than 60 minutes have the same mean average rating. Any observed difference is due to random chance.

Alternative Hypothesis: Recipes that take 60 minutes or less and recipes that take more than 60 minutes have different mean average ratings.

The test statistic I decided to use is the difference in mean average ratings between recipes that take 60 minutes or less and recipes that take more than 60 minutes. 

I used a significance level of 0.05 and a two sided permutation test because I wanted to see if there was a difference in either direction. The observed difference from the test was approximately 0.0159, with shorter recipes having the higher mean average ratings. After 1,000 permutations, the p-value was 0.002 which is less than the significance level, and led me to reject the null hypothesis. 
This result provides evidence that the mean average ratings differ between the two preparation time groups, however it still doesn't create a causal relationship.

## Framing a Prediction Problem
I will predict a recipes average rating using information readily available about the recipe itself. Since the average rating column is quantitative, it will be a regression problem. I'll evaluate the models using RMSE (Root Mean Squared Erorr), as it measures the prediction error in the same units as the response variable and it gives more weight to predictions that are farther from the actual rating. 

When I am doing the prediction, I'm assuming that the recipe has been submitted, but has not yet received any ratings from users. Hence, I'll only be using characteristics that are known when the recipe is submitted, such as the preparation time, number of steps, and number of ingredients in the recipe. I won't be using any information that has been derived from user ratings. 
## Baseline Model
I created a baseline Linear Regression model using three quantitative features; preparation time (minutes), number of recipe steps (n_steps), and number of ingredients (n_ingredients). Since all three of these features are quantitative, I didn't need to do any categorical encoding, and their numerical features were left unchanged. I implemented all of the preprocessing and model training using a single sklearn Pipeline. 

The model I built had a training RMSE of 0.642 and a test RMSE of 0.636. The fact that the training and test RMSE's are so similar suggests that the model does a good job of generalizing similarly to unseen data without overfitting a lot. However, there is still a lot of room for improvement in the model, as the RMSE we got means that there are meaningful prediction errors relative to the rating scale. So, incorporating more information with more informative features could help improve the model. 
## Final Model
For my final model, I used a Decision Tree Regressor and kept the three original baseline features (minutes, n_steps, and n_ingredients). I also added two engineered features which were minutes_per_step that worked to capture how time intensive each step actually was, and steps_per_ingredient that helped to represent the recipe complexity. I chose to create these features as they could provide more information about a recipes structure. 

I also used a 5-fold cross validation with GridSearchCV to tune the max depth with several possible tree depths. In the end, I found that the best performing value was a max depth of 2. My final model ended up having a training RMSE of 0.6413 and a test RMSE of 0.6352, when comparing these RMSE's to those of the baseline test (around 0.6360), I found that the final model produced a very small improvement on unseen data. The Decision Tree Regressor also allowed for nonlinear relationships to exist, that the Linear Regression baseline model wasn't able to capture. 
## Fairness Analysis
I tested my final model to see if it performs worse for recipes with longer preparation times. Group X consisted of recipes that take more than 60 minutes, while Group Y consisted of recipes that take 60 minutes or less to prepare. I used the RMSE as my evaluation metric and the difference between the RMSE's of Group X and Y as my test statistic, with the standard significance level of 0.05.

Null Hypothesis: The model is fair with respect to preparation time, it's RMSE for recipes taking more than 60 minutes is roughly the same as that of recipes taking 60 minutes or less. Any observed difference is due to chance. 

Alternative Hypothesis: The model performs worse for recipes taking more than 60 minutes, so it's RMSE is bigger than that of recipes whose preparation time is 60 minutes or less. 

The model has an RMSE of 0.663 for Group X and a RMSE of 0.626 for Group Y, which has an observed difference of 0.037. I used a one sided permutation test with 1,000 permutations, which produced a p-value of 0.023. Since that p-value is less than that of the significance level, I reject the null hypothesis and conclude that my final model does perform worse for recipes that take more than 60 minutes to prepare. 