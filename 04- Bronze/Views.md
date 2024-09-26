## Create Views for:

1- JSON Files

2- Partition file for Partition Purning purposes

![image](https://github.com/user-attachments/assets/6a338b5a-1583-47fb-9135-f5d5feedb2c9)

![image](https://github.com/user-attachments/assets/9fdadb4c-3cb2-46be-8ae0-96530aace251)

````sql

DROP VIEW IF EXISTS BRONZE.VW_Table
GO

CREATE VIEW BRONZE.VW_Table
AS
SELECT payment_type, description
FROM OPENROWSET(
    BULK 'raw/payment_type.json',
    DATA_SOURCE = 'nyc_taxi_src',
    FORMAT = 'CSV',
    FIELDTERMINATOR = '0x0b',
    FIELDQUOTE = '0x0b'
) WITH
( JSONDOC NVARCHAR(MAX)) AS payment_type
CROSS APPLY OPENJSON(jsonDoc)
WITH (
    payment_type SMALLINT,
    description VARCHAR(20) '$.payment_type_desc'
);
-------------------------------------------------------------------
DROP VIEW IF EXISTS BRONZE.VW_Table_CSV_Partition
GO
CREATE VIEW BRONZE.VW_Table_CSV_Partition
AS
SELECT
    result.filepath(1) AS year,
    result.filepath(2) AS month,
    result.*
FROM
    OPENROWSET(
        BULK 'raw/trip_data_green_csv/year=*/month=*/ *.csv',
        DATA_SOURCE = 'nyc_taxi_src',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE
    ) 
	WITH(
    VendorID INT,
    lpep_pickup_datetime datetime2(7),
    lpep_dropoff_datetime datetime2(7),
    store_and_fwd_flag CHAR(1),
    RatecodeID INT,
    PULocationID INT,
    DOLocationID INT,
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
