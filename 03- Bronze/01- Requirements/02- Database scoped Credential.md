
1. **Method 1: Using Storage Account Name and Key**
    ```sql
    CREATE DATABASE SCOPED CREDENTIAL AzureBlobCredential
    WITH IDENTITY = 'your-storage-account-name', 
    SECRET = 'your-storage-account-key';
    ```
    - **IDENTITY**: This is the name of your storage account.
    - **SECRET**: This is the storage account key.
    - **Use Case**: This method is typically used when you want to authenticate using the storage account's access key. It's a straightforward way to grant access to the storage account.

2. **Method 2: Using Shared Access Signature (SAS Token)**
    ```sql
    CREATE DATABASE SCOPED CREDENTIAL sqlondemand
    WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
    SECRET = 'your_shared_access_signature';
    GO
    ```
    - **IDENTITY**: This is set to 'SHARED ACCESS SIGNATURE', indicating that a SAS token is used for authentication.
    - **SECRET**: This is the actual SAS token.
    - **Use Case**: This method is used when you want to authenticate using a SAS token, which provides more granular and time-limited access to the storage resources¹².


- **Storage Account Name and Key**: Direct access using the storage account's access key.
- **Shared Access Signature (SAS)**: More secure and flexible access using a SAS token.

