# Can You Implement a Secure Azure Virtual Machine Network?

## Understand the scenario

You are an Azure® network administrator. You need to create a secure network for your Azure virtual machines. First, you will reconfigure an existing incoming security rule to filter the destination to only the back-end subnet by using a new application security group, and then you will create a firewall and route table. Next, you will configure the firewall by using an application rule, and then you will add a network rule to allow domain name system (DNS) queries. Finally, you will configure Azure to support Network Watcher.

## Understand your environment

You will be creating an Azure environment that contains two Azure resource groups named **RG10** and **NetworkWatcherRG** (East US), Inside a **RG10** a virtual network named **app-vnet** (Address space 10.1.0.0/16,  East US, subnets <u>frontend</u> 10.1.0.0/24 and <u>backend</u> 10.1.1.0/24), **app-vnet-nsg** assosiate to both frontend and backend subnets) and two virtual machines named **VM1** (ubuntu 18.04, 10.1.0.4, East US) and **VM2** (ubuntu 18.04, 10.1.1.4, East US, ssh port open for inbound for both virtual machine) . 

> Important: **do not start** until these resources have been created, you will not be guided through these tasks.

# Configure an application security group

- Sign in to the Azure portal as as your user account and password.

- Create a new application security group named **app-backend-asg** in the **(US) East US** region by using the **RG10** resource group.

> You are creating the application security group in the same region as the existing virtual network

- Verify that both the **VM1** and **VM2** virtual machines are running.
- Associate the **app-backend-asg** application security group to the **VM2-nic** network interface that is attached to **VM2**.
- VM2 is located in the back-end subnet of the app-vnet virtual network.

> You can use the Application security group option on the Networking page of a virtual machine resource to associate the virtual machine network interface card (NIC) to an application security group.

- Update the existing **AllowSsh** inbound security rule in the **app-vnet-nsg** network security group to use the **app-backend-asg** application security group as the destination.

## Check your work

- [ ] Verify that you have created an application security group named **app-backend-asg.**
- [ ] Verify that you have associated the application security group to the **VM2-nic** that is attached to **VM2**.
- [ ] Verify that you have reconfigured the **AllowSSH** incoming security rule to use **app-backend-asg** as the destination.

# Create a firewall and a route table

- Create a subnet named **AzureFirewallSubnet** in the **app-vnet** virtual network by using a subnet address range of 10.1.63.0/24.

- Create a firewall by using the values in the following table. For any property that is not specified, use the default value.

    | Property                    | Value                                                    |
    | :-------------------------- | :------------------------------------------------------- |
    | Resource group              | **RG10**                                                 |
    | Name                        | **app-vnet-firewall**                                    |
    | Firewall SKU                | **Standard**                                             |
    | Firewall management **(*)** | **Use Firewall rules (classic) to manage this firewall** |
    | Choose a virtual network    | **Use existing**                                         |
    | Virtual network             | **app-vnet (RG10)**                                      |
    | Public IP address           | Add new: **fwpip[Initials]**                             |

    > It may take approximately 6–8 minutes to create the firewall. 
    >
    >  **(*)** If you use other rules, the time of deployment can be great than 8 minutes. And the configuration is different.

- Record the **private IP address** of **app-vnet-firewall** in the following **Private IP Address** text box:

  - You can find the private IP address on the app-vnet-firewall Overview page.

  **Private IP Address**

  ```
  
  ```

- Record the **public IP address** of **app-vnet-firewall** in the following **Public IP Address** text box:

    - You can find the public IP address on the Overview page of **fwpip[Initials]** in the RG1 resource group.

    **Public IP Address**

    ```

- Create a route table named **app-vnet-firewall-rt** in the **RG10** resource group by using the **East US** region.

- Create a route named to-firewall in **app-vnet-firewall-rt** by using the 0.0.0.0/0 address prefix to route traffic to a **virtual appliance** at **Private IP Address** (reordered above).

- Associate the **app-vnet-firewall-rt** route table to the **frontend** and **backend** subnets in **app-vnet**.

## Check your work

- [ ] Verify that you created a subnet named **AzureFirewallSubnet** that uses a subnet address range of 10.1.63.0/24.
- [ ] Verify that you have created a firewall named **app-vnet-firewall.**
- [ ] Verify that you have created a route table named **app-vnet-firewall-rt**.
- [ ] Verify that you have associated the **app-vnet-firewall-rt** route table to the **frontend** and **backend** subnets.

# Configure the firewall

- Create an **application rule collection** in **app-vnet-firewall** that contains a single **Target FQDN** rule by using the values in the following table. For any property that is not specified, use the default value.

  | Property         | Value                              |
  | :--------------- | :--------------------------------- |
  | Name             | app-vnet-fw-arc-web                |
  | Priority         | 200                                |
  | Target FQDN name | AllowAzurePipelines                |
  | Source           | 10.1.0.0/23                        |
  | Protocol:Port    | https                              |
  | Target FQDNs     | dev.azure.com, azure.microsoft.com |

- Create a **network rule collection** that contains a single **IP Address** rule by using the values in the following table. For any property that is not specified, use the default value.

    | Property              | Value               |
    | :-------------------- | :------------------ |
    | Name                  | app-vnet-fw-nrc-dns |
    | Priority              | 200                 |
    | IP Address name       | AllowDns            |
    | Protocol              | **UDP**             |
    | Source                | 10.1.0.0/23         |
    | Destination addresses | 1.1.1.1, 1.0.0.1    |
    | Destination ports     | 53                  |

- Review the documentation on [creating a network rule collection](https://docs.microsoft.com/en-us/azure/firewall/tutorial-firewall-deploy-portal#configure-a-network-rule).

## Check your work

- [ ] Verify that you have created an application rule collection named **app-vnet-fw-arc-web** in the **app-vnet-firewall** firewall.
- [ ] Verify that you have created a network rule collection named **app-vnet-fw-nrc-dns** in the **app-vnet-firewall** firewall.

# Configure Azure to support Network Watcher

- Create a storage account named ***networkwatchersa[Initials]*** in the **RG10** resource group by using the **(US) Central US** region.

  > Remember: To implement network security group (NSG) flow logs in Network Watcher, you must provide a storage account in which the logs can be stored.

- Register the ***microsoft.insights*** resource provider with your subscription.

  > It may take approximately 3–5 minutes for the registration to complete. You may need to periodically refresh the Resource providers page. You may also need to close and then reopen the Resource providers page in order to display the updated status.

    >Remember: To implement NSG flow logs in Network Watcher, you must register the ***microsoft.insights*** resource provider.

- Register the ***Microsoft.OperationalInsights*** resource provider with your subscription.

  > Remember: To implement connection monitor in Network Watcher, you must register the Microsoft.OperationalInsights resource provider.

- Enable Network Watcher in the **Central US** region.

- Review the documentation on [enabling Network Watcher](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-create).

	> TIP : When you create the first virtual network in a region—in this case, when **VM1** was created for you during the challenge setup—Azure should automatically enable Network Watcher in that region. You can manually enable Network Watcher in other regions.

## Check your work

- [ ] Verify that you have created a storage account named ***networkwatchersa[Initials]*** in the Central US region.
- [ ] Verify that you have registered the ***microsoft.insights*** and ***Microsoft.OperationalInsights*** resource providers.
- [ ] Verify that you have enabled Network Watcher in the Central US region.

# Summary

Congratulations, you have completed the **Can You Implement a Secure Azure Virtual Machine Network?** challenge.

You have accomplished the following:

- Configured an application security group as the destination filter for an inbound security rule.
- Created a firewall.
- Created a subnet for a firewall.
- Configured an application rule and a network rule for a firewall.
- Created a route table that contains a firewall next hop.
- Associated a route table to existing subnets.
- Configured Azure Network Watcher.
