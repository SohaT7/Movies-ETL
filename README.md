# Movies ETL
## Table of Contents
- [Overview of the Analysis](#overview-of-the-analysis)
    - [Purpose](#purpose)
    - [About the Dataset](#about-the-dataset)
    - [Tools Used](#tools-used)
    - [Description](#description)
- [Results](#results)
    - [Reading in the three files](#Reading-in-the-three-files)
    - [Cleaning the Wikipedia Data](#Cleaning-the-Wikipedia-Data)
    - [Creating the Movie Database](#Creating-the-Movie-Database)
- [Summary](#summary)
- [Contact Information](#contact-information)

## Overview of the Analysis
### Purpose:
The purpose of this project is to create an automated ETL (Extract, Transform, Load) pipeline that takes data on movies (from multiple sources) as an input, transforms it, and then loads it into a relational (SQL) database. 

### About the Dataset:
The dataset comprises of the following three data files:
 - Wikipedia dataset: an already extracted JSON file
 - Kaggle dataset: movies_metadata.csv and ratings.csv

The Wikipedia dataset on movies contains information on movies from 1990-2018. The Kaggle dataset pulls from [the MovieLens dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset?resource=download), which contains about 26 million reviews on over 45,000 movies. The Kaggle dataset is a metadata file and contains information about movies from [The Movie Database (TMDb)](https://www.themoviedb.org). The two files used from the list of data files available in the Kaggle dataset are the movies_metadata.csv and ratings.csv.

### Tools Used:
 - Python (Pandas library)
 - PostgreSQL
 - SQL

### Description:
The three data files are read into three separate DataFrames. The Wikipedia data is then merged with the Kaggle metadata. 


## Results
### Reading in the three files:
The three data files are read into a function as DataFrames: wiki_movies_raw, kaggle_metadata, and ratings.

![wiki_movies_df](https://github.com/SohaT7/Movies-ETL/blob/main/Images/wiki_movies_df.png)

![kaggle_metadata](https://github.com/SohaT7/Movies-ETL/blob/main/Images/kaggle_metadata.png)

![ratings](https://github.com/SohaT7/Movies-ETL/blob/main/Images/ratings.png)

### Cleaning the Wikipedia Data:
The file [ETL_clean_wiki_movies](https://github.com/SohaT7/Movies-ETL/blob/main/ETL_clean_wiki_movies.ipynb) shows how the data in the wiki_movies_raw DataFrame has all TV shows filtered out, and is then cleaned by passing it through the clean_movie() function. The resultant data is added into a new DataFrame wiki_movies_df.

Next, IMDb IDs are extracted by using a regular expression string and dropping duplicates. A try-except block deals with any errors that pop up during this process. The box office, budget, release date, and running time columns are cleaned. A parse_dollars() function is written to ensure all values for money in the file have the same format, money unit, and column datatype. 

![wiki_movies_df](https://github.com/SohaT7/Movies-ETL/blob/main/Images/wiki_movies_df.png)

### Cleaning the Kaggle Data:
The file [ETL_clean_kaggle_data](https://github.com/SohaT7/Movies-ETL/blob/main/ETL_clean_kaggle_data.ipynb) shows how the Kaggle data in the kaggle_metadata DataFrame is cleaned. 

![kaggle_metadata](https://github.com/SohaT7/Movies-ETL/blob/main/Images/kaggle_metadata.png)

The Wikipedia data and Kaggle metadata are then merged by merging the DataFrames kaggle_metadata and wiki_movies_df, into a new DataFrame movies_df, which is then cleaned.

The DataFrames ratings_df and movies_df are then merged into a new DataFrame movies_with_ratings_df and cleaned. 

### Creating the Movie Database:
The file [ETL_create_database](https://github.com/SohaT7/Movies-ETL/blob/main/ETL_create_database.ipynb) contains all the previous code as well as code for creating the database.

The movies_df DataFrame and the MovieLens rating CSV data is added to a SQL database, using PostgreSQL. 

## Summary
The project created an ETL pipeline that extracts data from three data files (Wikipedia JSON data file, Kaggle metadata file, and MovieLens ratings CSV file), transforms it, and then load it into a SQL movies database.

## Contact Information
Email: st.sohatariq@gmail.com
