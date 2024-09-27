
![image](https://github.com/user-attachments/assets/f9866610-cb37-42c2-bbb5-611d3949af5f)

````sql


-- Examine data types for the columns
EXEC sp_describe_first_result_set N'SELECT
    TOP 100 *
FROM
    OPENROWSET(
        BULK ''abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv'',
        FORMAT = ''CSV'',
        PARSER_VERSION = ''2.0'',
        HEADER_ROW = TRUE
    ) AS [result]'
![image](https://github.com/user-attachments/assets/dc275ff0-a824-4245-a55e-89383e65cf78)


SELECT
    MAX(LEN(LocationId)) AS len_LocationId,
    MAX(LEN(Borough)) AS len_Borough,
    MAX(LEN(Zone)) AS len_Zone,
    MAX(LEN(service_zone)) AS len_service_zone
FROM
    OPENROWSET(
        BULK 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE,
        FIELDTERMINATOR = ',',
        ROWTERMINATOR = '\n'
    ) AS [result]

![image](https://github.com/user-attachments/assets/851f2253-3c18-470f-8a53-08866e93301e)



-- Use WITH clause to provide explicit data types
SELECT
    *
FROM
    OPENROWSET(
        BULK 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE,
        FIELDTERMINATOR = ',',
        ROWTERMINATOR = '\n'
    ) 
    WITH (
        LocationID SMALLINT,
        Borough VARCHAR(15),
        Zone VARCHAR(50),
        service_zone VARCHAR(15)
    )AS [result]

![image](https://github.com/user-attachments/assets/91842017-1096-4ea7-bc1b-9b4ed17ae9fc)

EXEC sp_describe_first_result_set N'SELECT
    *
FROM
    OPENROWSET(
        BULK ''abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv'',
        FORMAT = ''CSV'',
        PARSER_VERSION = ''2.0'',
        HEADER_ROW = TRUE,
        FIELDTERMINATOR = '','',
        ROWTERMINATOR = ''\n''
    ) 
    WITH (
        LocationID SMALLINT,
        Borough VARCHAR(15),
        Zone VARCHAR(50),
        service_zone VARCHAR(15)
    )AS [result]'


![image](https://github.com/user-attachments/assets/96ab2301-82f1-4add-8985-7f4c7b693ee9)


SELECT name, collation_name FROM sys.databases;

![image](https://github.com/user-attachments/assets/18c5ceb4-fa3f-47ae-a0c2-f157fe2e5ace)



-- Specify UTF-8 Collation for VARCHAR columns
SELECT
    *
FROM
    OPENROWSET(
        BULK 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE,
        FIELDTERMINATOR = ',',
        ROWTERMINATOR = '\n'
    ) 
    WITH (
        LocationID SMALLINT,
        Borough VARCHAR(15) COLLATE Latin1_General_100_CI_AI_SC_UTF8,
        Zone VARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC_UTF8,
        service_zone VARCHAR(15) COLLATE Latin1_General_100_CI_AI_SC_UTF8
    )AS [result]

-- Select only subset of columns 
SELECT
    *
FROM
    OPENROWSET(
        BULK 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE,
        FIELDTERMINATOR = ',',
        ROWTERMINATOR = '\n'
    ) 
    WITH (
        Borough VARCHAR(15),
        Zone VARCHAR(50)
    )AS [result]         

-- Read data from a file without header
SELECT
    *
FROM
    OPENROWSET(
        BULK 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone_without_header.csv',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        FIELDTERMINATOR = ',',
        ROWTERMINATOR = '\n'
    ) 
    WITH (
        Zone VARCHAR(50) 3,
        Borough VARCHAR(15) 2
    )
AS [result]     


-- Fix Column Names
SELECT
    *
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

