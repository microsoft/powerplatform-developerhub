---
title: Multi-auth on custom connectors | Microsoft Docs
description: Tutorial on how to configure a custom connector with multiple authentications
author: lnborbolla42
manager: olshyron
editor:
services: connectors
documentationcenter:
ms.assetid:
ms.service: connectors
ms.workload: connectors
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2021
ms.author: laborbol
ms.reviewer: angieandrews
---

# Multiple authentications on a custom connector

Multiple authentications (`multi-auth`) is a feature that allows users to create connections by providing them an option to choose which authentication method they want to use to create their connection, as opposed to being limited to just one authentication type.

For a connector, a collection of authentication types is defined via `connectionParameterSets` in the `apiProperties.json` file.

> [!IMPORTANT]
> Enabling multiple authentications on a custom connector is not supported on the Custom Connector Wizard yet. Use [Microsoft Power Platform Connectors CLI](paconn-cli.md) instead to create a custom connector with multi-auth.

## How to enable multi-auth

Add the `connectionParameterSets` in the `apiProperties.json` file, and populate the collection `connectionParameterSets.values` with as many `connectionParameters` needed for the connector.

> [!Note]
> To know more about connection parameters and authentication types, visit [Connection Parameters](connection-parameters.md).

The following is the structure of `connectionParameterSets`:

```json
"connectionParameterSets": {
  // uiDefinition for the parameter sets.
  "uiDefinition": {
    "displayname": "Select the authorization type",
    "description": "<<Enter here your description>>"
  },
  "values": [
    // Connection parameter set
    {
      "name": "<parameter set name>",
      // uiDefinition for this parameter set.
      "uiDefinition": {
        "displayname": "<display name>",
        "description": "<description text>"
      },
      "parameters": {
        // Schema matches existing "connectionParameters"
        "<parameter name>": {
          "type": "string | securestring | oauthsetting"
        },
        "<parameter name>:clientId": {
          "type": "string",
          "uiDefinition": {
            "schema": {
              // For string types, the description must be placed in
              // uiDefinition.schema.description to be shown in the description box
              "type": "string",
              "description": "<description text>"
            },
            "displayName": "<display name>",
          }
        }
      }
    },
    {
      "name": "<parameter set name 2>"
      // ...
    }
  ]
}
```

## Examples

## Configuration with API key and Basic Authentication

```json
"connectionParameterSets": {
    "uiDefinition": {
        "displayName": "Authentication Type",
        "description": "Type of authentication to be used."
    },
    "values": [
        {
            "name": "basic-auth",
            "uiDefinition": {
                "displayName": "Use your X credentials",
                "description": "Log in using your username and password for X."
            },
            "parameters": {
                "username": {
                    "type": "string",
                    "uiDefinition": {
                        "displayName": "X username",
                        "schema":{
                            "description": "The username for X",
                            "type": "string"
                        },
                        "tooltip": "Provide your X username",
                        "constraints": {
                            "required": "true"
                        }
                    }
                },
                "password": {
                    "type": "securestring",
                    "uiDefinition": {
                        "displayName": "X password",
                        "schema":{
                            "description": "The password for X",
                            "type": "securestring"
                        },
                        "tooltip": "Provide your X password",
                        "constraints": {
                            "required": "true"
                        }
                    }
                }
            }
        },
        {
            "name": "api-auth",
            "uiDefinition": {
                "displayName": "Use X API Key",
                "description": "Log in using X's API Key."
            },
            "parameters": {
                "api_key": {
                    "type": "securestring",
                    "uiDefinition": {
                        "constraints": {
                            "clearText": false,
                            "required": "true",
                            "tabIndex": 3
                        },
                        "schema":{
                            "description": "Enter your API Key for X",
                            "type": "securestring"
                        },
                        "displayName": "API Key generated in X"
                    }
                },
                "environment": {
                    "type": "string",
                    "uiDefinition": {
                        "displayName": "Environment",
                        "schema":{
                            "description": "The API environment to use; either production or sandbox",
                            "type": "string"
                        },
                        "tooltip": "Select an API environment to use",
                        "constraints": {
                            "required": "true",
                            "allowedValues": [
                                {
                                    "text": "Sandbox",
                                    "value": "YOUR_SANDBOX_VALUE_HERE"
                                },
                                {
                                    "text": "Production",
                                    "value": "YOUR_PROD_VALUE_HERE"
                                }
                            ]
                        }
                    }
                }
            }
        }
    ]
}
```

## Configuration with AAD and OAUTH 2.0

It's possible to have two connectionParameters of the same type with. In this case, both authorizations use `oauthSetting` type: one allows the user to create a connection using AAD while the other goes to a custom endpoint.

```json
"connectionParameterSets": {
    "uiDefinition": {
        "displayName": "Authentication Type",
        "description": "Type of authentication to be used."
    },
    "values": [
        {
            "name": "aad-auth",
            "uiDefinition": {
                "displayName": "Use default shared application",
                "description": "Log in using the standard X app."
            },
            "parameters": {
                "token": {
                    "oAuthSettings": {
                        "clientId": "YOUR_AAD_APPLICATION_ID_HERE",
                        "customParameters": {
                            "loginUri": {
                                "value": "https://login.windows.net"
                            },
                            "resourceUri": {
                                "value": "https://graph.microsoft.com"
                            },
                            "tenantId": {
                                "value": "common"
                            }
                        },
                        "identityProvider": "aad",
                        "properties": {
                            "IsFirstParty": "False"
                        },
                        "redirectMode": "Global",
                        "redirectUrl": "https://global.consent.azure-apim.net/redirect",
                        "scopes": [
                            "Group.ReadWrite.All offline_access"
                        ]
                    },
                    "type": "oauthSetting"
                }
            }
        },
        {
            "name": "custom-app-auth",
            "uiDefinition": {
                "displayName": "Use the X authentication app",
                "description": "Log in using X app."
            },
            "parameters": {
                "token": {
                    "type": "oauthSetting",
                    "oAuthSettings": {
                        "clientId": "YOUR_CLIENT_ID_HERE",
                        "identityProvider": "oauth2",
                        "redirectMode": "Global",
                        "redirectUrl": "https://global.consent.azure-apim.net/redirect",
                        "customParameters": {
                            "authorizationUrl": {
                                "value": "https://login.dummy.net/request"
                            },
                            "refreshUrl": {
                                "value": "https://login.dummy.net/token"
                            },
                            "tokenUrl": {
                                "value": "https://login.dummy.net/token"
                            }
                        }
                    }
                }
            }
        }
    ]
}
```

Take a look at **RescoCloud** connector [here](https://github.com/microsoft/PowerPlatformConnectors/blob/1ea774fad05608f1a8a983cc99ba6d922bead6fe/certified-connectors/RescoCloud/apiProperties.json) for a current example on how to implement multi-auth.
