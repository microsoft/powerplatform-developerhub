---
title: Evolve your API with version control | Microsoft Docs
description: Evolve your connector's API operations safely with versioning
author: camydee
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: camerost
ms.reviewer: angieandrews
---

# Evolve your API with version control

Custom connectors for Azure Logic Apps, Microsoft Power Automate, or Microsoft Power Apps must provide an OpenAPI specification file. This OpenAPI spec defines individual entry points known as operations. Each operation has a unique `operationId` and a unique `urlPath` and `HttpVerb` combination.

```json
{
    "/items": {
        "get": {
            "summary": "Get rows",
            "description": "This operation gets a list of items.",
            "operationId": "GetItems"
        },
        "post": {
            "summary": "Insert row",
            "description": "This operation inserts an item.",
            "operationId": "PostItem"
        }
    }
}
```

These operations can grow and change over time as features are added or expanded. Some changes are merely additive and don't necessarily break the contract that exists between clients and servers. Adding new parameters, returning more data, or allowing more flexible inputs might fall into this category.

However, many changes may actually break the contract described in the OpenAPI specification. Removing parameters, no longer supporting certain inputs, or changing the meaning and behavior of an input, output, or the operation itself fall into the "breaking change" category.

In order to evolve an API safely, it's important to follow a pattern that can be navigated by clients. It's the API's responsibility to maintain backward compatibility, communicate intention, and delineate versioning attributes. It's the client's responsibility to either show or hide operations that are deprecated, expired, or that may have newer versions available. In this way, operations can grow and develop over time without causing undue fragility on applications that rely on them.

## API Annotation

OpenAPI doesn't have intrinsic support for operational versioning. To accomplish our objective, much of the work is done through the `x-ms-api-annotation` object, which is applied at both the global scope and the operation scope. The global object contains properties that apply to the API as a whole:

```json
{
    "x-ms-api-annotation": {
        "status": "Preview"
    }
}
```

| Property   | Values | Default | Description | 
| ---------- | ------ | ------- | ----------- | 
| **status** | `"Preview"` `"Production"` | `"Preview"` | Status of the API as a whole&mdash;starting in Preview and escalating to Production as usage and stability dictate |

At the operational scope, this object contains more detailed properties. There are also additional properties outside the object that apply and participate in the versioning evolutionary process:

```json
{
    "deprecated": true,
    "x-ms-api-annotation": {
        "status": "Production",
        "family": "MyOperation",
        "revision": 2
    }
}
```

| Property                           | Values | Default | Description | 
| ---------------------------------- | ----- | ------- | ----------- | 
| [**deprecated**](#deprecated)      | `null` `false` `true` | `false` | Indicates whether the operation is deprecated |
| [**x-ms-visibility**](#visibility) | `null` `""` `"Important"` `"Advanced"` `"Internal"` | `""` | Intended visibility and prominence of this operation, where `null` or `""` implies a *Normal* state |
| [**status**](#status)             | `"Preview"` `"Production"` | `"Production"` | Status of the operation&mdash;this can differ from the state of the API itself, but if not specified will inherit from the top-level API status |
| [**family**](#family)             | {common operation name} | operationName | Name that applies to every revision of this operation |
| [**revision**](#revision)         | numeric (1,2,3...) | `1` | Revision of the specified operational family |
| [**expires**](#expires)           | ISO8601 date | (none) | Optional hint to client to indicate projected end of support |

<a id="deprecated">**Deprecated**</a> can be set to `true` when it's no longer desirable for clients to use this operation. This property exists in the [OpenAPI Fixed Fields](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md#fixed-fields-8) specification.

<a id="visibility">**Visibility**</a> is an indicator of the intended relative prominence of the operation. An `"Important"` visibility indicates that the operation should be toward the top of the list, displayed prominently. A _normal_ visibility (indicated by `null` or empty string `""`) is the default, and means that the operation will appear in the list, likely after the Important operations. An `"Advanced"` visibility indicates that the operation might be toward the bottom of the list or even hidden initially behind an expando control. Advanced operations might be harder to use, less popular, or more narrowly applicable. An `"Internal"` visibility indicates that the operation shouldn't be exposed to users, and should only be used internally. Internal operations are programmatically useful and valuable, but aren't intended for end-users. Internal operations might also be marked as such in order to hide them from any sort of UI during the process of deprecation without actually removing them from the API, which would otherwise cause a breaking change.

<a id="status">**Status**</a> indicates the stability of the API or operation. `"Preview"` indicates that the operation or API is new and potentially unproven. Preview is an indicator that production systems should be circumspect about assuming a dependency. Once the operation or API has become more established, and has proven that it meets standards of reliability, success rate, and scalability, it can be intentionally upgraded to `"Production"` status.

The following metric requirements generally apply to operations seeking `"Production"` status:
* 80% success rate for a period of three weeks
  * defined as percent of HTTP response codes in the 2xx range
* 99.9% reliability sustained for a period of three weeks
  * defined as percent of HTTP response codes in the non-5xx range 
  (502, 504, and 520 are excluded from this calculation)

<a id="family">**Family**</a> indicates the relationship between operations that are conceptually the same operation, but are different revisions with potentially breaking changes between them. Multiple operations will share the same family name if they should be considered revisions of one another and are sequenced by their unique revision numbers.

<a id="revision">**Revision**</a> indicates the evolutionary order of the operation within the family of operations. Each operation within a family will have a revision that is an integral index implying sequence. An empty revision will be considered as revision `1`. When newer revisions of an operation are available, clients should display them more prominently and recommend them more intentionally but still allow selection of potentially older revisions that aren't yet deprecated.

<a id="expires">**Expires**</a> is optional and indicates a potential end-of-life deadline where support for the operation is no longer guaranteed. This should only be set for deprecated operations, and is currently not reflected in any interface.

## Operational Lifetime

Operations have a predictable lifetime that can be shown by example.

### Starting Point

Initially, operations may not necessarily indicate anything about revisions. These operations have defaults applied, and are therefore considered as revision 1 in a family name equivalent to the `operationId`.

```json
{
    "/{list}/items": {
        "get": {
            "summary": "Get rows",
            "description": "This operation gets a list of items.",
            "operationId": "GetItems"
        }
    }
}
```

This is equivalent to the more explicit definition:

```json
{
    "/{list}/items": {
        "get": {
            "summary": "Get rows",
            "description": "This operation gets a list of items.",
            "operationId": "GetItems",
            "deprecated": false,
            "x-ms-api-annotation": {
                "status": "Production",
                "family": "GetItems",
                "revision": 1
            }
        }
    }
}
```

### Operation Initiation

Most evolutions of an API constitute the addition of an operation. New methods and new revisions of existing methods, for instance. To safely initiate a new revision, you adjust the OpenAPI spec in this way:

```json
{
    "/{list}/items": {
        "get": {
            "summary": "Get rows (V1 - downplayed)",
            "description": "This operation gets a list of items.",
            "operationId": "GetItems",
            "deprecated": false,
            "x-ms-visibility": "advanced",
            "x-ms-api-annotation": {
                "status": "Production",
                "family": "GetItems",
                "revision": 1
            }
        }
    }
    "/v2/{list}/items": {
        "get": {
            "summary": "Get rows (V2 - new hotness)",
            "description": "This operation gets a list of items.",
            "operationId": "GetItems_V2",
            "deprecated": false,
            "x-ms-api-annotation": {
                "status": "Preview",
                "family": "GetItems",
                "revision": 2
            }
        }
    }
}
```

> [!IMPORTANT]
> Notice that GetItems V2 has a unique `operationId`, and is initially listed in preview status.
> Also notice that GetItems v1 now has advanced visibility, so it will not be displayed as prominently.

### Operation Deprecation

Sometimes existing V1 entry points remain indefinitely if they continue to provide value and there's no compelling reason to sunset them. However, many V2 entry points intentionally supersede the V1 entry point. In order to safely do so, all traffic should reach nominal zero to the original operation. Once telemetry confirms this circumstance, the following change can be made:

```json
{
    "/{list}/items": {
        "get": {
            "summary": "Get rows (deprecated)",
            "description": "This operation gets a list of items.",
            "operationId": "GetItems",
            "deprecated": true,
            "x-ms-api-annotation": {
                "status": "Production",
                "family": "GetItems",
                "revision": 1
            }
        }
    }
    "/v2/{list}/items": {
        "get": {
            "summary": "Get rows",
            "description": "This operation gets a list of items.",
            "operationId": "GetItems_V2",
            "deprecated": false,
            "x-ms-api-annotation": {
                "status": "Production",
                "family": "GetItems",
                "revision": 2
            }
        }
    }
}
```

> [!IMPORTANT]
> Notice that GetItems V1 is now marked as deprecated. This is the final transition for deprecating operations. GetItems V2 has now completely replaced GetItems V1.

## Why bother?

There are many reasons to adhere to operational versioning. Primarily, doing so will ensure that clients such as Azure Logic Apps and Power Automate continue to work correctly when users integrate connector operations into their data flows. Operations should be versioned using the above method whenever:

* A new revision of an operation is added
* An existing operation adds or removes parameters
* An existing operation changes input or output significantly

## Strictly Speaking

There might be cases where you can get away without versioning&mdash;but you should be careful when doing so, and do plenty of testing to ensure you haven't overlooked edge cases where users might be broken unexpectedly. Here's the cautious short list of when you can get away without it:

* A new operation is added.

   This wouldn't specifically break existing clients.

* A new optional parameter is added to an existing operation.

   This wouldn't break existing calls, but must be carefully considered.

* An existing operation changes behavior subtly.

   This might not break existing callers based on the nature of the change and what users rely on. This is the most precarious one of all, since a significant difference in input acceptance, output generation, timing, or processing could impact the behavior of the operation in ways that may make it hard to determine the risk of the change.

It is always recommended to err on the side of caution and iterate a revision when you make any non-trivial API change.

[!INCLUDE[feedback](../includes/feedback.md)]
