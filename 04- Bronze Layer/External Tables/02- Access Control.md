````sql
CREATE DATABASE SCOPED CREDENTIAL AzureBlobCredential
WITH IDENTITY = 'your-storage-account-name', 
SECRET = 'your-storage-account-key';
