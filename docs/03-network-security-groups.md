# **Creating NSGs**

## **Intent**

Using a layered security approach, create NSGs for management and application subnets. Using the management subnet as a jumpbox to the application subnet, allow SSH to the management box from admin public IP and allow ssh from the management box to the application server.

### **SOP**

1.	Navigate to the “Network Security Groups” blade
2.	Click “create”
3.	Select pre-created resource group
4.	Name your NSG and select the region
5.	Select “review + create”

![image](/images/03-network-security-groups/03-create-NSG.png)

