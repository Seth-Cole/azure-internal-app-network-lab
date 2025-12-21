# **Creating VM's for administrator subnet (az-adsn-lab) and Pseudo server for workload subnet (az-wlsn-lab)**

## **Creating VM on admin subnet (az-adsn-lab)**

## **Intent**

Using the VM on admin subnet as a jumpbox to workload subnet to perform administrative tasks to the application server

### **SOP**

1. navigate to virtual machine blade and click “create” in the ribbon, select virtual machine
2. Follow setup wizard to create the VM
3. Near the bottom you will see a section labeled “Administrator Account”, we are going to select SSH public key and name the pair
4. Inbound ports will be 22 for SSH

   ![image](/images/04-management-and-application-VM/04-admin-vm-creation.png)

5. Select the networking blade from the ribbon and confirm that the VM is associated to the proper network/subnet
   - Note that we are creating a public IP for this VM, so that we can reach the jumpbox from external resource

   ![image](/images/04-management-and-application-VM/04-admin-vm-subnet-assignment.png)

6. Make sure to save the private key pair and wait for VM to be deployed to grab public IP address, from here we will test connection to the jumpbox
7. Public IP can be viewed from the created VM's overview page
8. Use PuttyGen to convert the private key to use with PuTTY
9. Open PuTTY, expand SSH -> Auth -> Credentials.
    - Select Browse beside the “Private Key file for authentication”
    - select the key created by PuTTYGen
10.	Navigate back to “Session” in PuTTY and input VM public IP, this will open SSH window and you can login with credentials

    ![image](/images/04-management-and-application-VM/04-admin-vm-ssh-validation.png)


## **Creating Pseudo Application Server in workload subnet (az-wlsn-lab)**

## **Intent**

Creating a VM that would represent a server that hosts companies internal application

### **SOP**

1. Navigate to Virtual Machines Blade
2. Follow setup wizard in Basics tab on ribbon then navigate to Networking tab.
3. Ensure that this VM doesn’t have a Public IP, select none in the Public IP section
4. Also make sure that it’s assigned to the workload subnet
  - If unsure follow previous SOP step 5

    ![image](/images/04-management-and-application-VM/04-app-vm-subnet-assignment.png)

5. Create VM and navigate back to VM blade to confirm both VM's are created

   ![image](/images/04-management-and-application-VM/04-VM-creation-validation.png)


