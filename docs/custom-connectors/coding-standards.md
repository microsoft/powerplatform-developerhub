---
title: Coding standards | Microsoft Docs
description: General guidelines for custom connector coding standards.
Keywords: connectors, custom connectors, canvas app, flow
author: lnborbolla42
services: connectors
ms.service: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: laborbol
ms.reviewer: angieandrews
---

# Coding standards

When creating a custom connector, follow these good practices in your code.

## API definition

The `apiDefinition.swagger.json`, also known as swagger, is where the connector's behavior is defined.

- Indentation: Use soft tabs with four spaces, don't use hard tabs.
- Remove trailing space. Trailing space includes all whitespace located at the end of a line when there's no other character following it. Hard tabs count as a trailing space if there is nothing following them.

### Structure of the file

To have a readable swagger definition, create an API definition with the following structure:

1. SwaggerOpenAPI version (currently, only 2.0 is supported)
2. Information about the connector (title, description, status, contacts)
3. Host and supported schemes
4. Consumes/produces section
5. Paths (definitions of actions and triggers)
6. Definitions (Data types used in actions and triggers)
7. Parameters (Parameters that can be used across operations)

### Actions and triggers

Use pascal casing for `operationId`: capitalize every word (including the first one). Don't add hyphens (`-`) or underscores (`_`) to separate words.

- **Good**

  ```json
  "operationId": "GetMessages",
  ```

- **Bad**

  ```json
  "operationId": "getMessages",
  "operationId": "Get-Messages",
  "operationId": "Get_Messages",
  ```

#### Summary and description

Operations should always have a summary and description. The summary will be the name of the operation so keep it short and precise, the description should give more information and end with a period (`.`).

Most descriptions are placed as hints in the parameter boxes, be sure to provide a short and meaningful text. Descriptions and summaries **should not** have the same content.

- **Good**

  ```json
  "summary": "List Name",
  "description": "The list to check for messages.",
  ```

- **Bad**

  ```json
  "summary": "List",
  "description": "List.",
  ```

### Responses

Define your success code by using an HTTP `2xx` response type. Don't use `default` as your only defined response, pair it with a success definition instead. If possible, also list the known `4xx`/`5xx` responses.

- Use `4xx` type errors when the problem originated on the client side, here are some examples:

  - `401 - The username is invalid (it can only contain lowercase letters and numbers)`

- Use `5xx` type errors when the problem originated on the server side

  - `504 - The request timed-out`

- **Good**

  ```json
  {
    "responses": {
      "200": {
        "description": "OK",
        "schema": {
          "$ref": "#/definitions/Message"
        }
      },
      "400": {
        "description": "Bad Request"
      },
      "404": {
        "description": "Not Found"
      },
      "401": {
        "description": "Unauthorized"
      },
      "403": {
        "description": "Forbidden"
      },
      "500": {
        "description": "Internal Server Error. Unknown error occurred"
      },
      "default": {
        "description": "Operation Failed."
      }
    }
  }
  ```

- **Bad**

  ```json
  {
    "responses": {
      "default": {
        "description": "OK",
        "schema": {
          "type": "string"
        }
      }
    }
  }
  ```

### Definitions and parameters

For parameters that appear on more than one operation, create a name parameter in the `parameters` section instead of defining this parameter in every operation that uses it.

- **Good**

  In this example, parameter `id` is defined in `IdInPath`, and used in operations `GetMemberGroups` and `RemoveMemberFromGroup`.

  ```json
  {
    "paths": {
      "/groups/{id}/getMemberGroups": {
        "post": {
          "summary": "Get groups of a user",
          "description": "Get the groups a user is a member of.",
          "operationId": "GetMemberGroups",
          "parameters": [
            {
              "$ref": "#/parameters/IdInPath"
            }
          ],
          "responses": {
            // response definitions
          }
        }
      },
      "/groups/{id}/members/{memberId}/delete": {
        "delete": {
          "summary": "Remove member",
          "description": "Delete a member from a group.",
          "operationId": "RemoveMemberFromGroup",
          "parameters": [
            {
              "$ref": "#/parameters/IdInPath"
            },
            {
              "name": "memberId",
              "in": "path",
              "required": true,
              "description": "The Id of the member to be deleted.",
              "x-ms-summary": "Member Id",
              "type": "string"
            }
          ],
          "responses": {
            // response definitions
          }
        }
      }
    },
    // definitions
    "parameters":{
      "IdInPath": {
        "name": "id",
        "in": "path",
        "description": "Unique identifer of a group (Ex. 'id_group1').",
        "required": true,
        "x-ms-summary": "Group Id",
        "type": "string"
      },
    }
  }
  ```

  Using references on parameters and definitions will make the ApiDefinition more maintainable. For example, if the description needs an updated, it will be done in just one place, instead of all the operations that use it.

- **Bad**

  ```json
  {
    "paths": {
      "/groups/{id}/getMemberGroups": {
        "post": {
          "summary": "Get groups of a user",
          "description": "Get the groups a user is a member of.",
          "operationId": "GetMemberGroups",
          "parameters": [
            {
              "name": "id",
              "in": "path",
              "description": "Unique identifer of a group (Ex. 'id_group1').",
              "required": true,
              "x-ms-summary": "Group Id",
              "type": "string"
            }
          ],
          "responses": {
            // response definitions
          }
        }
      },
      "/groups/{id}/members/{memberId}/delete": {
        "delete": {
          "summary": "Remove member",
          "description": "Delete a member from a group.",
          "operationId": "RemoveMemberFromGroup",
          "parameters": [
            {
              "name": "id",
              "in": "path",
              "description": "Unique identifer of a group (Ex. 'id_group1').",
              "required": true,
              "x-ms-summary": "Group Id",
              "type": "string"
            },
            {
              "name": "memberId",
              "in": "path",
              "required": true,
              "description": "The Id of the member to be deleted.",
              "x-ms-summary": "Member Id",
              "type": "string"
            }
          ],
          "responses": {
            // response definitions
          }
        }
      }
    }
  }
  ```

Create an entry on definitions and use that reference across operations that have the same object response.

- **Good**

  In this example, apart from using a referenced parameter, the operations `GetUser` and `UpdateUser` reference the same object response `UserResponse`, defined once in the `definitions` section.

  ```json
  {
    "paths": {
      "/v1.0/users/{id}": {
        "get": {
          "summary": "Get user",
          "description": "Get details for a user.",
          "operationId": "GetUser",
          "parameters": [
            {
              "$ref": "#/parameters/IdInPath"
            }
          ],
          "responses": {
            "200": {
              "description": "200",
              "schema": {
                "$ref": "#/definitions/UserResponse"
              }
            }
          }
        },
        "patch": {
          "summary": "Update user",
          "description": "Update the info for a user.",
          "operationId": "UpdateUser",
          "parameters": [
            {
              "$ref": "#/parameters/IdInPath"
            }
          ],
          "responses": {
            "201": {
              "description": "Accepted",
              "schema": {
                "$ref": "#/definitions/UserResponse"
              }
            }
          },
          "deprecated": false,
          "x-ms-no-generic-test": true
        }
      },
    },
    // definitions
    "definitions":{
     "UserResponse": {
        "type": "object",
        "properties": {
          "id": {
            "description": "The unique identifier for the user.",
            "type": "string",
            "x-ms-summary": "Id"
          },
          "email": {
            "description": "The email associated to the user.",
            "type": "string",
            "x-ms-summary": "Email"
          },
          // more fields here
        }
      }
    }
  }
  ```

- **Bad**

  ```json
  {
    "paths": {
      "/v1.0/users/{id}": {
        "get": {
          "summary": "Get user",
          "description": "Get details for a user.",
          "operationId": "GetUser",
          "parameters": [
            {
              "$ref": "#/parameters/IdInPath"
            }
          ],
          "responses": {
            "200": {
              "description": "200",
              "schema": {
                "$ref": "#/definitions/UserResponse"
              }
            }
          }
        },
        "patch": {
          "summary": "Update user",
          "description": "Update the info for a user.",
          "operationId": "UpdateUser",
          "parameters": [
            {
              "$ref": "#/parameters/IdInPath"
            }
          ],
          "responses": {
            "201": {
              "description": "Accepted",
              "schema": {
                "$ref": "#/definitions/UserResponse"
              }
            }
          },
          "deprecated": false,
          "x-ms-no-generic-test": true
        }
      },
    },
    // definitions
    "definitions":{
     "UserResponse": {
        "type": "object",
        "properties": {
          "id": {
            "description": "The unique identifier for the user.",
            "type": "string",
            "x-ms-summary": "Id"
          },
          "email": {
            "description": "The email associated to the user.",
            "type": "string",
            "x-ms-summary": "Email"
          },
          // more fields here
        }
      }
    }
  }
  ```

### More information

For more information, go to these references:

- [OpenAPI Specification - Version 2.0](https://swagger.io/specification/v2/)
- [Swagger Coding Style Guidelines](https://github.com/watson-developer-cloud/api-guidelines/blob/master/swagger-coding-style.md)

[!INCLUDE[feedback](../includes/feedback.md)]