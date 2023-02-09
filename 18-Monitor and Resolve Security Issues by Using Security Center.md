# Monitor and Resolve Security Issues by Using Security Center

## Challenge Overview

## Understand the scenario

You are a system administrator for a company that is migrating virtual machines to Azure. After an initial deployment and configuration of virtual machines, you need to review the security recommendations for a virtual machine, and then resolve the threat protection issues. First, you will connect to an Azure virtual machine by using RDP. Next, you will review security issues for an Azure virtual machine, and then you will resolve a security issue. Finally, you will review the Microsoft Antimalware installation details for a virtual machine.

## Understand your environment

You will be creating the following resources:

| Resource        | Name               | Notes                                                        |
| :-------------- | :----------------- | ------------------------------------------------------------ |
| Resource group  | **RG18**           |                                                              |
| Virtual machine | **VM2**            | Windows (Windows Server 2016 Datacenter), Standard A2 v2, East US |
| Storage account | **sacs[Initials]** |                                                              |

> Important: **do not start** until these resources have been created, you will not be guided through these tasks. 

# Connect to the VM2 virtual machine by using RDP

- Sign in to the Azure portal as your user account and password.

  Make sure you are signed in to Azure as the administrator.

- In the Azure portal, display the **VM2** virtual machine, and then use RDP to connect to **VM2** as student using Pa$$w0rd123456 as the password.
- Turn off **IE Enhanced Security** to enable browsing.
- Minimize the Remote Desktop window—you will use it in an upcoming task.

## Check your work

- [ ] Confirm that you can connect to the **VM2** virtual machine by using RDP.
- [ ] Confirm that IE Enhanced Security Configuration is off.

# Review and resolve Azure security alerts

- Display the **VM2** virtual machine, select **Microsoft Defender for Cloud**, and then review the security recommendations.

  >It may take up to 24 hours for the alerts and recommendations to be automatically generated on the resources that were provisioned for this challenge. If you do not see any recommendations, please wait a few minutes, and then refresh the page. If you still do not see any recommendations, please continue for guidance on manually installing the Microsoft Antimalware extension.

- Select the **Endpoint Protection** recommendation, and then install the **Microsoft Antimalware** solution (extension) on **VM2** by using the following settings:

  - Exclude C:\Windows.

  - Schedule a **Quick Scan** every **Sunday** with a scan time of 180 minutes.

- If you still do not see the Endpoint Protection recommendation, install manually the Microsoft Antimalware extension.

  - On the **VM2** page, in Settings, select Extensions + applications, and then select Add.
  - On the Install an Extension page, scroll down and select Load more, select Microsoft Antimalware, and then select Next.

  - On the Configure Microsoft Antimalware Extension blade, in Excluded files and locations, enter C:\Windows.

  - In Scan type, ensure that Quick is selected, in Scan day, select Sunday, and then in Scan time, enter 180.

  - Select Review + create, review the configuration, and then select Create.

  - After you manually install Microsoft Antimalware, if you don't see recommendations, you can confirm deployment by viewing the deployment confirmation notification on your Azure portal.

- When the installation is complete, verify that the Endpoint Protection recommendation state has changed to **Resolved**, if applicable.

## Check your work

- [ ] Confirm that you reviewed the current monitoring state of the **VM2** virtual machine.
- [ ] Confirm that you started the Endpoint Protection installation on **VM2**.
- [ ] Confirm that the Endpoint Protection state has changed to Resolved, if applicable.

# View the Microsoft Antimalware installation on VM2

> You may see the following notification message in the Azure portal: "Installation has started on 1 virtual machines. It may take a while to complete." You can continue with this task without waiting for the installation to complete.

- Configure an Azure Cloud Shell **PowerShell** session by using the existing **RG18** resource group, the existing storage account in the **East US** region, and a new file share named cloud-shell.
- Retrieve the status of the IaaSAntimalware extension on the **VM2** virtual machine in the **RG18** resource group by using the Get-AzureRMVMExtension cmdlet in the Azure Cloud Shell.
- Switch to the **VM2** Remote Desktop window, open **Task Manager**, and then verify that the **Antimalware Service Executable** process is running.

## Check your work

- [ ] Confirm that you created a PowerShell Cloud Shell environment using a file share named cloud-shell in the existing storage account.
- [ ] Confirm that you were able to view the current antimalware settings on **VM2** by using PowerShell in Cloud Shell.
- [ ] Confirm that the Antimalware Service Executable is running in Task Manager on **VM2**.

# Summary

Congratulations, you have completed the **Monitor and Resolve Security Issues by Using Azure Security Center** Challenge Lab.

You have accomplished the following:

- Connected to an Azure virtual machine using RDP.
- Reviewed security issues for an Azure virtual machine.
- Resolved a security issue.
- Viewed the Microsoft Antimalware installation details for a virtual machine.