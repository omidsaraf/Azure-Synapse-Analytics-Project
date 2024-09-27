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


