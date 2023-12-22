Problem: Given various performance statistics from incoming new players, can we fabricate a model to determine their overall grading from 0-100 (‘ova’) based on historical data? 


> Original shape  (17125, 107)
I dropped columns that were numerically redundant (stats that already were counted at each “grouping”, including the total stats) and would not affect the target from a categorical standpoint (name, id, wage, etc.). I believe that other attributes like value/wages, from the perspective of the PROBLEM would not affect the target, more so the physical attributes of the players themselves. 

> New shape (17125, 25)
Used info() to show both dtypes and null count
Next to work on changing some fields to their correct type
	age = categorical/object
	hits = categorical is correct but the field needs cleaning
		change values ending in K to actual numbers
		then group values, i.e. 0-99,100-249, 250-500,500-999,1000+
Then to fill NA’s in a/w and d/w, I considered dropping the NA the records, but since only 0.5% of each column were NA’s, I used ffill to replace/distribute this into the categories for each 

Check for multicollinearity, observations: 
      ‘bov’ and target ‘ova’ are very similar.
      ‘def’ and ‘defending’ are very similar.
‘dri’ and ‘mentality’ had many instances of high collinearity with other columns, dropped both.

> New shape (17125, 20)
Check distribution of the numerical columns
	Define columns related to numerical columns
	Anomalies: Attacking, 2 peaks. Skill, 2 peaks. Movement, right skewed. Defending, 2 peaks. Goalkeeping, high concentration on low values. Base Stats, random 
Data transformation: Tried various transformation techniques but log trans seemed to work best in most cases
	Goalkeeping – log trans.
	Sho – log trans.
	Defending – tried sq.rt and boxcox transformations but log trans made the most significant difference in making the distribution more regular.
	Transformed data was added to new columns respectively. Original columns for Goalkeeping, Sho and Defending were dropped. 

Categorical columns:
- Height : used string functions to replace and split the inches and foot indicators, and then multiplied the feet by 12 and added inches to make all records into total inches. Then, grouped them together to make less categories into the new column “height_group”
- Weight: mapped a function to remove “lbs” from records, then made another function which grouped based on values. I then mapped grouped categorical values into a new column “weight_group”
- Age: Early in my code I identified that this feature, though numerical in nature, is categorical so I converted the column to string. This helped when I was checking for distributions of numercials, etc. as I was focusing on dtypes that were np.number. However, when it came time to group them, I needed to convert the values back to int in my function, then create and map the age group to a new column “age_group” based on their value.
- Barplot charts helped to illustrate the relationship between categorical columns and the target ‘ova’. They also helped to show which categories needed to have their groupings adjusted to make a more even distribution so that there would be more representation for each during the Train Test Split. 

Encoded the categorical data using pd.get_dummies but when I went to do the linear regression, I received an error that the column titles were not of the same dtype so I changed the method to use the OneHotEncoder.
> Total number of ‘dummy’ columns after encoding: 30

I ran 3 different types of scalers (MinMax, Standard and Normalizer) to try to see what would give me the best result. I also adjusted the sample size and random state to observe the differences it made. 
> Total number of numerical columns: 11

I concatenated the results from each of the scaled data into different variables and re-ran the code multiple times to see and compare all the results. 

 ![image](https://github.com/valdesgabriel/data_mid_bootcamp_project_FIFA_MoneyBall/assets/149256193/208fe46b-61d6-4542-a10a-f94478d78757)
 
Lastly, when I wanted to compare unscaled data with the rest of the results, I had to change my code from using the OneHotEncoder because the column headers from the non-scaled data was dtype ‘str’ so then I did use the pd.getdummies method. 
