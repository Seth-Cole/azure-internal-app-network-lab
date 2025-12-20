# azure-internal-app-network-lab
Using resource groups, a virtual network with subnets, assigning public IPs, Private DNS, user-defined routes (UDRs), and Azure Firewall/NSGs to secure an internal application server in Azure.

## Architecture Overview
![Internal app network diagram](https://github.com/Seth-Cole/azure-internal-app-network-lab/blob/main/diagrams/WebAppDiagram.jpg)

-Resource Group: az-rg-lab
-VNet: az-vnet-lab (10.0.0.0/16)
-Subnets: AzureFirewallSubnet(10.0.1.0/26), Administrative Subnet(az-adsn-lab 10.0.2.0/24), Application Workload Subnet(az-wlsn-lab 10.0.3.0/24), AzureFirewallManagementSubnet(10.0.4.0/26)
-Azure Firewall Basic with outbound FQDN filtering
-Network Security Groups for both admin and workload subnets
-Private DNS zone(az.pdns.lab) with custom record
-Admin VM with Public IP for SSH access (az-adminvm-lab) and Pseudo application server (az-appvm-lab)


