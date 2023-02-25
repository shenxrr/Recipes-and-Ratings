# Data Science Report on Recipes and Ratings
**Names:**  Yi Zhang, Xinran Shen

## Introduction

The dataset we are using have the recipes and ratings from Food.com https://www.food.com/

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
                                                                 
We will only observe the columns that are relevant to our investigation since our goal is to infer the possible effect of cook time on healthiness of recipes.

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

<iframe src="assets/Histogram_of_minutes.html" width=800 height=600 frameBorder=0></iframe>


### Bivariate Analysis
We make a scatter plot to look at the correlation between cooking time and the amount of sugar the recipe contains. 
The graph shows a negative correlation between cooking time and sugar.



We make another scatter plot to look at the correlation between cooking time and the amount of protein the recipe contains. 



### Interesting Aggregates
---
## Assessment of Missingness

---
## Hypothesis Testing

---
