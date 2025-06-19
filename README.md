# Spotify Advanced SQL Project and Query Optimization 
Project Category: Advanced
[Click Here to get Dataset](https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset)

**Project Overview**
This project delves into a Spotify dataset to answer a series of business questions using SQL queries. It showcases skills in data manipulation, aggregation, and performance optimization within a relational database context.

 **Features**
**Data Ingestion & Cleaning:** Demonstrates initial data loading and handling of common data inconsistencies (e.g., incorrect data types, illogical values).
**Exploratory Data Analysis (EDA):** Uses SQL queries to understand the dataset's structure, size, and key characteristics.
**Business Problem Solving:** Addresses over 15 distinct business questions categorized by difficulty (Easy, Medium, Advanced).
**Advanced SQL Concepts:** Implements various SQL features including:
-GROUP BY and HAVING
-Subqueries
-Window Functions (DENSE_RANK())
-Conditional Aggregation (CASE WHEN within SUM())
-Common Table Expressions (CTEs)
**Query Optimization:** Explores techniques to improve query performance using EXPLAIN ANALYZE and CREATE INDEX commands.

**Technologies Used**
**SQL:** The primary language for all data analysis and manipulation.
**Database:** PostgreSQL.
**Microsoft Excel:** Used for initial data cleaning and preparation.

**Dataset**
The project utilizes a Spotify dataset containing information about tracks, artists, albums, and various audio features (danceability, energy, loudness, etc.), along with engagement metrics like views, likes, comments, and stream counts (potentially across Spotify and YouTube).

**Schema:**
```sql
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
```sql



## How to Run the Project
1. Install PostgreSQL and pgAdmin (if not already installed).
2. Set up the database schema and tables using the provided normalization structure.
3. Insert the sample data into the respective tables.
4. Execute SQL queries to solve the listed problems.
5. Explore query optimization techniques for large datasets.

---





