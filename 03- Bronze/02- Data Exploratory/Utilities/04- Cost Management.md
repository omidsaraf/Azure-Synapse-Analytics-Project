```sql
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
```
![image](https://github.com/user-attachments/assets/9423f5b2-edef-4b9d-9748-0cf6b8243b50)

