# **Setup Private DNS(PDNS) for VNET**

## **Intent**

Configuring PDNS which allow us to automatically register new nodes, have cross v-net name resolution (if we decide to expand), and allow for security and privacy as its only accessible to the internal network.

### **SOP**

1. Navigate to Private DNS zones blade, click create in the ribbon
2. Follow setup wizard (be sure to select the correct resource group)
3. Create custom name for the zone
4. Navigate to “Virtual Network Links” in the ribbon
   - Name the link (ours will be az-pdnsLink-lab)
   - Select the pre-built vnet
   - Enable auto registration
   - Click create

   ![image](/images/05-private-DNS/05-pdns-link.png)

5. Review + Create > Create > wait for deployment to process.
6. Navigate back to Private DNS zones blade and check for new zone to be listed

   ![image](/images/05-private-DNS/05-PDNS-validation.png)

7. Creating the RecordSet for az-appvm-lab
   - select the private DNS zone from the list.
   - Under DNS management, select record sets.
   - On the ribbon select add
   - name the record set, input private IP of application server
   - click add
   - confirm record is now listed

     ![image](/images/05-private-DNS/05-pdns-A-record-application-vm.png)

8. Testing the name resolution from az-adminvm-lab
   - make sure that both VM’s are started
   - Open PuTTY session on local node
   - login to admin vm
   - nslookup the FQDN with the suffix of the PDNS zone that we created

     ![image](/images/05-private-DNS/05-PDNS-resolve-validation.png)

   - we'll also resolve with FQDN using SSH

     ![image](/images/05-private-DNS/05-PDNS-ssh-using-FQDN.png)
