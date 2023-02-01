# Configure RBAC Access for the VM Contributor Role

### Challenge Overview

## Understand the scenario

You are an IT manager who has a mandate to implement the principle of least privilege as a best practice whenever possible. You want to ensure that some members of your IT staff are able manage virtual machines, but cannot assign permissions to the machines or manage the underlying Azure fabric. You determine that the best way to accomplish this is to assign some members of your IT staff the built-in [Virtual Machine Contributor role](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) to create and manage virtual machines.

In this Challenge Lab, you will create an Azure virtual machine, assign the Virtual Machine Contributor role to an existing user, and then verify the role permissions.

## Understand your environment

You must created Azure account that contains a resource group named **RG-Challenge01**. You will use your user account like owner of subscription to create resources and assign role permissions, and then you must create other user account to test the role permissions, (You must record the new user account name and password, you will use it later.


# Create an Azure VM by using the Azure Cloud Shell

- Sign in to the Azure portal as your user account and password.

- Configure an Azure Cloud Shell **PowerShell** session by using the **RG-Challenge01cs** resource group (If not exists create it), a new storage account named **cloudshsa[your initials]** in the **East US** region, and a new file share named **shell**.

- Create a Windows Datacenter 2016 Azure VM by using the Azure Cloud Shell [New-AzVM](https://docs.microsoft.com/en-us/powershell/module/az.compute/new-azvm?view=azps-8.2.0) cmdlet and the information in the following table. For any property that is not specified, use the default value.

  | Property             | Value              |
  | -------------------- | ------------------ |
  | Resource group       | **RG-Challenge01** |
  | Virtual machine name | TestVM             |
  | Location             | East US            |
  | Vnet                 | **vnet**           |
  | Subnet Name          | **Subnet1**        |
  | Size                 | Standard_DS1_v2    |
  | User                 | student            |
  | Password             | AzurePassw0rd!     |


> Run the following Powershell Command to create a new Azure Virtual Machine in the **RG-Challenge01** resource group:
>
> ```
> New-AzVM -ResourceGroupName "RG1lod28450521" -Location "East US" -Name "TestVM" -VirtualNetworkName "vnet" -SubnetName "Subnet1" -Size "Standard_DS1_v2"
> ```
>
> When prompted, enter student as the User and AzurePassw0rd! as the password for **TestVM**.
>
> Wait for the VM creation to complete.

## Check your work

- [ ] Confirm that you configured Azure Cloud Shell.
- [ ] Confirm that you created a virtual machine named **TestVM** by using Azure Cloud Shell.

# Add the Virtual Machine Contributor Role assignment to a user account

- Assign the new account create above the Virtual Machine Contributor role in the **RG-Challenge01** resource group.

  > <details><summary style="list-style: none; margin: 0px;">Expand this hint for guidance on assigning the Virtual Machine Contributor role.</summary><p></p></details>
  >
  > On the Azure portal home page, select Resource groups, and then select **RG-Challenge01**.
  >
  > On the **RG-Challenge01** resource menu, select Access control (IAM), and then in Grant access to this resource, select Add role assignment.
  >
  > On the Add role assignment blade, in Role, search for and select Virtual Machine Contributor, and then select Next.
  >
  > In Members, select Select members.
  >
  > On the Select members blade, search for and select the new user created above, and then select **Select**.
  >
  > On the Add role assignment blade, select **Next**, and then select **Review** **+ assign**.

## Check your work

- [ ] Confirm that new user account created above has the Virtual Machine Contributor role in the **RG-Challenge01** resource group. 

# Verify VM Contributor Role permissions

- Sign out of the Azure portal, and then sign in as the new account created above and password.

- Stop the **TestVM**.

  > <details><summary style="list-style: none; margin: 0px;">Expand this hint for guidance on stopping a virtual machine.</summary></details>
  >
  > On the Azure portal home page, select Virtual machines, and then select **TestVM**.
  >
  > On the **TestVM** Overview page, on the command bar, select Stop, and then select Yes.
  >
  > Wait for the Status of the **TestVM** to change from Running to Stopped— You may have to refresh the page to view the changes.

## Check your work

- [ ] Confirm that **TestVM** is stopped. 

# Summary

Congratulations, you have completed the **Configure RBAC Access for VM Contributor** Challenge Lab. You have accomplished the following:

- Created an Azure VM by using the Azure Cloud Shell.
- Added the Virtual Machine Contributor Role assignment to a user account.
- Verified VM Contributor Role permissions.
