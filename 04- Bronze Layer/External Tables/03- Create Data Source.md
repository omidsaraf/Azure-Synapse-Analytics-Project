## If We use External Data Lake then it needs to be defined in the Database
So, from now on we use the external storage (Data Lake Gen2)
![image](https://github.com/user-attachments/assets/06f7a97c-986f-4beb-979a-ff8defc8e741)

````sql
USE NYC_TAXI_DW
GO

IF NOT EXISTS (SELECT * FROM sys.external_data_sources WHERE name = 'NYD_TAXI_SRC')
BEGIN
    CREATE EXTERNAL DATA SOURCE NYD_TAXI_SRC
    WITH (
        LOCATION = 'abfss://medalian@dlstsynapstr18.dfs.core.windows.net/',
        CREDENTIAL = sqlondemand
    );
END;
GO
