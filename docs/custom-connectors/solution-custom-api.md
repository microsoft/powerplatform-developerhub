---
title: Manage solution custom connectors with Dataverse APIs | MicrosoftDocs
description: Learn how to manage solution custom connectors directly through Dataverse APIs. 
Keywords: connectors, custom connectors, solution custom connectors, Dataverse
author: samrat-gavale
ms.author: sagavale
ms.date: 10/03/2022
ms.service: connectors
ms.topic: article
ms.reviewer: angieandrews
---

# Manage solution custom connectors with Dataverse APIs

In addition to the Microsoft Power Automate and Power Apps portal, solution custom connectors can be managed directly through Microsoft Dataverse APIs.

## Compose HTTP requests

Solution connectors are stored in Dataverse. To learn how to compose HTTP requests for Dataverse APIs, including how to compose the URL and authorization token, go to [Power Automate Web API](/power-automate/web-api).

## Get a list of custom connectors

You can get the list of custom connectors by calling `GET` on connectors. Each connector has many properties. To learn about Dataverse properties, fields, and their usage, go to [Connector table/entity reference](/powerapps/developer/data-platform/reference/entities/connector).

You can also request specific properties, filter the list of connectors, and more. To learn about querying data with Dataverse, go to [Query data using the Web API](/powerapps/developer/data-platform/webapi/query-data-web-api).

For example, this query returns only the connector whose display name matches the one given in the filter:

```http
GET https://org00000000.crm0.dynamics.com/api/data/v9.1/connectors?$filter= displayname eq 'MyConnector1'

Accept: application/json
Authorization: Bearer ey…
```

## Get a specific connector

To retrieve a specific connector, add its Dataverse ID in the URL. The Dataverse ID is a GUID. It's different from the main ID that you see in the URL when you go to the connector's page. If you use the previous API to get a list of many connectors, their Dataverse IDs will be in the `connectorid` field.

The following is a sample request:

```http
GET https://org00000000.crm0.dynamics.com/api/data/v9.1/connectors(00000000-0000-0000-0000-000000000002)

Accept: application/json
Authorization: Bearer ey…
```

## Create a connector

Call `POST` on the connectors collection to create a connector. The required properties for a connector are: connectorid, name, displayname, openapidefinition, and connectortype. The connector type should be set to **1**. You can also provide a description.

The following is a sample request:

```http
POST https://org00000000.crm0.dynamics.com/api/data/v9.1/connectors

Accept: application/json
Authorization: Bearer ey…
Content-type: application/json

{
    "connectorid": "00000000-0000-0000-0000-000000000003",
    "name": "cr736_5Fdemosolnconnector",
    "displayname": "DemoSolnConnector",
    "openapidefinition": "{\"swagger\":\"2.0\",\"info\":{\"title\":\"demosolutionconnector\",\"description\":\"Sample description.\",\"version\":\"1.0\"},\"host\":\"api.demobackend.com\",\"basePath\":\"/\",\"schemes\":[\"https\"],\"consumes\":[],\"produces\":[],\"paths\":{\"/get\":{\"get\":{\"responses\":{\"default\":{\"description\":\"default\",\"schema\":{}}},\"summary\":\"Get\",\"description\":\"Get\",\"operationId\":\"Get\",\"parameters\":[{\"name\":\"foo1\",\"in\":\"query\",\"required\":false,\"type\":\"string\"},{\"name\":\"foo2\",\"in\":\"query\",\"required\":false,\"type\":\"string\"}]}}},\"definitions\":{},\"parameters\":{},\"responses\":{},\"securityDefinitions\":{},\"security\":[],\"tags\":[]}",
    "connectortype": 1
}
```

> [!NOTE]
> - Setting the custom connector icon isn't supported through the Web API. After a connector has been created, you can update it through the user interface to set the icon.
>
> - Microsoft doesn't store secrets in Dataverse for custom connectors. If any of the properties contain secrets, they'll be replaced with an empty string. You'll need to set the secret through the user interface. Doing this will save secrets in a separate location and populate them into the connector during management operations.

## Update a connector

You can call `PATCH` on the connector to update it. This call requires that you specify the connectorid in the URL.

For example, you can update the description and the owner of the connector with the following call:

```http
PATCH https://org00000000.crm0.dynamics.com/api/data/v9.1/connectors(00000000-0000-0000-0000-000000000002)

Accept: application/json
Authorization: Bearer ey...
Content-type: application/json
{
	"description" : "This is updated description of the custom connector.",
	"ownerid@odata.bind": "systemusers(00000000-0000-0000-0000-000000000005)",
}
```

> [!Note]
> The syntax for changing the owner uses the `odata.bind` format. This means instead of patching the _ownerid_value field directly, you append `@odata.bind` to the property name, and then wrap the ID with `systemusers()`.

## Delete a connector

Delete a connector with a `DELETE` call:

```http
DELETE https://org00000000.crm0.dynamics.com/api/data/v9.1/connectors(00000000-0000-0000-0000-000000000002)

Accept: application/json
Authorization: Bearer ey...
```

### See also

[Create custom connectors in solutions](CustomConnectorsSolutions.md)<br/>
[Connector table/entity reference](/powerapps/developer/data-platform/reference/entities/connector)
