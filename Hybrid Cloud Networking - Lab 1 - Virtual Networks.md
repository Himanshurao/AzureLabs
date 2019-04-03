# Lab 1 - Azure VM & VNet Peering
 
## Lab Summary
>> Creating multiple Azure Vnets & Subnets
>> Creating Azure Subnets 
>> Deploying Test VMs in each Subnets 
>> Enabling Azure Vnet Peering 
>> Testing connectivity between Azure Vnets
>> Adding  Data Disks, Enabling Caching & initializing in VM. 
>> Detaching Data Disks & Deleting it 
>> Cleaning up Resources

### Create three virtual networks
1.	Log in to the Azure portal at https://portal.azure.com and 	click on **+Create a resource**  on the upper left corner of the Azure portal.
2.	Select **Networking**, and then select **Virtual network**.
3.	Enter or select the following information, accept the defaults for the remaining settings, and then select **Create**:
    * Name: **vNet1**
    * Address Space: **10.1.0.0/16**
    * Resource Group: *Create New* **myVNets**
    * Location: *Choose a consistent and supported location*
    * Subnet Name: **subnet1**
    * Subnet address range: **10.1.1.0/24**

Repeat the steps above for vNet2:
* Name: **vNet2**
* Address Space: **10.2.0.0/16**
* Resource Group: **myVNets**
* Location: *Choose a consistent and supported location*
* Subnet Name: **subnet2**
* Subnet address range: **10.2.2.0/24**

 
### Create Two virtual machines

1.	Select **+ Create a resource** found on the upper left corner of the Azure portal.
2.	Select **Compute** and then select **Windows Server 2016 Datacenter**.
3.	Enter or select the following information, accept the defaults for the remaining settings:
    * Resource Group: *Create new*: **MyVMs**
    * Name: **VM1**
    * Region: *Choose a consistent and supported Region*
    * Size: *Change to **DS1_v2***
    * Username: pick a username
    * Password: pick a complex password (I recommend *Complex.Password*)
    * Confirm Password: pick a complex password  (I recommend *Complex.Password*)
    * Public inbound ports:  Open RDP, 3389
    * Select **Next:Disks** : Dont attach any Data Disk for now.
4.	Click **Next: Networking**.
5.	Set the virtual network to **vNet01** and then select **Next: Management >**.
6.	Under Diagnostic storage account click **Create new** and enter  *yourinitials* *shortdate* and ensure the name resolves (e.g. abc1009), click **OK**, and then click **Next: Guest config >**.
7.	Review the items and then click **Next: Tags >**.
8.	Review the items and then click **Next: Review + create .**.
9.	Once validation passes click **Create**.

#### Create the second VM
Complete the previous steps but use the following information:
1.* Resource Group: MyVMs
* Name: **VM2**
* Region: *Choose a consistent supported Region*
* Size: Change to **DS1_v2**
* Username: pick a username
* Password: pick a complex password
* Confirm Password: pick a complex password
* Public inbound ports: Open RDP, 3389
* Select **Next:Disks >** Dont attach any Data Disk for now.
* Click **Next: Networking >**
* Set the virtual network to vNet2 and then select **Next: Management >**
* Under Diagnostic storage account use the previously created Diagnostics storage account and then click **Next: Guest config >**.
* Review the items and then click **Next: Tags >**.
* Review the items and then click **Next: Review + create >**.
* Once validation passes click **Create**.

You now have two virtuals machines each in their own subnet and virtual network. Let's validate that.

1. From the Azure Dashboard, select **All services**.
2. Under **Networking**, select the following:
    * Application gateways
    * Network security groups
3. Under **Management + Governance** select the following:
    * Network Watcher
4. Click on **Monitor** from the left hand pane.
5. Under **Insights** select **Network**, then under **Monitoring** choose **Topology**.
6. User **Resource Group** select **MyVNets**.  In a moment a conceptual network diagram should be generated showing all two vNets and subnets.


### Connect to a VM and test connectivity
Before you begin this section, obtain the private and public IP addresses of VM1, VM2, and VM3.

1.	At the top of the Azure portal, enter **VM1**. When VM1 appears in the search results, select it. Select the **Connect** button.
 
2.	After selecting the Connect button, click on **Download RDP file**. 
3.	If prompted, select **Connect**. Enter the user name and password you specified when creating the VM. You may need to select **More choices**, then **Use a different account**, to specify the credentials you entered when you created the VM.
4.	Select **OK**.
5.	Click Yes on the Networks blade.
6.	From PowerShell, enter *ping vm2* (use vm2 Private IP -Copy from VM overview page of portal). Ping fails, why is that? **Each virtual network is isolated from other virtual networks.** 
7. To allow VM1 to ping other VMs in a later step, enter the following command from PowerShell, which allows ICMP inbound through the Windows firewall:
_New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4_.
8. Repeat these steps (connect to the VM and issue the PowerShell command) for VM2 and VM3.



### Connect virtual networks with virtual network peering using the Azure portal
You can connect virtual networks to each other with virtual network peering. These virtual networks can be in the same region or different regions (also known as Global VNet peering). Once virtual networks are peered, resources in both virtual networks are able to communicate with each other, with the same latency and bandwidth as if the resources were in the same virtual network. 

#### Peer virtual networks
1. In the Search box at the top of the Azure portal, begin typing **vNet1**. When vNet1 appears in the search results, select it.
2. Select **SETTINGS** then **Peerings**, and then select **+ Add**.
3. Enter, or select, the following information, accept the defaults for the remaining settings, and then select **OK**.
    * Name: **vNet1-vNet2**
    * Virtual network: **vNet2 (MyVNets)**
    * Configure virtual network access settings: **Enabled**
    * Configure forwarded traffic settings: **Disabled**
4. Peering status - If you don't see the status, refresh your browser.  Notice the status is *Initiated*.

In the Search box at the top of the Azure portal, begin typing **vNet2**. When **vNet2** appears in the search results, select it.

Complete steps 2-3 again, with the following changes, and then select **OK**:
* Name: **vNet2-vNet1**
* Virtual network: **vNet1 (MyVNets)**
* Configure virtual network access settings: **Enabled**
* Configure forwarded traffic settings: **Disabled**

Peering status - If you don't see the status, refresh your browser.  Notice the status is *Connected*.  
 Azure also changed the peering status for the vNet1-vNet2 peering from Initiated to Connected. Virtual network peering is not fully established until the peering status for both virtual networks is Connected.

 ### Test connectivity
 1. At the top of the Azure portal, enter **VM1**. When VM1 appears in the search results, select it. Select the **Connect** button.
 2. After selecting the Connect button, click on **Download RDP file**.
 3. If prompted, select **Connect**. Enter the user name and password you specified when creating the VM. You may need to select More choices, then Use a different account, to specify the credentials you entered when you created the VM. Select **OK**.
 4. Click **Yes** on the Networks blade.
5. From PowerShell, enter *ping vm2 private IP* . Ping succeeds, why is that? 

Let's examine our network topology now that we have peering enabled.
1.  Click on **Monitor** from the left hand pane.
2. Under **Insights** select **Network**, then under **Monitoring** choose **Topology**.
3. User **Resource Group** select **MyVNets**.  In a moment a conceptual network diagram should be generated showing all three vNets and subnets including the new peerings between vNet1 and vNet2.

 ### Add/Attach New Data Disk 
1. In the Azure portal, from the menu on the left, select Virtual machines.
2. Select VM1as virtual machine from the list.
3. On the Virtual machine page, select Disks.
4. On the Disks page, select Add data disk.
5. In the drop-down for the new disk, select Create disk.
6. In the Create managed disk page, type in a name for the disk and adjust the other settings as necessary. When you're done, select Create.
7. In the Disks page, select Save to save the new disk configuration for the VM.
8. After Azure creates the disk and attaches it to the virtual machine, the new disk is listed in the virtual machine's disk settings under Data disks.
 ### Detach a data disk using the portal
1. In the Azure portal, from the menu on the left, select Virtual machines.
2. Select VM1 as virtual machine from the list.
3. On the Virtual machine page, select Disks.
4. In the Disks pane, to the far right of the data disk that you would like to detach, click the Detach button image detach button.
5. After the disk has been removed, click Save on the top of the pane.
6. In the virtual machine pane, click Overview and then click the Start button at the top of the pane to restart the VM.

### Clean up resources 
When no longer needed, delete the resource group and all of the resources it contains:

1. Enter myResourceGroup in the Search box at the top of the portal. When you see myResourceGroup in the search results, select it.
2. Select Delete resource group.
3. Enter myResourceGroup for TYPE THE RESOURCE GROUP NAME: and select Delete.
Perform this steps for all resource group one by one which got created during this lab to clean up resources. 
