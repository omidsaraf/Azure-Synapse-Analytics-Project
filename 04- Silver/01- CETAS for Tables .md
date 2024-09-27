

```sql

USE NYD_TAXI_DW;

-- 01- TABLE TAXI_ZONE (CSV --> Parquet)
IF OBJECT_ID('SILVER.TAXI_ZONE') IS NOT NULL
    DROP EXTERNAL TABLE SILVER.TAXI_ZONE; -- only metadata will be removed, not data
GO

CREATE EXTERNAL TABLE SILVER.TAXI_ZONE
WITH (
    DATA_SOURCE = NYD_TAXI_SRC,
    LOCATION = 'silver/taxi_zone',
    FILE_FORMAT = parquet_file_format
) AS
SELECT *
FROM BRONZE.TAXI_ZONE;

-- 02- TABLE Calendar (CSV --> Parquet)
IF OBJECT_ID('SILVER.Calendar') IS NOT NULL
    DROP EXTERNAL TABLE SILVER.Calendar; -- only metadata will be removed, not data
GO

CREATE EXTERNAL TABLE SILVER.Calendar
WITH (
    DATA_SOURCE = NYD_TAXI_SRC,
    LOCATION = 'silver/Calendar',
    FILE_FORMAT = parquet_file_format
) AS
SELECT *
FROM BRONZE.Calendar;

-- 03- TABLE TAXI_Vendor (CSV --> Parquet)
IF OBJECT_ID('SILVER.Vendor') IS NOT NULL
    DROP EXTERNAL TABLE SILVER.Vendor; -- only metadata will be removed, not data
GO

CREATE EXTERNAL TABLE SILVER.Vendor
WITH (
    DATA_SOURCE = NYD_TAXI_SRC,
    LOCATION = 'silver/Vendor',
    FILE_FORMAT = parquet_file_format
) AS
SELECT *
FROM BRONZE.Vendor;

-- 04- TABLE Trip_Type (CSV --> Parquet)
IF OBJECT_ID('SILVER.Trip_Type) IS NOT NULL
   DROP EXTERNAL TABLE SILVER.Trip_Type --- not enough
GO
CREATE EXTERNAL TABLE SILVER.Trip_Type
WITH
(
    DATA_SOURCE = NYD_TAXI_SRC,
    LOCATION = 'silver/Trip_Type',
    FILE_FORMAT = Parquet_file_format
)AS SELECT 
FROM BRONZE.Trip_Type



```

This code creates external tables in the `SILVER` schema from the `BRONZE` schema, converting CSV data to Parquet format. If the external tables already exist, they are dropped first (only the metadata is removed, not the actual data).
