# Change the Size of an Azure Virtual Machine by Using PowerShell

## Challenge Overview

## Understand the scenario

You are a system administrator for a company that is migrating virtual machines to Azure. After an initial deployment and configuration of virtual machines, your company will use Azure PowerShell to automatically update the size of the machines to their production requirement. You need to configure Azure Cloud Shell, then use Cloud Shell to update the size of the virtual machine.

## Understand your environment

You will be creating the following resources:

| Resource        | Name               | Notes                            |
| :-------------- | :----------------- | -------------------------------- |
| Resource group  | **RG19**           |                                  |
| Virtual machine | **VM3**            | Windows, Standard D1 v2, East US |
| Storage account | **sacs[Initials]** |                                  |

> Important: **do not start** until these resources have been created, you will not be guided through these tasks. 

# Configure Azure Cloud Shell for use with PowerShell

- Sign in to the Azure portal as your user account and password.

  Make sure you are signed in to Azure as the administrator.

- Create a file share named **cloud-shell** in the existing storage account in the **East US** region, and then set the Quota to 6.

- In Cloud Shell, configure the **advanced settings** using the existing storage account and the file share, and then attach the storage.

## Check your work

- [ ] Confirm that a file share named **cloud-shell** exists in the storage account in the **RG19** resource group.
- [ ] Confirm that a Cloud Shell environment using **PowerShell** is open.

# View available virtual machine sizes in your location

- In the Azure portal, display the **Virtual machines** service, and then record the current virtual machine size, location, and resource group name in the following text boxes.

- Current VM Size:

  ```
  ```

- Location

  ```

- Resource Group

  ```

> You will use these values later in the challenge to change and confirm a new virtual machine size that is available in the region.

- Using PowerShell in Cloud Shell, list the available virtual machine sizes in your location, and then confirm that **Standard_D2_v2** is available.
- Create a variable name **$Size** and then set the value to **Standard_D2_v2**

## Check your work

- [ ] Confirm that your virtual machine size is **Standard_D1_v2**, the virtual machine is located in **East US**, and the resource group name is **RG19**.
- [ ] Confirm that you have created a variable named **$Size** with a value of **Standard_D2_v2**

# Change the size of the virtual machine

- In the Cloud Shell, use Get-azVM command to create a variable named **$VM** that will store your virtual machine details.
- Use the **HardwareProfile.VMSize** property to assign the value of the **$Size** variable.
- Update the virtual machine using the **$VM** variable.
- Confirm that the virtual machine has been successfully resized.

## Check your work

- [ ] Confirm that **$VM.HardwareProfile.VMSize** is set to **Standard_D2_v2**
- [ ] Confirm that the virtual machine size updated to **Standard_D2_v2** from **Standard_D1_v2**

# Summary

Congratulations, you have completed the **Change the Size of an Azure Virtual Machine by Using Azure PowerShell** challenge.

You have accomplished the following:

- Configured Azure Cloud Shell for use with PowerShell
- Viewed available virtual machine sizes in your location
- Changed the size of the virtual machine
- Confirmed the new size of the virtual machine