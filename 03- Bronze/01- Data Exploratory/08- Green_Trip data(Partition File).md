````
SELECT
    result.filepath(1) AS year,
    result.filepath(2) AS month,
    COUNT(1) AS record_count
FROM
    OPENROWSET(
        BULK 'trip_data_green_csv/year=*/month=*/*.csv',
        DATA_SOURCE = 'nyc_taxi_data_raw',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE
    ) AS [result]
--WHERE result.filepath(1) = '2020'
--  AND result.filepath(2) IN ('06', '07', '08')
GROUP BY result.filename(), result.filepath(1), result.filepath(2)
ORDER BY result.filename(), result.filepath(1), result.filepath(2);
````
![image](https://github.com/user-attachments/assets/b290aaee-8299-43e3-bd1f-54e8655a43a7)
