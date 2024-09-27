
#Set the folder paths so that it can be used later. 
````python
bronze_folder_path = 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw'
silver_folder_path = 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/silver'
gold_folder_path = 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/gold'
`````


#Set the spark config to be able to get the partitioned columns year and month as strings rather than integers
````python
spark.conf.set("spark.sql.sources.partitionColumnTypeInference.enabled", "false")
````
````sql
%%sql
-- Create database to which we are going to write the data

CREATE DATABASE IF NOT EXISTS nyc_taxi_ldw_spark
LOCATION 'abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/gold';
````

# Read the silver data to be processed. 
````python
trip_data_green_df = spark.read.parquet(f"{silver_folder_path}/trip_data_green") 

# Perform the required aggregations
# 1. Total trip count
# 2. Total fare amount
from pyspark.sql.functions import *

trip_data_green_agg_df = trip_data_green_df \
                        .groupBy("year", "month", "pu_location_id", "do_location_id") \
                        .agg(count(lit(1)).alias("total_trip_count"),
                        round(sum("fare_amount"), 2).alias("total_fare_amount"))

````




![image](https://github.com/user-attachments/assets/895228d1-db28-487a-bbca-f0077a6df7bf)

````

# Write the aggregated data to the gold table for consumption
````python
trip_data_green_agg_df.write.mode("overwrite").partitionBy("year", "month").format("parquet").saveAsTable("nyc_taxi_ldw_spark.trip_data_green_agg")
