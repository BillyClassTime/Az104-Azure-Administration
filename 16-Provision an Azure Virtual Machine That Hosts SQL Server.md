# Provision an Azure Virtual Machine That Hosts SQL Server

## Challenge Overview

## Understand the scenario

You are an Azure® administrator. You need to deploy an Azure virtual machine that hosts Microsoft SQL Server®. First, you will create an Azure virtual machine that hosts SQL Server. Next, you will add a data disk to the virtual machine. Finally, you will create a database by using the new data disk.

## Understand your environment

You will be using an Azure resource group named **RG-16** that contains no resources.

# Create an Azure virtual machine that hosts SQL Server

- Sign in to the Azure portal as your user account and password.

- Make sure you are signed in to Azure as the administrator.

- Create an Azure virtual machine that hosts SQL Server 2019 on Windows Server 2019 by using the values in the following table. For any property that is not specified, use the default value.

  | Property             | Value                                                        |
  | :------------------- | :----------------------------------------------------------- |
  | Resource group       | **RG-16**                                                    |
  | Virtual machine name | **SQLVM1**                                                   |
  | Region               | **(US) East US**                                             |
  | Image                | **Free SQL Server License: SQL 2019 Developer on Windows Server 2019 - Gen2** |
  | Size                 | **Standard_DS3_v2 - 4 vcpus 14GiB memory**                   |
  | Username             | AzureAdmin                                                   |
  | Password             | Az!28588029!                                                 |
  | OS disk type         | **Premium SSD**                                              |
  | Boot diagnostics     | **Disable**                                                  |
  | SQL connectivity     | **Private (within Virtual Network)**                         |
  | Port                 | 1433                                                         |
  | SQL Authentication   | **Enable**                                                   |
  
  - On the Azure portal home page, select Create a resource to display the Azure Marketplace.
  
  - In the Azure Marketplace list, search for and select SQL Server 2019 on Windows Server 2019.
  
  - On the SQL Server 2019 on Windows Server 2019 page, in Select a plan, select Free SQL Server License: SQL 2019 Developer on Windows Server 2019, and then select Create.
  - On the Create a virtual machine blade, on the Basics page, in Resource group, select **RG-16**.
  - In Virtual machine name, enter SQLVM1, in Region, ensure that **(US) East US** is selected, and then in Image, ensure that **Free SQL Server License: SQL Server 2019 Developer on Windows Server 2019 - Gen2** is selected.
  - In Size, select **See all sizes**.
  - On the Select a VM size page, in VM Size, select DS3_v2, and then select **Select**.
  - On the Basics page, in Username, enter AzureAdmin, and then in Password and Confirm password, enter Az!28588029!.
  - On the Disks page, in OS disk type, ensure that **Premium SSD** is selected.
  - On the Management page, in Boot diagnostics, select **Disable**.
  - On the SQL Server settings page, in SQL connectivity, ensure that **Private (within Virtual Network)** is selected, in Port, ensure that the value is set to 1433, and then in SQL Authentication, select **Enable**.
  - Select **Review + create**, review the virtual machine specifications, and then select **Create** to deploy the virtual machine.

  > Ignore any warnings about RDP ports as this virtual machine is being used for testing only.

  > The deployment will take approximately 5–10 minutes. Wait for the deployment to complete before moving on to the next task.

  > You can configure an instance of SQL Server in an Azure virtual machine image prior to deployment by using the [SQL Server settings](https://docs.microsoft.com/en-us/azure/azure-sql/virtual-machines/windows/create-sql-vm-portal#3-configure-sql-server-settings) page in the Azure portal.

  

## Check your work

- [ ] Confirm that you created a virtual machine named **SQLVM1** that hosts an instance of SQL Server.

# Add a data disk to the Azure virtual machine

- Create and attach a new data disk named DWDataFiles for **SQLVM1** by using **Standard HDD** storage and a size of 128 GiB.

  - On the Azure portal home page, select **Virtual machines**, and then select **SQLVM1**.

  - On the SQLVM1 resource menu, in Settings, select **Disks**.

  - On the Disks page, in Data disks, select **Create and attach a new disk**, in Disk name, enter DWDataFiles, in Storage type, select **Standard HDD**, and then in Size (GiB), enter 128.

  - On the Disks page, on the command bar, select **Save** to update the virtual machine.

  > You can create and attach a [new data disk](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/attach-managed-disk-portal#add-a-data-disk) to a virtual machine at any time to create extra disk drives for load balancing and performance. 

- Connect to **SQLVM1** by using **RDP**, and then sign in as AzureAdmin using Az!28588681! as the password.

  > Resize the RDP session window so that you can view the instructions for the challenge at the same time.

- Initialize and format the new disk as a simple volume by using the drive letter **H**, the **NTFS** file system, and a volume label of DWDataFiles.

  > If you are prompted to format the H: drive a second time, select Cancel.

- Create a new folder named DWDataFiles on the new disk.

  > Keep the RDP session open for the next task.

## Check your work

- [ ] Confirm that you added a new data disk to **SQLVM1**.
- [ ] Confirm that you initialized and formatted the new data disk.

# Create a database by using the new data disk

- In the RDP session, open **Microsoft SQL Server Management Studio** (SSMS), and then connect to the **SQLVM1** SQL server by using **Windows Authentication**.

  - In the RDP session, on the Start menu, in Microsoft SQL Server Tools 18, select **Microsoft SQL Server Management Studio 18**.

  - In the Connect to Server window, in Server name, ensure that **SQLVM1** is selected, in Authentication, ensure that **Windows Authentication** is selected, and then select **Connect**.

  > You may need to wait for SSMS to start for the first time.

  > In SQL Server, there are two possible [authentication modes](https://docs.microsoft.com/en-us/sql/relational-databases/security/choose-an-authentication-mode): Windows Authentication mode and mixed mode. Windows Authentication mode enables Windows Authentication without enabling SQL Server Authentication. Mixed mode enables both Windows Authentication and SQL Server Authentication. Windows Authentication is always available for any mode you select.

- Create a database named MyDWDB in the **SQLVM1** SQL server by using H:\DWDataFiles\ as the path for the data file and G:\log\ as the path for the log file.

  - In SSMS Object Explorer, right-click **Databases**, and then select **New Database**.
  - On the New Database page, in Database name, enter MyDWDB.


  - In Database files, in MyDWDB, in Path, enter H:\DWDataFiles\, in MyDWDB_log, in Path, ensure that the value is set to G:\log\, and then select OK to create the database.

- Verify that the **MyDWDB** MDF data file is stored in the **H:\DWDataFiles\** folder and the **MyDWDB_log** LDF log file is stored in the **G:\log\** folder.

  > The file extensions .mdf and .ldf may be hidden by default. To view the file extensions in File Explorer, you can enable File name extensions by using the View menu.

## Check your work

- [ ] Confirm that you connected to SQL Server by using Windows Authentication.
- [ ] Confirm that you created a new database by using the new data disk.

# Summary

Congratulations, you have completed the **Provision an Azure Virtual Machine That Hosts SQL Server** challenge.

You have accomplished the following:

- Created an Azure virtual machine that hosts SQL Server.
- Added a data disk to the Azure virtual machine.
- Created a database by using the new data disk.