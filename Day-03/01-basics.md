# Azure Resources

Azure resources are the building blocks of your cloud infrastructure in Microsoft Azure. These resources can be virtual machines, databases, storage accounts, or any other service offered by Azure. Each resource is a manageable item in Azure, and they are provisioned and managed individually.
- A resource is an instance of a service.

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/7136807e-b8bf-4258-9e37-6e60eb03b652)


## Resource Groups in Azure

A **Resource Group** in Azure is a logical container for resources that share the same lifecycle, permissions, and policies. It helps you organize and manage related Azure resources efficiently. Resources within a group can be deployed, updated, and deleted together as a single management unit.
- Resource Group is mandatory
- Required to track resources for different teams.
- Access
- Permissions
- Security
- Auditing
- Cost and more.
- We can create multiple resource groups for different env's
- We can also create multiple resource groups with different azure accounts.

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/cd855528-65c4-4de9-85ba-6bea3a874453)
![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/e2eac340-01a9-4a4f-9ade-b9606baf8615)


### Key Points about Resource Groups:

- **Lifecycle Management:** Resources within a group can be managed collectively, making it easy to handle deployments, updates, and deletions.

- **Resource Organization:** Grouping resources based on projects, environments, or applications helps keep your Azure environment well-organized.

- **Role-Based Access Control (RBAC):** Permissions and access control are applied at the resource group level, allowing you to manage who can access and modify resources within a group.

## Azure Resource Manager (ARM) Overview

**Azure Resource Manager (ARM)** is the deployment and management service for Azure. It provides a consistent management layer that enables you to deploy resources with declarative templates. ARM templates describe the resources you need and their configurations, allowing you to deploy and update resources in a predictable manner.

### Key Features of Azure Resource Manager:

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/2103d303-3b9c-4d88-aa71-7c38ae73fa18)

- **Template-Based Deployment:** ARM uses JSON templates to define the infrastructure and configuration of your Azure resources. This enables repeatable and consistent deployments.

- **Dependency Management:** ARM automatically handles dependencies between resources, ensuring they are deployed in the correct order.

- **Rollback and Roll-forward:** In case of deployment failures, ARM can automatically roll back changes to maintain the desired state, or roll forward to the last known good state.

- **Tagging and Categorization:** You can use tags to label and categorize resources, making it easier to manage and organize your Azure environment.

**Note:** Understanding Azure resources, resource groups, and Azure Resource Manager is fundamental to effectively utilize and manage your resources in the Azure cloud.

