
### 01- Using JSON File with Fother Path , SP (Array Type):
![image](https://github.com/user-attachments/assets/a4de353a-ab5b-45a0-89b6-9d0eb260c80d)


### 02- Create SP for each Create External Table (Bronze Layer) all in one SQL notebook

````sql
-----------------------------
CREATE OR ALTER PROC SILVER.USP_CETAS_Calendar
AS
SET NOCOUNT ON
BEGIN
IF OBJECT_ID('SILVER.Calendar') IS NOT NULL
    DROP EXTERNAL TABLE SILVER.Calendar;
CREATE EXTERNAL TABLE SILVER.Calendar
WITH
(
    DATA_SOURCE = NYD_Taxi_SRC,
    LOCATION = 'silver/calendar',
    FILE_FORMAT = parquet_file_format
)
AS SELECT *
FROM BRONZE.Calendar;
END;
---------------------------------------
CREATE OR ALTER PROC SILVER.USP_CETAS_TAXI_ZONE
AS
SET NOCOUNT ON
BEGIN
IF OBJECT_ID('SILVER.Taxi_Zone') IS NOT NULL
    DROP EXTERNAL TABLE SILVER.Taxi_Zone;
CREATE EXTERNAL TABLE SILVER.Taxi_Zone
WITH
(
    DATA_SOURCE = NYD_Taxi_SRC,
    LOCATION = 'silver/taxi_zone',
    FILE_FORMAT = parquet_file_format
)
AS SELECT *
FROM BRONZE.Taxi_Zone;
END;
------------------------------------------
CREATE OR ALTER PROC SILVER.USP_CETAS_Trip_Type
AS
SET NOCOUNT ON
BEGIN
IF OBJECT_ID('SILVER.Trip_Type') IS NOT NULL
    DROP EXTERNAL TABLE SILVER.Trip_Type;
CREATE EXTERNAL TABLE SILVER.Trip_Type
WITH
(
    DATA_SOURCE = NYD_Taxi_SRC,
    LOCATION = 'silver/trip_type',
    FILE_FORMAT = parquet_file_format
)
AS SELECT *
FROM BRONZE.Trip_Type;
END;
----------------------------------------------
CREATE OR ALTER PROC SILVER.USP_CETAS_Vendor
AS
SET NOCOUNT ON
BEGIN
IF OBJECT_ID('SILVER.Vendor') IS NOT NULL
    DROP EXTERNAL TABLE SILVER.Vendor;
CREATE EXTERNAL TABLE SILVER.Vendor
WITH
(
    DATA_SOURCE = NYD_Taxi_SRC,
    LOCATION = 'silver/vendor',
    FILE_FORMAT = parquet_file_format
)
AS SELECT *
FROM BRONZE.Vendor;
END;
------------------------------------------
CREATE OR ALTER PROC SILVER.USP_CETAS_Rate_Code
AS
SET NOCOUNT ON
BEGIN
IF OBJECT_ID('SILVER.Rate_Code') IS NOT NULL
    DROP EXTERNAL TABLE SILVER.Rate_Code;
CREATE EXTERNAL TABLE SILVER.Rate_Code
WITH
(
    DATA_SOURCE = NYD_Taxi_SRC,
    LOCATION = 'silver/rate_code',
    FILE_FORMAT = parquet_file_format
)
AS 
SELECT rate_code_id, rate_code
FROM OPENROWSET (
    BULK 'raw/rate_code.json',
    DATA_SOURCE = 'nyc_taxi_src',
    FORMAT = 'CSV',
    FIELDTERMINATOR = '0x0b',
    FIELDQUOTE = '0x0b',
    ROWTERMINATOR = '0x0b'
) WITH
( JSONDOC NVARCHAR(MAX)) AS rate_code
CROSS APPLY OPENJSON (jsonDoc)
WITH (
    rate_code_id TINYINT,
    rate_code VARCHAR(20));
--SELECT *  FROM BRONZE.VW_Rate_Code; ----> or using View
END;
---------------------------------------------------
CREATE OR ALTER PROC SILVER.USP_CETAS_Payment_Type
AS
SET NOCOUNT ON
BEGIN
IF OBJECT_ID('SILVER.Payment_Type') IS NOT NULL
    DROP EXTERNAL TABLE SILVER.Payment_Type;
CREATE EXTERNAL TABLE SILVER.Payment_Type
WITH
(
    DATA_SOURCE = NYD_Taxi_SRC,
    LOCATION = 'silver/rate_code',
    FILE_FORMAT = parquet_file_format
)
AS SELECT *
FROM BRONZE.VW_Payment_Type;
END;
````
### 03- Create Variable for Pipeline
![image](https://github.com/user-attachments/assets/18ddf0a3-d320-4080-a09b-97a250341ca8)


### 03- Deploy Foreach Component 
![image](https://github.com/user-attachments/assets/54772ad9-412d-4e36-bd09-e99cdecd38b8)

### 04- Components Inside Forach Container
![image](https://github.com/user-attachments/assets/831ceb21-2cf6-4e8e-8874-7b0b6b97d5a6)


