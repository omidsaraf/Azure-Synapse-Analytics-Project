## Read File
````sql
SELECT
    TOP 100 *
FROM
    OPENROWSET(
        BULK ''abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv'',
        FORMAT = ''CSV'',
        PARSER_VERSION = ''2.0'',
        HEADER_ROW = TRUE
    ) AS [result]'
````

![image](https://github.com/user-attachments/assets/7b0c6a3b-3374-48d2-8303-5f6b7886e886)

## Examine data types for the columns
````sql
EXEC sp_describe_first_result_set N'SELECT
    TOP 100 *
FROM
    OPENROWSET(
        BULK ''abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv'',
        FORMAT = ''CSV'',
        PARSER_VERSION = ''2.0'',
        HEADER_ROW = TRUE
    ) AS [result]'
````
![image](https://github.com/user-attachments/assets/4296afcd-5230-402f-b8ba-dffdf4720a5b)

## Check Maximum size of each column
````sql
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
````
![image](https://github.com/user-attachments/assets/3b9150c2-4203-4067-8e31-81a8e0535bd1)


## Mention Column names with the most efficient data type and size
````sql
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
````
![image](https://github.com/user-attachments/assets/91842017-1096-4ea7-bc1b-9b4ed17ae9fc)


## Check Maximum Size again (doule check)
````sql
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
````

![image](https://github.com/user-attachments/assets/7edda83a-4fcd-4d38-91ad-d1fa70cd8fcf)


## Select only subset of columns 
````sql
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
````


## Read data from a file without header
![image](https://github.com/user-attachments/assets/c0a050ec-038b-44b3-b0f7-42cca4e732f4)
![image](https://github.com/user-attachments/assets/f40e2cc2-bef5-43d1-90ce-11e0e130ce90)

### Fix Column Names
````sql
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
````
