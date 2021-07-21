## Azure Key Vault - Rotate Secrets using Function App Event Handler

**Current State** - Storage Account (ksripadastorageaccount)  is being used by multiple applications  for  product data feed

   * Handover secrets manually /email to the application owners\
   * Secrets have no expiry\
    
**Why** - MS Security recommendation is to rotate secrets, every 90 days

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

### 1.Create a secret in AKV
  Azure Key Vault is a cloud service for securely storing and accessing secrets.\
  A secret is anything that you want to tightly control access to, such as API keys, passwords, certificates, or cryptographic key \
      ![image](https://user-images.githubusercontent.com/8209932/126427470-e40735d2-6e02-44c2-a7b1-3419a1528314.png)\
      ![image](https://user-images.githubusercontent.com/8209932/126427997-b0b63c40-0101-41ec-89fc-36f19adb580c.png)


### 2.Rotate Secrets 
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



