---
title: Deprecate an operation | Microsoft Docs
description: Learn how to deprecate an operation.
author: lnborbolla42
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: laborbol
ms.reviewer: angieandrews
---

# Deprecate an operation

> [!IMPORTANT]
> Remember to follow the standards of deprecation by giving appropriate notice to users at least 30 days prior. If you're deprecating the operation of a certified connector, reach out to your Microsoft contact.

## How to mark an operation as deprecated

Deprecate any action or trigger in a custom connector by making the following modifications to the `apiDefinition.swagger.json` file:

1. Set the `deprecated` attribute for the operation to `true`.

1. Append `(deprecated)` to the `summary` and `description` attributes.

```json
{
  "/{list}/items": {
    "get": {
      "summary": "Get rows (deprecated)",
      "description": "This operation gets a list of items (deprecated).",
      "operationId": "GetItems",
      "deprecated": true
    }
  }
}
```

If a new version of the operation is going to be introduced to replace the deprecated one (for example, in a version 2), use `x-ms-api-annotation.family` and `x-ms-api-annotation.revision`, as shown in the following:

```json
{
  "/{list}/items": {
    "get": {
      "summary": "Get rows (deprecated)",
      "description": "This operation gets a list of items (deprecated).",
      "operationId": "GetItems",
      "deprecated": true,
      "x-ms-api-annotation": {
        "family": "GetItems",
        "revision": 1
      }
    }
  },
  "/v2/{list}/items": {
    "get": {
      "summary": "Get rows",
      "description": "This operation gets a list of items.",
      "operationId": "GetItems_V2",
      "deprecated": false,
      "x-ms-api-annotation": {
        "family": "GetItems",
        "revision": 2
      }
    }
  }
}
```

> [!Note]
> Note how both operations belong to the same `family`, and the `revision` number is increased.

## Deprecated operation behavior

After an operation is deprecated, expect the following Microsoft Power Platform behavior:

- Flows and apps that are already using this operation will continue to work as before (they *won't* stop working).
- The designer hides deprecated operations (new flows can't use them).

[!INCLUDE[feedback](../includes/feedback.md)]
