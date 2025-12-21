# Deploying Basic Firewall and Setting up a User Defined Route(UDR)

## Deploying and Configuring Basic Firewall

## Intent

Deploy Azure Firewall Basic to add a centralized security layer to the VNet and enable controlled outbound traffic management for the application workload subnet.

## SOP

1. Prior to creating the Basic Firewall, create a Public IP resource to assign as the **Management public IP address**. Once created, continue to the next steps.

2. You will also need to create an additional subnet in your VNet. Follow the subnet creation steps in the **02-virtual-network SOP**.
   - When selecting the subnet purpose, choose **Firewall Management** and Azure will auto-populate the required settings.

3. Navigate to the **Firewalls** blade.

4. Click **Create** on the ribbon and follow the setup wizard.

5. Select **Basic** for **Firewall SKU**.

6. Create a **Firewall policy** name.

7. Select **Use existing** when configuring the **Choose a virtual network** section.

8. Assign the previously created Public IP to the **Management public IP address** field.

   ![Azure Firewall creation wizard (Basics tab)](/images/06-firewall-and-udr/06-fw-creation.png)
   ![Azure Firewall creation wizard (VNet + public IP configuration)](/images/06-firewall-and-udr/06-fw-creation(1).png)

9. Select **Review + create** > **Create** to deploy the firewall.

---

## Creating the Routing Table and Defining UDR

## Intent

Force all outbound traffic from the workload subnet through Azure Firewall Basic. This adds an additional layer of security alongside NSGs by allowing only defined outbound destinations and denying everything else.

## SOP

1. Navigate to the **Route tables** blade > select **Create** on the ribbon.

2. Follow the setup wizard, selecting **No** for route propagation.

3. Select **Review + create** > **Create**.

   ![Create route table az-rt-lab](/images/06-firewall-and-udr/06-rt-creation.png)

4. Select the newly created route table from the list.

5. On the route table page, go to **Settings** > **Subnets** and associate the route table to the workload subnet. Select **OK**.

   ![Associate route table to workload subnet az-wlsn-lab](/images/06-firewall-and-udr/06-rt-subnet-association.png)

6. In the route table, go to **Settings** > **Routes**.

7. Select **Add** and configure the default route:
   - **Name:** *(name the route)*
   - **Destination type:** **IP Addresses**
   - **Destination IP addresses/CIDR ranges:** `0.0.0.0/0`
   - **Next hop type:** **Virtual appliance**
   - **Next hop address:** *(the private IP of the firewall)*

   ![Create UDR route (0.0.0.0/0) pointing to firewall private IP](/images/06-firewall-and-udr/06-UDR-creation.png)

8. Test the UDR by running `tracepath` from the application VM using the `-n` flag to confirm the firewall IP appears in the path.

   ![Validate routing through firewall using tracepath from the app VM](/images/06-firewall-and-udr/06-UDR-tracepath.png)

9. Confirm the workload subnet is associated to the route table (and traffic is being forced through the firewall).

   ![Workload subnet shows route table association in VNet subnet view](/images/06-firewall-and-udr/06-firewall-management-subnet-validation.png)

---

## Creating Firewall Policies and testing functionality

## Intent

1. Once the network is created, segmented, and the firewall is deployed, implement firewall policies to secure the application server.
2. Allow only specific outbound destinations:
   - GitHub (version control)
   - Ubuntu archive/security repositories (patching and updates)

## SOP

1. Navigate to the **Firewall Policies** blade and select the policy created during firewall deployment.

2. Select **Rule collections** > **Add** > **Rule collection**.

3. Create firewall rules to enforce outbound control:
   - Explicit **Allow** for GitHub and Ubuntu destinations
   - Explicit **Deny** for all other outbound web traffic

4. Allow rules (GitHub + Ubuntu)
   - Name the rule collection
   - **Collection type:** Application
   - **Priority:** `100` *(must be processed before the deny rule)*
   - **Action:** Allow
   - In the table at the bottom of the page:
     - Add a friendly name
     - Source is the application workload subnet
     - Protocols: HTTP and HTTPS
     - Destination type: FQDN
     - Destination: the allowed FQDN
     - Click **Save**
     - Repeat until all allow FQDNs are entered

   ![Firewall application rule collection: allow GitHub + Ubuntu FQDNs](/images/06-firewall-and-udr/06-firewall-app-rule-allow.png)

5. Deny rule (deny all remaining web traffic)
   - Name the rule collection
   - **Collection type:** Application
   - **Priority:** `101` *(must be processed after the allow rule collection)*
   - **Action:** Deny
   - In the table at the bottom of the page:
     - Add a friendly name
     - Source is the application workload subnet
     - Protocols: HTTP and HTTPS
     - Destination type: FQDN
     - Destination: `*` *(any/all)*
     - Click **Save**

   ![Firewall application rule collection: deny all remaining outbound web traffic](/images/06-firewall-and-udr/06-firewall-app-rule-deny.png)

6. Test the allow rules using `curl` from the application VM.

   ![Validation: curl to allowed FQDNs succeeds](/images/07-validation-and-tests/07-validation-firewall-rule-allow.png)
   ![Validation: additional successful allow-rule output](/images/07-validation-and-tests/07-validation-firewall-rule-allow(1).png)

7. Test the deny rule using `curl` from the application VM.

   ![Validation: curl to non-allowed FQDNs fails (deny rule enforced)](/images/07-validation-and-tests/07-validation-firewall-rule-deny(1).png)
