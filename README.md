# Movies ETL
## Table of Contents
- [Overview of the Analysis](#overview-of-the-analysis)
    - [Purpose](#purpose)
    - [About the Dataset](#about-the-dataset)
    - [Tools Used](#tools-used)
    - [Description](#description)
- [Results](#results)
    - [Extraction and Transformation of the Wikipedia Data](#Extraction-and-Transformation-of-the-Wikipedia-Data)
    - [Extraction and Transformation of the Kaggle Metadata](#Extraction-and-Transformation-of-the-Kaggle-Metadata)
    - [Extraction and Transformation of the MovieLens Ratings Data](#Extraction-and-Transformation-of-the-MovieLens-Ratings-Data)
    - [Loading the Data into the Movie Database](#Loading-the-Data-into-the-Movie-Database)
    - [Running Queries](#Running-Queries)
- [Summary](#summary)
- [Contact Information](#contact-information)

## Overview of the Analysis
### Purpose:
The purpose of this project is to create an automated ETL (Extract, Transform, Load) pipeline that takes in data on movies and movie ratings from two sources (and three different files), transforms that data, and then loads it into a SQL movies database. 

### About the Dataset:
The dataset comprises of the following three data files:
 - Wikipedia dataset: an already extracted JSON file
 - Kaggle dataset: movies_metadata.csv and ratings.csv

The Wikipedia dataset on movies contains information on movies from 1990-2018 in the file [wikipedia-movies-raw](https://github.com/SohaT7/Movies-ETL/blob/main/Resources/wikipedia-movies-raw.json). The Kaggle dataset pulls from [the MovieLens dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset?resource=download), which contains about 26 million reviews on over 45,000 movies. The Kaggle dataset is a metadata file and contains information about movies from [The Movie Database (TMDb)](https://www.themoviedb.org). The two files used from the list of data files available in the Kaggle dataset are the movies_metadata.csv and ratings.csv.

### Tools Used:
 - Python (Pandas library)
 - PostgreSQL
 - SQL
 - pgAdmin

### Description:
The three data files - Wikipedia data, Kaggle movies metadata, and MovieLens ratings data - are read into three separate DataFrames. A function (named extract_transform_load()) is written to extract and transform the three sets of data. The transformed Wikipedia data is merged with the transformed Kaggle metadata, to produce a new DataFrame for movies. The movies DataFrame is then merged with the ratings DataFrame to create a new DataFrame for movies in conjunction with the respective ratings they received. Finally, the movies and the ratings DataFrames are imported into the PostgreSQL database 'movie_data'.

[ETL_create_database](https://github.com/SohaT7/Movies-ETL/blob/main/ETL_create_database.ipynb) is the file that contains the entire code for this project. However, if the reader would like to view the code written as a process, they can view the following files in the order given below:
 - [ETL_clean_wiki_movies](https://github.com/SohaT7/Movies-ETL/blob/main/ETL_clean_wiki_movies.ipynb) - contains code that cleans the Wikipedia data only
 - [ETL_clean_kaggle_data](https://github.com/SohaT7/Movies-ETL/blob/main/ETL_clean_kaggle_data.ipynb) - contains code that cleans the Kaggle data, and creates the movies_df and movies_with_ratings_df DataFrames
 - [ETL_create_database](https://github.com/SohaT7/Movies-ETL/blob/main/ETL_create_database.ipynb) - contains code that connects with the movies database created in PostgreSQL

A query run on the tables in the PostgreSQL database returns the number of rows for [the movies](https://github.com/SohaT7/Movies-ETL/blob/main/Images/movies_query.png) and [the ratings](https://github.com/SohaT7/Movies-ETL/blob/main/Images/ratings_query.png) tables.

At the end, the wiki_movies_df, movies_with_ratings_df, and the movies_df are then stored back into their respective files: [wikipedia_movies.json](https://github.com/SohaT7/Movies-ETL/blob/main/Resources/wikipedia_movies.json), [movies_metadata.csv](https://github.com/SohaT7/Movies-ETL/blob/main/Resources/movies_metadata.csv), and ratings.csv.

## Results
### Extraction and Transformation of the Wikipedia Data:
A function (named as clean_movie) is written which takes in the argument 'movie' and cleans it. The main function (named as extract_transform_load()) is written which takes in the three data files as arguments and passes them into three DataFrames: wiki_movies_raw, kaggle_metadata, and ratings. The Wikipedia data (in the wiki_movies_raw DataFrame) is then cleaned by filtering out TV shows and passing the data through the clean_movie() function created earlier. This cleaned data is stored in the wiki_movies_df DataFrame, and then transformed as such:
 -  IMDb IDs are extracted by writing a 'regex' (regular expression) and dropping the duplicate IDs. A try-except block catches any errors during this process.
 - Columns with atleast 90% non-null values are kept.
 - The columns 'Box office', 'Budget', 'Release date', and 'Running time' are cleaned and transformed as need be.

 The resultant cleaned and transformed Wikipedia data is can be seen in the wiki_movies_df DataFrame below:

![wiki_movies_df](https://github.com/SohaT7/Movies-ETL/blob/main/Images/wiki_movies_df.png)

### Extraction and Transformation of the Kaggle Metadata:
The Kaggle movies metadata is cleaned and transformed as such:
 - 'Adult' movies are dropped.
 - Only the 'video' type is kept.
 - The values in the columns 'budget', 'id', and 'popularity' are converted to the correct datatype.
 
 The two DataFrames wiki_movies_df and kaggle_metadata are then merged into a new DataFrame movies_df. Unnecessary columns () are dropped from it. Missing values are filled in by passing in the DataFrame, Kaggle column, and the Wiki column into the fill_missing_kaggle_data() function. The DataFrame movies_df is then filtered to keep only specific columns and these are then renamed for consistency purposes. 

The resultant movies DataFrame can be seen below:

![movies_df](https://github.com/SohaT7/Movies-ETL/blob/main/Images/movies_df.png)

### Extraction and Transformation of the MovieLens Ratings Data:
The ratings DataFrame is cleaned and transformed as such:
 - Ratings counts are cleaned.
 - The movies_df and the cleaned ratings DataFrame are merged into a new movies_with_ratings_df DataFrame, containing movies and their respective ratings.
 - The empty values in the movies_with_ratings_df DataFrame are filled in with a "0".

 The movies_with_ratings_df DataFrame is shown below:

 ![movies_with_ratings_df](https://github.com/SohaT7/Movies-ETL/blob/main/Images/movies_with_ratings_df.png)

### Loading the Data into the Movie Database:
A connection is created to the PostgreSQL database 'movie_data'. The movies_df and the ratings DataFrames are then imported into this database.

### Running Queries:
A query run on the 'movies' and 'ratings' tables in the 'movie_data' database show that there are a total of 6,075 records in the movies table and a total of 24,289 records in the ratings table.

![movies_query](https://github.com/SohaT7/Movies-ETL/blob/main/Images/movies_query.png)

![ratings_query](https://github.com/SohaT7/Movies-ETL/blob/main/Images/ratings_query.png)

## Summary
This project successfully creats an automated ETL (Extract, Transform, Load) pipeline which takes in three data files (Wikipedia JSON data file, Kaggle metadata file, and MovieLens ratings CSV file), transforms the data therein, and then loads it into a SQL movies database.

## Contact Information
Email: st.sohatariq@gmail.com
