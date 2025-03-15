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
<iframe
  src="protein.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="total_fat.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
- **Univariate Analysis**:  
  - **Two box plots** for protein and total fat.  
  - Both have many **outliers**, with a very **small mean and IQR**.  
<iframe
  src="proteinxcal.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="totalxcal.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
- **Bivariate Analysis**:  
  - **Protein vs Calories**  
  - **Total Fat vs Calories**  
  - Both have a **positive trend**, with many data points clustering between the **0-10 PDV proportion range**.  
| protein_cat / cal_cat     | [0.0, 25.0) | [25.0, 50.0) |
|---------------------------|-------------|--------------|
| [0, 25000.0)              | 234414      | 7            |
| [25000.0, 50000.0)        | 7           | 1            |
For the **pivot table**, I used **cal_cat** and **protein_cat** as columns and **count** as the aggregation function. Looking at the table:  
- The majority of the data is in the range **[0,25.0) for protein PDV proportion** and **[25000.0,50000.0) calorie PDV proportion**.  
- This suggests a strong relationship between **protein and calorie** values in those ranges.
## Part3 
Looking at recipie_interaction.info() we can see that there are multiple columns with missing values and I believe that filtered_tags
is NMAR because it could be possible that there is a no tag for a given recipe because maybe there wasn't a tag that represneted how
long it takes to make that recipe. For the missingness permuation test I will be testing it on the rating column. I will be testing it on 
all of the numerical columns in our dataset. After performing our permuation test and appending those results into a list we can see that rating is MCAR for average_rating because we fail to reject the null
and MAR for all the other numerical columns
## Part 4
Null Hypothesis: The average calories content is the same across all protein categories.
Alternate Hypothesis: The average calorie content differs across protein categories. 
Test stat: absolute difference of means
sig-level=0.05
p-val:1.0.0
result: we reject the null that the average calore content is the same across all protein categories.
I believe this would be a good question because we will be able to determine if protein has a factor in the trend of calories
I believe this would be a good test statistic because it shows us the distribution of calories across the different groups and we can see if
that distribution where to change if we change of up the protein categories. Looking at our p-val we can conclude that it is not likely that the 
average calorie content across all protein cateogories are the same. 
## Part 5
The problem that we will be focusing on for our prediction model is 'Predict the number of calories'. This would be considered a regression model because we are trying to predict numerical values. The response variable that we will be foucsing on is the calorie column because our goal is trying to predict the number of calories in a given recipe. The metric that we will be using is RMSE because we are building a regression model and that it shows prediction error. 
## Part 6
The specific model that we will be using is a linear regression model because our bivariate analysis show that calories and protein have a linear relationship. The first two features that we will be using for our baseline model are protein and carbohydrates. They both are quantitative columns which mean we don't need to perform any transformations or encoding. After performing RMSE on our current baseline we get 248.69192187011018 on our test data. I believe this is bad because generally if RMSE is high that means that it's not a good model that makes the best predictions.
## Part 7 
For our final model I added three new features which are fat_cat, cal_cat, and sugars. I believe that fat_cat would help with predictions
because a recipe could likely have higher calories if the total fats in it were above a certain range. I believe that cal_cat would because if we where to now what range the number of calories are in a recipe than we would have an easier time of trying to predict what the actual number of calories of a given recipe could be. Finally I added sugars because generally foods with more sugars tend to have high calories. for fat_cat and cal_cat I performed OneHotEncoding and for Sugars I used Binarizer with a threshold of 151.3. After performing the necessary transformation I then fited it into a pipeline with the polynomial degree as my hyperparameters from 1-5. After fiting it into one pipeline I then put that inside of GridSearchCV to find the best hyperparameters for our model. I got 1 as the best degree and then performed RMSE on our best model against the test data and got an improvment from our base model with the new RMSE being 219.51823848043935.
## Part 8
Null Hypothesis: The RSME is the same for both protein PDV proportions that meet the threshold of 25 and don't
Alternative Hypothesis: The RSME is different between protein PDV proportions that meet the threshold of 25 and don't
My group X for the fairness model is protein PDV proportion values that are below 25 and my Y is protein PDV proportion values that are above 25.
The metric that I will be using is RMSE with my test statistic being the absolute difference of means and a test statistic of 0.05. After performing our test we see that we get a p-val of 0.0 which indicates that our model is likely not fair between the two groups.
