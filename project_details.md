# Project details: FIFA - MoneyBall

<!-- ![Project Banner: FIFA](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/project+banners/fifa-project.jpg) -->

## The challenge

Perform an end-to-end analysis putting into practice what you have learned so far. You will apply statistical or machine learning techniques and present your results to the class.

### Possible Outcomes

- Rank players by market value.
- Highlight the top players for their outstanding performances over a discrete season.
- Decide when to transfer a player.
- Decide the best replacement for a transferred player.

> You might suggest your own outcomes. Check with instructional staff before committing to a new option.

## Objectives

- Ask interesting and thoughtful questions and find the data to answer them.
- Focus on improving in areas that are hard for you or learning more about something with which you feel comfortable.
- Apply the statistical and machine learning techniques we have learned.
- Create useful and clear graphs.
- Present your insights in a thoughtful, clear, and accurate way.

## Dataset

In this project, you will use the provided [**fifa21_male2.csv**](https://github.com/ironhack-edu/data_mid_bootcamp_project_FIFA_MoneyBall/blob/master/fifa21_male2.csv) dataset.

Details about the dataset can be found here as well [https://www.kaggle.com/ekrembayar/fifa-21-complete-player-dataset?select=fifa21_male2.csv]

This data set includes:

1. **EA Sports FIFA 19 Game** data:

- Player Name
- Club of the Player
- League
- Position
- Pace
- Shooting
- Passing
- Dribbling
- Defending
- Physical

2. **Transfermarkt** extra info by player:

- Date of Birth
- Nationality
- Height
- Foot
- Day Joined the current club
- Day of Contract End
- Market Value of the Player

3. **Instagram and Facebook** data by player:

- Number of followers on Instagram
- Number of likes on Facebook of the club in which the player has a contract

4. **ESPN FC** data from the past 5 years performance of each player

- **GS:** Games Started
- **SB:** Games Substituted
- **G:** Goals Scored
- **A:** Assists
- **SH:** Shots
- **SG:** Shots on Goal
- **FC:** Fouls Committed
- **FS:** Fouls Suffered
- **YC:** Yellow Cards
- **RC:** Red Cards

## Instructions & Scope

- You **must plan your project.** Creating a Kanban or using [Trello](https://trello.com/) or a similar app for a digital board is mandatory.
- You **CAN'T CODE** until your project is planned.
- Create a `*.gitignore*` file and include it in your repository.
- You should include a linear regression question(s) on the data.

## Deliverables

- A well-commented Jupyter notebook with your analysis.
- The final dataset after all cleaning and transformations.
- Repository with your workflow + documentation + code.
- Visual documentation of Kanban or Trello board link.

## Tips & Tricks

- Organize yourself (don't get lost!). Respect deadlines.
- Ask for help but don't forget that Google is your friend.
- Define a simple approach first. You never know how the data can betray you. :wink:
- Document your work.
- Learn about the problem and what research has been done before you.
- Before making a graph, think about what you want to represent.








> Original shape  (17125, 107)
Dropping columns that were numerically redundant (stats that already were counted at each “grouping”, including the total stats) and would not affect the target from a categorical standpoint (name, id, wage, etc.)
> New shape (17125, 25)
Used info() to show both dtypes and null count
Next to work on changing some fields to their correct type
	age = categorical/object
	hits = categorical is correct but the field needs cleaning
		change values ending in K to actual numbers
		then group values by 0-99,100-249, 250-500,500-999,1000+
Then to fill NA’s in a/w and d/w (might drop the records)
Only 0.5% of each column were NA’s. Used ffill to replace/distribute this into the categories for each 

Check for multicollinearity, observations: 
       ‘bov’ and target ‘ova’ are very similar.
       ‘def’ and ‘defending’ are very similar.
‘dri’ and ‘mentality’ had many instances of high collinearity with other columns, dropped both.
New shape (17125, 20)
Check distribution of the numerical columns
	Define columns related to numerical columns
	Anomalies: Attacking, 2 peaks. Skill, 2 peaks. Movement, right skewed. Defending, 2 peaks. Goalkeeping, high concentration on low values. Base Stats, random 
Data transformation: Tried various transformation techniques bu log trans seemed to work best in most cases
	Goalkeeping – log trans.
	Sho – log trans.
	Defending – tried sq.rt and boxcox transformations but log trans made the most significant difference in making the distribution more regular.
	Dropped original columns for Goalkeeping, Sho and Defending 
Categoricals:
- Height : used string functions to replace and split the inches and foot indicators, and then multiplied the feet by 12 and added inches to make all records into inches. Then, grouped them together to make less categories
- Weight: mapped a function to remove “lbs” from records to then map grouped categorical values into a new column “weight_group”
- Age: Early in my code I identified that this feature, though numerical in nature, is categorical so I converted the column to string. This helped when I was checking for distributions of numercials, etc. as I was focusing on dtypes that were np.number. However, when it came time to group them, I needed to convert the values back to int in my function, then create and map the age group to a new column “age_group” based on their value.
- Charts helped to show which categories needed to be adjusted to make a more even distribution. 
- Barplots helped to illustrate the relationship between categorical columns and the target ‘ova’.
Encoded the categorical data using pd.get_dummies but when I went to do the linear regression, I received an error that the column titles were not of the same dtype so I changed the method to use the OneHotEncoder.

I ran 3 different types of scalers (MinMax, Standard and Normalizer) to try to see what would give me the best result. 
Results: MinMax, Standard and Normalizer)





	
	
		


