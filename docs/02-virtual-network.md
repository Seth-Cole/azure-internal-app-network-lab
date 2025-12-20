# **Creating the Virtual Network**

## **Intent**
Create a virtual network (VNET) to host all the subnets required to manage, secure, and host the application server.

### **SOP**
1.	Opent azure portal
2.	Search for “Virtual Networks” blade
3.	Select “create” on the ribbon
4.	Select subscription and resource group (group previously created)
5.	Name the virtual network
6.	Select region

![image](/images/02-virtual-network/02-create-virtual-network.png)

7. Next select IP addresses from the ribbon and input desired private IP for the VNET
8. Next click add subnet and create the subnets desired (in this case we will create the 3 listed in the diagram)

![image](/images/02-virtual-network/02-create-subnets.png)

9. Once VNET and subnets are created click “Review + create”, and after validation pass click “create”
10. Go back to “Virtual Networks” blade and you’ll see the network has been created

![image](/images/02-virtual-network/02-VNET-confirmation.png)
