---
title: Tutorial - Start Azure Databricks clusters during business hours
description: This tutorial teaches you to create, test, and publish a PowerShell Workflow runbook to start Azure Databricks clusters during business hours.
services: databricks
ms.subservice: process-automation
ms.date: 09/23/2021
ms.topic: tutorial 
#Customer intent: As a developer, I want use workflow runbooks so that I can automate the starting of VMs.
---

# Tutorial: Start Azure Databricks clusters during business hours

This tutorial walks you through the creation of a PowerShell Workflow runbook to start Azure Databricks clusters during business hours in Azure Automation.

In this tutorial, you learn how to:

> * Test PowerShell module working on the local machine. 
> * Create Azure Automation account.
> * Create a PowerShell Workflow runbook
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

![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Windows-PowerShell-RunAsAdmin.png)

**Step2:** Copy and Paste the following command to install this package using PowerShellGet.

```powershell
Install-Module -Name DatabricksPS
```
![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Install-Module.png)

**Step3:** Get details of the Azure Databricks clusters which you want to start.
To get Azure Databricks clusters details, you need to have Azure Databricks Endpoint URL and Personal Access Token.

```powershell
$accessToken = "<Personal_Access_Token>"
$apiUrl = "<Azure_Databricks_Endpoint_URL>"
Set-DatabricksEnvironment -AccessToken $accessToken -ApiRootUrl $apiUrl
Get-DatabricksCluster
```
![image](https://github.com/CHEEKATLAPRADEEP-MSFT/chepraacademy/blob/main/articles/databricks/Media/Get-ClusterDetails.png)

**Step4:** Get the cluster ID which is displayed in the step3 and create a PowerShell script as shown below.

```powershell
$accessToken = "<Personal_Access_Token>"
$apiUrl = "<Azure_Databricks_Endpoint_URL>"
Set-DatabricksEnvironment -AccessToken $accessToken -ApiRootUrl $apiUrl
Start-DatabricksCluster -ClusterID "<Cluster_ID>"
```





