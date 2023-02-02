# Can You Use RBAC and Design a Custom Role?

### Challenge Overview

## Understand the scenario

You are an Azure administrator for a company that is migrating its primary web app from its on-premises datacenter to Azure. You need to allow developers to create and deploy Azure virtual machines by assigning an appropriate role. You also need to design a custom role definition, as a proof of concept.

## Understand your environment

You are provided with an Azure resource group that initially contains no resources. You will create the necessary resources to complete the challenge.

You can expect to find the following resource:

Resource group **RG-Challenge02**

# Assign built-in roles and verify permissions

- Sign in to the Azure portal as your user account and password.

  Make sure you are signed in to Azure as the administrator.

- Use role-based access control (RBAC) to allow a developer **new_account_as_developer** (must be created if not exits) to manage certain resources in the resource group **RG-Challenge02** by adding role assignments for the developer. The developer should only be able to manage the following resources: <u>storage accounts</u>, <u>virtual machines</u>, and <u>networks</u>.

- 
  Verify the new access control by signing in to Azure as **new_account_as_developer**  and create a storage account named **cancreatesa[your initials]** default settings in the resource group  **RG-Challenge02** .

- It may take a few minutes for the new role assignments to be fully active.

- If you get an unexpected result when you check your work, please wait a few minutes and try again.


## Check your work
- [ ] Verify that you have assigned the correct roles to the developer user account.
- [ ] Verify that you have created the Azure Storage account successfully. [Assign Azure roles using the Azure portal - Azure RBAC | Microsoft Learn](https://learn.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal)


# Create a virtual machine as a developer

- Make sure you are signed in to Azure as the developer: **new_account_as_developer**

- In the **RG-Challenge02** resource group, create an Azure VM named vm1. Configure the VM to use Windows Server 2016 Datacenter.

- Set the size of the VM to Standard B2s with HDD managed disks and enable RDP access. Create a Server admin user named testuser with a password of Pa55w.rd1234. Turn off the Auto Shutdown option and disable all Monitoring options.

- It may take a few minutes for the audit log to be updated after the virtual machine has been created.

- If you get an unexpected result when you check your work, please wait a few minutes and try again.

## Check your work

- [ ] Verify that the Azure virtual machine has been created successfully.

- [ ] Verify that the audit log shows that the developer has created the virtual machine.




# Design a custom role

You need to design a custom role named Virtual Machine Operator that allows the user to view virtual machine information, and also start and shut down virtual machines.

Make sure you are signed in to Azure as the administrator, (you must use your account like owner of your subscription).

Start a CloudShell session by using Windows PowerShell. If prompted to create storage, select Show advanced settings. Then choose the **RG-Challenge01cs** resource group (If not exists create it), a new storage account named ** ****RG-Challenge06cs** **** in the **East US** region, and a new file share named **shell**.

Use the following command to identify the operations associated with virtual machines:

```powershell
Get-AzProviderOperation "Microsoft.Compute/virtualmachines/*" | FT Operation, Description -AutoSize
```

Note the operations for read, start, and deallocate.

Use the following command to retrieve the built-in role definition for Virtual Machine Contributor:

```powershell
Get-AzRoleDefinition -Name "Virtual Machine Contributor" | ConvertTo-Json | Out-File $home\clouddrive\VMOperatorRole.json
```

Open the VMOperatorRole.json file in the code editor by using the following commands:

```powershell
cd $home\clouddrive
code VMOperatorRole.json
```

In the code editor, update the following:

Change the Name property value to: **Virtual Machine Operator**
Delete the line with the **Id** property
Change the **IsCustom** property value to: **true**
Change the Description property value to: **Lets you view, start and stop virtual machines.**
Change the list of actions so that it contains only 3 actions (with no comma on end of the last one): **"Microsoft.Compute/*/read", "Microsoft.Compute/virtualMachines/start/action",**
**"Microsoft.Compute/virtualMachines/deallocate/action"**
Save the file by selecting the ellipsis and choosing Save. Close the code editor.

Attempt to create the new custom role by executing the following statement:

```powershell
New-AzRoleDefinition -InputFile "VMOperatorRole.json"
```

You will receive an expected error stating that you are not authorized to create a custom role.

## Check your work

- [ ] Verify that you have created the required CloudShell storage account successfully.
- [ ] Verify that you have created the required CloudShell file share successfully.

- [ ] Verify that you have created the required custom role JSON file successfully.


### Summary
Congratulations, you have completed the Can You Use RBAC and Design a Custom Role? challenge.

You have accomplished the following:

- Assigned built-in roles and verified permissions.

- Created a virtual machine as a developer.
- Designed a custom role.



