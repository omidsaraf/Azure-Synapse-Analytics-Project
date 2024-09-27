views only JSON to Parquet

````sql
USE NYD_TAXI_DW;
IF OBJECT_ID('SILVER.Rate_Code') IS NOT NULL
    DROP EXTERNAL TABLE SILVER.Rate_Code;
GO
CREATE EXTERNAL TABLE SILVER.Rate_Code
WITH
(
    DATA_SOURCE = NYD_TAXI_SRC,
    LOCATION = 'silver/Rate_Code',
    FILE_FORMAT = Parquet_file_format
)
AS
SELECT rate_code_id, rate_code
FROM OPENROWSET(
    BULK 'raw/rate_code.json',
    DATA_SOURCE = 'nyc_taxi_src',
    FORMAT = 'CSV',
    FIELDTERMINATOR = '0x0b',
    FIELDQUOTE = '0x0b',
    ROWTERMINATOR = '0x0b'
) 
WITH(jsonDoc NVARCHAR(MAX))AS rate_code
CROSS APPLY OPENJSON (jsonDoc)
WITH (
    rate_code_id TINYINT,
    rate_code VARCHAR(20)
);

SELECT * FROM SILVER.Rate_Code;
-------------------------------------------------------------------------
USE NYD_TAXI_DW;
IF OBJECT_ID('SILVER.Payment_Type') IS NOT NULL
    DROP EXTERNAL TABLE SILVER.Payment_Type
GO
CREATE EXTERNAL TABLE SILVER.Payment_Type
WITH
(
    DATA_SOURCE = NYD_TAXI_SRC,
    LOCATION = 'silver/Payment_Type',
    FILE_FORMAT = Parquet_file_format
)
AS
SELECT * 
FROM BRONZE.VW_Payment_Type



