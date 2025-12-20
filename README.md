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



# azure-internal-app-network-lab

Using resource groups, a virtual network with subnets, public IPs, Private DNS, user-defined routes (UDRs), and Azure Firewall/NSGs to secure an internal application server in Azure.

## Architecture Overview

![Internal app network diagram](diagrams/internal-app-network.jpg)

- Resource group: `az-rg-lab`
- VNet: `az-vnet-lab` (`10.0.0.0/16`)
- Subnets: firewall, management, workload, firewall management
- Azure Firewall Basic with outbound FQDN filtering
- Private DNS zone `az.pdns.lab` with `az-appvm-lab.az.pdns.lab -> 10.0.3.4`
- Admin VM (public IP) and app VM (no public IP)

## Documentation

- [Resource group and VNet](docs/01-resource-group-and-vnet.md)
- [NSGs and virtual machines](docs/02-nsgs-and-vms.md)
- [Private DNS configuration](docs/03-private-dns.md)
- [Azure Firewall and UDR routing](docs/04-firewall-and-udr.md)
- [Validation tests (curl, nslookup, tracepath)](docs/05-validation-tests.md)

## Skills Demonstrated

- Azure networking (VNet, subnets, UDRs)
- Network Security Groups and segmentation
- Azure Firewall Basic rule collections and FQDN allow-listing
- Private DNS for internal-only name resolution
- Linux administration (SSH keys, curl, nslookup, tracepath)



