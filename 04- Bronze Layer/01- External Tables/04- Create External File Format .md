## The following Steps are required for reading different file format with SQL Commands

![image](https://github.com/user-attachments/assets/707c0de1-a900-4ffc-8559-820fa78232f5)
![image](https://github.com/user-attachments/assets/7988e001-b8ca-4225-9b73-6aebde786cfa)
![image](https://github.com/user-attachments/assets/78e258ce-29a2-45d3-9b98-11c8865b4514)

````sql
USE NYC_TAXI_DW
GO

IF NOT EXISTS (SELECT * FROM sys.external_file_formats WHERE name = 'CSV_file_format')
CREATE EXTERNAL FILE FORMAT CSV_file_format
WITH (
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS (
        FIELD_TERMINATOR = ',',
        STRING_DELIMITER = '"',
        FIRST_ROW = 2,
        USE_TYPE_DEFAULT = True, ----> Replace Null with default value
        ENCODING = 'UTF8',
        PARSER_VERSION = '2.0')
---------------------------------------------------------
-- Create file format tsv_file_format for parser version 2.0
IF NOT EXISTS (SELECT * FROM sys.external_file_formats WHERE name = 'tsv_file_format')
    CREATE EXTERNAL FILE FORMAT tsv_file_format1
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS (
            FIELD_TERMINATOR = '\t',---> only differet with CSV
            STRING_DELIMITER = '',
            First_Row = 2,
            USE_TYPE_DEFAULT = FALSE,
            Encoding = 'UTF8',
            PARSER_VERSION = '2.0'
        )
    );
------------------------------------------------------------------------
-- Create file format tsv_file_format for parser version 1.0
IF NOT EXISTS (SELECT * FROM sys.external_file_formats WHERE name = 'tsv_file_format_pv1')
    CREATE EXTERNAL FILE FORMAT tsv_file_format2
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS (
            FIELD_TERMINATOR = '\t',
            STRING_DELIMITER = '',
            First_Row = 2,
            USE_TYPE_DEFAULT = FALSE,
            Encoding = 'UTF8',
            PARSER_VERSION = '1.0'
        )  );
------------------------------------------------------------------------
-- Create external file format for parquet_file_format
IF NOT EXISTS (SELECT * FROM sys.external_file_formats WHERE name = 'parquet_file_format')
    CREATE EXTERNAL FILE FORMAT parquet_file_format
    WITH (
        FORMAT_TYPE = PARQUET,
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
    );
------------------------------------------------------------------------
IF NOT EXISTS (SELECT * FROM sys.external_file_formats WHERE name = 'delta_file_format')
    CREATE EXTERNAL FILE FORMAT delta_file_format
    WITH (
        FORMAT_TYPE = DELTA,
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec');

