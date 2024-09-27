````sql
CREATE SCHEMA staging;
GO
IF NOT EXISTS (SELECT * FROM sys.external_file_formats WHERE name = 'parquet_file_format')
CREATE EXTERNAL FILE FORMAT parquet_file_format
WITH (
    FORMAT_TYPE = PARQUET
);
GO
IF NOT EXISTS (SELECT * FROM sys.external_data_sources WHERE name = 'nyc_taxi_data_src')
CREATE EXTERNAL DATA SOURCE nyc_taxi_data_src
WITH (
    LOCATION = 'abfss://nyc-taxi-data@nyctaxidata.dfs.core.windows.net'
);
GO
CREATE EXTERNAL TABLE staging.ext_trip_data_green_agg
(
    [pickup_datetime] datetime,
    [dropoff_datetime] datetime,
    [total_trip_count] bigint,
    [do_location_id] int,
    [pu_location_id] float,
    fare_amount float
)
WITH (
    LOCATION = 'nyc_taxi_data_green_agg/',
    DATA_SOURCE = nyc_taxi_data_src,
    FILE_FORMAT = parquet_file_format
);
GO

SELECT TOP 100 * FROM staging.ext_trip_data_green_agg;
![image](https://github.com/user-attachments/assets/58d49503-9357-4342-baa4-a4c6285ec5d4)
