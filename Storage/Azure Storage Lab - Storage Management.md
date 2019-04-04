# Lab 1 - Azure Storage Lab - Storage Management
 
**Lab Summary**
*  Task-1: Creating Storage Account & Blob container using Portal & Performing basic operations
*  Task-2: Assigning RBAC policy to new user to Blob storage container 
*  Task-3: Using Azure Storage Explorer and performing storage management 
*  Taks-4: Using AzCopy by Migrating data from local machine to Azure storage 
*  Task-5: Cleaning up Resources

## Task-1: Creating Storage Account & Blob container using Portal & Performing basic operations
To create a general-purpose v2 storage account in the Azure portal, follow these steps:

1. In the Azure portal, select All services. In the list of resources, type Storage Accounts. As you begin typing, the list filters based on your input. Select Storage Accounts.

2. On the Storage Accounts window that appears, choose Add.

3. Select the subscription in which to create the storage account.

4. Under the Resource group field, select Create new. Enter a name as **LabStorage**  for your new resource group.
5. Next, enter a name for your storage account as **teststorage01**. The name you choose must be unique across Azure. The name also must be between 3 and 24 characters in length, and can include numbers and lowercase letters only.
6. Select a location for your storage account, or use the default location.
7. Select Deployment Model as **Resource Manager**, Performance as **Standard** , account kind as **StorageV2 (general-purpose v2)** ,Replication as **Locally redundant storage (LRS)** and Access tier as **Hot**
8. Select Review + Create to review your storage account settings and create the account. and select Create

### Create Storage Container 
1. Click on overview and **Blob** and click on **Container** button. 
2. Keep Public access level default to **Private** and click on **OK** 

### Show Configuration blade
 This allow to change **performance** tier, enable **secure transfer** , **replication** mode and many more. 

### Enable Firwall & network 
 1. Got to Storage Account and select **Firewall and Virtual networks** blade
 2. Select Allow access from **Selected networks** option and under **virtual network** section click on _add existing virtual network_ link to protect existing storage account by allowing to access from that virtual network

### Enable Softdelete
Click on **Soft delete** under _Blob service_ section and Click on **Enabled** Soft Delete and set **Retention policies** as require

## Task-2: Assigning RBAC policy to new user to Blob storage container
1. In the Azure portal, navigate to your storage account **teststorage01** and display the Overview for the account.
2. Under Services, select **Blobs**.
3. Locate the container for which you want to assign a role, and display the container's settings.
4. Select **Access control (IAM)** to display access control settings for the container. Select the **Role assignments** tab to see the list of role assignments.
5. Click the **Add role assignment** button to add a new role.
6. In the Add **role assignment** window, select the Azure Storage role that you want to assign. Then search to locate the security principal to which you want to assign that role
7. Click **Save**. The identity to whom you assigned the role appears listed under that role.

AzCopy /Source:C:\TestData /Dest:https://mystorageaccountname.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"BRK1124.pptx" 

## Task-3: Using Azure Storage Explorer and performing storage management
1. Go to <https://azure.microsoft.com/en-us/features/storage-explorer/> to **download** and install free Azure Storage Explore tool 
2. Authenticate to specific **Subscription** and select appropriate **storage account** to perform storage management 

## Taks-3: Using AzCopy by Migrating data from local machine to Azure storage
1. Install AzCopy by going to <https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy>
2. Open AzCopy Application from start Menu OR go to command line and type cd "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy"
3. Follow below AzCopy command by modifiying parameters 
