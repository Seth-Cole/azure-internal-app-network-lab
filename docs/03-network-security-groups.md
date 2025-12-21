# **Netowrk Security Groups**

## **Creating the NSG**
## **Intent**

Using a layered security approach, create NSGs for management and application subnets.

### **SOP**

1.	Navigate to the “Network Security Groups” blade, click “create”
2.	Select pre-created resource group
3.	Name your NSG and select the region
4.	Select “review + create”
5.	Wait for validation and then click “create”
  ![image](/images/03-network-security-groups/03-create-NSG.png)

6. Navigate back to NSG blade and see that resource is created (repeat steps 1 - 5 for additional NSGs)
  ![image](/images/03-network-security-groups/03-workload-NSG-validation.png)


## **Configuring the NSG**
## **Intent**
1. Configure the management subnet to be used as a jumpbox to the application subnet by allowing SSH to the management box from admin public IP 
2. Allow ssh from the management box to the application server.

### **SOP**

1. Navigate to NSG blade
2. Select az-nsg-mgmt-lab NSG
3. In left-hand window select Settings > inbound security rules > in ribbon click add
4. Source will be the IP from which you will remote into admin VM, port will be any (can be specified but leaving any for lab)
5. Our destination will be the management subnet (az-adsn-lab) but can be a specific address or range
6. service is SSH and action is allow
7. Leaving default prio (lower number = higher prio)
8. Naming the rule and creating description for clarity

   ![image](/images/03-network-security-groups/03-management-NSG-allow-ssh.png)
   ![image](/images/03-network-security-groups/03-management-NSG-allow-ssh-2.png)
   
9. Selecting "add" will create the rule
10. Once created you can see it in the rule list on the "inbound security rules" blade

    ![image](/images/03-network-security-groups/03-management-NSG-allow-ssh-confirmation.png)

11.
