One of the main things that we need to consider is to find those tables mostly keeping Transactional and huge number of records. in our case, Trip Data has been stored by year and month so it is mostly required to be considered.

Path File: Year/Month/Trip Data.CSV

##Total Amount

We need to ensure that the number of records we have matches our expectations. If not, we should:
- Approach the data supplier.
- Use a different data source.

### Step 1
- Check Min, Max, and Avg for the entire dataset using the address ending with **.

### Step 2
- Check for Null values. The number of records based on the primary key and the `Total_Amount` column (which belongs to the data) should be the same; otherwise, there are Null values.

##Min, Max, AVG, Null
![image](https://github.com/user-attachments/assets/e66c8cf4-7d9c-4a57-ade7-e60dc96a4f99)
![image](https://github.com/user-attachments/assets/f2695c07-49de-44a9-8fa6-8747546f210c)


