# Imersao Azure Expert Online

Hands-on Lab

## Day 1
## Exercise #01 - Azure Calculator (15 minutes)

1. In a browser, navigate to the [Azure Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/) webpage.

2. Add the inventory data.

   ![Screenshot of the inventory data](/AllFiles/Images/IMG01.png)

**Important Notes**
- Development environment licensing will be purchased by Marketplace
- Include Reserved instances and Azure Hybrid Benefit in the Production Environment 
- Include VPN Gateway, Load Balancers, Public IP Address and 500 GB Outbound Data Transfer.

3. Scroll to the bottom of the Azure Pricing Calculator webpage to view total Estimated monthly cost.

4. Change the currency to Brazilian Real (R$), then select Export to download a copy of the estimate for offline viewing in Microsoft Excel (.xlsx) format.

## Lab #01 - Resource Groups (15 minutes)

1. Sign in to the [Azure portal](https://portal.azure.com).

1. Search for and select **Resource groups**. 

1. On the **Resource groups** blade, click **+ Create** and create a resource group with the following settings:

    |Setting|Value|
    |---|---|
    |Subscription| the name of the Azure subscription you will use in this lab |
    |Resource Group| **RG-TAE-VMs**|
    |Region| East US 2 |
    |Tags| project: azureexpert |
    | | |

1. Repeat and create the Resources groups name "RG-TAE-Networks" and "RG-TAE-Storage".

1. Click **Review + Create** and then click **Create**.

1. Explore properties to Resource groups.

## Lab #02 - Virtual Networks (10 minutes)

1. In the Azure portal, search for and select **Virtual networks**, and, on the **Virtual networks** blade, click **+ Create**.

1. Create a virtual network with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab |
    | Resource Group | the name of a resource group **RG-TAE-Networks** |
    | Name | **VNET-TAE-Hub** |
    | Region | the name of any Azure region available in the subscription you will use in this lab |
    | IPv4 address space | **10.1.0.0/16** |
    | Subnet name | **Default** |
    | Subnet address range | **10.1.1.0/24** |
     | | |
      
1. Accept the defaults and click **Review and Create**. Let validation occur, and hit **Create** again to submit your deployment.

1. Once the deployment completes browse for **Virtual Networks** in the portal search bar. Within **Virtual networks** blade, click on the newly created virtual network.

1. On the virtual network blade, click **Subnets** and then click **+ Subnet**. 

1. Create a subnet with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **FrontEnd** |
    | Address range (CIDR block) | **10.1.100.0/24** |
    | Network security group | **None** |
    | Route table | **None** |
    | | |

## Lab #03 - Virtual Machine (20 minutes)

1. In the Azure portal, search for and select **Virtual machines** and, on the **Virtual machines** blade, click **+ Create**.

1. On the **Basics** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab |
    | Resource group | the name of a new resource group **RG-TAE-VMs** |
    | Virtual machine name | **VMHUB01** |
    | Region | select same region the Resouce group | 
    | Availability options | **Availability sets** |
    | Availability set | **AS-VM** |
    | Image | **Windows Server 2019 Datacenter - Gen1** |
    | Azure Spot instance | **No** |
    | Size | **Standard_B2s** |
    | Username | **admaz** |
    | Password | **Azur3Exp3rt*** |
    | Public inbound ports | **RDP (3389) and HTTP (80)** |
    | Would you like to use an existing Windows Server license? | **No** |
    | | |

1. Click **Next: Disks >** and, on the **Disks** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | OS disk type | **Standard SSD** |
    | Enable Ultra Disk compatibility | **No** |

1. Click **OK** and, back on the **Networking** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Virtual Network | **VNET-TAE-HUB** |
    | Subnet | **FrontEnd** |
    | Public IP | **VMHUB01-PI** |
    | NIC network security group | **Basic** |
	| Inbound Ports | **RDP (3389)** and HTTP (80)|
    | Place this virtual machine behind an existing load balancing solution? | **No** |
    | | |

1. Click **Next: Management >** and, on the **Management** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Boot diagnostics | **Enable with custom storage account** |
    | Diagnostics storage account | create new |
    | Properties storage account | Name: **sataediag###**, Account kind: StorageV2, Performance: Standard, Replication: Locally-redundant-storage (LRS) |
    | Enable auto-shutdown | off    
    
1. Click **Next: Advanced >**, on the **Advanced** tab of the **Create a virtual machine** blade, review the available settings without modifying any of them, and click **Review + Create**.

1. On the **Review + Create** blade, click **Create**.

1. Connect Virtual machine.

1. On the virtual machine blade, click **Disks**, Under **Data disks** click **+ Create and attach a new disk**. 

1. Create a managed disk with the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Disk name | **VMHUB01-DataDisk01** |
    | Source type | **None** |
    | Account type | **Standard HD** |
    | Size | **50 GiB** |
    | | |

1. Connect Virtual machine and start disk.

1. Install the Web-Server feature in the virtual machine by running the following command in the **Administrator Windows PowerShell** command prompt. You can copy and paste this command.

   ```powershell
   Install-WindowsFeature -name Web-Server -IncludeManagementTools
   ```
1. Back in the portal, navigate back to the Overview blade of myVM and, use the Click to clipboard button to copy the public IP address, open a new browser tab, paste the public IP address into the URL text box, and press the Enter key to browse to it.

1. Explore properties to Virtual machines.

## Lab #04 - Configure Azure DNS for internal name resolution (15 minutes)

1. In the Azure portal, search for and select **Private DNS zones** and, on the **Private DNS zones** blade, click **+ Create**.

1. Create a private DNS zone with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource Group | **RG-TAE-Networks** |
    | Name | **azureexpert.corp** |

1. Click Review and Create. Let validation occur, and hit Create again to submit your deployment.

    >**Note**: Wait for the private DNS zone to be created. This should take about 2 minutes.

1. Click **Go to resource** to open the **azureexpert.corp** DNS private zone blade.

1. On the **azureexpert.corp** private DNS zone blade, in the **Settings** section, click **Virtual network links**

1. Click **+ Add** to create a virtual network link with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Link name | **VNL-Hub** |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **VNET-TAE-Hub** |
    | Enable auto registration | enabled |

1. Click **OK**.

    >**Note:** Wait for the virtual network link to be created. This should take less than 1 minute.

1. On the **azureexpert.corp** private DNS zone blade, in the sidebar, click **Overview**

1. Verify that the DNS records for **VMHUB01** appear in the list of record sets as **Auto registered**.

    >**Note:** You might need to wait a few minutes and refresh the page if the record sets are not listed.

1. Switch to the RDP session to **VMHUB01**.

1. In the Terminal console, run the following to test internal name resolution of the **VMHUB01** DNS record set in the newly created private DNS zone:

   ```powershell
   nslookup vmhub01.azureexpert.corp
   ```

1. Verify that the output of the command includes the private IP address of **VMHub01**.

## Lab #05 - Network Security groups (15 minutes)

1. In the Azure portal, search for and select **Network security groups**, and, on the **Network security groups** blade, click **+ Create**.

1. Create a network security group with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource Group | **RG-TAE-Networks** |
    | Name | **NSG-WEB** |
    | Region | the name of the Azure region where you deployed all other resources in this lab |

1. On the deployment blade, click **Go to resource** to open the **NSG-WEB** network security group blade. 

1. On the **NSG-WEB** network security group blade, in the **Settings** section, click **Inbound security rules**. 

1. Add an inbound rule with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Source | **Any** |
    | Source port ranges | * |
    | Destination | **Any** |
    | Destination port ranges | **80,443** |
    | Protocol | **TCP** |
    | Action | **Allow** |
    | Priority | **200** |
    | Name | **Allow-Port_80-443** |

1. Go to the Azure portal to view your **Network security groups**. Search for and select Network security groups is created.

    >**Note**: If another Network security groups rule is already attached to the Network interface, it is necessary to remove it.

1. On the **NSG-WEB** network security group blade, in the **Settings** section, click **Network interfaces** and then click **+ Associate**.

1. Associate in the **VMHUB01** network security group with the **Network interface**.

    >**Note**: It may take up to 5 minutes for the rules from the newly created Network Security Group to be applied to the Network Interface Card.

1. In the menu bar of the network security group, under Settings, you can view the Inbound security rules, Outbound security rules, Network interfaces, and Subnets that the network security group is associated to.

1. Under **Support + troubleshooting**, you can view Effective security rules.

1. Navigate back to your computer.

1. In the Virtual Machine Connection window, start Windows PowerShell and, in the **Administrator: Windows PowerShell** window run the following to set connection test. 

   ```powershell
   Test-NetConnection -ComputerName PUBLICIPADDRESS-VMHUB01 -Port 80 -InformationLevel 'Detailed'
   ```
1. Examine the output of the command and verify that the connection was successful.

1. Within the computer, start Browser and navigate to **PUBLICIPADDPRESS-VM**.

1. Examine the navegate was successful.

## Lab #06 - Azure Storage Blobs (20 minutes)

1. In the Azure portal, search for and select **Storage accounts** and, on the **Storage accounts** blade, select **+ Create**.

1. On the **Basics** tab of the **Create storage account** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group | the name of a new resource group **RG-TAE-Storage** |
    | Storage account name | any globally unique name between 3 and 24 in length consisting of letters and digits **sataeblob###** |
    | Location | the name of an Azure region where you can create an Azure Storage account  |
    | Performance | **Standard** |
    | Account kind | **StorageV2 (general purpose v2)** |
    | Replication | **Locally redundant storage (LRS)** |

1. Select **Next: Networking >**, on the **Networking** tab of the **Create storage account** blade, review the available options, accept the default option **Public endpoint (all networks}** and select **Next: Data protection >**.

1. On the **Data protection** tab of the **Create storage account** blade, review the available options, accept the defaults, select **Next: Advanced >**.

1. On the **Advanced** tab of the **Create storage account** blade, review the available options, accept the defaults, select **Review + Create**, wait for the validation process to complete and select **Create**.

1. On the Storage account blade, in the Blob service section, click Containers.

1. Select **Create Blob Container**, and use the empty text box to set the container name to **azureexpert**.

1. Select **azureexpert**, in the **azureexpert** pane, select **Upload**, and in the drop-down list, select **Upload Files**.

1. In the **Upload Files** window, select the ellipsis button next to the **Selected files** label, in the **Choose files to upload** window, select **files** select random files your computer, and select **Open**.

1. Back in the **Upload Files** window, select **Upload**

1. Within the Remote Desktop session to **Virtual machine**, in the Server Manager window, select **Local Server**, select the **On** link next to the **IE Enhanced Security Configuration** label, and, in the **IE Enhanced Security Configuration** dialog box, select both **Off** options.

1. Within the Remote Desktop session, start Browser and navigate to the download page of [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/)

1. Within the Remote Desktop session, download and install Azure Storage Explorer with the default settings. 

1. Navigate to the [Azure portal](https://portal.azure.com), and sign-in by providing credentials of the user account with the Owner role in the subscription you are using in this lab.

1. Navigate to the blade of the newly created storage account, select **Access keys** and review the settings of the target blade.

1. On the storage account blade, select **Shared access signature** and review the settings of the target blade.

1. On the resulting blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Allowed services | **Blob** |
    | Allowed resource types | **Service**, **Container** and **Object** |
    | Allowed permissions | **Read**, **List** |
    | Blob versioning permissions | disabled |
    | Start | 24 hours before the current time in your current time zone | 
    | End | 24 hours after the current time in your current time zone |
    | Allowed protocols | **HTTPS only** |
    | Signing key | **key1** |

1. Select **Generate SAS and connection string**.

1. Copy the value of **Blob service SAS URL** into Clipboard.

1. Within the Remote Desktop session, start Azure Storage Explorer. 

1. In the Azure Storage Explorer window, in the **Connect to Azure Storage** window, select **Use a shared access signature (SAS) URI** and select **Next**.

1. In the **Attach with SAS URI** window, in the **Display name** text box, type **sataeblob###**, in the **URI** text box, paste the value you copied into Clipboard, and select **Next**. 

    >**Note**: This should automatically populate the value of **Blob endpoint** text box.

1. In the **Connection Summary** window, select **Connect**. 

1. Select **Storage account" and "Blob containers", Open and download uploaded files

1. Leave the Azure Storage Explorer window open.

## Lab #07 - Azure Files (15 minutes)

1. In the Azure portal, search for and select **Storage accounts** and, on the **Storage accounts** blade and create a new Storage account **sataefiles**, in the **File service** section, click **File shares**.

1. Click **+ File share** and create a file share with the following settings:

    | Setting | Value |
    | --- | --- |
    | Name | **fs-azureexpert** |
    | Quota | **1024** |
    | Tiers | hot |

1. Click the newly created file share and click **Connect**.

1. On the **Connect** blade, ensure that the **Windows** tab is selected, and click **Copy to clipboard**.

1. In the Azure portal, search for and select **Virtual machines**, and, in the list of virtual machines.

1. In the Virtual Machine Connection window, start Windows PowerShell and, in the **Administrator: Windows PowerShell** window run the following to set map share. 

1. Verify that the script completed successfully. 

1. Connect share and upload files.

## Lab #08 - Azure VNET Peering (30 minutes)

1. In the Azure portal, search for and select **Virtual networks**, and, on the **Virtual networks** blade, click **+ Createt**.

1. Create a virtual network with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab |
    | Resource Group | the name of a resource group **RG-TAE-Spoke1** |
    | Name | **VNET-TAE-Spoke1** |
    | Region | west us 2 |
    | IPv4 address space | **10.10.0.0/16** |
    | Subnet name | **Default** |
    | Subnet address range | **10.10.1.0/24** |
     | | |
      
1. Accept the defaults and click **Review and Create**. Let validation occur, and hit **Create** again to submit your deployment.

1. Once the deployment completes browse for **Virtual Networks** in the portal search bar. Within **Virtual networks** blade, click on the newly created virtual network.

1. In the Azure portal, search for and select **Virtual machines**

1. On the **Virtual machines**, click **Create** and create a new Virtual machine, on the **VMSPOKE01**.

1. In the Azure portal, search for and select **Virtual networks**.

1. In the list of virtual networks, click **VNET-TAE-Spoke1**.

1. On the **VNET-TAE-Spoke1** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Specify the following settings (leave others with their default values) and click **Add**:

    | Setting | Value|
    | --- | --- |
    | This virtual network: Peering link name | **To-Hub** |
    | This virtual network: Traffic to remote virtual network | **Allow (default)** |
    | This virtual network: Traffic forwarded from remote virtual network | **Allow (default)** |
    | Virtual network gateway | **None** |
    | Remote virtual network: Peering link name | **To-Spoke1** |    
    | Virtual network deployment model | **Resource manager** |
    | I know my resource ID | unselected |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **VNET-TAE-Hub** |
    | Traffic to remote virtual network | **Allow (default)** |
    | Traffic forwarded from remote virtual network | **None** |
    | Virtual network gateway | **Use this virtual network's gateway** |

1. On the **VNET-TAE-HUB** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Add a peering with the following settings (leave others with their default values):

    | Setting | Value|
    | --- | --- |
    | This virtual network: Peering link name | **To-Spoke1** |
    | This virtual network: Traffic to remote virtual network | **Allow (default)** |
    | This virtual network: Traffic forwarded from remote virtual network | ****Allow (default)**** |
    | Virtual network gateway | **None*** |
    | Remote virtual network: Peering link name | **To-Hub** |    
    | Virtual network deployment model | **Resource manager** |
    | I know my resource ID | unselected |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **VNET-TAE-Spoke1** |
    | Traffic to remote virtual network | **Allow (default)** |
    | Traffic forwarded from remote virtual network | **Allow (default)** |
    | Virtual network gateway | **None** |

1. At the top of the Azure portal, enter the name of a **VMSPOKE1** that is in the running state, in the search box. When the name of the VM appears in the search results, select it.

1. Under Settings on the left, select **Networking**, and navigate to the network interface resource by selecting its name. View network interfaces.

1. On the left, select **Effective routes**. The effective routes for a network interface are shown.
    
1. In the Azure portal, search for and select **Virtual machines** on the Virtual Network Hub.

1. On the **Virtual machine** blade, click **Connect**, in the drop-down menu, click **RDP**, on the **Connect with RDP** blade, click **Download RDP File** and follow the prompts to start the Remote Desktop session.

1. Within the Remote Desktop session to **Virtual machine**, right-click the **Start** button and, in the right-click menu, click **Windows PowerShell (Admin)**.

1. In the Windows PowerShell console window, run the following to test connectivity to **VMSPOKE01**.

   ```pwsh
   Test-NetConnection -ComputerName IPADDRESS-VMHUB01 -Port 3389 -InformationLevel 'Detailed'
   ```
    >**Note**: The test uses TCP 3389 since this is this port is allowed by default by operating system firewall. 

1. Examine the output of the command and verify that the connection was successful.

1. In the Windows PowerShell console window, run the following to test connectivity to **VMHUB01** 

   ```pwsh
   Test-NetConnection -ComputerName IPADDRESS-VMSPOKE01 -Port 3389 -InformationLevel 'Detailed'
   ```
1. Examine the output of the command and verify that the connection was successful.

## Project #01 - Hub-spoke Archicture (60 minutes)

Implement a Hub-spoke topology

   ![Screenshot of the Hub-spoke](/AllFiles/Images/Hub-Spoke.png)

**Important Notes**
- Three Virtual Networks;
- Virtual machines on the Spoke2 network running Ubuntu server;
- VNET Peering connection Hub to spoke and vice verse
- VNET Peering connections to allow forwarded;
- Azure Firewall on the Hub network;
- Azure Bastion on the Hub network;
- Custom Route tables to address prefix "Address space Spoke 1 and Spoke 2" and next hop type to virtual applicance **IP Azure Firewall**;
- Network rule Azure Firewall all traffic.

References: [Hub-spoke network topology](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)

1. End of day 1.

## Day 2
## Lab #01 - Azure Load Balancer (30 minutes)

1. Sign in to the [Azure portal](http://portal.azure.com).

1. In the Azure portal, search for and select **Virtual machines** and, on the **Virtual machines** blade, click **+ Create**.

1. On the **Basics** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab |
    | Resource group | the name of a new resource group **RG-Network** |
    | Virtual machine name | **VMTAEWEB01** and **VMTAEWEB02** |
    | Region | select one of the regions that support availability zones and where you can provision Azure virtual machines | 
    | Availability options | **Availability zone** |
    | Availability zone | **1** and **2** |
    | Image | **Ubuntu Server 18.04 LTS - Gen1** |
    | Azure Spot instance | **No** |
    | Size | **Standard B1ms** |
    | Username | **admaz** |
    | Password | **Azur3Exp3rt*** |
    | Public inbound ports | **SSH (22)** |
    | Would you like to use an existing Windows Server license? | **No** |

1. Click **Next: Disks >** and, on the **Disks** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | OS disk type | **Standard SSD** |
    | Enable Ultra Disk compatibility | **No** |

1. Click **OK** and, back on the **Networking** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Virtual Network | the name of a virtual network **VNET-TAE-Hub** |
    | Subnet | **Default** |
    | Public IP | **VMTAEWEB01-PI** and **VMTAEWEB02-PI** |
    | NIC network security group | **None** |
    | Accelerated networking | **Off** |
    | Place this virtual machine behind an existing load balancing solution? | **No** |

1. Click **Next: Management >** and, on the **Management** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Boot diagnostics | **Enable with custom storage account** |
    | Diagnostics storage account | create new |
    | Properties storage account | Name: **sataediag####**, Account kind: StorageV2, Performance: Standard, Replication: Locally-redundant-storage (LRS) |
    | Enable auto-shutdown | off |   

1. Click **Next: Advanced >**, on the **Advanced** tab of the **Custom data and cloud init** blade, add script for install NGINX Web Server.

1. Copy and paste script, on the Create cloud-init config, [click here](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-load-balancer#create-virtual-machines)

1. Click **Review + Create**.

1. On the **Review + Create** blade, click **Create**.

1. Repeat a new Virtual Machine **VMTAEWEB02**

1. Check two Virtual machines create successful.

1. Connect the Virtual machines.

**Note**
Test open Browser to IP Address the Virtual machines.

1. In the Azure portal, search and select **Load balancers** and, on the **Load balancers** blade, click **+ Create**.

1. Create a load balancer with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group | **RG-TAE-Networks** |
    | Name | **ALBTAEWEB** |
    | Region| name of the Azure region into which you deployed all other resources in this lab |
    | Type | **Public** |
    | SKU | **Standard** |
    | Public IP address | **Create new** |
    | Public IP address name | **ALBTAEWEB-PI** |
    | Availability zone | **Zone-redundant** |
    | Add a public IPv6 address | **No** |

    > **Note**: Wait for the Azure load balancer to be provisioned. This should take about 2 minutes. 

1. On the deployment blade, click **Go to resource**.

1. On the **ALBTAEWEB** load balancer blade, click **Backend pools** and click **+ Add**.

1. Add a backend pool with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **BP-WEB** |
    | Virtual network | **VNET-TAE-Hub** |
    | IP version | **IPv4** |
    | Virtual machine | **VMTAEWEB01** | 
    | Virtual machine IP address | associate IP address |
    | Virtual machine | **VMTAEWEB02** |
    | Virtual machine IP address | associate IP address |

1. Wait for the backend pool to be created, click **Health probes**, and then click **+ Add**.

1. Add a health probe with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **HP-WEB** |
    | Protocol | **TCP** |
    | Port | **80** |
    | Interval | **5** |
    | Unhealthy threshold | **2** |

1. Wait for the health probe to be created, click **Load balancing rules**, and then click **+ Add**.

1. Add a load balancing rule with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **LBR-WEB** |
    | IP Version | **IPv4** |
    | Protocol | **TCP** |
    | Port | **80** |
    | Backend port | **80** |
    | Backend pool | **BP-WEB** |
    | Health probe | **HP-WEB** |
    | Session persistence | **None** |
    | Idle timeout (minutes) | **4** |
    | TCP reset | **Disabled** |
    | Floating IP (direct server return) | **Disabled** |
    | Use outbound rules to provide backend pool members access to the internet. | **Enabled** |

1. Wait for the load balancing rule to be created, click **Overview**, and note the value of the **Public IP address**.

1. In the Azure portal, search for and select **Network security groups**, and, on the **Network security groups** blade, click **+ Add**.

1. Create a network security group with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource Group | **RG-TAE-Networks** |
    | Name | **NSG-TAE-ALB-WEB** |
    | Region | the name of the Azure region where you deployed all other resources in this lab |

1. On the deployment blade, click **Go to resource** to open the **NSG-ALB-WEB** network security group blade. 

1. On the **NSG-TAE-ALB-WEB** network security group blade, in the **Settings** section, click **Inbound security rules**. 

1. Add an inbound rule with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Source | **Any** |
    | Source port ranges | * |
    | Destination | **Any** |
    | Destination port ranges | **80** |
    | Protocol | **TCP** |
    | Action | **Allow** |
    | Priority | **100** |
    | Name | **Allow-Port_80** |

1. On the **NSG-ALB-WEB** network security group blade, in the **Settings** section, click **Network interfaces** and then click **+ Associate**.

1. Associate the **NSG-TAE-ALB-WEB** network security group with the Network interfaces **VMTAEWEB01 and VMTAEWEB02**.

    >**Note**: It may take up to 5 minutes for the rules from the newly created Network Security Group to be applied to the Network Interface Card.

1. Go to the Azure portal to view your **Network security groups**. Search for and select Network security groups.

1. Select the name of your Network security group.

1. Start browser window and navigate to the IP address you identified in the previous step.

1. Verify that the browser window displays the message **Static page and servername**.

1. In Virtual machines, select **VMTAEWEB01 or VMTAEWEB02** and click **Stop**. 

1. Open another browser window but this time by using InPrivate mode and verify whether the target vm changes (as indicated by the message).

## Lab #02 - Azure Virtual Machine Scale Sets (30 minutes)

1. Log in to the **Azure portal** at https://portal.azure.com.

1. Type **Scale set** in the search box. In the results, under **Marketplace**, select **Virtual machine scale sets**. Select **Create** on the **Virtual machine scale sets** page, which will open the **Create a virtual machine scale set** page.

1. In the **Basics** tab, under **Project details**, make sure the correct subscription is selected and then choose to **Create new** resource group. Type **RG-VMSS** for the name and then select **OK**.

1. Type **VMSSWEB** as the name for your scale set.

1. In **Region**, select a region that is close to your area.

1. Leave the default value of **Uniform** for **Orchestration mode**.

1. Select a marketplace image **Windows Server 2019 Datacenter - Gen1** for **Image**.

1. Enter your desired username, and password.

1. Select **Next** to move the the other pages. 

1. Leave the defaults for the **Instance** and **Disks** pages.

1. On the **Networking** page, under **Load balancing**, select **Yes** to put the scale set instances behind a load balancer. 

1. In **Load balancing options**, select **Azure load balancer**.

1. In **Select a load balancer**, create a new Azure Load Balancer **ALBTAEVMSSWEB**.

1. For **Select a backend pool**, select **Create new**, type **BP-VMSSWEB**, then select **Create**.

1. On the **Scaling** page, under **Instance**, select **2** and **Scaling policy**, select **Manual**. 

1. On the **Management** page, under **Monitoring**, select **Enable with custom storage account**.

1. When you are done, select **Review + create**. 

1. After it passes validation, select **Create** to deploy the scale set.

    > **Note**: You might need to wait a few minutes.

1. In **Virtual Machine Scale Sets**, select **Instances** and connect on Virtual machines.

1. Install the Web-Server feature in the all Virtual machines by running the following command in the **Administrator Windows PowerShell** command prompt. You can copy and paste this command.

   ```powershell
   Install-WindowsFeature -name Web-Server -IncludeManagementTools
   ```
1. Back in the portal, navigate back to the Overview blade of **Virtual Machine Scale Sets** and, use the Click to clipboard button to copy the public IP address.

1. In a **Browser**, navigate to the Public IP Address the VMSS.

1. Sign in to the [**Azure portal**](http://portal.azure.com) and open Azure Cloud Shell.

1. In Cloud Shell, start the code editor and create a file named **Add-CustomExtension-VMSS.ps1**.

1. Add the following text to the script file:

  ```powershell
    # Set Script 
$customConfig = @{ 
  "fileUris" = (,"https://raw.githubusercontent.com/maiaacademy/azureexpert/master/Mod06-AllFiles/VMSS-Install-IIS_v1.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File VMSS-Install-IIS_v1.ps1" 
} 

    # Set VMSS variables
$rgname = "rg-vmss"
$vmssname = "vmssweb"
 
    # Get VMSS object 
$vmss = Get-AzVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
 
    # Add VMSS extension 
$vmss = Add-AzVmssExtension -Name "CustomScript" -VirtualMachineScaleSet $vmss -Publisher "Microsoft.Compute" -Type "CustomScriptExtension" -TypeHandlerVersion "1.9" -Setting $customConfig

    # Update VMSS 
Update-AzVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss
   ```

1. Press Ctrl+S to save the file. Then press Ctrl+Q to close the code editor.

1. Run the following command **./Add-CustomExtension-VMSS.ps1**.

1. In **VMSSWEB**, select **Extensions** and check a new extension.

1. Select **Instances** in **VMSSWEB**, click **Upgrade** for all instances.

    > **Note**: You might need to wait a few minutes.

1. Test the Public IP address in Browser.

1. In the Azure portal, navigate to the **Virtual Machine Scale Sets** entry, and on the **VMSSWEB** blade, select **Scaling**. 

1. On the **Scaling** blade, select the **Custom autoscale** option.

1. In the **Custom autoscale** section, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Scaling mode | **Scale based on a metric** |
    | Instance limits Minimum | **1** |
    | Instance limits Maximum | **3** |
    | Instance limits Default | **1** |

1. Select **+ Add a rule**.

1. On the **Scale rule** blade, specify the following settings and select **Add** (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Time aggregation | **Maximum** |
    | Metric namespace | **Virtual Machine Host** |
    | Metric name | **Percentage CPU** |
    | VMName Operator | **=** |
    | Operator | **Greater than** |
    | Metric threshold to trigger scale action | **1** |
    | Duration (in minutes) | **1** |
    | Time grain statistics | **Maximum** |
    | Operation | **Increase count by** |
    | Instance count | **1** |
    | Cool down (minutes) | **5** |

1. Back on the **Scaling** blade, select **+ Add a rule**.

1. On the **Scale rule** blade, specify the following settings and select **Add** (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Time aggregation | **Average** |
    | Metric namespace | **Virtual Machine Host** |
    | Metric name | **Percentage CPU** |
    | VMName Operator | **=** |
    | Operator | **Less than** |
    | Metric threshold to trigger scale action | **1** |
    | Duration (in minutes) | **1** |
    | Time grain statistics | **Minimum** |
    | Operation | **Decrease count by** |
    | Instance count | **1** |
    | Cool down (minutes) | **5** |

1. Back on the **Scaling** blade, select **Save**.

1. In the Azure portal, start a new **Bash** session in the Cloud Shell pane. 

1. From the Cloud Shell pane, run the following to trigger autoscaling of the Azure VM Scale Set instances in the backend pool of the Azure Application Gateway (replace the `<lb_IP_address>` placeholder with the IP address of the front end of the gateway you identified earlier):

   ```Bash
   for (( ; ; )); do curl -s <lb_IP_address>?[1-10]; done
   ```
1. In the Azure portal, on the **VMSSWEB** blade, review the **CPU (average)** chart and verify that the CPU utilization of the Application Gateway increased sufficiently to trigger scaling out.

    > **Note**: You might need to wait a few minutes.








