# azure-internal-app-network-lab

Using resource groups, a virtual network with subnets, public IPs, Azure Private DNS, user-defined routes (UDRs), and Azure Firewall/NSGs to secure an internal application server in Azure.

## Architecture Overview

![Internal app network diagram](https://github.com/Seth-Cole/azure-internal-app-network-lab/blob/main/diagrams/WebAppDiagram.jpg)

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

## Design Decisions

- `az-appvm-lab` has **no public IP** to keep the workload internal-only.
- Administrative access is restricted to `az-adminvm-lab` (jumpbox pattern).
- The workload subnet (`az-wlsn-lab`) is forced through **Azure Firewall** using a UDR (`0.0.0.0/0` → firewall private IP).
- Outbound web access from the workload subnet is **allow-listed by FQDN** (GitHub + Ubuntu repositories) and denied for everything else.
- Azure Private DNS (`az.pdns.lab`) provides internal name resolution so the admin VM can reach the app VM by FQDN.

## Validation Summary

Key checks are documented (with screenshots) in [Validation and tests](docs/07-validation-and-tests.md):

- From `az-adminvm-lab`, `nslookup az-appvm-lab.az.pdns.lab` resolves to `10.0.3.4`, and SSH by FQDN succeeds.
- `az-appvm-lab` has **no public IP** and cannot be reached directly from the internet.
- Workload VM effective routes show `0.0.0.0/0` routed to the firewall (forced egress).
- `curl` validation confirms allow-listed destinations succeed and non-allowed destinations fail.

## Documentation

- [Resource group](docs/01-resource-group.md)
- [Virtual Network and Subnets](docs/02-virtual-network.md)
- [Network Security Groups (NSG)](docs/03-network-security-groups.md)
- [Virtual Machines](docs/04-management-and-application-VM.md)
- [Private DNS (PDNS)](docs/05-private-DNS.md)
- [Firewall and User Defined Routes](docs/06-firewall-and-udr.md)
- [Validation and tests](docs/07-validation-and-tests.md)

## Skills Demonstrated

- Azure networking (VNet, subnets, UDRs, route association)
- Network Security Groups and segmentation
- Azure Firewall Basic rule collections and FQDN allow-listing
- Private DNS for internal-only name resolution
- Linux administration (SSH, curl, nslookup, tracepath)

## Notes

- Azure Firewall Basic is a paid resource. All lab components are deployed into a single resource group (`az-rg-lab`) so the environment can be removed in one teardown.
- Public IP addresses are intentionally redacted in screenshots and documentation.
- Host key fingerprints shown during first-time SSH are expected for temporary lab VMs.

## Future Improvements

- Add IaC deployment (Bicep or Terraform) for repeatable provisioning.
- Enable firewall diagnostics (Log Analytics) and include examples of allow/deny logs.
- Replace the jumpbox public IP approach with an Azure Bastion variant (no public IPs on VMs).
