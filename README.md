# ETL-Pipeline-Preparation
ETL Pipeline Preparation Disaster pipeline 

This document outlines the steps for preparing an Extract, Transform, Load (ETL) pipeline for processing and cleaning datasets. The pipeline takes raw datasets, processes them, and saves the cleaned data to an SQLite database.

Steps

**1. Import Libraries and Load Datasets**
Import Python Libraries

# import libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sqlalchemy import create_engine

# load messages dataset
messages = pd.read_csv('messages.csv')

# load categories dataset
categories = pd.read_csv('categories.csv')

**2. Merge Datasets**
# merge datasets
df = pd.merge(categories_unique_id ,messages_unique_ids, on='id' )

**3. Split categories into separate category columns.**
# create a dataframe of the 36 individual category columns
categories = categories['categories'].str.split(';', expand=True)
categories.head()

**4. Convert category values to just numbers 0 or 1.**
Iterate through the category columns in df to keep only the last character of each string (the 1 or 0). 
For example, related-0 becomes 0, related-1 becomes 1. Convert the string to a numeric value.

for column in categories:
    # set each value to be the last character of the string
    categories[column] = categories[column].astype(str).str[-1].astype(int)
    
    # convert column from string to numeric
    categories[column] = pd.to_numeric(categories[column])
categories.head()

**5. Replace categories column in df with new category columns.**
# drop the original categories column from `df`

df = df.drop('categories', axis=1)
df.head()

**6. Remove duplicates**.
# drop duplicates
df_unique = df.drop_duplicates(subset=['id', 'message'])
print(df_unique.tail(20))

**7. Save the clean dataset into an sqlite database.**
engine = create_engine('sqlite:///InsertDatabaseName.db')
df.to_sql('InsertTableName', engine, index=False)

