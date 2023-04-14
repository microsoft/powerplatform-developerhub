---
title: Implement test connection | Microsoft Docs
description: Enable a connector to dynamically validate connection parameters.
author: camydee
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: camerost
ms.reviewer: angieandrews
---

# Implement Test Connection

Test Connection is a simple entry point that can be defined for a connector for use with Azure Logic Apps, Power Automate, or Power Apps. By exposing an operation for test connection, the connector can provide design-time and runtime validation of connection parameters.

## Prerequisites

* Basic experience building [logic apps](/azure/logic-apps/logic-apps-create-a-logic-app) or [flows](/flow/get-started-logic-flow), and [custom connectors](define-openapi-definition.md).
* Basic understanding of the [OpenAPI Specification](http://swagger.io/specification/) (formerly known as Swagger).
* A [GitHub](https://www.github.com) account.
* The [sample OpenAPI definition](https://procsi.blob.core.windows.net/docs/githubWebhookSample.json) for this tutorial.


## Add a new test connection operation

Adding an operation for TestConnection is a very simple process. You have the option of using any existing operation as a means of testing the connection, or adding a specific operation whose job is only to test the connection parameters. The operation must be a "get" and support a call with no parameters, or with hard-coded parameters.

Adding a new operation for this purpose might look like this in the OpenAPI specification:

```json
    "/diagnostics/testconnection": {
      "get": {
        "tags": [ "Diagnostics" ],
        "operationId": "TestMyAPIConnection",
        "consumes": [],
        "produces": [],
        "responses": {
          "200": { "description": "OK" },
          "default": { "description": "Operation Failed." }
        },
        "x-ms-visibility": "internal"
      }
    }
```

> [!IMPORTANT]
> Notice that this operation is marked as `internal`. If you add a new entry point for this purpose, it's highly encouraged to hide this operation from the user by marking the visibility as such. 

The endpoint meant to be used as test connection must be identified by adding an extension to the API at the top-level, like so:

```json
 "x-ms-capabilities": {
    "testConnection": {
      "operationId": "TestMyAPIConnection",
      "parameters": {}
    }
  }
```

The `operationId` specified in this attribute must exist within this same OpenAPI specification to be valid. 

## Reuse an existing operation for test connection

Often it's simpler and more manageable to identify an existing operation that can validate the viability of the connection without incurring much cost or latency. This can be accomplished without adding a new operation, but by simply indicating which operation to use, and which parameters to pass (if any).

The following example uses an existing "get" operation called `GetTables`, which should succeed if the connection is valid and the parameters are correct. To ensure that test connection operation executes as quickly as possible, the example also adds a parameter to the call to specify that only the first row should be returned.

```json
 "x-ms-capabilities": {
    "testConnection": {
      "operationId": "GetTables",
      "parameters": {
        "$top": 1
      }
    }
  }
```

## Implement test connection

If you need to implement test connection, and no other existing operation is appropriate for this purpose, you can do so with a simple backend call. The operation does not need to take any parameters or return any content. The URL path is also unimportant, and can be selected based on your preference. The only measure of success for a test connection call is a successful response (200, for example) from the HTTP call. Within the test connection operation, the contract requests that the connector validates the authentication context and the connection parameters.

This can be accomplished by querying for something simple on the backend, which will make use of the authentication parameters and any database or scoping that might be implied. Querying for the top row of a simple table is a good example of one test connection approach.

[!INCLUDE[feedback](../includes/feedback.md)]