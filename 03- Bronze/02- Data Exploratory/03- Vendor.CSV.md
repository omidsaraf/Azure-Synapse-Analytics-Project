````sql
--- 
Method1- Escape Comma
SELECT *
  FROM OPENROWSET(
      BULK 'vendor_escaped.csv',
      DATA_SOURCE = 'nyc_taxi_data_raw',
      FORMAT = 'CSV',
      PARSER_VERSION = '2.0',
      HEADER_ROW = TRUE,
      ESCAPECHAR = '\\'
  ) AS vendor;

Method2- Delimitator (Quote)

SELECT *
  FROM OPENROWSET(
      BULK 'vendor.csv',
      DATA_SOURCE = 'nyc_taxi_data_raw',
      FORMAT = 'CSV',
      PARSER_VERSION = '2.0',
      HEADER_ROW = TRUE,
      FIELDQUOTE = '"'
  ) AS vendor;

````
![image](https://github.com/user-attachments/assets/25685a77-55c6-4037-81a2-a5eec1f51706)
