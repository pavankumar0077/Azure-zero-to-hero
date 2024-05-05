# Azure Networking Interview Q&A

### What is the difference between NSG and ASG ?
**ASGs are applied to VMs and are used in conjunction with NSGs. By associating an ASG tag with a network security rule, you can define rules that apply to a group of VMs sharing the same tag.
ASGs simplify the management of security rules in a multi-tier application by grouping VMs that belong to the same application tier. This makes it easier to apply and manage security policies for a specific application.**

```
In Azure, Network Security Groups (NSGs) and Application Security Groups (ASGs) are both used to manage security:
NSGs
Control network traffic flow at the subnet, virtual network, and network interface levels. NSGs filter traffic based on IP addresses, ports, and protocols. They can be used to make rules for subnets and NICs.
ASGs
Group VMs based on applications, which allows for better role-based management and control. ASGs are used to manage security between different application tiers. They can be used to define NSG policies using logical application groupings, which simplifies network security rule management. ASGs are especially useful for distributed application architectures where VMs need to communicate safely across network boundaries
```
![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/d6779047-3c29-443c-8366-ba2562d15951)

- NSG $ ASG - Don't compare with NACL $ SG in AWS
- Assume ASG doesn't exist and we have 2 SUBNET'S - one is App'n subnet where application is there and other one is DB subnet where database is there.
- In Application subnet we have a lof of applications like logging, logout, payment, transactions, bill and customers and etc -- And these applications might be created using Virtual Machine Scale Set, Thre can be multiple replica's of the applications.
- Now the required is that Login and logout these 2 things should be allowed to connect to the DATABASE. Rest of the application are allowed to connect to the DATABASE. Like other applications can send and receive the data from database. but login and logout should be accessed to the database.
- That means we don't allow entire subnet, we have to only allow particular instances.
- If ASG doesn't exists we can still use NSG ot the subnet or we can place for the Database, Let's say you have attached NSG to the Database Instance and go to the inbound rules and add all the instances ip address for the application's that are in application subnet
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/9c8b4f94-6a55-46e1-849b-c4e5b8c33af8)
- But what if the list of ip address means instances are more picking the ip and adding is not easy right, These applications can be put in front of load balancers or we have a Virtual machine scale set
- Providing IP addresses will be a practical problem, IN THIS PARTICULAR USE CASE ASG COMES IN HANDY
- ASG + NSG used in combination, So we will group the application virtual machine or we can assign VMSS, GROUP THE VM's and ASSIGN ASG IN THE SOURCE WE CAN SELECT AS APPLICATION SECURITY GROUP
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/972574bc-9bce-4960-99fd-d2e91aa393bb)
- EVEN ASSIGNING ASG IS DIFFICULT, BUT ADVANTAGE WE WILL GET IS ONE IS WE DON'T NEED TO PROVIDE SOME MANY IP ADDRESS IN THE NSG THEY MIGHT CHANGE 


### How can you block the access to a your vm from a subnet ?
- Assume we have a subnet where we deployed DATABASE APPLICATION, AND THERE ARE MULTIPLE SUBNETS IN THE VNET VIRTUAL NETWORK
- BY DEFAULT IN VNET THE SUBNETS CAN ACCESS THE APPLICATION WITH IN THE SUBNET, IT WILL ALLOW TRAFFIC BTW VM's
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/c80651c0-957c-4063-8772-8fc09d4c1d55)
- By default Even Every VM has VNET IN BOUND rule, ALLOW TRAFFIC WITHIN THE VIRTUAL NETWORK
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/7f163b32-d4e4-4ab6-8565-ed7498c4916b)
- TO DENY THE TRAFFIC WE NEED TO CREATE A DENY RULE AND PRIORITY SHOULD BE LESSER THEN THE VNETINBOUND RULE
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/e4a3b4ec-98b1-4612-acf6-229619302341)


**By default traffic is allowed between subnets with in the VNet in Azure. This is because of a default NSG rule “AllowVnetInBound”.** 

The priority of this rule is 65000, so we need to create a Deny rule with less than 65000 priority number.

### Are Azure NSGs stateful or stateless ?
They are stateful in nature. That means if you allow a port for inbound traffic traffic to receive the request. You don’t have to open the port in outbound rules to send response back.

Example: If you host a host an application on port 80 in Azure VM and allow inbound traffic for customers to access it. You don’t need to open port 80 in outbound traffic to send response back to the customer.

### What is the difference between Azure Firewall and NSG ?
Firewall:
Designed for controlling both outbound and inbound traffic to and from resources within a Virtual Network (VNet).

NSG:
Typically associated with subnets or individual network interfaces to control traffic within a VNet and between VNets.

### What are the advantages of resource groups in azure ?
- Logical Organization
- Lifecycle Management
- Resource Group Tagging
- Role-Based Access Control (RBAC)
- Cost Management
- Resource Group Templates (Azure Resource Manager Templates)
- Resource Locks

### What is the difference between Azure User Data and Custom Data ?
User data is a new version of custom data and it offers added benefits. User data persists and lives in the cloud, accessible and updatable anytime. Custom data vanishes after first boot, accessible only during VM creation.

### What is the difference between Azure App Gateway and Azure LB ?

#### Azure Application Gateway:
Operates at Layer 7 (Application layer) of the OSI model.
Provides advanced application-level routing, SSL termination, and web application firewall (WAF) capabilities.
Suited for distributing traffic based on application awareness.

#### Azure Load Balancer:
Operates at Layer 4 (Transport layer) of the OSI model.
Distributes network traffic based on IP address and port.
Suitable for generic TCP/UDP load balancing without application-specific features.

### Assume your company is using all the ideal Azure Networking setup and your application is deployed in the web subnet , Explain the traffic flow to your app ?

#### User Access:
- External users access the application over the internet.
- DNS resolves the application's domain name to the associated public IP address.

#### Internet Traffic to Azure:
-Incoming internet traffic reaches Azure through Azure Front Door, Azure Application Gateway, or Azure Load Balancer, depending on the specific requirements of the application.
- These services provide load balancing, SSL termination, and other application-level features.

#### Traffic Routing Within Azure:
- Traffic is directed to the public IP address associated with the Azure Application Gateway or Load Balancer.
- The gateway or load balancer routes traffic to the backend pool of the web servers in the web subnet.

#### Network Security Group (NSG) Enforcement:
- Network Security Groups associated with the web subnet control inbound and outbound traffic.
- NSG rules ensure that only required traffic is allowed, providing security at the network layer.
- Azure Virtual Network (VNet) Components:
- The web subnet is part of an Azure Virtual Network, which acts as an isolated network environment.
- Subnets within the VNet communicate with each other through the VNet's internal routing.

#### Application Servers:
- Web servers within the web subnet process incoming requests

#### Describe the purpose of Azure Bastion and when it is used for secure remote access to virtual machines.
- Secure Remote Access:
- Elimination of Public Internet Exposure:
- Reduced Attack Surface:
- Azure Bastion Integration:
- Simplified Connectivity:
- Azure Portal-Based Access:
- Role-Based Access Control (RBAC):
- Multi-Factor Authentication (MFA):
- Audit and Monitoring:




