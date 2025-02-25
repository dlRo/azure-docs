---
title: Parameterize linked services
titleSuffix: Azure Data Factory & Azure Synapse
description: Learn how to parameterize linked services in Azure Data Factory and Azure Synapse Analytics pipelines, and pass dynamic values at run time.
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 09/09/2021
author: chez-charlie
ms.author: chez
---

# Parameterize linked services in Azure Data Factory and Azure Synapse Analytics

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

You can now parameterize a linked service and pass dynamic values at run time. For example, if you want to connect to different databases on the same logical SQL server, you can now parameterize the database name in the linked service definition. This prevents you from having to create a linked service for each database on the logical SQL server. You can parameterize other properties in the linked service definition as well - for example, *User name.*

You can use the UI in the Azure portal or a programming interface to parameterize linked services.

> [!TIP]
> We recommend not to parameterize passwords or secrets. Store all secrets in Azure Key Vault instead, and parameterize the *Secret Name*.

> [!Note]
> There is open bug to use "-" in parameter names, we recommend to use names without "-" until the bug is resolved.

For a seven-minute introduction and demonstration of this feature, watch the following video:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Parameterize-connections-to-your-data-stores-in-Azure-Data-Factory/player]

## Supported linked service types

All the linked service types are supported for parameterization.

**Natively supported in UI:** When authoring linked service on UI, the service provides built-in parameterization experience for the following types of linked services. In linked service creation/edit blade, you can find options to new parameters and add dynamic content. Refer to [UI experience](#ui-experience).

- Amazon Redshift
- Amazon S3
- Amazon S3 Compatible Storage
- Azure Blob Storage
- Azure Cosmos DB (SQL API)
- Azure Data Lake Storage Gen2
- Azure Database for MySQL
- Azure Databricks
- Azure Key Vault
- Azure SQL Database
- Azure SQL Managed Instance
- Azure Synapse Analytics 
- Azure Table Storage
- Generic HTTP
- Generic REST
- MySQL
- Oracle
- Oracle Cloud Storage
- SQL Server

**Advanced authoring:** For other linked service types that are not in above list, you can parameterize the linked service by editing the JSON on UI:

- In linked service creation/edit blade -> expand "Advanced" at the bottom -> check "Specify dynamic contents in JSON format" checkbox -> specify the linked service JSON payload. 
- Or, after you create a linked service without parameterization, in [Management hub](author-visually.md#management-hub) -> Linked services -> find the specific linked service -> click "Code" (button "{}") to edit the JSON. 

Refer to the [JSON sample](#json) to add ` parameters` section to define parameters and reference the parameter using ` @{linkedService().paramName} `.

## UI Experience

# [Azure Data Factory](#tab/data-factory)

![Add dynamic content to the Linked Service definition](media/parameterize-linked-services/parameterize-linked-services-image-1.png)

![Create a new parameter](media/parameterize-linked-services/parameterize-linked-services-image-2.png)

# [Azure Synapse](#tab/synapse-analytics)

![Add dynamic content to the Linked Service definition](media/parameterize-linked-services/parameterize-linked-services-image-1-synapse.png)

![Create a new parameter](media/parameterize-linked-services/parameterize-linked-services-image-2-synapse.png)

---

## JSON

```json
{
	"name": "AzureSqlDatabase",
	"properties": {
		"type": "AzureSqlDatabase",
		"typeProperties": {
			"connectionString": "Server=tcp:myserver.database.windows.net,1433;Database=@{linkedService().DBName};User ID=user;Password=fake;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
		},
		"connectVia": null,
		"parameters": {
			"DBName": {
				"type": "String"
			}
		}
	}
}
```
