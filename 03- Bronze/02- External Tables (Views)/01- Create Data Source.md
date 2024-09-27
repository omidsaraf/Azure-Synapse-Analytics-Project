## Create Data Source for Bronze Layer

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
