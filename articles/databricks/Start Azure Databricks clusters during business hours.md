---
title: Tutorial - Start Azure Databricks clusters during business hours
description: This tutorial teaches you to create, test, and publish a PowerShell runbook to start Azure Databricks clusters during business hours.
services: databricks
ms.subservice: process-automation
ms.date: 09/23/2021
ms.topic: tutorial 
#Customer intent: As a developer, I want use PowerShell runbooks so that I can automate the starting of VMs.
---

# Tutorial: Start Azure Databricks clusters during business hours

This tutorial walks you through the creation of a PowerShell runbook to start Azure Databricks clusters during business hours in Azure Automation.

In this tutorial, you learn how to:

> * Test PowerShell module working on the local machine. 
> * Create Azure Automation account.
> * Import modules to Azure Automation account
> * Create a PowerShell runbook
> * Test and publish the runbook
> * Run and track the status of the runbook job

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

## Prerequisites
* Make sure to have a Azure Databricks cluster in your workspace.
* Azure Databricks Endpoint URL
* AzureDatabricks Personal Token Access

## Test PowerShell module working on the local machine. 

You need to get the PowerShell module to test the PowerShell script working on the local machine.

**Step1:** Open Windows PowerShell as Run as Administrator.

  ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Windows-PowerShell-RunAsAdmin.png)

**Step2:** Copy and Paste the following command to install this package using PowerShellGet.

```powershell
Install-Module -Name DatabricksPS
```
  ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Install-Module.png)

**Step3:** Get details of the Azure Databricks clusters which you want to start.
To get Azure Databricks clusters details, you need to have Azure Databricks Endpoint URL and Personal Access Token.

```powershell
$accessToken = "<Personal_Access_Token>"
$apiUrl = "<Azure_Databricks_Endpoint_URL>"
Set-DatabricksEnvironment -AccessToken $accessToken -ApiRootUrl $apiUrl
Get-DatabricksCluster
```
  ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Get-ClusterDetails.png)

**Step4:** Get the cluster ID which is displayed in the step3 and create a PowerShell script as shown below.

```powershell
$accessToken = "<Personal_Access_Token>"
$apiUrl = "<Azure_Databricks_Endpoint_URL>"
Set-DatabricksEnvironment -AccessToken $accessToken -ApiRootUrl $apiUrl
Start-DatabricksCluster -ClusterID "<Cluster_ID>"
```
## Create Azure Automation account.
1. Sign in to the [Azure portal](https://portal.azure.com).

1. From the top menu, select **+ Create a resource**.

1. Under Categories**, select **IT & Management Tools**, and then select **Automation**.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Create-Automation-Account.png)
1. From the **Add Automation Account** page, provide the following information:

    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Create-Automation-Account-Details.png)
1. Select **Create** to start the Automation account deployment. The creation completes in about a minute.

## Import modules to Azure Automation account

Before create a new runbook, as you rememeber we had installed `DatabricksPS` module on the local machine. So, lets add a module in Azure Automation account.

1. From your open Automation account page, under **Shared Resources**, select **Modules** and click on **Browse gallery**
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Automation-Modules.png)

1. Search **DatabricksPS**.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Automation-Modules-Browse.png)
1. Select **DatabricksPS** and click on **Import** and click ok.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Automation-Modules-Browse-Import.png)
1. Wait untill `DatabricksPS` module is imported succesfully where status is available. 
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Automation-Modules-Available.png)

## Create new runbook
One advantage of Windows PowerShell is the ability to perform a set of commands in parallel instead of sequentially as with a typical script

1. From your open Automation account page, under **Process Automation**, select **Runbooks** and Select **+ Create a runbook**.
    1. Name the runbook `ADBRunbook-BusinessHours`.
    1. From the **Runbook type** drop-down menu, select **PowerShell**.
    1. Select **Create**.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Create-Runbook.png)

## Add code to the runbook

You can either type code directly into the runbook, or you can select cmdlets which we created on the local machine, runbooks, and assets from the Library control and add them to the runbook with any related parameters. For this tutorial, you type code directly into the runbook.

```powershell
$accessToken = "<Personal_Access_Token>"
$apiUrl = "<Azure_Databricks_Endpoint_URL>"
Set-DatabricksEnvironment -AccessToken $accessToken -ApiRootUrl $apiUrl
Start-DatabricksCluster -ClusterID "<Cluster_ID>"
```
1. From your open Automation account page, under **Process Automation**, select **Runbooks** and Select **ADBRunbook-BusinessHours** runbook and click on **Edit** and paste the above code and save the runbook by selecting **Save**.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Code-to-Runbook.png)
    
## Test the runbook

Before you publish the runbook to make it available in production, you should test it to make sure that it works properly. Testing a runbook runs its Draft version and allows you to view its output interactively.

1. Select **Test pane** to open the **Test** page, delect **Start** to start the test.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Test-Runbook.png)

> [!Note]
>  Here you can check whether the Azure Databricks cluster is starting.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/ADB-Starting.png)    
 
## Publish and start the runbook

The runbook that you've created is still in Draft mode. You must publish it before you can run it in production. When you publish a runbook, you overwrite the existing Published version with the Draft version. In this case, you don't have a Published version yet because you just created the runbook.

1. Select **Publish** to publish the runbook and then **Yes** when prompted.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Publish-Runbook.png)
    
## Create a new schedule in the Azure portal
Using schedule option you can create a start time of the Azure Databricks clusters based on the your bussiness hours. Here in the below example I had created a recurring schedule to start everyday at 8 AM.
1. From your Automation account, on the left-hand pane select **Schedules** under **Shared Resources**.
2. On the **Schedules** page, select **Add a schedule**.
3. On the **New schedule** page, enter a name and optionally enter a description for the new schedule.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Schedule.png)
    
Example for business hours on weekdays:
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Schedule-Weekdays.png)
    
## Link a schedule to a runbook with the Azure portal

1. In the Azure portal, from your automation account, select **Runbooks** under **Process Automation** and select the `ADBRunbook-BusinessHours`
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Link-Schedule-select.png)
1. Click on **Link to Schedule**
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Link-Schedule-selectOne.png)
1. select previous created schedule named `ADB-BusinessHours`.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Link-Schedule-selecttwo.png)
    
> [!Note]
> Make sure all the changes are published.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Runbook-Published.png)
 
## Manual test: 
As per the manual start we are successfully started Azure Databricks clusters during business hours.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Job-completed.png)

## As per the business hours schedule:
As we have schedule to run everyday at 8 AM, it has succusfully ran the job at 8 AM.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Job-8AM.png)
You can also check out the next run time to confirm if it works based on the recurring schedule.
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Job-NextRun.png)

Note: If your Business Hours (8AM to 6PM which means 10 Hours x 60 minutes) you can set the property Terminate after `600` minutes of inactivity as shown below:
    ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/ADB-Terminate.png)

    
 ## Any Questions?
 If you have any questions while following the article, please do create a new issue on this repro.
      ![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Start%20Azure%20Databricks%20clusters%20during%20business%20hours/Open-Issues.png)
 
