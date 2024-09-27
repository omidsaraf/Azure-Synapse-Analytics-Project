````sql

SELECT
    MAX(LEN(Column1)) AS len_Column1,
    MAX(LEN(Column2)) AS len_Column1,
    MAX(LEN(Column3)) AS len_Column1,
    .
    .
    .
FROM
    OPENROWSET(
        BULK '<Path File>',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE,
        FIELDTERMINATOR = ',',
        ROWTERMINATOR = '\n'
    ) AS [result]
