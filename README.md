# UDacity Blog and Code: Problem Definition & Data Collection

This repo was created for UDacity's Blog Post Assignment. Python code, the dataset processed, and this readme file is included.

Data from 2021 to 2024 was collected from the Stack Overflow Annual Developer Survey [1]. The survey data was used to evaluate whether age, education, or work role impacted coding experience and which of those variables influenced years of coding the most. The data contained upwards of around forty-eight available columns (depending on year) so the most relevant columns were selected after a preliminary data review. 

# Running Code

This code works inside of a jupyter notebook so it should run without any setup. In case you decide to copy the code into an IDE, import the following libraries:

import pandas as pd

import numpy as np

import matplotlib.pyplot as plt

import seaborn as sns

from sklearn.model_selection import train_test_split

from sklearn.ensemble import RandomForestRegressor

from sklearn.metrics import mean_squared_error, r2_score

from uuid import uuid4

import requests

import zipfile

import os

The code should run fine without having to download the datasets because the code already reads the data from the links and convert the csv's into dataframes. If you encounter issues with the code, you can download the data from this repo or from the attached links below:

https://survey.stackoverflow.co/datasets/stack-overflow-developer-survey-2024.zip

https://survey.stackoverflow.co/datasets/stack-overflow-developer-survey-2023.zip

https://survey.stackoverflow.co/datasets/stack-overflow-developer-survey-2022.zip

https://survey.stackoverflow.co/datasets/stack-overflow-developer-survey-2021.zip


# Potential Challenges
You will run into issues with one row of data that displays "unset" randonmly across several columns. It changes everytime you run the code from scratch. Displat multiple attempts to remove this row, I was not succesful,





# Data Preparation

Since the data columns and types varied between 2021 and 2025, frequently available columns were extracted by year and then concatenated before data cleaning started. The following seven columns were retained after evaluating the data:

•	Age – object data type

•	Country – object data type

•	DevType – object data type

•	EdLevel – object data type

•	LearnCode – object data type

•	MainBranch – object data type

•	YearsCode – object data type

While the learn code column was not used in this study, it remained unparsed for future use. The response ID column was removed because it varied across years. For example, one year number two was someone from the United States and the following year that number was assigned to a Canadian. The OID column, with randomly assigned values, replaced the response ID column. This was done to avoid data duplication throughout the data cleaning process. The following columns were parsed:

•	Age – object data type

•	Country – object data type

•	DevType – object data type

•	EdLevel – object data type

•	MainBranch – object data type

•	YearsCode – object data type

#
Parsing began in order of importance.

•	YearsCode column: NULL and non-numerical values were removed followed by modifying the data type to integer from object. 

•	YearsNorm column (added): Vaues from the YearCode column copied into this column and converted to float from integer to allow for data normalization.

•	EdLevel column: Updated the values with a concise readable string. Null values were kept and converted to string ‘Unknown’. A ghost value (<unset>) was identified during the parsing process and could not be removed despite multiple attempts. 

•	EdLvlNew column (added): String values copied from EdLevel column. Values then  were converted into integers based on education level.

•	Age column: Slight modifications were made to this column for conciseness. 

•	AgeNew column (added): The age string values were converted into integers to enable model training.

•	DevType column: Parsing this column took the longest since 17640 unique values existed which then needed to be manually evaluated to determine which specific category to convert the verbose values into. Result of parsing was a concise string value.

•	DevTypeNew column (added): The string values were copied from the DevType column and converted into integers to aid model training.

•	MainBranch column: Verbose strings were simplified into simple string categories.

•	Country column: The string values were reviewed and converted into concise values to enable better data readability.

#
A data review and subsequent parsing yielded the following eleven columns and associated data types:

•	Age – object

•	AgeNew – integer

•	Country – string

•	DevType - string

•	DevTypeNew – integer

•	EdLevel – object

•	EdLvlNew – integer

•	MainBranch - object

•	OID - object

•	YearsCode – integer

•	YearsNorm – float

#
The last two data preparation items involved normalizing the YearsNorm column (the target variable) followed by creating a smaller dataframe with the feature options to study.

•	AgeNew (X feature 1)

•	DevTypeNew (X feature 2)

•	EdLvlNew (X feature 3)

•	YearsNorm (y or target variable)

# Modeling

After cleaning and normalizing the data; the X and y variables were assigned and the data was trained and split into X_train, X_test, y_train, and y_test with the standard test/train split of 20/80 percent and a random state of 42 assigned based on the data size and chosen model.
Tree-based models were a clear choice for the data due to the features’ categorical values and the targets’ continuous data but the linear regression model was appealing based on simplicity and interpretability, though it was not attempted due to the categorical nature of the features with less than eight ordinal levels. The ordinal data presented clear challenges and involved additional processing, so the linear regression model was not attempted. 
Effort was applied against tree classification and random forest regression models. The classification model generated errors due to the lack of bimodal features even after processing the data with an ordinal encoding python library. The random forest regression model executed successfully without the need for the encoding library. Random forest regression worked because the target variable was continuous and the feature variables were categorical. For this survey data, the random forest model proved to be the best method out of all the methods learned so far. This method is very reliable and commonly used in machine learning .

Random Forest Regressor Metrics:

Mean squared error: 0.0150

Root mean squared error: 0.1226

Mean absolute squared error: 0.0903

R-squared score: 0.6392

# Evaluation

The r-squared value of 0.6392 indicates the model fits the data well at 63.92%. The mean squared error (MSE) values of 0.0150 means the model’s predictions are great because the average difference is low between the predicted value and the actual value. Between the four metrics, I would use MSE to explain the model’s performance which means the model’s ability to predict years of coding experience, based on the age, work role, and education, is off by less than a year.

 Source

1 -  https://survey.stackoverflow.co/
