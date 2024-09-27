
In fact, collation specifies the bit patterns that represent each character in a dataset. They also define the rules for sorting and comparing data. Therefore, it is crucial to specify the correct collation for your data to prevent any unintended conversions.

After appropriately assigning the Data Type for each column, it is important to pay attention to warning messages when creating queries related to characters. If we look at character columns, by default, they contain UTF-8 encoded text. Synopsis uses the second version of UTF-8 characters for implementation and our characters, which is not what we want.

Understanding and correctly setting collation is essential because it directly impacts how data is stored, sorted, and compared in your database. Using the wrong collation can lead to unexpected behavior, such as incorrect sorting order or failed comparisons, which can affect the integrity and performance of your data operations. By ensuring the correct collation, you can avoid these issues and maintain consistent and accurate data handling.


Idenmtify
````
SELECT name, collation_name FROM sys.databases;
````
![image](https://github.com/user-attachments/assets/fec06d68-70ad-4548-8767-31f062d24a5f)


Method1- Specify UTF-8 Collation for VARCHAR columns
````
-- 
SELECT
    *
FROM
    OPENROWSET(
        BULK '<File Path>',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE,
        FIELDTERMINATOR = ',',
        ROWTERMINATOR = '\n'
    ) 
    WITH (
        Column1 SMALLINT,
        Column2 VARCHAR(15) COLLATE Latin1_General_100_CI_AI_SC_UTF8,
        Column3 VARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC_UTF8,
        Column4 VARCHAR(15) COLLATE Latin1_General_100_CI_AI_SC_UTF8
    )AS [result]


````
Method2- Apply for Database
````
ALTER DATABASE nyc_taxi_discovery COLLATE Latin1_General_100_CI_AI_SC_UTF8;

