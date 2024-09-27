
```sql
DROP VIEW IF EXISTS SILVER.VW_Trip_Data_Parquet
GO
CREATE VIEW SILVER.VW_Trip_Data_Parquet
AS
SELECT
    result.filepath(1) AS year,
    result.filepath(2) AS month,
    result.*
FROM
    OPENROWSET(
        BULK 'silver/trip_data/year=*/month=*/*.parquet',
        DATA_SOURCE = 'NYD_TAXI_SRC2',
        FORMAT = 'PARQUET'
     ) 
WITH(
    vendor_id INT,
    lpep_pickup_datetime datetime2(7),
    lpep_dropoff_datetime datetime2(7),
    store_and_fwd_flag CHAR(1),
    rate_code_ID INT,
    pu_location_id INT,
    do_location_id INT,
    passenger_count INT,
    trip_distance FLOAT,
    fare_amount FLOAT,
    extra FLOAT,
    mta_tax FLOAT,
    tip_amount FLOAT,
    tolls_amount FLOAT,
    ehail_fee INT,
    improvement_surcharge FLOAT,
    total_amount FLOAT,
    payment_type INT,
    trip_type INT,
    congestion_surcharge FLOAT
	)
	AS [result];
go
SELECT * FROM
SILVER.VW_Trip_Data_Parquet




