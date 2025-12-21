# Creating the Resource Group

## Intent

Keep all resources needed for the lab in a single resource group for easier management and simple cleanup (delete the group when the lab is no longer needed).

## SOP

1. Log in to the **Azure portal**.

2. In the left-hand menu (or search bar), navigate to **Resource groups** and select **Create**.

3. Under **Project details**, choose your subscription  
   - Example: **Azure for Students**

4. Under **Resource group**, enter a name for the resource group  
   - Example: `az-rg-lab`

5. Select the **Region** for the lab  
   - Example: **(US) West US 2**  
   - Using a single region keeps latency low and pricing predictable.

6. Click **Review + create**, then click **Create**.

   ![Create a resource group in the Azure portal](/images/01-resource-group/Create-Resource-Group.png)

7. After deployment completes, return to the **Resource groups** blade and refresh. Confirm that the new resource group appears in the list.

   ![Resource group successfully created](/images/01-resource-group/verify-resource-group.png)
