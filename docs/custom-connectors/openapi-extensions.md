---
title: Extend an OpenAPI definition for a custom connector | Microsoft Docs
description: Extend your custom connector's OpenAPI definition with advanced functionality
author: sunaysv
manager: maheshp
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: sunayv
ms.reviewer: angieandrews
---

# Extend an OpenAPI definition for a custom connector

One way to create custom connectors for Azure Logic Apps, Microsoft Power Automate, or Microsoft Power Apps is to provide an OpenAPI definition file, which is a language-agnostic, machine-readable document that describes your API's operations and parameters. Along with OpenAPI's out-of-the-box functionality, you can also include the following OpenAPI extensions when you create custom connectors for Logic Apps and Power Automate:

* [`summary`](#summary)
* [`x-ms-summary`](#x-ms-summary)
* [`description`](#description)
* [`x-ms-visibility`](#x-ms-visibility)
* [`x-ms-api-annotation`](#x-ms-api-annotation)
* [`x-ms-operation-context`](#x-ms-operation-context)
* [`x-ms-capabilities`](#x-ms-capabilities)
* [`x-ms-trigger`](#x-ms-trigger)
* [`x-ms-trigger-hint`](#x-ms-trigger-hint)
* [`x-ms-notification-content`](#x-ms-notification-content)
* [`x-ms-notification-url`](#x-ms-notification-url)
* [`x-ms-url-encoding`](#x-ms-url-encoding)
* [`x-ms-dynamic-values and x-ms-dynamic-list`](#use-dynamic-values )
* [`x-ms-dynamic-schema and x-ms-dynamic-properties`](#use-dynamic-schema)

The following sections describe these extensions.

## summary

Specifies the title for the action (operation).

Applies to: Operations </br>
Recommended: Use sentence case for `summary`. </br>
Example: "When an event is added to calendar" or "Send an email"

!["summary" for each operation.](./media/openapi-extensions/summary.png)

``` json
"actions" {
  "Send_an_email": {
    /// Other action properties here...
    "summary": "Send an email",
    /// Other action properties here...
  }
},
```

<a name="x-ms-summary"></a>

## x-ms-summary

Specifies the title for an entity.

Applies to: Parameters, response schema </br>
Recommended: Use title case for `x-ms-summary`. </br>
Example: "Calendar ID", "Subject", "Event Description"

!["x-ms-summary" for each entity.](./media/openapi-extensions/x-ms-summary.png)

``` json
"actions" {
    "Send_an_email": {
        /// Other action properties here...
        "parameters": [{
            /// Other parameters here...
            "x-ms-summary": "Subject",
            /// Other parameters here...
        }]
    }
},
```

<a name="description"></a>

## description

Specifies a verbose explanation about the operation's functionality or an entity's format and function.

Applies to: Operations, parameters, response schema </br>
Recommended: Use sentence case for `description`. </br>
Example: "This operation is triggered when a new event is added to the calendar", "Specify the subject of the mail"

!["description" for each operation or entity.](./media/openapi-extensions/description.png)

``` json
"actions" {
    "Send_an_email": {
        "description": "Specify the subject of the mail",
        /// Other action properties here...
    }
},
```

<a name="visibility"></a>

## x-ms-visibility

Specifies the user-facing visibility for an entity.

Possible values: `important`, `advanced`, and `internal` </br>
Applies to: Operations, parameters, schemas

* `important` operations and parameters are always shown to the user first.
* `advanced` operations and parameters are hidden under an additional menu.
* `internal` operations and parameters are hidden from the user.

> [!Note] 
> For parameters that are `internal` and `required`, you *must* provide default values.

Example: The **See more** and **Show advanced options** menus are hiding `advanced` operations and parameters.

!["x-ms-visibility" for showing or hiding operations and parameters.](./media/openapi-extensions/x-ms-visibility.png)

``` json
"actions" {
    "Send_an_email": {
        /// Other action properties here...
        "parameters": [{
            "name": "Subject",
            "type": "string",
            "description": "Specify the subject of the mail",
            "x-ms-summary": "Subject",
            "x-ms-visibility": "important",
            /// Other parameter properties here...
        }]
        /// Other action properties here...
    }
},
```

## x-ms-api-annotation

Used for versioning and life cycle management of an operation.

Applies to: Operations

  - `family`&mdash;A string denoting the operation family folder.
  - `revision`&mdash;An integer denoting the revision number.
  - `replacement`&mdash;An object containing the replacement API information and operations.

  ```json
  "x-ms-api-annotation": {
          "family": "ListFolder",
          "revision": 1,
          "replacement": {
            "api": "SftpWithSsh",
            "operationId": "ListFolder"
          }
        }
  ```

## x-ms-operation-context

Used for simulating the firing of a trigger to enable testing of a trigger-dependent flow.

Applies to: Operations

  ```json
  "x-ms-operation-context": {
          "simulate": {
            "operationId": "GetItems_V2",
            "parameters": {
              "$top": 1
            }
          }
  ```

## x-ms-capabilities

When used on the connector level, this is an overview of the capabilities that are offered by the connector, including specific operations.

Applies to: Connectors

  ```json
  "x-ms-capabilities": {
    "testConnection": {
      "operationId": "GetCurrentUser"
    },
  }
  ```

When used on the operation level, this is used to identify that the operation supports chunking upload and/or static chunk size, and can be provided by the user. </br>

Applies to: Operations

  - `chunkTransfer`&mdash;A Boolean value to indicate whether chunk transfer is supported.

  ```json
  "x-ms-capabilities": {
    "chunkTransfer": true
  }
  ```

## x-ms-trigger

Identifies whether the current operation is a trigger that produces a single event. The absence of this field means this is an `action` operation.

Applies to: Operations

  - `single`&mdash;Object response
  - `batch`&mdash;Array response

  ```json
  "x-ms-trigger": "batch"
  ```

## x-ms-trigger-hint

A description of how to fire an event for a trigger operation.

Applies to: Operations

  ```json
  "x-ms-trigger-hint": "To see it work, add a task in Outlook."
  ```

## x-ms-notification-content

Contains a schema definition of a webhook notification request. This is the schema for a webhook payload posted by external services to the notification URL.

Applies to: Operations

  ```json
  "x-ms-notification-content": {
        "schema": {
          "$ref": "#/definitions/WebhookPayload"
        }
      },
  ```

## x-ms-notification-url

Identifies in a Boolean value whether a webhook notification URL should be placed in this parameter/field for a webhook registration operation. 

Applies to: Parameters/input fields
 
  ```json
  "x-ms-notification-url": true
  ```

## x-ms-url-encoding

Identifies whether the current path parameter should be double URL-encoded `double` or single URL-encoded `single`. The absence of this field indicates `single` encoding.

Applies to: Path parameters

  ```json
  "x-ms-url-encoding": "double"
  ```

## Use dynamic values

Dynamic values are a list of options for the user to select input parameters for an operation.  

Applies to: Parameters  

![Dynamic-values for showing lists.](./media/openapi-extensions/x-ms-dynamic-values.png)

### How to use dynamic values


>[!Note]
>A *path string* is a JSON pointer that doesn't contain the leading forward slash. So, this is a JSON pointer: **/property/childProperty**, and this is a path string: **property/childProperty**.

There are two ways to define dynamic values: 

1. Use `x-ms-dynamic-values` 

    |Name|Required|Description|
    ---|----|---|
    operationId |Yes|The operation that returns the values. 
    parameters |Yes| An object that provides the input parameters that are required to invoke a dynamic-values operation. 
    value-collection |No|A path string that evaluates to an array of objects in the response payload. If value-collection isn't specified, the response is evaluated as an array.
    value-title |No|A path string in the object inside value-collection that refers to the value's description. 
    value-path |No|A path string in the object inside value-collection that refers to the parameter value.

    ```json
    "x-ms-dynamic-values": {
        "operationId": "PopulateDropdown",
        "value-path": "name",
        "value-title": "properties/displayName",
        "value-collection": "value",
        "parameters": {
            "staticParameter": "<value>",
            "dynamicParameter": {
                "parameter": "<name of the parameter to be referenced>"
            }
        }
    }  
    ```
    >[!Note]
    >It's possible to have ambiguous parameter references when using dynamic values. For instance, in the following definition of an operation, the dynamic values reference the field **id**, which is ambiguous from the definition because it's not clear whether it references the parameter **id** or the property **requestBody/id**. 
 
    ```json
    {
        "summary": "Tests dynamic values with ambiguous references",
        "description": "Tests dynamic values with ambiguous references.",
        "operationId": "TestDynamicValuesWithAmbiguousReferences",
        "parameters": [{
            "name": "id",
            "in": "path",
            "description": "The request id.",
            "required": true
        }, {
            "name": "requestBody",
            "in": "body",
            "description": "query text.",
            "required": true,
            "schema": {
                "description": "Input body to execute the request",
                "type": "object",
                "properties": {
                    "id": {
                        "description": "The request Id",
                        "type": "string"
                    },
                    "model": {
                        "description": "The model",
                        "type": "string",
                        "x-ms-dynamic-values": {
                            "operationId": "GetSupportedModels",
                            "value-path": "name",
                            "value-title": "properties/displayName",
                            "value-collection": "value",
                            "parameters": {
                                "requestId": {
                                    "parameter": "id"
                                }
                            }
                        }
                    }
                }
            }
        }],
        "responses": {
            "200": {
                "description": "OK",
                "schema": {
                    "type": "object"
                }
            },
            "default": {
                "description": "Operation Failed."
            }
        }
    }
    ``` 

1. `x-ms-dynamic-list`

   There's no way to reference parameters unambiguously. This feature might be provided in the future. If you want your operation to take advantage of any new updates, add the new extension `x-ms-dynamic-list` along with `x-ms-dynamic-values`. Also, if your dynamic extension references properties within parameters, you have to add the new extension `x-ms-dynamic-list` along with `x-ms-dynamic-values`. The parameter references that point to properties must be expressed as path strings.

    - `parameters`&mdash;This property is an object where each input property of the dynamic operation being called is defined with either a static value field or a dynamic reference to the source operation's property. Both of these options are defined in the following.

    - `value`&mdash;This is the literal value to be used for the input parameter. In the following example, the input parameter of the operation **GetDynamicList** operation named **version** is defined by using a static value of **2.0**.

      ``` json
      {
          "operationId": "GetDynamicList",
          "parameters": {
            "version": {
              "value": "2.0"
            }
          }
      }
      ```

    - `parameterReference`&mdash;This is the full parameter reference path, starting with the parameter name followed by the path string of the property to be referenced. For example, the input property of the operation **GetDynamicList** named **property1**, which is under the parameter **destinationInputParam1**, is defined as a dynamic reference to a property named **property1** under parameter **sourceInputParam1** of the source operation.

      ``` json
      {
          "operationId": "GetDynamicList",
            "parameters": {
                "destinationInputParam1/property1": {
                  "parameterReference": "sourceInputParam1/property1"
          }
        }
      }
      ```

      >[!Note]
      > If you want to reference any property that's marked as internal with a default value, you have to use the default value as a static value in the definition here, instead of `parameterReference`. The default value from the list isn't used if it's defined by using `parameterReference`.

      |Name|Required|Description|
      ---|----|---|
      operationId |Yes|The operation that returns the list. 
      parameters|Yes|An object that provides the input parameters required to invoke a dynamic list operation. 
      itemsPath|No|A path string that evaluates to an array of objects in the response payload. If `itemsPath` isn't provided, the response is evaluated as an array. 
      itemTitlePath|No|A path string in the object inside `itemsPath` that refers to the value's description. 
      itemValuePath |No|A path string in the object inside `itemsPath` that refers to the item's value.  

      With `x-ms-dynamic-list`, use parameter references with the path string of the property you're referencing. Use these parameter references for both the key and the value of the dynamic operation parameter reference. 

      ```json
      {
        "summary": "Tests dynamic values with ambiguous references",
        "description": "Tests dynamic values with ambiguous references.",
        "operationId": "TestDynamicListWithAmbiguousReferences",
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "description": "The request id.",
            "required": true
          },
          {
            "name": "requestBody",
            "in": "body",
            "description": "query text.",
            "required": true,
            "schema": {
              "description": "Input body to execute the request",
              "type": "object",
              "properties": {
                "id": {
                  "description": "The request id",
                  "type": "string"
                },
                "model": {
                  "description": "The model",
                  "type": "string",
                  "x-ms-dynamic-values": {
                    "operationId": "GetSupportedModels",
                    "value-path": "name",
                    "value-title": "properties/displayName",
                    "value-collection": "cardTypes",
                    "parameters": {
                      "requestId": {
                        "parameter": "id"
                      }
                    }
                  },
                  "x-ms-dynamic-list": {
                    "operationId": "GetSupportedModels",
                    "itemsPath": "cardTypes",
                    "itemValuePath": "name",
                    "itemTitlePath": "properties/displayName",
                    "parameters": {
                      "requestId": {
                        "parameterReference": "requestBody/id"
                      }
                    }
                  }
                }
              }
            }
          }
        ],
        "responses": {
          "200": {
            "description": "OK",
            "schema": {
              "type": "object"
            }
          },
          "default": {
            "description": "Operation Failed."
          }
        }
      } 
      ```

## Use dynamic schema

The dynamic schema indicates that the schema for the current parameter or response is dynamic. This object invokes an operation that's defined by the value in this field, dynamically discovers the schema, and displays the appropriate user interface for collecting user input or shows the available fields. 
 
Applies to: Parameters, responses

This image shows how the input form changes, based on the item that the user selects from the list:

![The form changes based on the selection the user makes.](./media/openapi-dynamic-schema/x-ms-dynamic-schema.png)

This image shows how the outputs change, based on the item that the user selects from the dropdown list. In this version, the user selects **Cars**:

![The user selects Cars.](./media/openapi-dynamic-schema/x-ms-dynamic-schema-output1.png)

In this version, the user selects **Food**:

![The user selects Food.](./media/openapi-dynamic-schema/x-ms-dynamic-schema-output2.png)

### How to use dynamic schema

>[!Note]
>A *path string* is a JSON pointer that doesn't contain the leading forward slash. So, this is a JSON pointer: **/property/childProperty**, and this is a path string: **property/childProperty**.

There are two ways to define dynamic schema: 

1. `x-ms-dynamic-schema`:

    |Name|Required|Description|
    ---|---|---
    operationId | Yes | The operation that returns the schema.
    parameters |Yes | An object that provides the input parameters that are required to invoke a dynamic-schema operation.
    value-path | No | A path string that refers to the property that has the schema. If not specified, the response is assumed to contain the schema in the root object's properties. If specified, the successful response must contain the property. For an empty or undefined schema, this value should be null.

    ```json
      {
      "name": "dynamicListSchema",
      "in": "body",
      "description": "Dynamic schema for items in the selected list",
      "schema": {
          "type": "object",
          "x-ms-dynamic-schema": {
              "operationId": "GetListSchema",
              "parameters": {
                  "listID": {
                      "parameter": "listID-dynamic"
                  }
              },
              "value-path": "items"
          }
        }
      }
    ``` 

    >[!Note]
    >There might be ambiguous references in the parameters. For instance, in the following definition of an operation, the dynamic schema references a field named **query**, which can't be deterministically understood from the definition&mdash;whether it's referencing the parameter object **query** or the string property **query/query**. 

    ```json
    {

        "summary": "Tests dynamic schema with ambiguous references",
        "description": "Tests dynamic schema with ambiguous references.",
        "operationId": "TestDynamicSchemaWithAmbiguousReferences",
        "parameters": [{
            "name": "query",
            "in": "body",
            "description": "query text.",
            "required": true,
            "schema": {
                "description": "Input body to execute the request",
                "type": "object",
                "properties": {
                    "query": {
                        "description": "Query Text",
                        "type": "string"
                    }
                }
            },
            "x-ms-summary": "query text"
        }],
        "responses": {
            "200": {
                "description": "OK",
                "schema": {
                    "x-ms-dynamic-schema": {
                        "operationId": "GetDynamicSchema",
                        "parameters": {
                            "query": {
                                "parameter": "query"
                            }
                        },
                        "value-path": "schema/valuePath"
                    }
                }
            },
            "default": {
                "description": "Operation Failed."
            }
        }
    }
      ``` 
      ###### Examples from open source connectors
      
      Connector|Scenario|Link
      ---------|----------------------------------------|-----------------------------------------------
      Ticketing|Get schema for details of a selected event|[Ticketing](https://github.com/microsoft/PowerPlatformConnectors/blob/070009ba5681fd57ee6cc61d2a380123712a088f/certified-connectors/Ticketing.events/apiDefinition.swagger.json)

1. `x-ms-dynamic-properties`:

    There's no way to reference parameters unambiguously. This feature might be provided in the future. If you want your operation to take advantage of any new updates, add the new extension `x-ms-dynamic-properties` along with `x-ms-dynamic-schema`. Also, if your dynamic extension references properties within parameters, you have to add the new extension `x-ms-dynamic-properties` along with `x-ms-dynamic-schema`. The parameter references that point to properties must be expressed as path strings. 

    - `parameters`&mdash;This property is an object where each input property of the dynamic operation being called is defined with either a static value field or a dynamic reference to the source operation's property. Both of these options are defined in the following.

    - `value`&mdash;This is the literal value to be used for the input parameter. In the following example, the input parameter of the **GetDynamicSchema** operation named **version** is defined with a static value of **2.0**.

      ``` json
      {
          "operationId": "GetDynamicSchema",
          "parameters": {
            "version": {
              "value": "2.0"
            }
          }
      }
      ```

    - `parameterReference`&mdash;This is the full parameter reference path, starting with the parameter name followed by the path string of the property to be referenced. For example, the input property of the operation **GetDynamicSchema** named **property1**, which is under the parameter **destinationInputParam1**, is defined as a dynamic reference to a property named **property1** under parameter **sourceInputParam1** of the source operation.

      ``` json
      {
          "operationId": "GetDynamicSchema",
            "parameters": {
                "destinationInputParam1/property1": {
                  "parameterReference": "sourceInputParam1/property1"
          }
        }
      }
      ```

      >[!Note]
      > If you want to reference any property that's marked as internal with a default value, you have to use the default value as a static value in the definition here, instead of `parameterReference`. The default value from the schema isn't used if it's defined by using `parameterReference`.
      >
  
      |Name|Required|Description
        ---|---|---
      operationId |Yes | The operation that returns the schema. 
      parameters | Yes | An object that provides the input parameters that are required to invoke a dynamic-schema operation. 
      itemValuePath | No| A path string that refers to the property that has the schema. If not specified, the response is assumed to contain the schema in the root object. If specified, the successful response must contain the property. For an empty or undefined schema, this value should be null.

      With `x-ms-dynamic-properties`, parameter references can be used with the path string of the property to be referenced, both for the key and the value of the dynamic operation parameter reference. 

      ```json
          {
          "summary": "Tests dynamic schema with ambiguous references",
          "description": "Tests dynamic schema with ambiguous references.",
          "operationId": "TestDynamicSchemaWithAmbiguousReferences",
          "parameters": [{
              "name": "query",
              "in": "body",
              "description": "query text.",
              "required": true,
              "schema": {
                  "description": "Input body to execute the request",
                  "type": "object",
                  "properties": {
                      "query": {
                          "description": "Query Text",
                          "type": "string"
                      }
                  }
              },
              "x-ms-summary": "query text"
          }],
          "responses": {
              "200": {
                  "description": "OK",
                  "schema": {
                      "x-ms-dynamic-schema": {
                          "operationId": "GetDynamicSchema",
                          "parameters": {
                              "version": "2.0",
                              "query": {
                                  "parameter": "query"
                              }
                          },
                          "value-path": "schema/valuePath"
                      },
                      "x-ms-dynamic-properties": {
                          "operationId": "GetDynamicSchema",
                          "parameters": {
                              "version": {
                                  "value": "2.0"
                              },
                              "query/query": {
                                  "parameterReference": "query/query"
                              }
                          },
                          "itemValuePath": "schema/valuePath"
                      }
                  }
              },
              "default": {
                  "description": "Operation Failed."
              }
            }
          }
      ``` 

## Next step

[Create a custom connector from an OpenAPI definition](define-openapi-definition.md)

### See also

[Custom connectors overview](index.md)

[!INCLUDE[feedback](../includes/feedback.md)]
