# BigQuery-Analysis
Public Dataset Analysis with Google BigQuery
Project by: Md Faizan Raza
1. Project Goal
The objective of this project was to analyze a large-scale public dataset (over 1TB) using Google BigQuery to demonstrate proficiency in advanced SQL, data extraction, and fundamental ETL (Extract, Transform, Load) principles. The chosen dataset for this analysis was the NYC Taxi & Limousine Commission Trips dataset, focusing on identifying ride patterns and peak demand hours.

2. Tools Used
Google BigQuery: For querying and processing the massive dataset.

SQL: The primary language used for data manipulation, aggregation, and analysis.

Google Sheets: For final visualization of the aggregated results.

3. Methodology
The analysis followed a structured four-step process:

Data Exploration: Initial queries were run to understand the schema, data types, and distribution of key columns like pickup_datetime, fare_amount, and passenger_count.

Data Cleaning & Transformation: SQL CASE statements and WHERE clauses were used to handle null values and filter out erroneous data (e.g., trips with zero fare amount or negative trip distance).

Aggregation & Analysis: Advanced SQL queries involving GROUP BY, PARTITION BY, and window functions (ROW_NUMBER(), LAG()) were written to aggregate data and calculate key metrics.

Data Extraction & Visualization: The final aggregated result set was exported from BigQuery and linked to Google Sheets to create clear, simple visualizations presenting the key findings.

4. Sample Advanced SQL Queries
Below are examples of the SQL queries written during the analysis phase.

Query 1: Identify Peak Pickup Hours by Day of the Week

This query uses a Common Table Expression (CTE) to extract the hour and day of the week, then counts the number of trips to find the busiest times.

WITH T AS (
  SELECT
    EXTRACT(DAYOFWEEK FROM pickup_datetime) AS day_of_week,
    EXTRACT(HOUR FROM pickup_datetime) AS pickup_hour
  FROM
    `bigquery-public-data.new_york_taxi_trips.tlc_green_trips_2021`
  WHERE
    fare_amount > 0
)
SELECT
  CASE
    WHEN day_of_week = 1 THEN 'Sunday'
    WHEN day_of_week = 2 THEN 'Monday'
    WHEN day_of_week = 3 THEN 'Tuesday'
    WHEN day_of_week = 4 THEN 'Wednesday'
    WHEN day_of_week = 5 THEN 'Thursday'
    WHEN day_of_week = 6 THEN 'Friday'
    WHEN day_of_week = 7 THEN 'Saturday'
  END AS day,
  pickup_hour,
  COUNT(*) AS trip_count
FROM T
GROUP BY day, pickup_hour
ORDER BY trip_count DESC
LIMIT 10;

Query 2: Top 5 Busiest Routes (Pickup to Dropoff)

This query identifies the most popular trip routes.

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


5. Key Findings & Visualizations
Peak Demand: The analysis revealed that peak taxi demand consistently occurs on Friday and Saturday evenings between 6 PM and 10 PM.

Fare Distribution: A histogram of fare_amount showed that the vast majority of trips cost between $5 and $15, with a long tail of higher-fare trips, likely to airports.

Busiest Routes: The most frequent trips were identified as short-distance travels within Manhattan's business and entertainment districts.

(These findings would be presented with charts created in Google Sheets, such as a bar chart for Peak Hours and a table for Busiest Routes).
