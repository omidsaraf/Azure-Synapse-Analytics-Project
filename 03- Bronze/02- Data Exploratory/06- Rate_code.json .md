````sql

SELECT rate_code_id, rate_code
  FROM OPENROWSET(
      BULK 'rate_code.json',
      DATA_SOURCE = 'nyc_taxi_data_raw',
      FORMAT = 'CSV',
      FIELDTERMINATOR = '0x0b',
      FIELDQUOTE = '0x0b',
      ROWTERMINATOR = '0x0b'
  )
  WITH
  (
      jsonDoc NVARCHAR(MAX)
  ) AS rate_code
  CROSS APPLY OPENJSON(jsonDoc)
  WITH(
      rate_code_id TINYINT,
      rate_code VARCHAR(20) 
  );
````
![image](https://github.com/user-attachments/assets/67290121-fe19-42b5-a018-3e9c77df19d3)


Extra:
```
 -- Process multi line file
  SELECT rate_code_id, rate_code
  FROM OPENROWSET(
      BULK 'rate_code_multi_line.json',
      DATA_SOURCE = 'nyc_taxi_data_raw',
      FORMAT = 'CSV',
      FIELDTERMINATOR = '0x0b',
      FIELDQUOTE = '0x0b',
      ROWTERMINATOR = '0x0b'
  )
  WITH
  (
      jsonDoc NVARCHAR(MAX)
  ) AS rate_code
  CROSS APPLY OPENJSON(jsonDoc)
  WITH(
      rate_code_id TINYINT,
      rate_code VARCHAR(20) 
  );



