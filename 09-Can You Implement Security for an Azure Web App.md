# Can You Implement Security for an Azure Web App?

## Challenge Overview

## Understand the scenario

You are an Azure® administrator. You need to allow developers to create a secure web app. First, you will configure account security by using role-based access control. Next, you will create a web app. Finally, you will create a new virtual network, and then you will integrate the web app with the virtual network.

## Understand your environment

You will be using an Azure environment that contains an Azure resource group named **RG9**.

# Configure account security by using role-based access control

- Sign in to the Azure portal as as your user account and password.

- Create the user **user_developer**, it will be used next to complete the objective of this challenge

- Assign the **Website Contributor**, **Web Plan Contributor**, **Network Contributor**, and **Storage Account Contributor** roles to **user_developer** in the **RG9** resource group.

- You can choose something of the following five methods of assigning Azure roles:

  - The Azure portal
  
  
    - Azure PowerShell®
  
  
    - The Azure command-line interface (CLI) 2.0
  
  
    - The REST API
  
  
    - An Azure Resource Manager (ARM) template
  


- The **user_developer** user account will now be able to manage only storage accounts, web plans, websites, and networks in the **RG9** resource group.

- You can sign in as a different account by using the **Sign in** with a different account option on the Azure portal command bar.

- If you going to work in Azure® cloud shell, then create a storage account named **sa[initials]** in the **RG9cs** resource group by using default settings.

## Check your work

- [ ] Verify that you have assigned the required roles to the developer user account.
- [ ] Verify that you have created an Azure storage account.

# Create a web app as a developer

- Create a web app by using the values in the following table. For any property that is not specified, use the default value.

  | Property               | Value                                                      |
  | :--------------------- | :--------------------------------------------------------- |
  | Resource Group         | **RG9**                                                    |
  | Name                   | **MyWebApp-[initials]**                                    |
  | Runtime stack          | **.NET 6 (LST)** or the most recent version not in preview |
  | Operating System       | **Windows**                                                |
  | Region                 | **East US**                                                |
  | Windows Plan (East US) | Create new ***AppPlan1***                                  |
  | Sku and size           | **Standard S1**                                            |

- Configure **MyWebApp-[initials]** to deploy and build code from an External Git repository by using the master branch at https://github.com/BillyClassTime/WebAppMasterDotNet6.git.

	- Consider clone the above repository to your work folder, and then follow the tips of its readme.md file.
- Open a browser window, go to **https://mywebapp-[initials].azurewebsites.net** to verify that **MyWebApp-[initials]** is pulling code from the external Git,  and then close the ASP.NET application browser window.

## Check your work

- [ ] Verify that you have created a web app named **MyWebApp-[initials]**.
- [ ] Verify that the audit log shows that you were signed in as the developer when you created the web app.
- [ ] Verify that you can navigate to the newly implemented app service website.

# Integrate the web app with a virtual network

- Create a virtual network named **VNET-[initials]** by using the **RG9** resource group, the **(US) East US** region, the default address space, and the default subnet address range.

- Integrate **MyWebApp-[initials]** with the **VNET-[initials]** virtual network by using the default subnet.

## Check your work

- [ ] Verify that you have created a virtual network named **VNET**.
- [ ] Verify that **MyWebApp-[initials]**  has been integrated with the VNET virtual network for added security.

# Summary

Congratulations, you have completed the Can You Implement Security for an Azure Web App? challenge.

You have accomplished the following:

- Configured account security by using role-based access control.
- Created a web app as a developer.
- Integrated the web app with a virtual network for added security.