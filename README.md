# BigQuery-Analysis
NYC Taxi Trips Analysis with Google BigQuery 🚕
An in-depth analysis of a 1TB+ public dataset to uncover ride patterns and peak demand hours using advanced SQL in Google BigQuery.

🎯 Project Goal
The objective of this project was to demonstrate proficiency in handling large-scale data, writing advanced SQL queries, and performing foundational ETL (Extract, Transform, Load) processes. By analyzing the NYC Taxi Trips dataset, I aimed to extract actionable insights about taxi usage patterns in New York City.

🛠️ Tech Stack & Tools
Category

Technology / Tool

Platform

Google Cloud Platform (GCP)

Data Warehouse

Google BigQuery

Query Language

SQL (including CTEs and Window Functions)

Visualization

Google Sheets

⚙️ Analysis Workflow
The project followed a structured data analysis process:

** exploratory Data Analysis (EDA):**

Investigated the table schema to understand data types and column content.

Ran initial COUNT, MIN, and MAX queries to grasp the scale and range of the data.

🧹 Data Cleaning & Transformation:

Used WHERE clauses to filter out invalid records, such as trips with zero fare amount or negative passenger counts.

Employed CASE statements to handle categorical data and create new, more descriptive columns.

📊 Aggregation & Insight Generation:

Wrote advanced SQL queries with GROUP BY and window functions to aggregate data and identify trends.

Focused on key metrics like trip frequency by hour, day, and location.

🎨 Visualization:

Exported the final, aggregated data from BigQuery.

Connected the data to Google Sheets to create clear and simple charts illustrating the key findings.

🚀 Sample Advanced SQL Queries
<details>
<summary><strong>Click to Expand: Query 1 - Identify Peak Pickup Hours by Day</strong></summary>

/*
This query uses a Common Table Expression (CTE) to first extract the
hour and day of the week, then counts trips to find the busiest times.
*/
WITH TripTimes AS (
  SELECT
    EXTRACT(DAYOFWEEK FROM pickup_datetime) AS day_of_week_num,
    EXTRACT(HOUR FROM pickup_datetime) AS pickup_hour
  FROM
    `bigquery-public-data.new_york_taxi_trips.tlc_green_trips_2021`
  WHERE
    fare_amount > 0 AND passenger_count > 0
)
SELECT
  CASE
    WHEN day_of_week_num = 1 THEN 'Sunday'
    WHEN day_of_week_num = 2 THEN 'Monday'
    WHEN day_of_week_num = 3 THEN 'Tuesday'
    WHEN day_of_week_num = 4 THEN 'Wednesday'
    WHEN day_of_week_num = 5 THEN 'Thursday'
    WHEN day_of_week_num = 6 THEN 'Friday'
    WHEN day_of_week_num = 7 THEN 'Saturday'
  END AS day_of_week,
  pickup_hour,
  COUNT(*) AS trip_count
FROM TripTimes
GROUP BY day_of_week, pickup_hour
ORDER BY trip_count DESC
LIMIT 10;

</details>

<details>
<summary><strong>Click to Expand: Query 2 - Top 5 Busiest Routes</strong></summary>

/*
This query identifies the most popular trip routes by counting
the occurrences of each pickup-to-dropoff location pair.
*/
SELECT
    PULocationID,
    DOLocationID,
    COUNT(*) AS route_count
FROM
    `bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2021`
WHERE
    PULocationID IS NOT NULL
    AND DOLocationID IS NOT NULL
GROUP BY
    PULocationID,
    DOLocationID
ORDER BY
    route_count DESC
LIMIT 5;

</details>

📈 Key Findings & Visualizations
The analysis produced several key insights into taxi usage patterns.

1. Peak Demand Occurs on Weekend Evenings
The data clearly shows that the highest demand for taxis occurs on Friday and Saturday evenings, specifically between 6 PM and 10 PM.

Replace this image with a screenshot of your chart from Google Sheets.

2. Most Trips are Short-Distance within Manhattan
The most frequent routes were identified as short trips within Manhattan's core business and entertainment districts, indicating high usage for local travel.

Replace this image with a screenshot of your table from Google Sheets.

Project by: Md Faizan Raza
