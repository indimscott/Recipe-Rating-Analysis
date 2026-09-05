# Recipe-Rating-Analysis
# Does Recipe Preparation Time Impact Rating?
By Indianola Scott
## Introduction
This project uses two datasets from Food.com that have information on different recipes and their respective ratings. The main question investigated in this project is: Does recipe preparation time impact recipe ratings?
The merged recipe and interactions datasets have 234,428 rows, with the most relevant columns being recipe id which is unique to each recipe, minutes which includes the total amount of minutes the recipe takes to prepare, and rating which is a column containing the average rating given to each unique recipe. Understanding the relationship between preparation time and ratings can help determine whether or not the time required to make a recipe is associated with how users rate it. 
## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
The original datasets had information about the same recipes, but split across two datasets. The first one contained information about the recipes, and the second contained those recipes respective ratings. When cleaning the data I first loaded both datasets in and cleaned the rating column by replacing all ratings of 0 with NaN, since a rating of 0 represents a missing rating instead of an actual rating on the datasets 1-5 rating scale. This prevents the missing ratings from artificially lowering the average ratings of recipes.
Next I merged the recipes dataset with the interactions dataset by matching them on unique recipe id's. After this merge, each row represented a single user interaction with a recipe and all of the recipes information. 
Lastly, I converted the submitted and date columns into date time objects so that those values are stored as dates that can be used correctly in any type of time based analysis.

Below are the first five rows of the merged and cleaned dataset, and their data for the most relevant columns.
| name                                 |     id |   minutes | submitted           |   rating | date                |
|:-------------------------------------|-------:|----------:|:--------------------|---------:|:--------------------|
| 1 brownies in the world    best ever | 333281 |        40 | 2008-10-27 00:00:00 |        4 | 2008-11-19 00:00:00 |
| 1 in canada chocolate chip cookies   | 453467 |        45 | 2011-04-11 00:00:00 |        5 | 2012-01-26 00:00:00 |
| 412 broccoli casserole               | 306168 |        40 | 2008-05-30 00:00:00 |        5 | 2008-12-31 00:00:00 |
| 412 broccoli casserole               | 306168 |        40 | 2008-05-30 00:00:00 |        5 | 2009-04-13 00:00:00 |
| 412 broccoli casserole               | 306168 |        40 | 2008-05-30 00:00:00 |        5 | 2013-08-02 00:00:00 |

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
