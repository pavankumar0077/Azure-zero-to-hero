# Steps to deploy storage account arm template

### Create resource group

```
az group create --name vscode --location 'Central US'
```

### Create the storage account

Switch to the folder where you have the `01-storage-account.json` or similar file

```
az deployment group create --resource-group vscode --template-file 01-storage-account.json
```

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/d159a628-7424-4266-9242-203c8721d8f5)
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/0cdd8c43-87d6-40f8-bcac-a63136eeb237)

```
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "functions": [],
    "variables": {},
    "resources": [
        {
            "name": "pavan007",
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2023-01-01",
            "tags": {
                "displayName": "pavan007"
            },
            "location": "[resourceGroup().location]",
            "kind": "StorageV2",
            "sku": {
                "name": "Premium_LRS",
                "tier": "Premium"
            }
        }
    ],
    "outputs": {}
}
```

- Schema - Validate the template
- ContentVersion - It is a version control
- Parameters - Instead of hardcoding the values, we can define the values that we need.
- variables - Use a particular field multiple times then we use variables
- resources - It is the actual field, we can create multiple resources from here
- outputs - What we want to see in output, Like public IP of VM, etc
- functions - If we want to write logic in the creation of VM, for Example for resource name we can use dynamic like with timestamp like that.
