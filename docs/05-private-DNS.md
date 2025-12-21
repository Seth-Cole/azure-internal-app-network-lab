# Setup Private DNS (PDNS) for VNet

## Intent

Configure a Private DNS zone so internal hosts can resolve the application VM by name (no public DNS required), keeping name resolution private to the virtual network.

## SOP

1. In the **Azure portal**, search for **Private DNS zones** and select **Create**.

2. On the **Basics** tab, select:
   - **Resource group:** `az-rg-lab`
   - **Name:** `az.pdns.lab`

3. Select **Review + create**, then select **Create** and wait for deployment to complete.

4. Open the new Private DNS zone (`az.pdns.lab`), then select **Virtual network links** and choose **Add**.
   - **Link name:** `az-pdnsLink-lab`
   - **Virtual network:** `az-vnet-lab`
   - Select **Create**.

   ![Create a virtual network link for the Private DNS zone](/images/05-private-DNS/05-pdns-link.png)

5. Wait for the virtual network link to deploy, then confirm the link is listed under **Virtual network links**.

6. Navigate back to the **Private DNS zones** blade and confirm `az.pdns.lab` is listed.

   ![Private DNS zone successfully created and listed](/images/05-private-DNS/05-PDNS-validation.png)

7. Create an **A record** for the application VM:
   - Open `az.pdns.lab` from the list.
   - Under **DNS management**, select **Record sets**.
   - Select **+ Record set** / **Add**.
   - **Name:** `az-appvm-lab`
   - **Type:** `A`
   - **IP address:** `10.0.3.4`
   - Select **Add**, then confirm the record is now listed.

   ![Create an A record for the application VM](/images/05-private-DNS/05-pdns-A-record-application-vm.png)

8. Test name resolution from `az-adminvm-lab`:
   - Confirm both VMs are running.
   - From your local machine, open a PuTTY session and sign in to `az-adminvm-lab`.
   - Run:
     - `nslookup az-appvm-lab.az.pdns.lab`
   - Confirm the name resolves to `10.0.3.4`.

   ![Validate Private DNS resolution using nslookup from the admin VM](/images/05-private-DNS/05-PDNS-resolve-validation.png)

   - Verify connectivity using SSH via FQDN:
     - `ssh testuser@az-appvm-lab.az.pdns.lab`

   ![SSH to the application VM using the Private DNS FQDN](/images/05-private-DNS/05-PDNS-ssh-using-FQDN.png)
