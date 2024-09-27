
#### 01- Deploy Script Activity
![image](https://github.com/user-attachments/assets/1e1604aa-bf69-4f51-8bc2-74b06c333761)
````sql
USE NYC_Taxi_DW;
SELECT DISTINCT YEAR, MONTH
FROM 
BRONZE.VW_Trip_Data
````
#### 02- Deploy Foreach Loop Container to recive Year and Month passed by Script Activity
![image](https://github.com/user-attachments/assets/02fa2cd6-13f2-4692-8ef2-c74b24e3c8a8)

#### 03- Deploy Delete and Store Procedure Activities
![image](https://github.com/user-attachments/assets/855aa0b0-0c75-4f5b-9eee-13ec6d60152f)

![image](https://github.com/user-attachments/assets/067e93ac-e706-4c7b-9521-f702a3063423)

#### 04- Script Activity for Create View

![image](https://github.com/user-attachments/assets/fcc2dc9e-e108-4beb-b0cc-8e3131ae7f3a)
````sql
-- first------------------------------------------
USE NYC_Taxi_DW;
-- second-----------------------------------------
DROP VIEW IF EXISTS SILVER.VW_Trip_Data_Parquet;
-- third------------------------------------------
CREATE VIEW SILVER.VW_Trip_Data_Parquet
AS
SELECT
    result.filepath(1) AS year,
    result.filepath(2) AS month,
    result.*
FROM
    OPENROWSET(
        BULK 'silver/trip_data/year=*/month=*/*.parquet',
        DATA_SOURCE = 'NYD_TAXI_SRC2',
        FORMAT = 'PARQUET'
     ) 
WITH(
    vendor_id INT,
    lpep_pickup_datetime datetime2(7),
    lpep_dropoff_datetime datetime2(7),
    store_and_fwd_flag CHAR(1),
    rate_code_ID INT,
    pu_location_id INT,
    do_location_id INT,
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
````




![image](https://github.com/user-attachments/assets/8ce2bfdd-3ecf-4fbc-a095-d497e8c7f628)

