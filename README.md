# Spotify Advanced SQL Project and Query Optimization 
Project Category: Advanced
[Click Here to get Dataset](https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset)

**Project Overview**
This project delves into a Spotify dataset to answer a series of business questions using SQL queries. It showcases skills in data manipulation, aggregation, and performance optimization within a relational database context.

 **Features:**

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

**Technologies Used:-**

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
```

**Setup and Usage:-**

To replicate this project:

**1.Database Setup:**

-Install PostgreSQL (or your preferred SQL database).

-Create a new database (e.g., spotify_analysis).

-Use the CREATE TABLE statement provided above to set up the spotify table.

**2.Data Import:**

-Obtain a Spotify-like dataset.

-Import the data into your spotify table. Be mindful of data type conversions and potential errors (e.g., handling quotes in text, ensuring correct integer/float parsing).

**3.Run Queries:**

-Execute the SQL queries provided in the sql_queries/ directory (or directly from this README) in your database management tool (e.g., PG Admin 4).

-Observe the results and the insights derived.

**SQL Queries & Analysis:-**

**1.Retrieve the names of all tracks that have more than 1 billion streams.**
```
SELECT* FROM spotify
WHERE stream>1000000000;
```
**2.List all albums along with their respective artists.**
```
SELECT
	DISTINCT album,artist
FROM spotify
ORDER BY 1;
```
**3.Get the total number of comments for tracks where licensed = TRUE.**
```
SELECT 
	SUM(comments) AS total_number_of_comments
FROM spotify
WHERE licensed = 'TRUE';
```
**4.Find all tracks that belong to the album type single.**
```
SELECT*
	FROM spotify
WHERE album_type = 'single';
```
**5.Count the total number of tracks by each artist.**
```
SELECT
	artist, --1
	COUNT(*)AS total_no_songs	--2
FROM spotify
GROUP BY artist
ORDER BY 2;
```
**6.Calculate the average danceability of tracks in each album.**
```
SELECT
	album,
	avg(danceability)AS avg_danceability
FROM spotify
GROUP BY album
ORDER BY 2 DESC;
```
**7.Find the top 5 tracks with the highest energy values.**
```
SELECT
	track,
	AVG(energy)
FROM spotify
GROUP BY track
ORDER BY 2 DESC
LIMIT 5;
```
**8.List all tracks along with their views and likes where official_video = TRUE.**
```
SELECT
	 track,
	 SUM(views)AS total_views,
	 SUM(likes)AS total_likes
FROM spotify
WHERE official_video = 'TRUE'
GROUP BY track
ORDER BY total_views DESC;
```
**9.For each album, calculate the total views of all associated tracks.**
```
SELECT
	album,
	track,
	SUM(views) AS total_views 
FROM spotify
GROUP BY album,track
ORDER BY total_views DESC;
```
**10.Retrieve the track names that have been streamed on Spotify more than YouTube.**
```
SELECT * FROM
(SELECT
	track,
	--most_played_on,
	COALESCE(SUM( CASE WHEN most_played_on = 'Youtube' THEN stream END) as streamed_on_youtube,
	COALESCE(SUM( CASE WHEN most_played_on = 'Spotify' THEN stream END) as streamed_on_Spotify
FROM spotify
GROUP BY 1
) AS t1
WHERE 
	streamed_on_spotify> streamed_on_youtube
	AND
	streamed_on_youtube<>0;
```
**11.Find the top 3 most-viewed tracks for each artist using window functions.**
```
WITH ranking_artist
AS
(SELECT
	artist,
	track,
	SUM(views)AS total_view,
	DENSE_RANK() OVER(PARTITION BY artist ORDER BY SUM(views)DESC)as rank
FROM spotify
GROUP BY 1,2
ORDER BY 1,3 DESC
)
SELECT* FROM ranking_artist
WHERE rank<=3;
```
**12.Write a query to find tracks where the liveness score is above the average.**
```
SELECT
	track,
	artist,
	liveness
FROM spotify
WHERE liveness > (SELECT AVG (liveness)FROM spotify);
```
**Q 13 Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.**
```
WITH cte
AS
(SELECT
	album,
	MAX(energy) AS highest_energy,
	MIN(energy) AS lowest_energy
FROM spotify
GROUP BY 1
)
SELECT
	album,
	highest_energy - lowest_energy as energy_diff
FROM cte
ORDER BY 2 desc;
```

**Query Optimization**

**Original Query Performance Analysis:**
```
EXPLAIN ANALYZE
SELECT artist, track, views
FROM spotify
WHERE artist ='Gorillaz' AND most_played_on = 'Youtube'
ORDER BY stream DESC LIMIT 25;
```
(Observe the execution and planning times, and the query plan details.)

**Index Creation:**
```
CREATE INDEX artist_index ON spotify(artist);
```
(Run EXPLAIN ANALYZE on the query again after creating the index to observe the significant performance improvement.)

**Business Impact**

The project showcases how structured SQL queries can lead to actionable business insights:

**Content Strategy:** Identifying top-performing tracks, artists, and albums (Q.1, Q.5, Q.7, Q.11) helps content creators and platforms understand what resonates with their audience.

**Platform Performance:** Comparing stream counts across platforms (Q.10) can inform distribution strategies.

**Audience Engagement:** Analyzing comments and likes on licensed content (Q.3, Q.8) provides feedback on audience interaction.

**Data Quality & Reliability:** The initial cleaning steps and the focus on data integrity are crucial for ensuring that insights are based on accurate information.

**Operational Efficiency:** Query optimization techniques (like indexing) are vital for managing large datasets, reducing query execution times, and improving the responsiveness of data-driven applications. A demonstrated 700% improvement in query speed for specific artist lookups highlights this impact.








