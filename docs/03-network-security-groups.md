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
