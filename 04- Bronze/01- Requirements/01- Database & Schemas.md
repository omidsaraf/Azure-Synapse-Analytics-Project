# Create Database and Schemas for each layer

![image](https://github.com/user-attachments/assets/1f3eb961-ecf5-49cd-a675-de2d83b20ac2)

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
