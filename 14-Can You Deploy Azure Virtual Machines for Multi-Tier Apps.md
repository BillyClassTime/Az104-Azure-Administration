# Can You Deploy Azure Virtual Machines for Multi-Tier Apps?

## Challenge Overview

## Understand the scenario

You are an Azure® administrator. You need to create and deploy multiple Azure virtual machines for a web app in a multi-tier architecture. First, you will create multiple virtual networks. Next, you will create multiple virtual machines. Finally, you will verify connectivity for a multi-tier web app.

## Understand your environment

You will be creating an Azure resource group named **RG14** that initially contains no resources.

# Create Azure virtual networks for a multi-tier web app

- Sign in to the Azure portal as your user account and password.

  Make sure you are signed in to Azure as the administrator.

- Create a virtual network for the web tier by using the values in the following table. For any property that is not specified, use the default value.

  | Property             | Value        |
  | :------------------- | :----------- |
  | Resource group       | **RG14**     |
  | Name                 | **webVNET**  |
  | IPv4 address space   | 10.10.0.0/16 |
  | Subnet name          | web          |
  | Subnet address range | 10.10.0.0/25 |

- Create a second virtual network for the app tier by using the values in the following table. For any property that is not specified, use the default value.

  | Property             | Value        |
  | :------------------- | :----------- |
  | Resource group       | **RG14**     |
  | Name                 | **appVNET**  |
  | IPv4 address space   | 10.20.0.0/16 |
  | Subnet name          | app          |
  | Subnet address range | 10.20.0.0/25 |

- Create a third virtual network for the database tier by using the values in the following table. For any property that is not specified, use the default value.

  | Property             | Value        |
  | :------------------- | :----------- |
  | Resource group       | **RG14**     |
  | Name                 | **dbVNET**   |
  | IPv4 address space   | 10.30.0.0/16 |
  | Subnet name          | db           |
  | Subnet address range | 10.30.0.0/25 |

- Create virtual network peering connections from **webVNET** to **appVNET** by using the values in the following table. For any property that is not specified, use the default value.

  | Property                                   | Value                  |
  | :----------------------------------------- | :--------------------- |
  | This virtual network - Peering link name   | **webVNET-to-appVNET** |
  | Remote virtual network - Peering link name | **appVNET-to-webVNET** |
  | Virtual network                            | **appVNET**            |

- Create virtual network peering connections from **appVNET** to **dbVNET**, by using the values in the following table. For any property that is not specified, use the default value.

  | Property                                   | Value                 |
  | :----------------------------------------- | :-------------------- |
  | This virtual network - Peering link name   | **appVNET-to-dbVNET** |
  | Remote virtual network - Peering link name | **dbVNET-to-appVNET** |
  | Virtual network                            | **dbVNET**            |

## Check your work

- [ ] Verify that you have created three virtual networks named **webVNET**, **appVNET**, and **dbVNET**.
- [ ] Verify that you have created virtual network peering connections between the **webVNET** and **appVNET** virtual networks.
- [ ] Verify that you have created virtual network peering connections between the **appVNET** and **dbVNET** virtual networks.

# Deploy Azure virtual machines for a multi-tier web app

- Create an availability set for each tier in the **RG14** resource group by using the names **webAV**, **appAV**, and **dbAV**, configure each availability set to use 2 fault domains and 5 update domains, and then enable **use managed disks**.

- Create two Azure virtual machines named **webVM1** and **webVM2** by using the common values in the following table. For any property that is not specified, use the default value.

  | Property             | Value                                     |
  | :------------------- | :---------------------------------------- |
  | Resource group       | **RG14**                                  |
  | Availability options | **Availability set**                      |
  | Availability set     | **webAV**                                 |
  | Image                | **Windows Server 2019 Datacenter - Gen1** |
  | Size                 | **Standard_B2ms - 2 vcpus 8GiB memory**   |
  | Username             | AzureAdmin                                |
  | Password             | Az!28583583!                              |
  | Virtual network      | **webVNET**                               |
  | Subnet               | **web (10.10.0.0/25)**                    |
  | Boot diagnostics     | **Disable**                               |

- Create two Azure virtual machines named **appVM1** and **appVM2** by using the common values in the following table. For any property that is not specified, use the default value.

  | Property             | Value                                   |
  | :------------------- | :-------------------------------------- |
  | Resource group       | **RG14**                                |
  | Availability options | **Availability set**                    |
  | Availability set     | **appAV**                               |
  | Image                | **Ubuntu Server 18.04 LTS - Gen1**      |
  | Size                 | **Standard_B2ms - 2 vcpus 8GiB memory** |
  | Authentication type  | **Password**                            |
  | Username             | AzureAdmin                              |
  | Password             | Az!28583583!                            |
  | Virtual network      | **appVNET**                             |
  | Subnet               | **app (10.20.0.0/25)**                  |
  | Boot diagnostics     | **Disable**                             |

- Create an Azure virtual machine that hosts SQL Server 2019 on Windows Server 2019 by using the values in the following table. For any property that is not specified, use the default value.

  | Property             | Value                                                        |
  | :------------------- | :----------------------------------------------------------- |
  | Resource group       | **RG14**                                                     |
  | Virtual machine name | **dbVM1**                                                    |
  | Availability options | **Availability set**                                         |
  | Availability set     | **dbAV**                                                     |
  | Image                | **Free SQL Server License: SQL 2019 Developer on Windows Server 2019 - Gen1** |
  | Size                 | **Standard_B2ms - 2 vcpus 8GiB memory**                      |
  | Username             | AzureAdmin                                                   |
  | Password             | Az!28583583!                                                 |
  | Virtual network      | **dbVNET**                                                   |
  | Subnet               | **db (10.30.0.0/25)**                                        |
  | Boot diagnostics     | **Disable**                                                  |
  | SQL connectivity     | **Private (within Virtual Network)**                         |
  | SQL Authentication   | **Enable**                                                   |

## Check your work

- [ ] Verify that you have created three availability sets named **webAV**, **appAV**, and **dbAV**.
- [ ] Verify that you have created two virtual machines named **webVM1** and **webVM2** for the web tier.
- [ ] Verify that you have created two virtual machines named **appVM1** and **appVM2** for the app tier.
- [ ] Verify that you have created one virtual machine named **dbVM1** for the db tier.

# Verify connectivity in a multi-tier web app

- Connect to the **webVM1** virtual machine by using **RDP**, and then sign in as AzureAdmin using Az!28583583! as the password.

- Verify the IP address by using the ipconfig utility.

  > The IPv4 address should be in the 10.10.0.0/25 subnet—for example, 10.10.0.4.

- At a command prompt, use the ping utility to verify connectivity between the webVNET virtual network and the **appVNET** virtual network at 10.20.0.4.

  > The ping command should successfully verify a virtual network peering connection between the 10.10.0.0/25 (webVNET/web) subnet and the 10.20.0.0/25 (appVNET/app) subnet.

  > The Ubuntu virtual machines in the **appVNET** allow inbound ICMP (ping) traffic by default.

## Check your work

- [ ] Verify that you have enabled connectivity between the **webVNET** and **appVNET** virtual networks.

# Summary

Congratulations, you have completed the **Can You Deploy Azure Virtual Machines for Multi-Tier Apps?** challenge.

In this challenge, you have accomplished the following:

- Created Azure virtual networks for a multi-tier web app.
- Deployed Azure virtual machines for a multi-tier web app.
- Verified connectivity in a multi-tier web app.