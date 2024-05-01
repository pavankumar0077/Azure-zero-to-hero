# Azure Networking Advanced

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/82132e4d-dfbe-4def-9d14-a78bc2073d49)

### VNET
- When we create a Virtual Network in Azure, By default this VNET is deployed on multiple Availability Zones that are there in that particular region.
- Let's say you requested for VNET in east-us region, by default the VNET that we get will be available in Zone 1, Zone 2 & Zone 3 of east-us
- Let's say we have deployed a web-app'n in web-subnet that is created and for some reason availability zone 1 has gone down, If this VNET is created only in availability zone 1 then the problem is that the
complete VNET will go down along with app'n that we have deployed and instance that we have deployed and all the resources that are available in this availability zone.
The recommended way is if a critical application then keep multiple copies of it, One in AZ 1 and the other in AZ 2.

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/5a43784a-2581-44f7-adf7-15be516eb2e9)

## Azure App Gateway & WAF

Azure Application Gateway is a web traffic load balancer that enables you to manage and route traffic to your web applications. Web Application Firewall (WAF) provides protection against web vulnerabilities. Key features include:

- **Load Balancing**: Distributes incoming traffic across multiple servers to ensure no single server is overwhelmed.

- **SSL Termination**: Offloads SSL processing, improving the efficiency of web servers.

- **Web Application Firewall (WAF)**: Protects web applications from common web vulnerabilities and exploits.

## Azure Load Balancer

Azure Load Balancer distributes incoming network traffic across multiple servers to ensure no single server is overwhelmed. Key features include:

- **Load Balancing Algorithms**: Supports different algorithms for distributing traffic, such as round-robin and least connections.

- **Availability Sets**: Works seamlessly with availability sets to ensure high availability.

- **Inbound and Outbound Traffic**: Balances both inbound and outbound traffic.

## Azure DNS

Azure DNS is a scalable and secure domain hosting service. It provides name resolution using the Microsoft Azure infrastructure. Key features include:

- **Domain Hosting**: Hosts domain names and provides name resolution within Azure.

- **Integration with Azure Services**: Easily integrates with other Azure services like App Service and Traffic Manager.

- **Global Availability**: Provides low-latency responses globally.

## Azure Firewall

Azure Firewall is a managed, cloud-based network security service that protects your Azure Virtual Network resources. Key features include:

- **Stateful Firewall**: Allows or denies traffic based on rules and supports stateful inspection.

- **Application FQDN Filtering**: Filters traffic based on fully qualified domain names.

- **Threat Intelligence Integration**: Integrates with threat intelligence feeds for enhanced security.

## Virtual Network Peering and VNet Gateway

### Virtual Network Peering

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/a6bcbecc-b759-46de-ae10-30c533d15844)


Virtual Network Peering allows connecting Azure Virtual Networks directly, enabling resources in one VNet to communicate with resources in another. Key features include:

- **Global VNet Peering**: Peering can be established across regions.

- **Transitive Routing**: Traffic between peered VNets flows directly, improving performance.

### VNet Gateway

VNet Gateway enables secure communication between on-premises networks and Azure Virtual Networks. Key features include:

- **Site-to-Site VPN**: Connects on-premises networks to Azure over an encrypted VPN tunnel.

- **Point-to-Site VPN**: Enables secure remote access to Azure resources.

## VPN Gateway

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/ee6637b2-363f-444b-a170-9f092da72cbd)

Azure VPN Gateway provides secure, site-to-site connectivity between your on-premises network and Azure. Key features include:

- **IPsec/IKE VPN Protocols**: Ensures secure communication over the Internet.

- **High Availability**: Supports active-active and active-passive configurations for high availability.

- **BGP Support**: Allows dynamic routing between your on-premises network and Azure.
