## Create External Tables for Bronze Layer with Underlying data in Data Lake

![image](https://github.com/user-attachments/assets/b928dc1c-d1ae-4d16-9716-60f8516cb314)

only for example:
````sql

IF OBJECT_ID('Bronze.Table_name') IS NOT NULL
    DROP EXTERNAL TABLE Bronze.Table_name;

-- Create taxi_zone table
CREATE EXTERNAL TABLE Bronze.Table_name (
    location_id SMALLINT,
    borough VARCHAR(15),
    zone VARCHAR(50),
    service_zone VARCHAR(15)
)
WITH (
    LOCATION = 'raw/taxi_zone.csv',
    DATA_SOURCE = nyc_taxi_src,
    FILE_FORMAT = csv_file_format2,
    REJECT_VALUE = 10,
    REJECTED_ROW_LOCATION = 'rejections/taxi_zone'----->Rejected Logs Location
);


