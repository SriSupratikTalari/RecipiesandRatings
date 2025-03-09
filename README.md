# All About Calories
DSC80 Final project UCSD 2025
## Part 1
The dataset that I will be using is Recipes and Ratings which is comprised of two different cvs
Recipes and Ratings going all the way back to 2008. In total there is 15 columns from the original dataset
all realting to the deatils about each recipe and each review. The main question that we will be exploring 
throughout this project is "What types of recipes tend to have the most calories?". This is very important 
because most people don't understand nutrition to the fullest extent only understanding that too many calories 
means that it is not good for you. I decided to pursue this question to explain what exactly would be a good indication
of reciepe having too many calories so in the future if people were to make their own verison of reciepe the would be able to 
understand that too much fats might lead to too much calories and so on. After merging the two dataframes into one 
which we will be calling receipes_interactions there is a total of 234429 rows. The main columns of our interests would 
be id, nutrition, and calories. The id column contains the unqiue recipe id of each recipe. The nutrition column contains lists 
of strings about nutrional facts about each recipe like calories, fats, protein, sugars, etc. The calories column contains 
a float representation of the total number of calories in a recipe.
### Part 2
need portion about data cleaning. For the majority of our analysis we need the information inside the nutrition column meaning we have to create
separate columns for elements inside nutrition. We can do that by using .transform(lambda x: float(x[index])) function to extract each value based on its position in the list and then assign an new coloumn to dataframe containing all of the values of x[index]. Next we have to perform univariate 
analysis on each of the new columns that we added so that we can understand each distribution. The first column that we will take a look at 
is protein which we will be using a boxplot to represent the distribution. Looking at the box plot we can see that protein has many outliers and
a very small mean and a large range. Next we will work with bivariate analysis to see if there are any trends between protein and calories. Looking at the figure we can see that the majority of the data between 0-10 grams or percent of protein afterwards it seems that there is linear  trend 
of calories and protein. 
### Part3 
I dont know anything
#### Part 4
