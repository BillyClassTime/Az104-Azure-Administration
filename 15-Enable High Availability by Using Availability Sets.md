# Enable High Availability by Using Availability Sets 

## Challenge Overview

## Understand the scenario

You are a system administrator for a company that is migrating its web services from its own datacenter to Azure®. You need to configure high availability by using availability sets. First, you will create an availability set. Next, you will deploy two Azure virtual machines to the availability set. Finally, you will configure load balancing.

## Understand your environment
You will be creating an Azure resource group named **RG15** that contains no resources.

# Create an availability set

- Sign in to the Azure portal as your user account and password.

  Make sure you are signed in to Azure as the administrator.

- Create an availability set named **AVSet-[sequence]** in the **RG15** resource group, configure 3 fault domains and 5 update domains, and then configure support for managed disks.

## Check your work

- [ ] Confirm that you created an availability set named **AVSet-[sequence]** that has three fault domains, five update domains, and supports managed disks.

# Deploy Azure virtual machines to an availability set

- Create a virtual machine by using the values in the following table. For any property that is not specified, use the default value.

  | Property             | Value                                     |
  | :------------------- | :---------------------------------------- |
  | Resource group       | **RG15**                                  |
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
  | Resource group       | **RG15**                                  |
  | Virtual machine name | **VMFE2-[sequence]**                      |
  | Availability options | **Availability set**                      |
  | Availability set     | **AVSet-[sequence]**                      |
  | Image                | **Windows Server 2016 Datacenter - Gen2** |
  | Size                 | **Standard B2s**                          |
  | Username             | AzureUser                                 |
  | Password             | AzurePassw0rd!                            |
  | Select inbound ports | **RDP (3389)**                            |
  | OS disk type         | **Standard HDD**                          |

  > After you deploy the virtual machines, each one should be in a separate fault and update domain.

## Check your work

- [ ] Confirm that you created two virtual machines named **VMFE1-[sequence]** and **VMFE2-[sequence]** in the availability set.
- [ ] Confirm that each virtual machine is in a separate fault and update domain.

# Configure an Azure load balancer

- Create a public Azure load balancer named **LBFE-[sequence]** in the **RG15** resource group, use the **Standard** SKU, and then configure a new public, static IP address named **LBIP-[sequence]** for the load balancer.

- Add a backend pool named **LBBE-[sequence]** to the **LBFE-[sequence]** load balancer, use the **RG15-vnet** virtual network, associate the backend pool to virtual machines, and then add **VMFE1-[sequence]** and **VMFE2-[sequence]** to the backend pool.

- Add a health probe named **LBPB-[sequence]** on TCP port 80 (HTTP) to the **LBFE-[sequence]** load balancer, and then add a load balancing rule named **LBRL-[sequence]** for TCP port 80 (HTTP).

  > You may need to refresh the display to add the load balancing rule after creating the health probe.

## Check your work

- [ ] Confirm that you created an Azure load balancer named **LBFE1-[sequence]**.
- [ ] Confirm that you added a backend pool named **LBBE-[sequence]** that contains the two virtual machines that are part of the **AVSet-[sequence]** availability set.
- [ ] Confirm that you added a health probe named **LBPB-[sequence]** on TCP port 80 (HTTP) to the load balancer.
- [ ] Confirm that you added a load balancing rule named **LBRL-[sequence]** for TCP port 80 (HTTP) to the load balancer.

## Summary

Congratulations, you have completed the **Enable High Availability by Using Availability Sets** challenge.

You have accomplished the following:

- Created an availability set for high availability.
- Deployed two Azure virtual machines to an availability set.
- Added an Azure load balancer to an availability set.
- Added a health probe to a load balancer.
- Added a load balancing rule to a load balancer.