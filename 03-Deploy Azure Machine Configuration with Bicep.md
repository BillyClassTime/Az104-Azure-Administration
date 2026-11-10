# Deploy Azure Machine Configuration with Bicep

## Challenge overview

Deploy a Windows virtual machine and assign the built-in **Azure Windows Baseline** with Azure Machine Configuration. The Bicep template is stored in this repository, so the deployment does not depend on a separately hosted template or configuration package.

> **Technology update:** This challenge replaces the legacy [DSC VM extension](https://learn.microsoft.com/azure/virtual-machines/extensions/dsc-windows), which is scheduled to retire on **March 31, 2028**. Azure Machine Configuration is Microsoft's recommended service for guest configuration. The former DSC exercise remains available in the repository's Git history.

## Prerequisites

- An Azure account with permission to register the `Microsoft.GuestConfiguration` resource provider at subscription scope, and to create virtual machines, networking resources, guest configuration assignments, and system-assigned identities in the resource group.
- Azure PowerShell with Bicep deployment support (`Az.Resources` 5.6.0 or later).
- The `Microsoft.GuestConfiguration` resource provider registered in the subscription. The template creates a system-assigned managed identity and installs the Machine Configuration extension.
- Outbound HTTPS connectivity from the VM. The template attaches a public IP for outbound access; its network security group has no inbound allow rules.
- A resource group named `RG-Challenge03` in a region that supports the selected VM size and Windows Server image.

## Deploy the template

1. Review [`challenge-03-machine-configuration.bicep`](templates/challenge-03-machine-configuration.bicep). It defines a Windows Server 2025 VM, its network, the Machine Configuration extension, and an assignment of the built-in `AzureWindowsBaseline` configuration.
2. Sign in to the correct Azure tenant and subscription, then select the subscription:

   ```powershell
   Connect-AzAccount
   Set-AzContext -SubscriptionId '<subscription-id>'
   ```

3. Check the Guest Configuration provider registration. If it is not registered, register it and wait until its state is `Registered`:

   ```powershell
   Register-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
   Get-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration' |
       Select-Object ProviderNamespace, RegistrationState
   ```

   Continue when `RegistrationState` is `Registered`.

4. From the repository root, deploy the template. Enter a unique VM name and create a strong local administrator password when prompted:

   ```powershell
   $adminPassword = Read-Host 'Enter a strong VM administrator password' -AsSecureString
   New-AzResourceGroupDeployment `
       -ResourceGroupName 'RG-Challenge03' `
       -TemplateFile './templates/challenge-03-machine-configuration.bicep' `
       -vmName 'VM1' `
       -adminUsername 'azureuser' `
       -adminPassword $adminPassword
   ```

   Keep the password private; do not put it in the template or a committed parameters file. The default VM size is `Standard_D2s_v7`. If that size is unavailable in the chosen region or subscription, pass an available size with `-vmSize`.

5. In the Azure portal, search for **Guest Assignments**, open the `AzureWindowsBaseline` assignment for **VM1**, and wait for its first compliance report. The template uses **Audit** mode, so it reports machine state without applying changes. The initial report can take several minutes to appear.

### Check your work

- [ ] The deployment completed successfully and created `VM1` in `RG-Challenge03`.
- [ ] The VM has an `AzureWindowsBaseline` guest configuration assignment.
- [ ] The assignment reports compliance details in the Azure portal.

### Clean up

When finished, delete `RG-Challenge03` if it contains only resources created for this challenge. The VM and its network resources incur charges while they exist.

## Summary

You deployed Azure resources from a repository-hosted Bicep template and assigned a built-in Azure Machine Configuration baseline to a Windows VM.
