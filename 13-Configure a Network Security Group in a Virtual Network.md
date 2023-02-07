# Configure a Network Security Group in a Virtual Network

## Challenge Overview
## Understand the scenario
You are an Azure® administrator assigned to manage network security in an Azure virtual network. You need to configure a network security group to allow secure shell (SSH) connections to a virtual machine. First, you will create an application security group, and then you will create a network security group. Next, you will associate the network security group to a subnet in the virtual network, and then you will associate the application security group to the network interface on the virtual machine. Finally, you will add an inbound security rule, and then you will verify that you can connect to the virtual machine in the subnet by using SSH.

## Understand your environment

You will be using an Azure resource group named **RG13** that contains a storage account named **sa[Initials]** for use with Cloud Shell, a virtual network named **VNET** (Linux Ubuntu 18.0.4 Standard DS1 v2 ) that contains a subnet named **frontend**, and a virtual machine named **VM1.** (use  AzurePassw0rd!  like password).

# Create Azure security groups
- Sign in to the Azure portal as your user account and password.

  Make sure you are signed in to Azure as the administrator.

- Create an application security group named **webapp201-asg** in the (US) East US region by using the **RG13** resource group.

  > Remember: **Application Security Group** is a independent resource on Azure portfolio services.
  >
  > There are two types of **Azure security groups** that you can use to filter network traffic.
  >
  > You use an **application security group** to group Azure virtual machines together.
  >
  > You use a **network security group** to define rules that are used to allow or deny network traffic based on source, source port, destination, destination port, and protocol.
  >
  > In an **application security group**, you can deploy multiple applications in the same subnet but keep the applications isolated from one another. 
  >
  > In a **network security group**, you can define network security policies that are based on the application security group. This allows you to define those policies based on your workloads rather than on explicit IP addresses.

- Create a network security group named **webapp201-nsg** in the (US) East US region by using the **RG13** resource group.

  > It may take approximately 1–2 minutes to deploy the network security group. Wait for the deployment to finish before beginning the next step.

## Check your work

- [ ] Confirm that you created an application security group named **webapp201-asg**.
- [ ] Confirm that you created a network security group named **webapp201-nsg**.

# Associate Azure security groups to Azure resources

- Associate the **webapp201-nsg** network security group to the **frontend** subnet in the **VNET** virtual network.

  > Remember: A network security group can be associated to the network interface card (NIC) of a virtual machine or to a subnet in the same region as the application security group. You should associate the network security group to either a subnet or a NIC but not to both, in order to avoid conflicting rules that can cause communication issues.

- Associate the webapp201-asg application security group to the **RG13** NIC on the VM1 virtual machine.

  - On the Azure portal home page, select Resource groups, select **RG13**, and then select VM1.
  - On the VM1 resource menu, in Settings, select Networking.
  - On the Networking page, select Application security groups, and then select Configure the application security groups.
  - On the Configure the application security groups blade, expand **Application security groups**, select the **webapp201-asg** check box, and then on the command bar, select **Save**.

- Record the **public IP address** of **VM1** in the following **Public IP Address** text box:

  **Public IP Address**

  ```
  ```

  You will use the public IP address in an upcoming task.

## Check your work

- [ ] Confirm that you associated the **webapp201-nsg** network security group to the frontend subnet in the VNET virtual network.
- [ ] Confirm that you associated the **webapp201-asg** application security group to the NIC on **VM1**.
- [ ] Confirm that you recorded the public IP address of **VM1**.

# Connect to a Linux virtual machine by using SSH

- Configure an Azure Cloud Shell Bash session by using the existing **RG13** resource group, an existing storage account named **sa[Initials]** in the East US region, and a new file share named cloud-shell-share.

- In Cloud Shell, attempt to connect to the Linux virtual machine at azureadmin@<PIP> by using a [secure shell (SSH)](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys#ssh-into-your-vm) connection.

  > The SSH connection is expected to time out because the network security group does not yet have an incoming rule to allow SSH.

## Close the Cloud Shell window.

- [ ] Check your work
- [ ] Confirm that you created a Bash session in Cloud Shell.
- [ ] Confirm that you were unable to connect to the virtual machine by using SSH.

# Create an inbound security rule to allow SSH

- Create an inbound security rule named AllowSSH in the **webapp201-nsg** network security group, and then configure the rule to allow incoming TCP traffic on port 22 to the **webapp201-asg** application security group.

  > Wait for the rule creation to finish before moving on to the next step.

- Open Cloud Shell, connect to the virtual machine at azureadmin@<PIP> by using an SSH connection, and then when prompted, enter AzurePassw0rd! as the password.

  > Because this is a Linux virtual machine, you will not see the password displayed in Cloud Shell.

## Check your work

- [ ] Confirm that you created an inbound security rule named AllowSSH in the **webapp201-nsg** network security group that allows incoming traffic on port 22 to the **webapp201-asg** application security group.
- [ ] Confirm that you connected to **VM1** by using SSH.

## Summary

Congratulations, you have completed the Configure a Network Security Group in a Virtual Network challenge.

In this challenge, you accomplished the following:

- Created an application security group and a network security group in Azure.
- Associated Azure security groups to Azure resources.
- Configured Azure Cloud Shell.
- Created an inbound security rule to allow SSH.