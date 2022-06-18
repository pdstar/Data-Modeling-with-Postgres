# Data-Modeling-with-Postgres
Udacity project

# Data Modeling for Sparkify with Postgres

## Introduction: Sparkify and Their Analytical Goals

A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

This project creates a Postgres database with tables designed to optimize queries on song play analysis. 


## How to run the Python scripts
etl.py process the entire datasets. This is how we run it:
1. Open the Terminal Session
2. Run sql_queries.py in the terminal through the following command: python sql_queries.py. This script contains all sql queries, and is imported into create_tables.py and etl.py. It defines Fact and dimensional tables for the star schema. The queries specify  columns for each of the five tables with the right data types and conditions. 
3. Run create_tables.py in the terminal through the following command: python create_tables.py. The script connects to the Sparkify database, drops any tables if they exist, and creates the tables
4. Run test.ipynb to confirm the records were successfully inserted into each table.

## Files in the Repository

### Song Dataset - `song_data`
The dataset is a subset of real data from the [Million Song Dataset](http://millionsongdataset.com/)Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. 

### Log Dataset - `log_data`
The second dataset consists of log files in JSON format generated by [this event simulator](https://github.com/Interana/eventsim) based on the songs in the dataset above. These simulate activity logs from a music streaming app based on specified configurations. The log files in the dataset are partitioned by year and month

## Data Model 

### Database Schema Design 

#### Sparkify Song Play Star Schema

<img src="https://github.com/pdstar/Data-Modeling-with-Postgres/blob/main/Star_Schema.png" width="750" height="750">
<img src="data/Star_Schema.png" width="750" height="750">

This Star Schema optimized for queries on song play analysis. This includes the following tables.

#### Fact Table
1. songplays - records in log data associated with song plays i.e. records with page `NextSong`
songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

#### Dimension Tables
2. **users** - users in the app
    - *user_id, first_name, last_name, gender, level*

3. **songs** - songs in music database
    - *song_id, title, artist_id, year, duration*

4. **artists** - artists in music database
    - *artist_id, name, location, latitude, longitude*

5. **time** - timestamps of records in **songplays** broken down into specific units
    - *start_time, hour, day, week, month, year, weekday*

### Why Use Star Schema
With Star Schema there is no need for complex joins when querying data. This makes it very easy to use for business and BI teams. And as a results, queries also run faster as there are no elaborate joins. 

Its is also easy to understand once built and hence any modification is also simple

### ETL Pipeline
1. Perform ETL on `song_data`, to create the `songs` and `artists` dimensional tables
2. Perform ETL on the second dataset, `log_data`, to create the `time` and `users` dimensional tables, as well as the `songplays` fact table.
