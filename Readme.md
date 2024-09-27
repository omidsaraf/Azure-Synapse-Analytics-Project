# Azure Synapse Analytycs - NYD Taxi :
### End to End Solution

![image](https://github.com/user-attachments/assets/b7b402b7-1759-4f60-b6b6-8eac41f72a4d)



### Project Overview

#### Phase1- Develop Medalion Architecture
1. **Landing Data**: Initial stage where data is ingested into Bronze Layer.
   -  Ingest Raw Data into Bronze Layer (Storage's Container)
   -  Define the most efficient Queries to read data from Storage using OpenROWSet
   -  Create External Tables and Views for Underlying Data stored in Data Lake
3. **Data Cleansing**: Bronze Layer to Silver Layer - Cleaning the data in the subsequent layer
    - Using serverless SQL Pool
    - UsingParquet Format to store data in a columnar format to perform better with analytical queries.
    - Stored as Table/views
    - Ability to use T-SQL
    - Using Pay Per Query (do not use Dedicated Resource)
5. **Aggregation**: Silver Layer to Gold Layer (this stage meets Business' Requirements)
     - Using Spark Pool
     - Creating Notebook for Transform Silver to Gold Layer
     - Join Key Information required for Analytics & reporting into New Table.
     - Must be able to use PySpark for Transformed Data
     - Stored in Parquet Format
#### Data Explorotary
- Data Exploration Capability
- Schema Applied to Raw Data
- Discovery Using T-SQL
- Discovery Using Serverless SQL Pool
- whenever sitting data into any layers, we should create a seperate notebook for data discovery.
- these notebooks must be reusable for data analysis purposes.

![image](https://github.com/user-attachments/assets/d6c57b98-cdd8-4210-87e2-0d585e02ac2d)
![image](https://github.com/user-attachments/assets/a8b8c309-dbb8-457c-bbd0-e722f97f1f3a)

#### Phase2- ELTL via Synapse Pipeline
- Create 2 Pipelines for Bronze to Silver (Partitioned Files) and Single files
- Create 1 Pipeline for Silver to Gold
- Create a Master Pipeline
![image](https://github.com/user-attachments/assets/7dfd19d4-7042-4274-a86a-56592d5e5fc9)

#### Phase3- Data Warehousing
- **Staging**: Create External Table which used underlying storage for Gold Layer.
- **Synapse DW**: Create Datawarehouse via CETAS method and store data in a Dedicated SQL Pool Table.
- using Dedicated SQL Pool

#### Phase4- Reporting
- Using PowerBI Service within Synpase
- Modify Report via Power BI desktop
- Taxi Demand: We want to generate a report to identify taxi demand on different days of the week and in various locations of the city, etc.
- Credit Card Campaign: Assist a campaign to encourage credit card payments instead of cash payments.
- Operational Reporting: Create reports on data from IoT devices related to taxis.

![image](https://github.com/user-attachments/assets/51d3917c-65c0-4bdb-bfb2-c68b71dadc61)


**Scheduling Requirement:**
- Run at Regular Intervals
- Ability to Monitor
- Ability to re-Run Failed Pipelines
- Ability to set up alerts on Failures



### Requirements

1. **Azure Setup**:
    - Create a Subscription account, then create a Resource Group under it, and then create Resources under it.
    - Create Storage accounts for Data Lake Gen2.
    - Upload Dataset into Bronze Zone Container.
    - Create Azure Synapse Workspace
    - Meet Security Measures for the Storage Access Control and apply RBAC.
    - Create Linked Service for Storage Account.
2. **Access Control**:   
    - **Key Vault**: Keep secrets for Storage Account and SQL Server Password. Give List and Read access to ADF.
    - **System-assigned Managed Identity**:
        - Automatically created and managed by Azure.
        - Tied to the lifecycle of the ADF instance.
        - Ideal for temporary and service-specific access needs.
        - Use for accessing resources like Azure Key Vault and Azure Storage without managing credentials.
3. **Pools**:
     - Deploy Dedicated SQL Pool for Data Warehousing in Synapse
     - Deploy Spark Pool for Aggregate and moving data from Silver to Gold Layer with Pyspark notebook via Synapse pipeline.
     - Serverless SQL Pool must be running untill end of the project
     - Dedicated SQL Pool And SPark Pool must be running during Datawarehousing and Processing.
4. **Power BI**:
   -Desktop
   -Service
   -Using same account as Azure Portal account
   
4. **Cost Conrol**:
   - It is recommended to defined Threshhould for Considered Budget.
   - Activate Sending Email for Cost Alert
   - it must be done withing Azure Cost Analysis
   - using Optimised Queries to Read and Write data
  
  Note: by end of the project please stop all SQL and Spark Pools.







































