# **azure-internal-app-network-lab**

Using resource groups, a virtual network with subnets, public IPs, Private DNS, user-defined routes (UDRs), and Azure Firewall/NSGs to secure an internal application server in Azure.

## **Architecture Overview**

![Internal app network diagram](/diagrams/WebAppDiagram.jpg)

- **Resource group:** `az-rg-lab`
- **Virtual network:** `az-vnet-lab` (`10.0.0.0/16`)
- **Subnets:**
  - `AzureFirewallSubnet` (`10.0.1.0/26`) – Azure Firewall data plane
  - `az-adsn-lab` (`10.0.2.0/24`) – administrative / jumpbox subnet
  - `az-wlsn-lab` (`10.0.3.0/24`) – application workload subnet
  - `AzureFirewallManagementSubnet` (`10.0.4.0/26`) – firewall management plane
- **Azure Firewall Basic** with outbound FQDN allow-listing for the app subnet
- **Network Security Groups** for both admin and workload subnets
- **Azure Private DNS** zone `az.pdns.lab` with a custom A record for the app VM
- **Virtual machines:**
  - `az-adminvm-lab` – admin VM with public IP for SSH from trusted IPs
  - `az-appvm-lab` – internal app server with **no public IP**

## Documentation

- [Resource group](docs/01-resource-group.md)
- [Virtual Network and Subnets](docs/02-virtual-network.md)
- [Network Security Groups (NSG)](docs/03-network-security-groups.md)
- [Virtual Machines](docs/04-management-and-application-VM.md)
- [Private DNS (PDNS)](docs/05-private-DNS.md)
- [Firewall and User Defined Routes](docs/06-firewall-and-udr.md)
- [Validation and tests](docs/07-validation-and-tests.md)

## Skills Demonstrated

- Azure networking (VNet, subnets, UDRs)
- Network Security Groups and segmentation
- Azure Firewall Basic rule collections and FQDN allow-listing
- Private DNS for internal-only name resolution
- Linux administration (SSH keys, curl, nslookup, tracepath)



