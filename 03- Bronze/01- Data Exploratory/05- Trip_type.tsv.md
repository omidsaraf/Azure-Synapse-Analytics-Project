```sql
SELECT *
  FROM OPENROWSET(
      BULK 'trip_type.tsv',
      DATA_SOURCE = 'nyc_taxi_data_raw',
      FORMAT = 'CSV',
      PARSER_VERSION = '2.0',
      HEADER_ROW = TRUE,
      FIELDTERMINATOR = '\t'
  ) AS trip_type;
````
![image](https://github.com/user-attachments/assets/1037cb4c-f7e8-4189-bacd-98c1659c1375)
