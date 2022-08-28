# Movies ETL
## Overview of the Analysis
### Purpose:
The purpose of this project is to create an automated pipeline that takes data as an input, transforms it according to the defined needs, and then loads it into exisitng tables. 

### Description:
An ETL (Extract, Transform, Load) pipeline is created by taking in raw data from three data files, extracting and transforming it, in order to load it into a SQL database, using PostgreSQL.

### About the Dataset:
The dataset comprises of three data files on movies:

Wikipedia JSON data

Kaggle metadata

MovieLens rating data

### Tools Used:
Python (Pandas)

PostgreSQL

SQL

## Results
An ETL function is written to read in the three data files into three DataFrames: wiki_movies_df, kaggle_metadata, and ratings.

The Wikipedia data then undergoes the processes of extraction and transformation. All TV shows are filtered out, extracted, and the cleaned data type was then converted to a DataFrame. 

Next, the Kaggle metadata is cleaned and converted into a DataFrame. This is followed by the Wikipedia movies DataFrame and the Kaggle metadata DataFrame being merged into a 'movies' DataFrame.

Finally, the MovieLens rating data DataFrame is merged with the cleaned 'movies' DataFrame to produce the 'movies with ratings' DataFrame. 

Finally, the 'movies' DataFrame and the MovieLens rating CSV data are added to a SQL database on movies.  

## Summary
The project created an ETL pipeline that takes in data from three data files (Wikipedia JSON data file, Kaggle metadata file, and MovieLens ratings CSV file), extracts and transforms it, to then load it into a SQL movies database.

## Contact Information
Email: st.sohatariq@gmail.com
