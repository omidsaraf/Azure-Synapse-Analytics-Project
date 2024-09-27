### Key Billing Factors in Serverless SQL Pool

1. **Billed for Data Processed**:
   - This is the primary metric for billing in Serverless SQL Pool. You are charged based on the amount of data processed by your queries, making it a true pay-per-use model⁸.

2. **Amount of Data Read from Storage**:
   - This includes all data read from storage during query execution. It encompasses data read while accessing files and metadata, such as Parquet file formats⁶.

3. **Amount of Data in Intermediate Results**:
   - This refers to the data temporarily stored and transferred among nodes during query execution. It includes data transferred to your endpoint in an uncompressed format⁶.

4. **Amount of Data Written to Storage**:
   - If you use CETAS (Create External Table As Select) to export results to storage, the amount of data written is added to the data processed for the SELECT part of CETAS⁶.

### Additional Notes

- **Data Processed Rounding**:
  - The data processed is rounded to the nearest megabyte (MB), which can slightly affect the billing amount⁶.

### Importance of Understanding Billing

Understanding these factors is crucial for managing and estimating costs effectively. By monitoring the amount of data processed, read, and written, you can optimize your queries and control expenses. Azure Synapse Analytics also provides cost management features to help set budgets and monitor spending⁷.



![image](https://github.com/user-attachments/assets/5d73d30f-a150-4adb-add4-d1d7c19e932c)


Method1- SQL pools settings
![image](https://github.com/user-attachments/assets/d5ecd32a-ea0b-4260-a618-ede06aa3c755)

Method2- SQL Query

![image](https://github.com/user-attachments/assets/b6af1082-3397-45ff-aeb6-13835a38c73e)
```
SELECT * FROM sys.dm_external_data_processed;

SELECT * FROM sys.configurations
WHERE name LIKE 'Data Processed %';

sp_set_data_processed_limit
    @type = N'monthly',
    @limit_tb = 2;

sp_set_data_processed_limit
    @type = N'weekly',
    @limit_tb = 1;

sp_set_data_processed_limit
    @type = N'daily',
    @limit_tb = 1;
````   
![image](https://github.com/user-attachments/assets/0f6a17f6-7dc4-4041-bee2-f3211b0f5787)
