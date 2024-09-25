# Azure Synapse Analytycs - NYD Taxi :
### End to End Solution

![image](https://github.com/user-attachments/assets/b7b402b7-1759-4f60-b6b6-8eac41f72a4d)



### Project Overview
The goal of this project is to ingest data from various departments with different file formats. The process involves:

#### Phase1- Develop Medalion Architecture
1. **Landing Data**: Initial stage where data is ingested into Bronze Layer.
2. **Data Cleansing**: Bronze Layer to Silver Layer - Cleaning the data in the subsequent layer using serverless SQL Pool
3. **Aggregation**: Silver Layer to Gold Layer (this stage meets Business' Requirements) using Spark Pool

##### Data Explorotary
- whenever sitting data into any layers create a seperate notebook for datadiscover
- these notebooks must be reusable for data analysis purposes

##### Phase2- ELTL via Synapse Pipeline
- Create 2 Pipelines for Bronze to Silver (Partitioned Files) and Single files
- Create 1 Pipeline for Silver to Gold
- Create a Master Pipeline

#### Phase3- Data Warehousing
1. **Staging**: Create External Table which used underlying storage for Gold Layer.
2. **Synapse DW**: Create Datawarehouse via CETAS method and store data in a Dedicated SQL Pool Table.
3. using Dedicated SQL Pool

#### Phase4- Reporting
1. Using PowerBI Service within Synpase
2. Modify Report via Power BI desktop


## Requirements

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
1. **Power BI**:
   -Desktop
   -Service
   -Using same account as Azure Portal account
4. **Cost Conrol**:
   - It is recommended to defined Threshhould for Considered Budget.
   - Activate Sending Email for Cost Alert
   - it must be done withing Azure Cost Analysis
  
  Note: by end of the project Stop all SQL and Saprk Pools.






This ETL project utilizes the Northwind Dataset as the data source. The project involves several key steps to ensure data is efficiently extracted, transformed, and loaded into a Data Warehouse.

### 1. Creating the Data Warehouse in SSMS
- **Setup**: Establish the Data Warehouse environment in SSMS, ensuring it is optimized for performance and scalability.

### 2. Creating Dimension and Fact Tables
- **T-SQL Scripts**: Use T-SQL scripts in SSMS to define the structure of Dimension and Fact tables, ensuring they are designed to support efficient querying and reporting.

### 3. Creating the Staging Database in SSMS






































