- One of the main things that we need to consider is to find those tables mostly keeping Transactional and huge number of records. in our case, Trip Data has been stored by year and month so it is mostly required to be considered.
- Path File: Year/Month/Trip Data.CSV
- Total Amount of Records
- We need to ensure that the number of records we have matches our expectations. If not, we should:
  -Approach the data supplier.
  -Use a different data source.
- Check Min, Max, and Avg for the entire dataset using the address ending with **.
- Check for Null values. The number of records based on the primary key and the `Total_Amount` column (which belongs to the data) should be the same; otherwise, there are Null values.

![image](https://github.com/user-attachments/assets/e66c8cf4-7d9c-4a57-ade7-e60dc96a4f99)
![image](https://github.com/user-attachments/assets/f2695c07-49de-44a9-8fa6-8747546f210c)

Find Negative Values
![image](https://github.com/user-attachments/assets/5d32c1da-ba2f-4168-9386-3ae24f6bf7a9)
![image](https://github.com/user-attachments/assets/5309c559-a8f7-4a2a-a05a-3aee275f5a96)

Find Null Values
![image](https://github.com/user-attachments/assets/abcafd8c-c04d-4ce8-8787-726086b58c5a)



## Check for duplicates in the Taxi Zone data

````sql
SELECT
    location_id,
    COUNT(1) AS number_of_records
FROM
    OPENROWSET(
        BULK 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        FIRSTROW = 2,
        FIELDTERMINATOR = ',',
        ROWTERMINATOR = '\n'
    ) 
    WITH (
        location_id SMALLINT 1,
        borough VARCHAR(15) 2,
        zone VARCHAR(50) 3,
        service_zone VARCHAR(15) 4
    )AS [result]
GROUP BY location_id
HAVING COUNT(1) > 1;


SELECT
    borough,
    COUNT(1) AS number_of_records
FROM
    OPENROWSET(
        BULK 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        FIRSTROW = 2,
        FIELDTERMINATOR = ',',
        ROWTERMINATOR = '\n'
    ) 
    WITH (
        location_id SMALLINT 1,
        borough VARCHAR(15) 2,
        zone VARCHAR(50) 3,
        service_zone VARCHAR(15) 4
    )AS [result]
GROUP BY borough
HAVING COUNT(1) > 1;

`````

# Identify any data quality issues in trip total amount
`````sql

SELECT
    TOP 100 *
FROM
    OPENROWSET(
        BULK 'trip_data_green_parquet/year=2020/month=01/',
        FORMAT = 'PARQUET',
        DATA_SOURCE = 'nyc_taxi_data_raw'
    ) AS [result]    
  
SELECT
    MIN(total_amount) AS min_total_amount,
    MAX(total_amount) AS max_total_amount,
    AVG (total_amount) AS avg_total_amount,
    COUNT(1) AS total_number_of_records,
    COUNT(total_amount) AS not_null_total_number_of_records
FROM
    OPENROWSET(
        BULK 'trip_data_green_parquet/year=2020/month=01/',
        FORMAT = 'PARQUET',
        DATA_SOURCE = 'nyc_taxi_data_raw'
    ) AS [result]  

SELECT
    payment_type, COUNT(1) AS number_of_records
FROM
    OPENROWSET(
        BULK 'trip_data_green_parquet/year=2020/month=01/',
        FORMAT = 'PARQUET',
        DATA_SOURCE = 'nyc_taxi_data_raw'
    ) AS [result] 
-- WHERE total_amount < 0 
GROUP BY payment_type
ORDER BY payment_type;




