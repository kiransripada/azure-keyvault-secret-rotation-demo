## Azure Key Vault - Rotate Secrets using Function App Event Handler

**Usecase** - Rotate( Storage Account) Secrets\
**Current State** 
   * Storage Account (ksripadastorageaccount)  is being used by multiple applications  for  product data feed\
   * Handover secrets manually /email to the application owners\
   * Secrets have no expiry\
    
**Why** - MS recommendation is to rotate secrets, every 90 days

**How** - Rotate secrets using Azure Key Vault to rotate secrets.
          Other available  options HashiCorp vault 
 
**Demo** - 

Rotate & retrieve secrets using  Azure KV.   

**Future State**
  * Ability to rotate secrets  on a 90 day frequency
  * The ability for applications to retrieve secrets instead of handing secrets to owners

**Demo Highlights** - 


 1. Create secret in AKV
      acces_keys for ksripadastorageaccount  
 1. Rotate Secrets using MS recommended option
 1. Access Secrets 
       (Client Applications, Processess)
     Retrieve secrets
     Secure  secret retrieval
1. Notify & Alert(nice to have )
1. Learnings / Next steps

#### 1.Create a secret in AKV
  Azure Key Vault is a cloud service for securely storing and accessing secrets.\
  A secret is anything that you want to tightly control access to, such as API keys, passwords, certificates, or cryptographic key \
      ![image](https://user-images.githubusercontent.com/8209932/126427470-e40735d2-6e02-44c2-a7b1-3419a1528314.png)\
      ![image](https://user-images.githubusercontent.com/8209932/126427997-b0b63c40-0101-41ec-89fc-36f19adb580c.png)


#### 2.Rotate Secrets 
   The azure key vault provides the option to set the expiry when we provision/store an entity in the Key Vault.\
   We can then monitor events related to an upcoming expiry date.\
          ![image](https://user-images.githubusercontent.com/8209932/126428577-86ac6c85-a2ac-4402-b6af-ddbb35bd3dc6.png)
          ![image](https://user-images.githubusercontent.com/8209932/126428632-380e31ef-a0e9-4b67-9a0c-94d5c43d2e57.png)

   Rotate keys
        ![image](https://user-images.githubusercontent.com/8209932/126428721-6bc991e6-4584-4369-85f8-5b35649730e3.png)
         
          Prerequisites
            An Azure App Service plan
            A storage account to manage function app triggers
            An access policy to access secrets in Key Vault
            The Storage Account Key Operator Service role is assigned to the function app so it can access storage account access keys
            A key rotation function with an event trigger and an HTTP trigger (on-demand rotation)
            An Event Grid event subscription for the SecretNearExpiry eve
#### 3.Access Secrets 
    Enable a system-assigned managed identity for the application
    Register the application with your Azure AD tenant
 Option #2
  Applications can   access secrets using service principal
    ![image](https://user-images.githubusercontent.com/8209932/126428991-fcc33b1a-ed1b-4341-ab3c-afd88518c719.png)

  Applications can   access secrets using service principal
    ![image](https://user-images.githubusercontent.com/8209932/126429308-695d3043-e7d5-45e2-b679-dc4878297954.png)
    
#### 4. Notify & Alert 
   notify-webhook copy![image](https://user-images.githubusercontent.com/8209932/126429574-8502fae6-2b41-474f-bce9-b559eb4b8e59.png)

#### 5. Learnings
  Intermittent Errors / Exceptions encountered during POC
  
 1. [**Error**] *Singleton lock renewal failed for blob 'ksripada-storagekey-rotation-fna/host' with error code 409:LeaseIdMismatchWithLeaseOperation.*\
            QuickFix:-  Restart Function app event handler
      
  2. [**Error**]*Azure.KeyVault.Models.KeyVaultErrorException: Operation returned an invalid status code 'Forbidden'at           Microsoft.Azure.KeyVault.KeyVaultClient.GetSecretWithHttpMessagesAsy*\
            QuickFix:-  Validate permissions to function app .  the Restart function app event handler 




References # \
https://github.com/Azure-Samples/KeyVault-Rotation-StorageAccountKey-PowerShell.git \
https://dev.to/cheahengsoon/configure-and-manage-azure-key-vault-3foj \
https://github.com/Azure-Samples/event-grid-node-publish-consume-events

