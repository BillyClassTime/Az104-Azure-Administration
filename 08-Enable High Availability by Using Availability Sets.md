# Enable High Availability by Using Availability Sets

## Challenge Overview
## Understand the scenario
You are a system administrator for a company that is migrating its web services from its own datacenter to Azure®. You need to configure high availability by using availability sets. First, you will create an availability set. Next, you will deploy two Azure virtual machines to the availability set. Finally, you will configure load balancing.

# Understand your environment
You will be using an Azure resource group named **RG08** that contains no resources.

# Create an availability set

- Sign in to the Azure portal as as your user account and password.

- Create an availability set named AVSet-[sequence] in the **RG08** resource group, configure 3 fault domains and 5 update domains, and then configure support for managed disks.

  - On the Azure portal home page, select **Create a resource** to open the Azure Marketplace.

  - In the Azure Marketplace, search for and select Availability Set, and then select **Create**.

  - On the Create availability set blade, on the Basics page, in Resource group, select **RG08**, and then in Name, enter **AVSet-[sequence]**.

  - In Fault domains, move the slider to the right to set the value to 3.

  - In Update domains, set the value to 5.

  - In Use managed disks, ensure that **Yes (Aligned)** is selected.

  - Select **Review + create**, review the configuration, and then select **Create**.

## Check your work

- [ ] Confirm that you created an availability set named **AVSet-[sequence]** that has three fault domains, five update domains, and supports managed disks.

# Deploy Azure virtual machines to an availability set

- Create a virtual machine by using the values in the following table. For any property that is not specified, use the default value.

  | Property             | Value                                     |
  | :------------------- | :---------------------------------------- |
  | Resource group       | **RG08**                                  |
  | Virtual machine name | **VMFE1-[sequence]**                      |
  | Availability options | **Availability set**                      |
  | Availability set     | **AVSet-[sequence]**                      |
  | Image                | **Windows Server 2016 Datacenter - Gen2** |
  | Size                 | **Standard B2s**                          |
  | Username             | AzureUser                                 |
  | Password             | AzurePassw0rd!                            |
  | Select inbound ports | **RDP (3389)**                            |
  | OS disk type         | **Standard HDD**                          |

- Create a second virtual machine by using the values in the following table. For any property that is not specified, use the default value.

  | Property             | Value                                     |
  | :------------------- | :---------------------------------------- |
  | Resource group       | **RG08**                                  |
  | Virtual machine name | **VMFE2-[sequence]**                      |
  | Availability options | **Availability set**                      |
  | Availability set     | **AVSet-[sequence]**                      |
  | Image                | **Windows Server 2016 Datacenter - Gen2** |
  | Size                 | **Standard B2s**                          |
  | Username             | AzureUser                                 |
  | Password             | AzurePassw0rd!                            |
  | Select inbound ports | **RDP (3389)**                            |
  | OS disk type         | **Standard HDD**                          |

  After you deploy the virtual machines, each one should be in a separate fault and update domain.

## Check your work

- [ ] Confirm that you created two virtual machines named **VMFE1-[sequence]** and **VMFE2-[sequence]** in the availability set.
- [ ] Confirm that each virtual machine is in a separate fault and update domain.

# Configure an Azure load balancer

- Create a public Azure load balancer named **LBFE-[sequence]** in the **RG08** resource group, use the **Standard** SKU, and then configure a new public, static IP address named **LBIP-[sequence]** for the load balancer.

  - On the Azure portal home page, select **Create a resource** to open the Azure Marketplace.

  - In the Azure Marketplace, search for and select Load Balancer, and then select **Create**.

  - On the Create load balancer blade, on the Basics page, in Resource group, select **RG08**, and then in Name, enter **LBFE-[sequence]**.

  - In SKU, select **Standard**, and then select **Next : Frontend IP configuration**.

  - Select **Add a frontend IP configuration**, and then in name, enter **LBIP-[sequence]**.

  - In Public IP address, select **Create new**, in Name, enter LBIP-[sequence], select **OK**, and then **Add** to add the frontend IP configuration.

  - Select **Review + create**, review the configuration, and then select **Create**.

- Add a backend pool named **LBBE-[sequence]** to the **LBFE-[sequence]** load balancer, use the **RG08-vnet** virtual network, associate the backend pool to virtual machines, and then add **VMFE1-[sequence]** and **VMFE2-[sequence]** to the backend pool.

- Add a health probe named **LBPB-[sequence]** on TCP port 80 (HTTP) to the **LBFE-[sequence]** load balancer, and then add a load balancing rule named **LBRL-[sequence]** for TCP port 80 (HTTP).

  > Remember: You may need to refresh the display to add the load balancing rule after creating the health probe.

  - On the Load balancer resource menu, in Settings, select **Health probes**, and then select **Add**.


  - In Name, enter **LBPB-[sequence]**, ensure that **TCP** port 80 is selected, and then select **Add**.


  - In Settings, select **Load balancing rules**, and then select **Add**.


  - In Name, enter **LBRL-[sequence]**.


  - In Frontend IP address select **LBIP-[sequence]**, and then in Backend pool, select **LBBE-[sequence]**.


  - In Port and Backend port, enter 80.


  - In Health probe, select **LBPB-[sequence]**, and then select **Add**.


## Check your work

- [ ] Confirm that you created an Azure load balancer named **LBFE1-[sequence]**.
- [ ] Confirm that you added a backend pool named **LBBE-[sequence]** that contains the two virtual machines that are part of the **AVSet-[sequence]** availability set.
- [ ] Confirm that you added a health probe named **LBPB-[sequence]** on TCP port 80 (HTTP) to the load balancer.
- [ ] Confirm that you added a load balancing rule named **LBRL-[sequence]** for TCP port 80 (HTTP) to the load balancer.

# Summary

Congratulations, you have completed the **Enable High Availability by Using Availability Sets** challenge.

You have accomplished the following:

- Created an availability set for high availability.
- Deployed two Azure virtual machines to an availability set.
- Added an Azure load balancer to an availability set.
- Added a health probe to a load balancer.
- Added a load balancing rule to a load balancer.
