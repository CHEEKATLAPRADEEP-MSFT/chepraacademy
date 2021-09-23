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



