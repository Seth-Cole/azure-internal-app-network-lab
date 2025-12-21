# **Deploying Basic Firewall and Setting up a User Defined Route(UDR)**

## **Deploying and Configuring Basic Firewall**

## **Intent**

Deploying the Basic Firewall for an layered security, bolstering the security posture of the VNET. Allowing traffic management to and from the application workload subnet.

### **SOP**

1. Prior to creating the Basic Firewall you'll need to create a public IP to assign as the "Management Public IP address", once created continue to next steps
2. You will also need to create an additional subnet in your VNET, follow subnet creation steps in 02-virtual-network SOP. When selecting subnet purpose choose "Firewall Management" and it will auto populate
3. Navigate to Firewalls blade
4. Click create on the ribbon and follow the setup wizard
5. Select Basic Firewall in "Firewall SKU"
6. Create a policy name
7. Select "Use existing" when configuring the "Choose a virtual network" section
8. Assign the previously created public IP to the "Management public IP address" section

   ![image](/images/06-firewall-and-udr/06-fw-creation.png)
   ![image](/images/06-firewall-and-udr/06-fw-creation(1).png)
