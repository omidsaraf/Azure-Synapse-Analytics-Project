

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
