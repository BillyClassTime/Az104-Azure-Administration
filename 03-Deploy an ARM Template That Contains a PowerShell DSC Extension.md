# Deploy an ARM Template That Contains a PowerShell DSC Extension

## Challenge Overview

## Understand the scenario
You are a cloud operations engineer assigned to configure a Windows® virtual machine in Azure®. You need to replace a hard-coded version number of a PowerShell® Desired State Configuration (DSC) extension in an Azure Resource Manager (ARM) template that creates a web app. First, you will edit an ARM template. Next, you will deploy the template. Finally, you will verify that the web app loads successfully.

## Understand your environment
You will be using an Azure resource group named  **RG-Challenge03**

# Update a custom deployment ARM template

- Sign in to the Azure portal as as your user account and password.

- Open the [Windows VM with PowerShell DSC Extension](https://github.com/LODSContent/ChallengeLabs_Resources/blob/master/ais-008/) Azure Resource Manager (ARM) template on GitHub as an Azure Custom deployment.
- Select **Windows VM with PowerShell DSC Extension** to open the link in GitHub, and then select **Deploy to Azure**.

- If prompted, sign in to Azure as your uesr account and password

- Record the most recent version of the [Azure Desired State Configuration extension](https://docs.microsoft.com/en-us/azure/automation/automation-dsc-extension-history) in the following **DSE** text box:

- ```
  ```

> Periodically, Microsoft will deploy new versions of the Azure DSC extension. You can find information about the extension history on the Azure Desired State Configuration extension history page.

> You may see that the documentation no longer matches the image displayed in the hint. You should always check the documentation to ensure that you are referencing the most recent version available.

- Edit the ARM template to include a new string parameter named **extensionVersion** before the **dscConfigurationUrl** parameter, and then assign **<DSEVersion>** as the default value.

- On the Custom deployment blade, select **Edit template**.
- Locate the dscConfigurationUrl parameter on approximately line **28**.
- Add a blank line before the dscConfigurationUrl parameter, and then enter the following code to create a new string parameter that has a default value set to the latest version of the DSC extension:

```json
"extensionVersion": {
          "type": "string",
          "defaultValue": "<DSEVersion>"
        },
```

- In the ARM template, replace the hard-coded value for the **typeHandlerVersion** DSC extension resource with the following value, and then save the template:

```json
"[parameters('extensionVersion')]"
```

Locate the typeHandlerVersion property for the DSC extension resource on approximately line **217**.

- Replace the line with the following code:

```
"typeHandlerVersion": "[parameters('extensionVersion')]",
```

- On the Edit template page, select **Save**.

- Verify that the **Extension Version** parameter is visible on the template deployment page, as shown in the following screenshot:

![](img\03-02.png)

- Leave the Custom deployment page open.

## Check your work

- [ ] Confirm that you added the Extension Version parameter to the Custom deployment blade.

# Create a virtual machine that has a DSC extension by using a custom deployment ARM template

- Deploy the custom template by using the values in the following table. For any property that is not specified, use the default value.

  | Property                   | Value                                                        |
  | :------------------------- | :----------------------------------------------------------- |
  | Resource group             | **RG-Challenge03**                                           |
  | Vm Name                    | VM1                                                          |
  | Vm Size                    | Standard_DS1_v2                                              |
  | Admin Password             | zQw6j@AqgMA64+                                               |
  | DSC Configuration Url      | https://raw.githubusercontent.com/LODSContent/ChallengeLabs_Resources/master/ais-008/WebApp.zip |
  | DSC Configuration Script   | WebApp.ps1                                                   |
  | DSC Configuration Function | WebApp                                                       |

- On the custom deployment blade, in Resource group, select **RG-Challenge03**, in Vm Name, enter VM1, and then in Vm Size, ensure that the value is set to Standard_DS1_v2.
- In Admin Username, ensure that the value is set to azureuser, and then in Admin Password, enter zQw6j@AqgMA64+.
- In DSC Configuration Url, enter https://raw.githubusercontent.com/LODSContent/ChallengeLabs_Resources/master/ais-008/WebApp.zip, and then in Dsc Configuration Script, enter WebApp.ps1.
- In DSC Configuration Function, enter WebApp, and then select **Review + create**.
- Review the configuration, and then select **Create**.

> In this challenge, you MUST specify the exact virtual machine size or the deployment will fail.

> It may take approximately 5–10 minutes for the virtual machine to be deployed.

Record the **public IP address** of **VM1** in the following **Public IP Address** text box:

**Public IP Address**

```
```

- Open a new browser window, go to http://<PIP>, and then verify that the **Test Web App Deployment** page is displayed.

## Check your work

- [ ] Confirm that you created a virtual machine named VM1 and a DSC extension by using a custom deployment template.
- [ ] Confirm that you displayed the Test Web App Deployment page that was installed by the DSC extension.

# Summary

Congratulations, you have completed the **Deploy an ARM Template That Contains a PowerShell DSC Extension** challenge.

In this challenge, you accomplished the following:

- Updated a custom deployment ARM template.
- Created a virtual machine and a DSC extension by using a custom deployment ARM template.

