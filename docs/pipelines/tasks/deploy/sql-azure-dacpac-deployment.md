---
title: Azure SQL Database Deployment task
description: Deploy Azure SQL DB using DACPAC or run scripts using SQLCMD
ms.topic: reference
ms.assetid: CE85A08B-A538-4D2B-8589-1D37A9AB970F
ms.custom: seodec18
ms.author: ronai
author: RoopeshNair
ms.date: 07/05/2019
monikerRange: 'azure-devops'
---

# Azure SQL Database Deployment task

**Azure Pipelines**

Use this task to deploy to Azure SQL DB using a DACPAC or run scripts using SQLCMD.

> [!IMPORTANT]
> This task is supported only in a Windows environment. If you are trying to use Azure Active Directory (Azure AD) integrated authentication, you must create a private agent. Azure AD integrated authentication is not supported for hosted agents.

::: moniker range="> tfs-2018"

## YAML snippet

[!INCLUDE [temp](../includes/yaml/SqlAzureDacpacDeploymentV1.md)]

::: moniker-end

## Arguments

<table><thead><tr><th>Argument</th><th>Description</th></tr></thead>
<tr><td>Azure Connection Type</td><td>(Optional) </td></tr>
<tr><td>Azure Classic Subscription</td><td>(Required) Target Azure Classic subscription for deploying SQL files</td></tr>
<tr><td>Azure Subscription</td><td>(Required) Target Azure Resource Manager subscription for deploying SQL files</td></tr>
<tr><td>Authentication Type</td><td>(Required) Type of database authentication, can be <b>SQL Server Authentication</b>, <b>Active Directory - Integrated</b>, <b>Active Directory - Password</b>, or <b>Connection String</b>. Integrated authentication means that the agent will access the database using its current Active Directory account context.</td></tr>
<tr><td>Azure SQL Server</td><td>(Required except when Authentication Type is <b>Connection String</b>) Azure SQL Server name, like Fabrikam.database.windows.net,1433 or Fabrikam.database.windows.net.</td></tr>
<tr><td>Database</td><td>(Required) Name of the Azure SQL Database, where the files will be deployed.</td></tr>
<tr><td>Login</td><td>(Required when Authentication Type is <b>SQL Server Authentication</b> or <b>Active Directory - Password</b>) Specify the Azure SQL Server administrator login or Active Directory user name.</td></tr>
<tr><td>Password</td><td>(Required when Authentication Type is <b>SQL Server Authentication</b> or <b>Active Directory - Password</b>) Password for the Azure SQL Server administrator or Active Directory user.<br>It can accept variables defined in build/release pipelines as &#39;$(passwordVariable)&#39;.<br>You may mark the variable type as &#39;secret&#39; to secure it.</td></tr>
<tr><td>Connection String</td><td>(Required when Authentication Type is <b>Connection String</b>) The connection string, including authentication information, for the Azure SQL Server.</td></tr>
<tr><td>Deploy Type</td><td>(Optional) Specify the type of artifact, <b>SQL DACPAC file</b>, <b>SQL Script file</b>, or <b>Inline SQL Script</b>.</td></tr>
<tr><td>DACPAC File</td><td>(Required when Deploy Type is <b>SQL DACPAC file</b>) Location of the DACPAC file on the automation agent or on a UNC path accessible to the automation agent like, \BudgetIT\Web\Deploy\FabrikamDB.dacpac. Predefined system variables like, $(agent.releaseDirectory) can also be used here.</td></tr>
<tr><td>SQL Script</td><td>(Required when Deploy Type is <b>SQL Script file</b>) Location of the SQL script file on the automation agent or on a UNC path accessible to the automation agent like, \BudgetIT\Web\Deploy\FabrikamDB.sql. Predefined system variables like, $(agent.releaseDirectory) can also be used here.</td></tr>
<tr><td>Inline SQL Script</td><td>(Required when Deploy Type is <b>Inline SQL Script</b>) Enter the SQL script to execute on the Database selected above.</td></tr>
<tr><td>Publish Profile</td><td>(Optional) Publish profile provides fine-grained control over Azure SQL Database creation or upgrades. Specify the path to the publish profile XML file on the agent machine or a UNC share. If the publish profile contains secrets like credentials, upload it to the <a href="../../library/secure-files.md" data-raw-source="[secure files](../../library/secure-files.md)">secure files</a> library where it is securely stored with encryption. Then use the <a href="../utility/download-secure-file.md" data-raw-source="[Download secure file](../utility/download-secure-file.md)">Download secure file</a> task at the start of your pipeline to download it to the agent machine when the pipeline runs and delete it when the pipeline is complete. Predefined system variables like $(agent.buildDirectory) or $(agent.releaseDirectory) can also be used in this field.</td></tr>
<tr><td>Additional SqlPackage.exe Arguments</td><td>(Optional) Additional SqlPackage.exe arguments that will be applied when deploying the Azure SQL Database, in case DACPAC option is selected like, /p:IgnoreAnsiNulls=True /p:IgnoreComments=True. These arguments will override the settings in the publish profile XML file (if provided).</td></tr>
<tr><td>Additional Invoke-Sqlcmd Arguments</td><td>(Optional) Additional Invoke-Sqlcmd arguments that will be applied when executing the given SQL query on the Azure SQL Database like, -ConnectionTimeout 100 -OutputSqlErrors</td></tr>
<tr><td>Specify Firewall Rules Using</td><td>(Required) For the task to run, the IP Address of the automation agent has to be added to the &#39;Allowed IP Addresses&#39; in the Azure SQL Server&#39;s Firewall. Select auto-detect to automatically add firewall exception for range of possible IP Address of automation agent or specify the range explicitly.</td></tr>
<tr><td>Start IP Address</td><td>(Required) The starting IP Address of the automation agent machine pool like 196.21.30.50.</td></tr>
<tr><td>End IP Address</td><td>(Required) The ending IP Address of the automation agent machine pool like 196.21.30.65.</td></tr>
<tr><td>Delete Rule After Task Ends</td><td>(Optional) If selected, then after the task ends, the IP Addresses specified here are deleted from the &#39;Allowed IP Addresses&#39; list of the Azure SQL Server&#39;s Firewall.</td></tr>


<tr>
<th style="text-align: center" colspan="2"><a href="~/pipelines/process/tasks.md#controloptions" data-raw-source="[Control options](../../process/tasks.md#controloptions)">Control options</a></th>
</tr>

</table>

## Open source

This task is open source [on GitHub](https://github.com/Microsoft/azure-pipelines-tasks). Feedback and contributions are welcome.
