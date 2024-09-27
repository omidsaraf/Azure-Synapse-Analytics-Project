###### incrementally loading data into fact tables (staging table) use Table Distribution: Round-Robin, Table Structure: Heap
###### CTAS on MCD HEAP target tables is not supported. Instead, use INSERT SELECT as a workaround to load data into MCD HEAP tables.
###### Users can set MAXDOP to an integer value to control the maximum degree of parallelism. When MAXDOP is set to 1, the query is executed by a single thread.
###### For fact table creation: Clustered Column Store will by default have 60 partitions. And to achieve best compression we need at least 1 million rows per partition, hence minimum number of rows that Table1 should contain before you create partitions = 60 Million.


````sql

Create Data Warehouse

CREATE SCHEMA dwh;
GO
CREATE TABLE dwh.trip_data_green_agg
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN---> Copy all table in all 60 Computed Node
)
AS SELECT * FROM staging.ext_trip_data_green_agg;
````
![image](https://github.com/user-attachments/assets/5ddb30cc-9644-4a83-8555-da60e08e7b58)
