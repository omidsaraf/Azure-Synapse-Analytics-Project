## Create SP to Move Partitioned Data from Bronze to Silver that includes two inputs:
- Year
- Month

Data moving with CETAS then Exteral Table will be droped

```sql

USE NYC_TAXI_DW;
GO
CREATE OR ALTER PROCEDURE SILVER.USP_TAXI_DATA_CSV
    @YEAR NVARCHAR(10), 
    @MONTH NVARCHAR(10)
AS
BEGIN
    SET NOCOUNT ON;
    -- Define variables
    DECLARE @EXT_TABLE NVARCHAR(MAX);
    DECLARE @DROP_SQL NVARCHAR(MAX);
    DECLARE @CREATE_SQL NVARCHAR(MAX);
    DECLARE @TABLE_EXISTS INT;
    -- Set the external table name dynamically
    SET @EXT_TABLE = N'SILVER.Trip_Data_csv_' + @YEAR + '_' + @MONTH;
    -- Check if the external table exists
    SELECT @TABLE_EXISTS = COUNT(*)
    FROM sys.external_tables
    WHERE name = PARSENAME(@EXT_TABLE, 1) -- Table name without schema
    AND schema_id = SCHEMA_ID(PARSENAME(@EXT_TABLE, 2)); -- Schema name
    -- Drop the table if it exists
    IF @TABLE_EXISTS > 0
    BEGIN
        SET @DROP_SQL = N'DROP EXTERNAL TABLE ' + @EXT_TABLE;
        EXEC sp_executesql @DROP_SQL;
    END
    -- Construct the CREATE EXTERNAL TABLE SQL
    SET @CREATE_SQL = N'
    CREATE EXTERNAL TABLE ' + @EXT_TABLE + '
    WITH (
        DATA_SOURCE = NYD_TAXI_SRC2,
        LOCATION = ''silver/trip_data/year=' + @YEAR + '/month=' + @MONTH + ''',
        FILE_FORMAT = parquet_file_format
    )
    AS
    SELECT 
        [VendorID] AS vendor_id,
        [lpep_pickup_datetime],
        [lpep_dropoff_datetime],
        [store_and_fwd_flag],
        [total_amount],
        [payment_type],
        [trip_type],
        [congestion_surcharge],
        [extra],
        [mta_tax],
        [tip_amount],
        [tolls_amount],
        [ehail_fee],
        [improvement_surcharge],
        [RatecodeID] AS rate_code_id,
        [PULocationID] AS pu_location_id,
        [DOLocationID] AS do_location_id,
        [passenger_count],
        [trip_distance],
        [fare_amount]
    FROM BRONZE.VW_Trip_Data_csv 
    WHERE year = ''' + @YEAR + ''' 
    AND month = ''' + @MONTH + '''';
    -- Execute the CREATE EXTERNAL TABLE SQL
    EXEC sp_executesql @CREATE_SQL;
    -- Check if the external table exists
    SELECT @TABLE_EXISTS = COUNT(*)
    FROM sys.external_tables
    WHERE name = PARSENAME(@EXT_TABLE, 1) -- Table name without schema
    AND schema_id = SCHEMA_ID(PARSENAME(@EXT_TABLE, 2)); -- Schema name
    -- Drop the table if it exists
    IF @TABLE_EXISTS > 0
    BEGIN
        SET @DROP_SQL = N'DROP EXTERNAL TABLE ' + @EXT_TABLE;
        EXEC sp_executesql @DROP_SQL;
    END

END;
)
````

## Static Soution:
````sql

EXEC SILVER.USP_TAXI_DATA_CSV '2020', '01';
EXEC SILVER.USP_TAXI_DATA_CSV '2020', '02';
EXEC SILVER.USP_TAXI_DATA_CSV '2020', '03';
EXEC SILVER.USP_TAXI_DATA_CSV '2020', '04';
EXEC SILVER.USP_TAXI_DATA_CSV '2020', '05';
EXEC SILVER.USP_TAXI_DATA_CSV '2020', '06';
EXEC SILVER.USP_TAXI_DATA_CSV '2020', '07';
EXEC SILVER.USP_TAXI_DATA_CSV '2020', '08';
EXEC SILVER.USP_TAXI_DATA_CSV '2020', '09';
EXEC SILVER.USP_TAXI_DATA_CSV '2020', '10';
EXEC SILVER.USP_TAXI_DATA_CSV '2020', '11';
EXEC SILVER.USP_TAXI_DATA_CSV '2020', '12';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '01';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '02';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '03';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '04';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '05';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '06';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '07';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '08';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '09';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '10';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '11';
EXEC SILVER.USP_TAXI_DATA_CSV '2021', '12';
```
