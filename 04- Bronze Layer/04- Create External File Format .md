## The following Steps are required for reading different file format with SQL Commands

![image](https://github.com/user-attachments/assets/707c0de1-a900-4ffc-8559-820fa78232f5)
![image](https://github.com/user-attachments/assets/7988e001-b8ca-4225-9b73-6aebde786cfa)
![image](https://github.com/user-attachments/assets/78e258ce-29a2-45d3-9b98-11c8865b4514)

````sql
USE NYC_TAXI_DW
GO
CREATE OR ALTER EXTERNAL FILE FORMAT CSV_file_format
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

-- Create calendar table  ---> CSV
IF OBJECT_ID('bronze.calendar') IS NOT NULL
    DROP EXTERNAL TABLE bronze.calendar;
CREATE EXTERNAL TABLE bronze.calendar (
    date_key INT,
    date DATE,
    year SMALLINT,
    month TINYINT,
    day TINYINT,
    day_name VARCHAR(10),
    day_of_year SMALLINT,
    week_of_month TINYINT,
    week_of_year TINYINT,
    month_name VARCHAR(10),
    year_month INT,
    year_week INT
)
WITH (
    LOCATION = 'raw/calendar.csv',
    DATA_SOURCE = nyc_taxi_src,
    FILE_FORMAT = csv_file_format2,
    REJECT_VALUE = 10,
    REJECTED_ROW_LOCATION = 'rejections/calendar'
);
SELECT * FROM bronze.calendar;
---------------------------------------------------------
-- Create vendor table    ---> CSV
IF OBJECT_ID('bronze.vendor') IS NOT NULL
    DROP EXTERNAL TABLE bronze.vendor;

CREATE EXTERNAL TABLE bronze.vendor (
    vendor_id TINYINT,
    vendor_name VARCHAR(50)
)
WITH (
    LOCATION = 'raw/vendor.csv',
    DATA_SOURCE = nyc_taxi_src,
    FILE_FORMAT = csv_file_format2,
    REJECT_VALUE = 10,
    REJECTED_ROW_LOCATION = 'rejections/vendor'
);
SELECT * FROM bronze.vendor;
---------------------------------------------------------------------------------
-- Create trip_type table   ---> TSV
IF OBJECT_ID('bronze.trip_type') IS NOT NULL
    DROP EXTERNAL TABLE bronze.trip_type;

CREATE EXTERNAL TABLE bronze.trip_type (
    trip_type TINYINT,
    trip_type_desc VARCHAR(50)
)
WITH (
    LOCATION = 'raw/trip_type.tsv',
    DATA_SOURCE = nyc_taxi_src,
    FILE_FORMAT = tsv_file_format2,
    REJECT_VALUE = 10,
    REJECTED_ROW_LOCATION = 'rejections/trip_type'
);

SELECT * FROM bronze.trip_type;
