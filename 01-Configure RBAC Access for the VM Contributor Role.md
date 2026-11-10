# Configure RBAC Access for the Virtual Machine Contributor Role

## Challenge overview

Assign a user the built-in **Virtual Machine Contributor** role at resource-group scope. Verify that the user can manage a virtual machine but cannot grant Azure RBAC access. This demonstrates least-privilege access for VM operations.

## Prerequisites

- An Azure subscription where you can create resources and role assignments (Owner or User Access Administrator plus the permissions needed to create the resources).
- Two accounts: an administrator account and a separate test user. The test user must be able to sign in to the Azure portal.
- A resource group named **RG-Challenge01** in **East US**, containing a virtual network named **vnet** with a subnet named **Subnet1**. Create these beforehand if needed.
- Azure Cloud Shell with the **PowerShell** environment. On first use, follow the portal prompts to configure persistent storage. The storage account and file share used by Cloud Shell are separate from the lab resource group.

Do not use a shared or real password in the lab instructions. Enter a unique password when prompted and store it securely.

## Create the virtual machine

1. Sign in to the Azure portal as the administrator and open **Cloud Shell**. Select **PowerShell**.
2. Confirm the active subscription and select the one containing **RG-Challenge01**:

   ```powershell
   Get-AzContext
   # If needed:
   Set-AzContext -Subscription "<subscription-id>"
   ```

3. Create a Windows Server 2025 VM in the existing virtual network and subnet, matching the course labs' Windows Server version. The `New-AzVM` parameter set below is supported by the current Az.Compute module. It uses the available `Standard_D2s_v7` size and the Windows Server 2025 Datacenter Gen2 image. Check size availability and quota in your subscription and region before deployment. When prompted, enter a unique local administrator password:

   ```powershell
   $credential = Get-Credential -UserName "student" -Message "Enter a unique VM administrator password"
   New-AzVM `
       -ResourceGroupName "RG-Challenge01" `
       -Location "East US" `
       -Name "TestVM" `
       -VirtualNetworkName "vnet" `
       -SubnetName "Subnet1" `
       -Size "Standard_D2s_v7" `
       -Image "MicrosoftWindowsServer:WindowsServer:2025-datacenter-g2:latest" `
       -Credential $credential
   ```

   See Microsoft's [New-AzVM reference](https://learn.microsoft.com/en-us/powershell/module/az.compute/new-azvm) and [Windows VM PowerShell quickstart](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-powershell) for current syntax and image details.

4. Wait for deployment to finish, then verify the VM exists:

   ```powershell
   Get-AzVM -ResourceGroupName "RG-Challenge01" -Name "TestVM" -Status
   ```

### Check your work

- [ ] Cloud Shell is running PowerShell in the intended subscription.
- [ ] **TestVM** is deployed in **RG-Challenge01**.

## Assign the role to the test user

1. In the Azure portal, open **Resource groups** > **RG-Challenge01** > **Access control (IAM)**.
2. Select **Add** > **Add role assignment**.
3. Select **Virtual Machine Contributor**, then **Next**.
4. Select the test user as a member, then review and assign the role.
5. In **Role assignments**, verify that the test user has **Virtual Machine Contributor** at the **RG-Challenge01** scope.

### Check your work

- [ ] The assignment names the intended test user, role, and resource-group scope.

## Verify the test user's permissions

1. Sign out of the administrator account and sign in as the test user. If the new assignment is not yet visible, sign out and back in again after a few minutes.
2. Open **TestVM** in the portal and select **Stop**. Confirm the VM reaches the **Stopped (deallocated)** state.
3. Open **Access control (IAM)** for **RG-Challenge01** and try to add a role assignment. Confirm that the user cannot complete the assignment because this role does not grant permission to assign Azure RBAC roles. Cancel the operation.
4. Sign back in as the administrator and start **TestVM** if you want to keep it available. Delete the lab resources when finished to avoid ongoing charges.

### Check your work

- [ ] The test user can stop **TestVM**.
- [ ] The test user cannot grant Azure RBAC role assignments.

## Summary

You created a VM, assigned **Virtual Machine Contributor** at resource-group scope, and verified both an allowed VM action and the role's access-assignment boundary.
