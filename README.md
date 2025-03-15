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

These columns will contain the **bin** that the calorie, fat, or protein value belongs to for a given row. We do this by:  
- Creating three different functions.  
- Applying them to their specific column.  
- Assigning the results to the dataframe.  

### Visualization  
We need to create at least **two plots** for **univariate analysis** and another **two** for **bivariate analysis**.  

- **Univariate Analysis**:  
  - **Two box plots** for protein and total fat.  
  - Both have many **outliers**, with a very **small mean and IQR**.  
- **Bivariate Analysis**:  
  - **Protein vs Calories**  
  - **Total Fat vs Calories**  
  - Both have a **positive trend**, with many data points clustering between the **0-10 PDV proportion range**.  

For the **pivot table**, I used **cal_cat** and **protein_cat** as columns and **count** as the aggregation function. Looking at the table:  
- The majority of the data is in the range **[0,25.0) for protein PDV proportion** and **[25000.0,50000.0) calorie PDV proportion**.  
- This suggests a strong relationship between **protein and calorie** values in those ranges.  


<iframe src="protein.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="total_fat.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="proteinxcal.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="totalxcal.html" width="800" height="600" frameborder="0"></iframe>
