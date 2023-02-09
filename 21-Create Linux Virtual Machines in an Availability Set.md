# Create Linux Virtual Machines in an Availability Set

## Challenge Overview

## Understand the scenario

You are a system administrator for a company that is migrating a legacy, server–based application that uses Linux from their datacenter to Azure®. You need to implement redundancy for the front-end server tier to maintain application availability. First, you will create an availability set. Next, you will create an SSH key pair. Finally, you will create two Linux virtual machines in the availability set.

## Understand your environment

You will be creating the following resources:

| Resource        | Name           | Notes                          |
| :-------------- | :------------- | ------------------------------ |
| Resource group  | **RG21**       |                                |
| Virtual Network | **app-vnet**   | 10.0.0.0/16 East US            |
| Subnet          | frontend       | 10.0.0.0/24 beloging app-nvent |
| Storage account | sacs[Initials] |                                |

> Important: **do not start** until these resources have been created, you will not be guided through these tasks. 

# Create an availability set

- Sign in to the Azure portal as your user account and password.

  Make sure you are signed in to Azure as the administrator.

- Create a managed availability set named **app-frontend-avset** in the **RG21** resource group by using 2 fault domains and 5 update domains.

  >You can use an availability set to provide redundancy for the front-end server tier. This helps to maintain application availability in the event of a power source or network switch failure, or when you need to reboot a server.
  >
  >Virtual machines in an availability set should use managed disks for improved reliability.

## Check your work

- [ ] Confirm that you created a managed availability set named **app-frontend-avset** that has two fault domains and five update domains.

# Create an SSH key pair

- Configure an Azure Cloud Shell **Bash** session by using the existing **RG21** resource group, an existing storage account named **sacs[Initials]** in the **East US** region, and a new file share named cloud-shell-share.

- Create an SSH key pair that uses the rsa algorithm, a key size of 4096 bits, the default file path, and no passphrase by using the [ssh-keygen](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys#supported-ssh-key-formats) command.

  > Remember: You can create an [SSH key pair](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys#provide-an-ssh-public-key-when-deploying-a-vm) by using the ssh-keygen command, and then you can use the key pair when you create a Linux virtual machine in Azure.

- Run the following command to display the generated RSA public key:

  ```
  cat ~/.ssh/id_rsa.pub
  ```

- Copy the public key, and then record the public key in the following **RSA Public Key** text box.

  **RSA Public Key**

  ```
  ```

  > You will use this key in the next task.

- Close the **Cloud Shell** window.

  > Remember: Cloud Shell uses either Bash or PowerShell®. The [bash](http://manpages.ubuntu.com/manpages/focal/en/man1/bash.1.html) manpage contains information about the syntax used in bash commands. You use the [cat](http://manpages.ubuntu.com/manpages/focal/en/man1/cat.1plan9.html) command to read the contents of a file.

## Check your work

- [ ] Confirm that you created a Bash session in Cloud Shell.
- [ ] Confirm that you created and recorded an SSH key pair.

# Create Linux virtual machines in an availability set

- Create an Azure virtual machine by using the values in the following table. For any property that is not specified, use the default value.

  | Property              | Value                                           |
  | :-------------------- | :---------------------------------------------- |
  | Resource group        | **RG21**                                        |
  | Name                  | app-frontend-vm1                                |
  | Availability options  | **Availability set**                            |
  | Availability set      | **app-frontend-avset**                          |
  | Image                 | **Ubuntu Server \*latest version\* LTS - Gen1** |
  | Size                  | DS1_v2                                          |
  | SSH public key source | **Use existing public key**                     |
  | SSH public key        | <RSAKey>                                        |
  | Boot diagnostics      | **Disable**                                     |

  > Alert: In this challenge, you MUST specify the exact virtual machine size or the deployment will fail.

  > You can create an availability when you create a virtual machine; however, in this challenge, you pre-created the availability set.

  > Remember: You can create an Azure virtual machine by using the [Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal), [Azure PowerShell](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-powershell), the Azure command-line interface [CLI 2.0](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-cli), or an [Azure Resource Manager (ARM) template](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-template).

- Create a second Azure virtual machine by using the values in the following table. For any property that is not specified, use the default value.

  | Property              | Value                                           |
  | :-------------------- | :---------------------------------------------- |
  | Resource group        | **RG21**                                        |
  | Name                  | app-frontend-vm2                                |
  | Availability options  | **Availability set**                            |
  | Availability set      | **app-frontend-avset**                          |
  | Image                 | **Ubuntu Server \*latest version\* LTS - Gen1** |
  | Size                  | DS1_v2                                          |
  | SSH public key source | **Use existing public key**                     |
  | SSH public key        | <RSAKey>                                        |
  | Boot diagnostics      | **Disable**                                     |

- Verify that **app-frontend-vm1** and **app-frontend-vm2** are in the **app-frontend-avset** availability set.

  > The two virtual machines are in different fault and update domains.

## Check your work

- [ ] Confirm that you created two virtual machines named **app-frontend-vm1** and **app-frontend-vm2** in the **app-frontend-avset** availability set.

# Summary

Congratulations, you have completed the **Create Linux Virtual Machines in an Availability Set** challenge.

You have accomplished the following:

- Created a managed availability set.
- Created an SSH key pair.
- Created two Linux virtual machines in an availability set.