
### Join Files
1. **Find Common Columns**
   - Identify common columns between datasets to join them effectively.
2. **Check Column Quality**
   - Ensure columns used for joins do not contain NULL values.
3. **Join Files Using OpenRowSet Instead of Table**
   - Use OpenRowSet for joining files directly.
4. **Aggregation**
   - Perform aggregation operations like SUM, COUNT, etc.
5. **Visualization**
   - Use tools like Power BI for data visualization.
  
````sql
USE nyc_taxi_discovery;

-- Identify number of trips made from each borough! 

SELECT
    TOP 100 *
FROM
    OPENROWSET(
        BULK 'trip_data_green_parquet/year=2020/month=01/',
        FORMAT = 'PARQUET',
        DATA_SOURCE = 'nyc_taxi_data_raw'
    ) AS [result]
WHERE PULocationID IS NULL

SELECT taxi_zone.borough, COUNT(1) AS number_of_trips
  FROM OPENROWSET(
                    BULK 'trip_data_green_parquet/year=2020/month=01/',
                    FORMAT = 'PARQUET',
                    DATA_SOURCE = 'nyc_taxi_data_raw'
                ) AS trip_data
        JOIN
        OPENROWSET(
                    BULK 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv',
                    FORMAT = 'CSV',
                    PARSER_VERSION = '2.0',
                    FIRSTROW = 2
                ) 
                WITH (
                    location_id SMALLINT 1,
                    borough VARCHAR(15) 2,
                    zone VARCHAR(50) 3,
                    service_zone VARCHAR(15) 4
                )AS taxi_zone
        ON trip_data.PULocationID = taxi_zone.location_id
GROUP BY taxi_zone.borough
ORDER BY number_of_trips;

````
### Transformations
1. **Data Formatting Using Cast**
   - Format data types using CAST.
2. **Combine Columns**
   - Concatenate multiple columns into one.
3. **Derived Columns**
   - Create new columns based on existing data.
4. **Aggregation Using Count and Sum**
5. **Handle Null Values Using ISNULL and Left Join**
   - Replace NULL values and use LEFT JOIN.
6. **Proper Use of Decimal and Division Expressions**
   - Ensure proper handling of decimal values and division.
7. **Use of CTE for Clarity and Simplification**
   - Use Common Table Expressions (CTE) for better readability.

````sql
-- Number of trips made by duration in hours

SELECT 
    DATEDIFF(minute, lpep_pickup_datetime, lpep_dropoff_datetime) / 60 AS from_hour,
    (DATEDIFF(minute, lpep_pickup_datetime, lpep_dropoff_datetime) / 60) + 1 AS to_hour,
    COUNT(1) AS number_of_trips
FROM
    OPENROWSET(
        BULK 'trip_data_green_parquet/**',
        FORMAT = 'PARQUET',
        DATA_SOURCE = 'nyc_taxi_data_raw'
    ) AS trip_data
GROUP BY DATEDIFF(minute, lpep_pickup_datetime, lpep_dropoff_datetime) / 60,
(DATEDIFF(minute, lpep_pickup_datetime, lpep_dropoff_datetime) / 60) + 1
ORDER BY from_hour, to_hour;
