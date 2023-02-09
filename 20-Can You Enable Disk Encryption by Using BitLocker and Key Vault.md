# Can You Enable Disk Encryption by Using BitLocker and Key Vault?

## Challenge Overview

## Understand the scenario

You are a system administrator for a company that is migrating their servers to Azure. You need to ensure that the disks used by the virtual machines are encrypted using BitLocker and Azure Key Vault. Since you plan on automating this process in the future, you will use Azure Cloud Shell to enable disk encryption on one virtual machine named **VM4** using.

## Understand your environment

You will be creating the following resources:

| Resource        | Name               | Notes                                                        |
| :-------------- | :----------------- | ------------------------------------------------------------ |
| Resource group  | **RG20**           |                                                              |
| Virtual machine | **VM4**            | Windows (Windows Server 2016 Datacenter), Standard A2 v2, East US |
| Storage account | **sacs[Initials]** |                                                              |

> Important: **do not start** until these resources have been created, you will not be guided through these tasks. 

# Create an Azure key vault

- Sign in to the Azure portal as your user account and password.

  Make sure you are signed in to Azure as the administrator.

- Create a key vault named **KeyVault[Initials]** in the **RG20** resource group, using the **East US** location and the **Standard** pricing tier.

## Check your work

- [ ] Verify that an Azure Key Vault exists

# Configure Azure Cloud Shell

- Create a file share named **cloud-shell** in the existing storage account, and then set the Quota to 6.
- In Cloud Shell, configure the PowerShell **advanced settings** using the existing storage account in the **East US** region and the cloud-shell file share, and then attach the storage.

## Check your work

- [ ] Confirm that you have created a file share named **cloud-shel**l and that the Quota is set to 6.
- [ ] Confirm that PowerShell is open in Azure Cloud Shell.

# Create an Azure Active Directory application and service principal

- In Cloud Shell, create a variable named **appName** to hold the application name VaultApp[Initials].

  ```PowerShell
  $appName = "VaultApp[Initials]"
  ```

- Create a password credential named **securePassword** to hold the application password P@ssw0rd!.

  ```PowerShell
  $securePassword = [Microsoft.Azure.PowerShell.Cmdlets.Resources.MSGraph.Models.ApiV10.MicrosoftGraphPasswordCredential]@{ 
      "SecretText" = "P@ssw0rd!"
  }
  ```

- Create the Azure Active Directory application to be used by the key vault using the variables **$appName** and **$securePassword**.

  >Remember to use: New-AzADApplication with DisplayName  and PasswordCredentials parameters.

- Create the new Azure Active Directory service principal.

  > Remember to use: New-AzureADServicePrincipal with adequate parameters.

## Check your work

- [ ] Verify the application and service principal configuration.

# Add an Azure key vault key and configure the key vault access policy

- Create an Azure key vault key named **VaultKey** in the **KeyVault[Initials]** key vault using the Software destination.
- Set the **KeyVault[Initials]** key vault access policy to allow the **VaultApp[Initials]** application and **service principal** to have the following permissions:
  - PermissionsToKeys "WrapKey"
  - PermissionsToSecrets "Set"

- Enable the key vault for **Azure Disk Encryption for volume encryption** in the advanced access policy settings of the access policy.

  > Consider to use: Set-AzKeyVaultAccessPolicy or review [Creating and configuring a key vault for Azure Disk Encryption on a Windows VM - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-key-vault?tabs=azure-portal)

## Check your work

- [ ] Verify that the **VaultApp[Initials]** application was added to the access policy of the key vault.
- [ ] Verify that disk encryption was enabled in the key vault access policy.

# Enable Azure disk encryption on the virtual machine

> The virtual machine **VM4** must be running to complete this task.

- Create the variables to be used by the Set-AzVMDiskEncryptionExtension command

  ```
  $keyVault = Get-AzKeyVault -VaultName KeyVault[Initials] -ResourceGroupName RG20
  
  $diskEncryptionKeyVaultUrl = $keyVault.VaultUri
  
  $keyVaultResourceId = $keyVault.ResourceId
  
  $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName KeyVault[Initials] -Name VaultKey).Key.kid
  ```

- Enable Azure disk encryption on **VM4** using the Set-AzVMDiskEncryptionExtension command .

  >Helpful parameter values
  >
  >-ResourceGroupName RG20
  >
  >-VMName "VM4"
  >
  >-AadClientID $app.ApplicationId
  >
  >-AadClientSecret (New-Object >PSCredential "user",$securePassword).GetNetworkCredential().Password
  >
  >-DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl
  >
  >-DiskEncryptionKeyVaultId $keyVaultResourceId
  >
  >-KeyEncryptionKeyUrl $keyEncryptionKeyUrl
  >
  >-KeyEncryptionKeyVaultId $keyVaultResourceId

> Do not continue until you receive a successful message. This can take 10-15 minutes. If you need to repeat the task, ensure that the virtual machine is running.

## Check your work

- [ ] Verify that the disk of **VM4** has been encrypted.

# Summary

Congratulations, you have completed the **Can You Enable Disk Encryption by Using Bitlocker and Key Vault?** challenge.

You have accomplished the following:

- Created an Azure Key Vault
- Configured Azure Cloud Shell
- Created an Azure Active Directory application and service principal
- Added an Azure Key Vault key
- Configured a key vault access policy
- Enabled Azure disk encryption on a virtual machine