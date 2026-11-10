# Can You Use RBAC and Design a Custom Role?

## Challenge overview

As an Azure administrator, delegate access to a developer so they can manage storage accounts, virtual machines, and network resources in one resource group. Then design a custom role for basic VM operations and verify that the developer cannot create role definitions.

## Prerequisites

- An active Azure subscription and an administrator account that can create resources in **RG-Challenge02** and assign Azure RBAC roles at that scope. For example, use **Contributor** plus **Role Based Access Control Administrator** at the resource-group scope, or **Owner**.
- An existing developer user in the same Microsoft Entra tenant as the subscription. The user must be able to sign in to the Azure portal and must not have other inherited Azure roles that would broaden the permissions being tested.
- An empty resource group named **RG-Challenge02** in **East US**. If it does not exist, the administrator needs permission at subscription scope to create it.
- Azure Cloud Shell with **PowerShell**. Persistent storage is optional for this lab; the custom role definition file only needs to remain available during the Cloud Shell session.

Use a unique, compliant VM password during deployment. Do not put real credentials in lab files. The VM and storage account can incur charges; delete the lab resource group when finished.

## Assign built-in roles to the developer

1. Sign in to the Azure portal as the administrator.
2. Open **Resource groups** > **RG-Challenge02** > **Access control (IAM)** > **Add** > **Add role assignment**.
3. Assign each of these built-in roles to the developer at the **RG-Challenge02** scope:

   | Role | Purpose |
   | --- | --- |
   | **Storage Account Contributor** | Create and manage storage accounts. This role does not grant access to storage data. |
   | **Virtual Machine Contributor** | Create and manage virtual machines. |
   | **Network Contributor** | Manage network resources, such as virtual networks and network interfaces. |

4. Sign out. 
5. Sign in to the portal as the developer. Allow a few minutes for the assignments to take effect; sign out and back in again if needed.
6. Create a storage account in **RG-Challenge02**. Use a globally unique, lowercase alphanumeric name such as `cancreatesa<initials>` (3–24 characters). Leave other settings at their defaults.

### Check your work

- [ ] All three roles are assigned to the developer at **RG-Challenge02** scope.
- [ ] The developer can create a storage account in **RG-Challenge02**.

## Create a virtual machine as the developer

1. While signed in as the developer, create a virtual machine named **vm1** in **RG-Challenge02** and **East US**.
2. Select **Windows Server 2025 Datacenter - x64 Gen2** as the image. Select **Standard_D2s_v7** if it is available in the region and for the subscription; otherwise choose an available general-purpose size with at least 2 vCPUs.
3. Use a **Standard HDD** managed OS disk. Choose a local administrator name and enter a unique password.
4. Enable RDP (port **3389**) only from your current public IP address where the portal provides that option. Leave unrelated settings at their defaults.
5. Complete the deployment. In **RG-Challenge02** > **Activity log**, verify the developer's VM creation operation. Activity Log entries can take a few minutes to appear.

### Check your work

- [ ] **vm1** was created in **RG-Challenge02** by the developer.
- [ ] The Activity Log shows the VM creation operation under the developer's identity.

## Design and test a custom VM role

The **Virtual Machine Operator** role is a proof of concept that permits reading VM information and starting or deallocating VMs. In this section, stay signed in as the developer. The assigned built-in roles do not include permission to create Azure role definitions, so the final command should be denied.

1. Open **Cloud Shell** and select **PowerShell**. Confirm that the active account is the developer and the active subscription contains **RG-Challenge02**:

   ```powershell
   Get-AzContext
   ```

2. (Optional) Review the relevant VM provider operations:

   ```powershell
   Get-AzProviderOperation "Microsoft.Compute/virtualMachines/*" |
       Format-Table Operation, Description -AutoSize
   ```

3. Build a custom role definition scoped to **RG-Challenge02**. The scope is derived from the active subscription:

   ```powershell
   $subscriptionId = (Get-AzContext).Subscription.Id
   $roleScope = "/subscriptions/$subscriptionId/resourceGroups/RG-Challenge02"
   $rolePath = Join-Path $HOME "VMOperatorRole.json"

   $roleDefinition = [ordered]@{
       Name             = "Virtual Machine Operator"
       IsCustom         = $true
       Description      = "Can read, start, and deallocate virtual machines."
       Actions          = @(
           "Microsoft.Compute/*/read"
           "Microsoft.Compute/virtualMachines/start/action"
           "Microsoft.Compute/virtualMachines/deallocate/action"
       )
       NotActions       = @()
       DataActions      = @()
       NotDataActions   = @()
       AssignableScopes = @($roleScope)
   }

   $roleDefinition | ConvertTo-Json -Depth 5 |
       Set-Content -Path $rolePath
   Get-Content -Path $rolePath
   ```

4. Attempt to create the role definition:

   ```powershell
   New-AzRoleDefinition -InputFile $rolePath
   ```

   The expected result is an authorization error because the developer does not have `Microsoft.Authorization/roleDefinitions/write`. Do not switch to the administrator account for this test.

### Check your work

- [ ] The JSON names the role **Virtual Machine Operator**, lists the three VM actions, and uses the **RG-Challenge02** assignable scope.
- [ ] The developer can prepare the definition but cannot create the custom role.

## Summary

You assigned built-in roles at resource-group scope, verified that the developer could create storage and VM resources, and designed a custom role definition that the developer was not authorized to create.

## References

- [Azure built-in roles](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles)
- [Azure custom roles](https://learn.microsoft.com/en-us/azure/role-based-access-control/custom-roles)
- [Create or update Azure custom roles using Azure PowerShell](https://learn.microsoft.com/en-us/azure/role-based-access-control/custom-roles-powershell)
