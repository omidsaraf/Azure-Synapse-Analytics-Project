### Data Quality Check Documentation

#### Key Metrics
- **Minimum, Maximum, and Average Values of `<total amount>`**
- **Count Check**: Ensure `Count(<total amount>)` equals `Count(PK)` to check for null values.
- **Negative Values**: Negative values for monetary amounts are illogical and should be removed.

#### Further Investigation
- **Identify Negative Records**: Filter data to find records with negative values for further analysis.
- **Null Payment Types**: Identify records with null payment types.

#### Handling Null Values
- **Map Null Payment Types**: Map null payment types to type 5 (unknown) for consistency.
- **Negative Values**: Consider excluding or transforming negative values based on reporting or machine learning needs.

### Transactional Data Considerations
One of the main things to consider is identifying tables that primarily store transactional data with a large number of records. In our case, Trip Data is stored by year and month, so it is essential to consider this structure.

- **Path File**: `Year/Month/Trip Data.CSV`
- **Total Amount of Records**: Ensure the number of records matches expectations. If not, approach the data supplier or use a different data source.
- **Min, Max, and Avg**: Check these metrics for the entire dataset using the address ending with `**`.
- **Null Values**: Ensure the number of records based on the primary key and the `Total_Amount` column are the same; otherwise, there are null values.

### SQL Queries

#### Check for Duplicates in Taxi Zone Data
```sql
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
    ) AS [result]
GROUP BY location_id
HAVING COUNT(1) > 1;
```

```sql
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
    ) AS [result]
GROUP BY borough
HAVING COUNT(1) > 1;
```

#### Identify Data Quality Issues in Trip Total Amount

```sql
SELECT
    MIN(total_amount) AS min_total_amount,
    MAX(total_amount) AS max_total_amount,
    AVG(total_amount) AS avg_total_amount,
    COUNT(1) AS total_number_of_records,
    COUNT(total_amount) AS not_null_total_number_of_records
FROM
    OPENROWSET(
        BULK 'trip_data_green_parquet/year=2020/month=01/',
        FORMAT = 'PARQUET',
        DATA_SOURCE = 'nyc_taxi_data_raw'
    ) AS [result];
```

```sql
SELECT
    payment_type, COUNT(1) AS number_of_records
FROM
    OPENROWSET(
        BULK 'trip_data_green_csv/year=*/month=*/*.csv',
        DATA_SOURCE = 'nyc_taxi_data_raw',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE
    ) AS [result]
GROUP BY payment_type
ORDER BY payment_type;
```

#### Identify Null Values

````sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'trip_data_green_csv/year=*/month=*/*.csv',
        DATA_SOURCE = 'nyc_taxi_data_raw',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE
    ) AS [result]
WHERE
    total_amount IS NULL;

---------------------------------
SELECT
    COUNT(*) AS null_total_amount_count
FROM
    OPENROWSET(
        BULK 'trip_data_green_csv/year=*/month=*/*.csv',
        DATA_SOURCE = 'nyc_taxi_data_raw',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE
    ) AS [result]
WHERE
    total_amount IS NULL;




