# Cloud - Azure - Windows VMs Opspack

Azure Windows VMs allow you to monitor various performance metrics for Windows virtual machines, these include metrics for Logical Disk, Memory, Processor, System and Heartbeat. The service checks in this Opspack are designed to complement the service checks in [Azure - Virtual Machines Opspacks](https://github.com/opsview/cloud-azure-virtual-machines).

## What You Can Monitor

This Opspack allows you to monitor the performance metrics for Windows virtual machines.

## Service Checks

| Host Template | Service Check | Description |
|:------------- |:------------- |:----------- |
| Cloud - Azure - Windows VMs | Azure - Windows VM - Memory | The percentage of used memory and the available memory in bytes [Default Timespan = 5mins, Granularity = 5mins] |
| Cloud - Azure - Windows VMs | Azure - Windows VM - Processor Time | The percentage of processor time [Default Timespan = 5mins, Granularity = 5mins] |
| Cloud - Azure - Windows VMs | Azure - Windows VM - Processor Queue Length | The processor queue length [Default Timespan = 5mins, Granularity = 5mins] |
| Cloud - Azure - Windows VMs | Azure - Windows VM - Logical Disk Space | The percentage of free space and the amount of free space in bytes [Default Timespan = 5mins, Granularity = 5mins] |
| Cloud - Azure - Windows VMs | Azure - Windows VM - System Uptime | The system uptime [Default Timespan = 5mins, Granularity = 5mins] |
| Cloud - Azure - Windows VMs | Azure - Windows VM - Heartbeat | The time between the last two heartbeats [Default Timespan = 5mins, Granularity = 5mins] |
| Cloud - Azure - Windows VMs | Azure - Windows VM - Restarts | The number of restarts, and information on the user and comment history [Default Timespan = 1440mins, Granularity = 1440mins] |
| Cloud - Azure - Windows VMs | Azure - Windows VM - Shutdowns | The number of shutdowns, and information on the user and comment history [Default Timespan = 1440mins, Granularity = 1440mins] |

## Prerequisites

* Ensure your Opsview Monitor version is newer than 07 September 2018. Check [Opsview Release Notes](https://knowledge.opsview.com/v6.0-EA/docs/whats-new) for the latest version of Opsview Monitor.


## Setup Azure for Monitoring

To monitor your Azure environment, you need to configure it for monitoring. This requires Administrator access on Azure. You need to retrieve the following credentials:

* Subscription ID
* Tenant/Directory ID
* Client/Application ID
* Secret Key

#### Step 1: Find Subscription ID

The Subscription ID can be found in the **Subscriptions** section under the **All services** section from the Azure dashboard.

![Find Azure Subscription ID](/docs/img/azure_find_subscription_id_1.jpg?raw=true)

![Find Azure Subscription ID](/docs/img/azure_find_subscription_id_2.jpg?raw=true)

#### Step 2 : Find the Tenant/Directory ID

The Tenant/Directory ID can be found in the **Azure Active Directory** under the **Properties** section from the Azure dashboard.

![Find Azure Tenant/Directory ID](/docs/img/azure_find_directory_id.png?raw=true)

#### Step 3: Find the Client/Application ID for your application

You need to create and register your application if you haven't already. Use the following documentation from Microsoft: [Create an Azure Active Directory application](
https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#create-an-azure-active-directory-application)

The Client/Application ID can be found in **Azure Active Directory** under the **App registrations** section from the Azure dashboard.

![Find Azure Client/Application ID](/docs/img/azure_find_application_id.png?raw=true)

#### Step 4: Generate the Secret Key for your application

You will need to create a Secret Key for your application, once this has been created its value will be hidden so save the value during creation.

To create the Secret Key, select your application from the list, select the **Settings** within your application and then select the **Keys** option.

There you can create a new key by adding the description and expiration period and the value will be generated.

![Create Secret Key](/docs/img/azure_create_secret_key.png?raw=true)

#### Step 5: Provide access to the subscription you wish to monitor

Navigate to the **Subscriptions** section and select the Subscription you selected before.

In the Subscription to be monitored, click **Access Control (IAM)**.

Then click the **Add** button, select the required role and select the application, once for each of the following roles:
* **Reader**

![Add Subscription to Application](/docs/img/azure_add_subscription_1.png?raw=true)

![Add Subscription to Application](/docs/img/azure_add_subscription_2.png?raw=true)

If you are running more than one subscription these steps will need to be done for each one you wish to monitor.

## Setup Azure Log Analytics

This Opspack utilises Azure Log Analytics to collect data directly from your Azure Virtual Machines. To enable Azure Log Analytics follow the steps outlined below. If you already have an Azure Workspace, you can skip Step 1.

#### Step 1: Create a Workspace

In the Azure portal, click **All Services** and filter for **Log Analytics**. Select **Log Analytics**.

![Filter Log Analytics](/docs/img/azure_log_analytics_filter.png?raw=true)

Click **Add**, and then fill in the fields as described:

* Provide a name for your new **OMS Workspace**, such as DefaultWorkspace.
* Select a **Subscription** from the drop-down list.
* Select an existing **Resource Group** or create a new one.
* Select the **Location** your VMs are deployed to.
* Select the **Pricing Tier** you use.

![Create a Workspace](/docs/img/azure_log_analytics_create_workspace.png?raw=true)

After providing the required fields, on the **OMS Workspace** pane, click **OK**.

From the list of Log Analytics workspaces, select the workspace you just created.

From the left-hand menu, select **Overview**.

Make a note of the Workspace Id as this will be required when configuring the Opspack.

![Find Workspace Id](/docs/img/azure_log_analytics_workspace_id.png?raw=true)

#### Step 2: Enable the Log Analytics VM Extension

Log Analytics can be enabled on existing Azure Linux and Windows VMs using the Log Analytics agent.

In the Azure portal, click **All Services** and filter for **Log Analytics**. Select **Log Analytics**.

![Find Log Analytics](/docs/img/azure_log_analytics_filter.png?raw=true)

From the list of Log Analytics workspaces, select the workspace you created earlier.

From the left-hand menu, under **Workspace Data Sources**, select **Virtual Machines**.

![Find Virtual Machines](/docs/img/azure_log_analytics_virtual_machines.png?raw=true)

From the list of virtual machines, select the virtual machine you wish to install the agent on. Notice that the **OMS connection status** for the VM indicates that it is **Not connected**.

![Log Analytics Connection Status](/docs/img/azure_log_analytics_not_connected.png?raw=true)

In the details pane for your virtual machine, Select **Connect**. The agent will be installed and configured for your Log Analytics workspace. Please note, this process may take a few minutes to complete.

![Connect Log Analytics](/docs/img/azure_log_analytics_connect.png?raw=true)

After the agent has been installed and connected, the **OMS connection status** will be updated to **This workspace**.

![Log Analytics Connected](/docs/img/azure_log_analytics_connected.png?raw=true)

#### Step 3: Collect performance data

In the Azure portal, click **All Services** and filter for **Log Analytics**. Select **Log Analytics**.

![Find Log Analytics](/docs/img/azure_log_analytics_filter.png?raw=true)

From the list of Log Analytics workspaces, select the workspace you created earlier.

From the left-hand menu, under Settings, select **Advanced Settings**.

![Find Advanced Settings](/docs/img/azure_log_analytics_advanced_settings.png?raw=true)

Select **Data**, and then select **Windows Performance Counters**.


Enable the following performance counters, by typing in their name and then clicking the plus sign +:

* LogicalDisk(*)\% Free Space
* LogicalDisk(*)\Free Megabytes
* Memory(*)\% Committed Bytes In Use
* Memory(*)\Available Mbytes
* Processor(_Total)\% Processor Time
* System(*)\Processor Queue Length
* System(*)\System Up Time

After enabling the required performance counters, click **Save** to save the configuration.

![Windows Performance Counters](/docs/img/azure_log_analytics_windows_counters.png?raw=true)

Then select the **Windows Event Logs** tab.

Enable the following data sources, by typing in their name and then clicking the plus sign +:

* System
* Application

Make sure all checkboxes under **ERROR**, **WARNING** and **INFORMATION** are ticked as shown.

![Windows Data Sources](/docs/img/azure_log_analytics_windows_sources.png?raw=true)

Finally, select the **Storage Account Logs** option under your workspace, and add the storage account related to your Virtual Machine.
![Storage Account Logs](/docs/img/azure_log_analytics_storage_account_logs.png?raw=true)


## Setup and Configuration

To configure and utilize this Opspack, you simply need to add the 'Cloud - Azure - Windows VMs' Opspack to your Opsview Monitor system.

#### Step 1: Import the Opspack

Download the *cloud-azure-windows-vms.opspack* file from the **Releases** section of this repository.
Navigate to **Host Template Settings** inside Opsview Monitor and select **Import Opspack** in the top left corner.

![Add Variables](/docs/img/host-template-settings.png?raw=true)

Then click **Browse** and select the *cloud-azure-windows-vms.opspack* file. Click **Upload** and then click **Import** when the file is uploaded.
You may see a 'CONFLICT' warning message after uploading - this is because all 'Cloud - Azure' Opspacks utilize the same variable (AZURE_CREDENTIALS) for authorizing access to your resources. Just click **Overwrite** and the Opspack should import successfully.

![Add Variables](/docs/img/import-opspack.png?raw=true)

#### Step 2: Add the host template

Add the relevant host template (as listed in the **Service Checks** table above).
If this is a resource that is applicable for a host check (has a valid hostname or IP) then you can fill in the **Primary Hostname/IP** field with this, and then open the **Advanced** section at the bottom and change the **Host Check Command** type to *TCP Port 80 (HTTP)*. If the resource has no hostname or public IP, then change **Host Check Command** to *Always assumed to be UP*.

![Add Host Template](/docs/img/host-template.png?raw=true)

#### Step 3: Add and configure variables required for this host

Add 'AZURE_CREDENTIALS' to the host, then override the Subscription ID, Client ID, Secret Key and Tenant ID to match the values retrieved earlier.

![Add Variables](/docs/img/variable-AZURE_CREDENTIALS.png?raw=true)

Depending on your host template, you will require different additional variables declared as specified below:

| Host Template | Variables Required |
|:------------- |:------------------ |
| Cloud - Azure - Windows VMs | AZURE_RESOURCE_DETAILS, AZURE_LOG_ANALYTICS_DETAILS |

These can be filled out as follows:

**AZURE_RESOURCE_DETAILS**:


![Add Variables](/docs/img/variable-AZURE_RESOURCE_DETAILS.png?raw=true)

**AZURE_LOG_ANALYTICS_DETAILS**:


![Add Variables](/docs/img/variable-AZURE_LOG_ANALYTICS_DETAILS.png?raw=true)


#### Step 4: Reload and the system will now be monitored


![View Output](/docs/img/output-cloud-azure-windows-vms.png?raw=true)
