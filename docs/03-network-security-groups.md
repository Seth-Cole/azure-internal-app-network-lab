# **Netowrk Security Groups**

## **Creating the NSGs**
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


## **Configuring az-adsn-lab subnet NSG**
## **Intent**

Configure the NSG to use management subnet as a jumpbox to the application subnet by allowing SSH through admin public IP.

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

11. Associate the NSG to the proper subnet by clicking “subnets” on the blade
12.	Click “+Associate” on the riboon
13.	Select the subnet 
14.	Click ok

    ![image](/images/03-network-security-groups/03-management-NSG-subnet-association.png)

## **Configuring az-wlsn-lab subnet NSG**
## **Intent**

Allowing inbound traffic only from the management subnet. Outbound will be restricted to approved sites and will be implemented through routing traffic from app subnet to firewall.

### **SOP**

1. Navigate to NSG blade and select az-nsg-app-lab (workload subnet NSG)
2. Select “Inbound Security Rules” > “add”
3. Source will be admin subnet IP range/any port. Destination will be app workload subnet IP range/any port

   ![image](/images/03-network-security-groups/03-workload-NSG-allow-managementSubnet.png)
   ![image](/images/03-network-security-groups/03-workload-NSG-allow-managementSubnet-2.png)

4. Follow steps 9 - 14 on previous SOP for saving the rule, confirming creation, and associating to the az-wlsn-lab subnet

   ![image](/images/03-network-security-groups/03-workload-NSG-allow-managementSubnet-confirmation.png)
   ![image](/images/03-network-security-groups/03-workload-NSG-subnet-association.png)

