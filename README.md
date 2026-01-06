# All About Calories  
**DSC80 Final Project - UCSD 2025**  

## Part 1  
The dataset that I will be using is **Recipes and Ratings**, which is comprised of two different CSV files: Recipes and Ratings, going all the way back to 2008. In total, there are 15 columns from the original dataset, all relating to the details about each recipe and each review.  

The main question that we will be exploring throughout this project is:  
**"What types of recipes tend to have the most calories?"**  

This is very important because most people don't understand nutrition to the fullest extent, only understanding that too many calories means that it is not good for you. I decided to pursue this question to explain what exactly would be a good indication of a recipe having too many calories. This way, if people were to make their own version of a recipe in the future, they would understand that too much fat might lead to too many calories, and so on.  

After merging the two dataframes into one, which we will be calling **receipes_interactions**, there is a total of **234,429 rows**. The main columns of interest are:  
- **id**: Contains the unique recipe ID of each recipe.  
- **nutrition**: Contains lists of strings about nutritional facts such as calories, fats, protein, sugars, etc.  
- **calories**: Contains a float representation of the total number of calories in a recipe.
- **tags** : Contains Food.com tags for recipe
- **nsteps**:Contians number of steps in recipe
- **rating**: Contians Rating given


## Part 2  
First, looking at the considerations section, we are told to replace all values of `0` in **rating** with `np.nan`, which we can accomplish by using `.replace()`.  

The majority of the information that we need for this project comes from the **nutrition** column. Each row of the nutrition column has a string representation of a list with **7 elements** in total. To extract these elements:  
1. Remove the brackets in each element.  
2. Split the string at `','`, which creates a list of all unique elements.  
3. Apply a **transform function** to convert each element into floats and, for some, divide by 100 to have proportions instead of percentages.  
4. Assign all of those columns to the dataframe.  

Now, looking at the **tags** column, I want to create a new column that only contains the **time tag** of each row. We apply a **lambda function** to the column using **regex** to find the biggest time tag or return `np.nan` if it can't find any.  

Next, I want to create a **True/False column** that represents whether a given row in the dataframe has a null value for **rating**.  

Finally, I want to create three new **categorical columns**:  
- **cal_cat** (calories category)  
- **fat_cat** (fat category)  
- **protein_cat** (protein category)
And one **numerical column**:
-**average_rating** (average rating for each recipe) 

These columns will contain the **bin** that the calorie, fat, or protein value belongs to for a given row. We do this by:  
- Creating three different functions.  
- Applying them to their specific column.  
- Assigning the results to the dataframe.  

### Visualization  

We need to create at least **two plots** for **univariate analysis** and another **two** for **bivariate analysis**.

<iframe src="protein.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="total_fat.html" width="800" height="600" frameborder="0"></iframe>

#### Univariate Analysis
- Two **box plots** were created for **protein** and **total fat**.
- Both plots contain many **outliers**, with a very **small mean and IQR**, indicating a right-skewed distribution.

<iframe src="proteinxcal.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="totalxcal.html" width="800" height="600" frameborder="0"></iframe>

#### Bivariate Analysis
- **Protein vs Calories**
- **Total Fat vs Calories**
- Both plots show a **positive trend**, with many data points clustering between the **0–10 PDV proportion range**.

For the **pivot table**, I used **cal_cat** and **protein_cat** as columns and **count** as the aggregation function. Looking at the table:
- The majority of the data lies in the range **[0, 25.0)** for protein PDV proportion and **[25,000.0, 50,000.0)** for calorie PDV proportion.
- This suggests a strong relationship between **protein** and **calorie** values in these ranges.

---

## Part 3: Missingness Analysis  

Looking at `recipe_interaction.info()`, we can see that there are multiple columns with missing values. I believe that **filtered_tags** is **NMAR**, because it is possible that there is no tag for a given recipe if no tag accurately represents how long the recipe takes to make.

For the missingness permutation test, I tested the **rating** column against all numerical columns in the dataset. After performing the permutation tests and appending the results into a list, we observe that:
- **rating** is **MCAR** with respect to **average_rating**, because we fail to reject the null hypothesis.
- **rating** is **MAR** with respect to all other numerical columns.

---

## Part 4: Hypothesis Testing  

**Null Hypothesis:**  
The average calorie content is the same across all protein categories.

**Alternative Hypothesis:**  
The average calorie content differs across protein categories.

- **Test statistic:** Absolute difference of means  
- **Significance level:** 0.05  
- **p-value:** 1.0  

**Result:**  
We reject the null hypothesis that the average calorie content is the same across all protein categories.

This is a good question to explore because it allows us to determine whether protein has a role in the trend of calories. The chosen test statistic is appropriate because it compares calorie distributions across different protein groups and allows us to observe how the distribution changes when protein categories change. Based on the p-value, it is unlikely that the average calorie content is the same across all protein categories.

---

## Part 5: Prediction Problem  

The prediction problem focuses on **predicting the number of calories** in a recipe. This is a **regression problem**, since we are predicting a numerical value.

- **Response variable:** Calories  
- **Evaluation metric:** RMSE  

RMSE is appropriate because it measures the average prediction error magnitude for regression models.

---

## Part 6: Baseline Model  

The baseline model used is **linear regression**, since the bivariate analysis showed a linear relationship between calories and protein.

The first two features used in the baseline model are:
- **Protein**
- **Carbohydrates**

Both features are quantitative, so no transformations or encoding were required.

After evaluating the baseline model, the RMSE on the test data is:

- **RMSE:** 248.69192187011018  

This RMSE value is relatively high, indicating that the baseline model does not make strong predictions.

---

## Part 7: Final Model  

For the final model, three additional features were added:
- **fat_cat**
- **cal_cat**
- **sugars**

These features were chosen because recipes with higher fat or sugar content are more likely to have higher calorie values, and categorical calorie ranges may help improve predictions.

- **fat_cat** and **cal_cat** were OneHotEncoded.
- **sugars** was transformed using a **Binarizer** with a threshold of **151.3**.

All transformations were included in a pipeline with polynomial degrees ranging from **1 to 5** as hyperparameters. This pipeline was then passed into **GridSearchCV** to find the optimal degree.

- **Best polynomial degree:** 1  
- **Final test RMSE:** 219.51823848043935  

This represents an improvement over the baseline model.

---

## Part 8: Fairness Evaluation  

**Null Hypothesis:**  
The RMSE is the same for protein PDV proportions that meet the threshold of 25 and those that do not.

**Alternative Hypothesis:**  
The RMSE differs between protein PDV proportions that meet the threshold of 25 and those that do not.

- **Group X:** Protein PDV proportion < 25  
- **Group Y:** Protein PDV proportion ≥ 25  
- **Metric:** RMSE  
- **Test statistic:** Absolute difference of means  
- **Significance level:** 0.05  

After performing the fairness test, the resulting **p-value is 0.0**, which indicates that the model is likely **not fair** between the two protein PDV proportion groups.
