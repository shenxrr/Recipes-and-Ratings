# Data Science Report on Cooking time and healthiness
**Names:**  Yi Zhang, Xinran Shen

## Introduction

The dataset we are using have the recipes and ratings from Food.com [food.com](https://www.food.com/)

Our research question is about the relationship between the average cooking time and the healthiness of the recipe.

It is important to investigate since cooking time can help people make healthier food choices. Cooking time can affect the healthiness of the food. For example, it takes just a few minutes to boil chicken breast or make salad, and they are considered as healthy food. However, when dealing with food for a longer time tends to have more steps and processes to deal with the food, like making desserts. This information can be valuable for individuals with specific dietary needs or health concerns. This dataset includes a relatively big amount of food recipes and details. It is useful to refer to this dataset when cooking.

There are 83782 rows from the dataset after we clean our data. The columns that are relevant to our question is “name”, “minutes”, “n_steps”,”nutritions” .
Column “name” includes recipe names. Column “minutes” include the cooking time information for each recipe in minutes. Column “n_steps” indicates the number of steps in the recipe. The column “nutrition” include nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; The unit PDV stands for “percentage of daily value””. We particularly focus on the data of “sugar”and “protein ”from “nutrition” , which we take in consideration as closely related to the healthiness of the food.


## Data Cleaning and Exploratory Data Analysis

### Data loading and cleaning
First, we loaded the two datasets:

The 'recipes' datasets contains recipes information.

The 'interactions' datasets contains reviews and ratings submitted for the recipes in RAW_recipes.csv.

We merged two datasets into one large dataframe. We filled ratings of 0 with np.NaN since the lowest rating a recipe can get on the website is 1. Hence, having a 0 in rating simply means that there is no rating of this recipe and should be replaced with np.NaN in order to get the accurate calculation for average rating. After filling np.Nan would make the average rating of the recipe more accurate without having bias to those ratings which do not exist.

Then, we've calculated the average rating for each recipe in the dataset accordingly and added to the dataframe. The column name is "average rating"
                                                                 
We will only observe the columns that are relevant to our investigation since our goal is to infer the possible effect of cooking time on healthiness of recipes.

Then we manipulated the nutrition column to have more detailed information about the nutrition. We seperated each component in nutrition column into multiple columns. we've changed the data type of each of the nutrients to floats since they were strings in a list from the original dataset and which is hard to calculate directly.

The following table shows the first five rows of our cleaned Dataframe. 



|     id | name                                 |   minutes | nutrition                                     |   n_steps |   average rating |
|-------:|:-------------------------------------|----------:|:----------------------------------------------|----------:|-----------------:|
| 333281 | 1 brownies in the world    best ever |        40 | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]      |        10 |                4 |
| 453467 | 1 in canada chocolate chip cookies   |        45 | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]  |        12 |                5 |
| 306168 | 412 broccoli casserole               |        40 | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]     |         6 |                5 |
| 286009 | millionaire pound cake               |       120 | [878.3, 63.0, 326.0, 13.0, 20.0, 123.0, 39.0] |         7 |                5 |
| 475785 | 2000 meatloaf                        |        90 | [267.0, 30.0, 12.0, 12.0, 29.0, 48.0, 2.0]    |        17 |                5 |


### Univariate Analysis

We make a Histogran to look at the distribution of minutes. The graph is skewed to the right, 
the longer the time gets, there are less recipes. 

<iframe src="assets/Histogram-of-minutes.html" width=800 height=600 frameBorder=0></iframe>


### Bivariate Analysis
We make a scatter plot to look at the correlation between cooking time and the amount of sugar the recipe contains. 
The graph shows a negative correlation between cooking time and sugar. 
As the time increases, the amount of sugar in food recipe decreases. 

<iframe src="assets/time_sugar.html" width=800 height=600 frameBorder=0></iframe>


We make another scatter plot to look at the correlation between cooking time and the amount of protein the recipe contains. 
As the time increases, the amount of protein in food recipe decreases.

<iframe src="assets/time_protein.html" width=800 height=600 frameBorder=0></iframe>

 
### Interesting Aggregates

We make a pivot table with minutes and healthiness. We have three levels of healthiness represented by -1,0, and 1 and we calculate the mean and count of minutes in the pivot table. Therefore it is more straightforward to see the difference in means for each level of healthiness, so we can check how cooking time varies for different levels of healthiness.


|   healthiness |     mean |   count |
|--------------:|---------:|--------:|
|            -1 | 171.446  |   19833 |
|             0 | 102.872  |   38228 |
|             1 |  89.6013 |   25721 |


## Assessment of Missingness
### NMAR Analysis

We think the column 'name' is MCAR because there is only one row that have 'name' missing, and we cannot get a pattern behind the missingness.

The 'description' column has 70 rows that are missing. Our guess is that users that felt the recipe is simple enough might not want to write a decription for it. As such, we believe the data is not missing at random, i.e. NMAR, since the missingness depends on the value itself.

### Missingness Dependency
investigate the missingness of 'average rating' column

We run a permutation test to check if the 'average rating' column depends on 'minutes' column

Null Hypothesis: The missingness in 'average rating' columns does not depend on 'minutes' column, i.e. they are from the same distribution

Alternative Hypothesis: The missingness in 'average rating' columns does depend on 'minutes' column, i.e. they are not from the same distribution

Comparing average rating missing and not-missing distribution:

<iframe src="assets/miss_1.html" width=800 height=600 frameBorder=0></iframe>

After running permutation test, we got a p value of 0.0388, which is strictly smaller than our p-value cutoff, 0.05.
Therefore, we reject the null, and conclude that the missingness in column 'average rating' is MAR dependent on 'minutes'.


We run a permutation test to check if the 'average rating' column depends on 'high_protein' column

Null Hypothesis: The missingness in 'average rating' columns does not depend on 'high_protein' column, i.e. they are from the same distribution

Alternative Hypothesis: The missingness in 'average rating' columns does depend on 'high_protein' column, i.e. they are not from the same distribution

Comparing average rating missing and not-missing distribution:

<iframe src="assets/miss_2.html" width=800 height=600 frameBorder=0></iframe>

After running permutation test, we got a p value of 0.1316, which is strictly larger than our p-value cutoff, 0.05.
Therefore, we fail to reject the null, and conclude that the missingness in column 'average rating' does not depend on column 'high_protein'.

---
## Hypothesis Testing

Our hypothesis is that cooking time(minutes) is a significant predictor of healthiness of a recipe. We state our hypothesis as the following:
- Null Hypothesis: healthiness and cooking time(minutes) of recipes **are not** related – the low average cooking time of recipes that are rated healthy(1) is due to chance alone
- Alternative hypothesis: healthiness and cooking time(minutes) of recipes **are** related – the low average cooking time of recipes that are rated healthy(1) is not due to chance alone

We pick the average cooking time of recipes that are rated healthy as our test statistics.
This is reasonable to use since minutes are numeric and we can generate an empirical distribution based on the test statistic to see if our observed test statistics was drawn from the distribution. 
Since our question is about finding the relationship between the cooking time and healthiness of food, we can simulat  results based on our null hypothesis and compare it with our observed test statistics. The alternative hypothesis raises the opposite opinions with our null hypothesis and which is valid.

We choose 0.05 as our significance level. This is a good value to use since it is a common cutoff for hypothesis testing 
The resulting p-value is 0.05668 , which is slightly greater than 0.05
Therefore we failed to reject the null hypothesis.
We can conclude that the low average cooking time of recipes that are rated  healthy is due to chance alone. 
We have also visualized the empirical distribution of the test statistic:
<iframe src="assets/hypo.html" width=800 height=600 frameBorder=0></iframe>
---
