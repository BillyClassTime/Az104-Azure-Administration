# Provision an Azure Storage Table

## Challenge Overview
## Understand the scenario

You are a system administrator for a company that is migrating their document processing system from their data center to Azure. You need to create an Azure Storage Table that will store the document processing history. You will first create an Azure Storage Account. You will then create a Storage Table within the Storage Account and activate logging for the Storage Account. Next, you will add the connection string for the Storage Account to a Web Application. Finally, you will test the Storage Table with the Web Application and verify that activity is being logged.

## Understand your environment
You must create an Azure Resource Group that contains a Azure Web App. The configuration of the Web must deploy the following application localized on GitHub. 

```https://github.com/BillyClassTime/Provision_an_azure_storage_table.git```

Take a moment to familiarize yourself with what you have in the environment.

You must create the following resources:

- Resource group: **RG11**
- App Service Plan: **Asp-[Initials]** (Windows, Standard S1)
- Web app: **lods[Initials]**
  - Configure **lods[Initials]** to deploy and build code from an External Git repository by using the master branch at https://github.com/BillyClassTime/ArchitectureChallange.git.
    - Consider clone the above repository to your work folder, and then follow the tips of its readme.md file.

# Configure account security by using role-based access control

- Sign in to the Azure portal as as your user account and password.

- Create the user **user_developer**, it will be used next to complete the objective of this challenge

- Assign the **Website Contributor**, **Web Plan Contributor**,  **Storage Account Contributor**, and  **Monitor Contributor** roles to **user_developer** in the **RG11** resource group.

- You can choose something of the following five methods of assigning Azure roles:

  - The Azure portal


    - Azure PowerShell®


    - The Azure command-line interface (CLI) 2.0


    - The REST API


    - An Azure Resource Manager (ARM) template


- The **user_developer** user account will now be able to manage only storage accounts, web plans, websites, and monitor in the **RG11** resource group.
- You can sign in as a different account by using the **Sign in** with a different account option on the Azure portal command bar.
- If you going to work in Azure® cloud shell, then create a storage account named **cssa[initials]** in the **RG11cs** resource group by using default settings.

## Check your work

- [ ] Verify that you have assigned the required roles to the developer user account.
- [ ] Verify that you have created an Azure storage account.

# Create a Storage Account

- Sign in to the Azure portal as **user_developer** using **P455wrd.rd1234** as the password.
- Create a general purpose Azure Storage Account named **sa[Initials]** with **Locally-redundant storage** in the **RG11** resource group. Verify the Performance is set to **Standard**.
- Navigate to the **sa[Initials]** blade in the portal.
- Add a Storage Table named **history** to the **sa[Initials]** Storage Account.
- Navigate to the **Diagnostics settings (classic)** window of the **sa[Initials]** blade.
- Activate **Read** and **Write** logging under **Table properties** and **Save** the changes.

## Check your work

- [ ] A storage Account named **sa[Initials]** has been created.
- [ ] The Storage Account contains a Storage Table named **history**.
- [ ] Read and Write logging is active for the **history** Storage Table.

# Update the Web App

- Navigate to the **Access keys** page of the **sa[Initials]** blade.

- Copy the **Connection string** for **key1** and paste it here:

  ```
  
  ```

- Navigate to the **lods[Initials]** Web App blade.

- Open the **Configuration settings** page.

- Add a new application setting named **storageConnectionString**.

- Set the connection string **Value** to <key>.

- **Save** the changes.



## Check your work

- [ ] The connection string for key1 of the **sa[Initials]** Storage Account appears in the correct box.
- [ ] A connection string named **storageConnectionString** has been added to the **lods[Initials]** Web App with the value of the Storage Account connection string.


# Test the Storage Table

- Open the test page at https://lods[Initials].azurewebsites.net/.

- Any value other than **ID:234** is invalid. It is caused due misspelling or empty connections string.

- Return to the Azure portal.

- Open a **Cloud Shell** session.

- Select **Bash (Linux)** as the shell type.

- Select **Show advanced settings** .

- Verify that the existing **RG11** Resource group and **sa[Initials]** Storage account are selected.

  > Remember: If your storage account doesn't appear, change the location to be the same location that you used for the storage account.

- Create a new **File share** named **bash** and then select **Create storage**.

- Run the following command to verify that logging is active for the **sa[Initials]** Storage Account. If the command returns results, logging is active.

  ```bash
  key=$(az storage account keys list -n sa[Initials] -g RG11 --query "[*] | [0].value")
  az storage container show  --account-name sa[Initials] --account-key $key --name "\$logs"
  ```
  
  > Remember: Storage Account logs are written to a container named **$logs**. This container is hidden unless is it specifically identified in commands.
  
- Run the following command to download the Storage Account log file(s).

  ```bash
  az storage blob download-batch --destination . --source "\$logs" --account-name sa[Initials] --account-key $key
  ```

- It may take 10-15 minutes for the log files to appear in the **$logs** Storage Container. You do not need to wait for the file to appear.

## Check your work

- [ ] The Web App has loaded rows to the Storage Table named **history** in the **sa[Initials]** Storage Account.
- [ ] The Web App has read rows from the Storage Table named **history** in the **sa[Initials]** Storage Account.
- [ ] The Storage Table activity is logged under the **$logs** Blob storage container.

## Summary

Congratulations! You have completed the Provision an Azure Storage Table challenge.

You have accomplished the following:

- Provisioned an Azure Storage Account
- Created a table in the Storage Account
- Updated a Web Application to work with the Storage Account
- Tested the table with a custom Web Application