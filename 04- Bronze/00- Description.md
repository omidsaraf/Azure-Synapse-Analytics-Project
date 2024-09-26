##for Bronze Layer:
- Ingest Raw Data without any changes
- Using Serverless SQL Pool
- Create External Tables for Single files with CSV Format
- Create Views for JSON Files and Partitioned Files

````sql
USE master
GO

CREATE DATABASE NYC_TAXI_DW
COLLATE Latin1_General_100_BIN2_UTF8
GO
-------------------------------------
USE NYC_TAXI_DW
GO

CREATE SCHEMA BRONZE
GO
CREATE SCHEMA SILVER
GO
CREATE SCHEMA GOLD
GO

