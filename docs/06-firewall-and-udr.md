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
   
9. Revew + Create > Create to deploy the firewall


## **Creating the Routing Table and Defining UDR**

## **Intent**
Route all traffic through f Basic Firewall which will serve as an extra layer of security with NSG's in place. Allowing the application server to access defined websites and exclude all other external access.

### **SOP**

1. Navigate to Route Tables blade > select route tables on the side bar > select create on the ribbon
2. Follow setup wizard, selecting no on propagation
3. Review + Create > Create

   ![image](/images/06-firewall-and-udr/06-rt-creation.png)

4. Select newly created RT from the list on the Route tables blade
5. On the overview page of the RT go to settings > subnets > associating it to the workload subnet, click ok.

   ![image](/images/06-firewall-and-udr/06-rt-subnet-association.png)
   
6. Select the RT from the list on the RT blade
7. Under settings go to Routes
8. Follow setup wizard
   - Namie the route
   - Destination type will be address and the IP addresses will be any or 0.0.0.0/0
   - Next hop type will be virtual appliance and the address will be the private IP of the firewall

   ![image](/images/06-firewall-and-udr/06-UDR-creation.png)

9. Testing UDR by sending tracepath from application VM using the -n flag to confirm we see firewall IP

   ![image](/images/06-firewall-and-udr/06-UDR-tracepath.png)

10. Confirms that traffic is being routed through AzureFirewallSubnet
   
   ![image](/images/06-firewall-and-udr/06-firewall-management-subnet-validation.png)
   

## **Creating Firewall Policies and testing functionality**

## **Intent**

1. Once network is created, segmented, and firewall deployed we implement firewall policies to secure the application server.
2. Allowing two connections, GitHub for version control and archive/security Ubuntu for patching and updates.

### **SOP**

1. Navigate to Firewall Policies blade, Select the policy created during firewall creation
2.	Select rule collections > add > rule collection
3.	Creating 3 rules
   - Explicit allow for GitHub and Ubunutu websites
   - Explicit deny for everything else
5.	Allow rules (all 3)
   - Name the rule
   - Collection type is application
   - prio is 100 (want these rules to have more prio than the explicit deny)
   - Action is allow
   - In the Table at bottom of the page
     - Add friendly name
     - Source is the application workload subnet
     - Protocols HTTP, and HTTPS
     - Destination type is FQDN
     - Destination is URL to website
     - click save
     - repeat until all rules are entered

   ![image](/images/06-firewall-and-udr/06-firewall-app-rule-allow.png)

6. Deny Rule
   - Name the rule
   - Collection type is application
   - prio is 101 (want this rule parsed after the allow rules)
   - Action is deny
   - In the Table at bottom of the page
     - Add friendly name
     - Source is the application workload subnet
     - Protocols HTTP, and HTTPS
     - Destination type is FQDN
     - Destination is * (or any/all)
     - click save
   
  ![image](/images/06-firewall-and-udr/06-firewall-app-rule-deny.png)

7. Testing the allow rules using curl in the application VM

   ![image](/images/07-validation-and-tests/07-validation-firewall-rule-allow.png)
   ![image](/images/07-validation-and-tests/07-validation-firewall-rule-allow(1).png)

8. Testing the deny rule using curl in the application VM

   ![image](/images/07-validation-and-tests/07-validation-firewall-rule-deny(1).png)
