````sql

![image](https://github.com/user-attachments/assets/3ff50bf8-739b-4bcc-a1a8-53006974f18e)

![image](https://github.com/user-attachments/assets/9f76c134-082a-4cc5-9586-8291ab7f3c65)

-- Examine the data types for the columns
EXEC sp_describe_first_result_set N'SELECT
    TOP 100 *
FROM
    OPENROWSET(
        BULK ''abfss://nyc-taxi-data@synapsecoursedl.dfs.core.windows.net/raw/taxi_zone.csv'',
        FORMAT = ''CSV'',
        PARSER_VERSION = ''2.0'',
        HEADER_ROW = TRUE
    ) AS [result]'
