# Configure a Near Real-Time Metric Alert 

## Challenge Overview

## Understand the scenario

You are a cloud operations engineer assigned to monitor a Linux virtual machine in Azure®. You need to configure a near real-time metric alert. First, you will create a Linux virtual machine. Next, you will create an action group, and then you will create a near real-time metric alert that uses the action group. Finally, you will test the alert.

## Understand your environment

You will create the necessary resources to complete the challenge. Azure environment must contains a resource group named **RG12** and a storage account named **cs[initials]**

# Create a Linux virtual machine
- Sign in to the Azure portal as your user account and password.

  Make sure you are signed in to Azure as the administrator.

- Configure an Azure Cloud Shell Bash session by using the existing **RG12** resource group, an existing storage account named **cs[initials]** in the East US region, and a new file share named **cloud-shell-share**.

- Create a new Linux virtual machine that generates an SSH key pair by using the **az vm create command** and the values in the following table:

	| Property       | Value           |
	| :------------- | :-------------- |
	| Resource group | **RG12**        |
	| Name           | VM1             |
	| Image          | UbuntuLTS       |
	| Size           | Standard_DS1_v2 |
	| Admin Username | azureuser       |

- Record the public IP address of VM1 in the following Public IP Address text box:

  - Public IP Address

    ```
    ```

- In Cloud Shell, create an [SSH](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys#ssh-into-your-vm) connection to the virtual machine by using azureuser@<PIP>.

- Update the Linux virtual machine by using the **sudo apt-get update** and the update command.

- Close the **Cloud Shell** window.

## Check your work

- [ ] Confirm that you created a Bash session in Cloud Shell.
- [ ] Confirm that you created a Linux virtual machine named **VM1** in the **RG12** resource group.
- [ ] Confirm that you recorded the public IP address of **VM1** for later use.
- [ ] Confirm that you updated the VM1 virtual machine

# Create an action group

- Create an action group by using the values in the following table:

  | Setting           | Value                               |
  | :---------------- | :---------------------------------- |
  | Resource group    | **RG12**                            |
  | Action group name | Cloud Operations                    |
  | Short name        | CloudOps                            |
  | Notification type | **Email/SMS/Push/Voice**            |
  | Notification name | Email CloudOps                      |
  | Email             | Your personal or work email address |

  > Remember: Actions groups are manage actions of Monitor services.
  >
  > You can create an action group before or during the creation of an alert rule by using the Azure portal, the ***Set-AzActionGroup*** cmdlet , or the ***az monitor action-group create*** Azure CLI 2.0 command.
  >
  > You can use action groups to configure preferences for actions that you want Azure to take when a specific monitored event occurs.

- Verify that your personal email has been added to the Cloud Operations action group.

## Check your work

- [ ] Confirm that you created an action group named **Cloud Operations** that is configured to send an email to your personal email account.

# Create a virtual machine metric alert

- Create an alert rule named Percentage CPU greater than 85 for **VM1** that will send a notification that has a description of Alert when the average Percentage CPU is greater than 85 to the **Cloud Operations** alert group.

  > Remember: You can create alert from monitoring category of virtual machine Azure Portal.

- Add a condition that will trigger the alert when the average Percentage CPU is greater than a threshold of 85 for a period **1 minute**.

  > Remember: You can create a metric alert rule by using the Azure portal, the **Add-AzMetricAlertRuleV2** cmdlet, or the **az monitor alert create** Azure CLI 2.0 command.
  >
  > An alert rule has three parts:
  >
  > - **Scope** - Select the target(s) you want to monitor.
  > - **Condition** - Specify the logic that causes the alert to fire.
  > - **Actions** - Configure the actions to take when the alert fires—email notifications, text messages, web hooks, runbooks, functions, logic apps, or integration with external IT Service Management (ITSM) solutions.

## Check your work

- [ ] Confirm that you created an alert rule named **Percentage CPU greater than 85** that will send an email to the Cloud Operations alert group when the average percentage CPU on **VM1** is greater than a threshold of 85 for a period of one minute.

# Test the near real-time metric alert

- Open **Cloud Shell**, and then run the following command to re-establish an SSH connection to the virtual machine:

  ```
  ssh azureuser@<PIP>
  ```

- Run the following command to install the stress tool:

  ```
  sudo apt-get install stress
  ```

- Run the following command to generate a CPU load of 8 hogs on the virtual machine for a period of 480 seconds:

  ```
  sudo stress --cpu 8 --timeout 480 &
  ```

  > Leave the Cloud Shell window open to ensure that the generated CPU load triggers a notification alert.
  >
  > It may take up to 10 minutes for the alert to become active.

- Verify that you have received an email notification that contains the text **Azure monitoring rule 'Percentage CPU greater than 85' was activated for 'virtualMachines/VM1'**, and then close the **Cloud Shell** window.

## Check your work

- [ ] Confirm that you generated a CPU load of 8 hogs on the virtual machine for a period of 480 seconds.
- [ ] Confirm that you received an email notification stating that the *Azure monitoring rule 'Percentage CPU greater than 85' was activated for 'virtualMachines/VM1'*.

## Summary

Congratulations, you have completed the **Configure a Near Real-Time Metric Alert** challenge.

In this challenge, you accomplished the following:

- Created a Linux virtual machine.
- Created an action group.
- Created a near real-time metric alert on a virtual machine.
- Tested the near real-time metric alert.
