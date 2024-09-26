````sql

USE NYC_TAXI_DW;
GO


Method1
--------------------------------------------
CREATE DATABASE SCOPED CREDENTIAL AzureBlobCredential
WITH IDENTITY = 'your-storage-account-name', 
SECRET = 'your-storage-account-key';


Method1
-----------------------
-- Create a database scoped credential
CREATE DATABASE SCOPED CREDENTIAL sqlondemand
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
SECRET = 'your_shared_access_signature';
GO
