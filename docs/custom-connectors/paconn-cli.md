---
title: Microsoft Power Platform Connectors CLI | Microsoft Docs
description: Use the Power Platform Connectors CLI to develop connectors for Microsoft Power Automate and Microsoft Power Apps.
author: mamurshe
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/16/2022
ms.author: mamurshe
ms.reviewer: angieandrews
---

# Microsoft Power Platform Connectors CLI

>[!Note]
>*These release notes describe functionality that may not have been released yet.* To find out when this functionality is planned to release, go to [What's new and planned for Common Data Model and Data Integration](/business-applications-release-notes/April19/cdm-data-integration/planned-features). Delivery timelines and projected functionality might change or might not ship (go to [Microsoft policy](https://go.microsoft.com/fwlink/p/?linkid=2007332)).

The `paconn` command-line tool is designed to aid Microsoft Power Platform custom connectors development.

## Installing

1. Install Python 3.5+ from [https://www.python.org/downloads](Python downloads). Select the **Download** link on any version of Python greater than Python 3.5. For Linux and macOS X, follow the appropriate link on the page. You can also install using an OS-specific package manager of your choice.

2. Run the installer to begin installation and be sure to check the box 'Add Python X.X to PATH'.

3. Make sure the installation path is in the PATH variable by running:

    `python --version`

4. After python is installed, install `paconn` by running:

    `pip install paconn`

   If you get errors saying 'Access is denied', consider using the `--user` option or running the command as an Administrator (Windows).

## Custom Connector Directory and Files

A custom connector consists of two to four files: an Open API swagger definition, an API properties file, an optional icon for the connector, and an optional csharp script file. The files are generally located in a directory with the connector ID as the name of the directory.

Sometimes, the custom connector directory may include a `settings.json` file. Although this file isn't part of the connector definition, it can be used as an argument-store for the CLI.

### API Definition (Swagger) File

The API definition file describes the API for the custom connector using the OpenAPI specification. It's also known as the swagger file. For more information about API definitions used to write custom connector, go to [Create a custom connector from an OpenAPI definition](define-openapi-definition.md). Also review the tutorial in the [Extend an OpenAPI definition for a custom connector](openapi-extensions.md) article.

### API Properties File

The API properties file contains some properties for the custom connector. These properties aren't part of the API definition. It contains information such as the brand color, authentication information, and so on. A typical API properties file looks like the following sample:

```json
{
  "properties": {
    "capabilities": [],
    "connectionParameters": {
      "api_key": {
        "type": "securestring",
        "uiDefinition": {
          "constraints": {
            "clearText": false,
            "required": "true",
            "tabIndex": 2
          },
          "description": "The KEY for this API",
          "displayName": "KEY",
          "tooltip": "Provide your KEY"
        }
      }
    },
    "iconBrandColor": "#007EE6",
    "scriptOperations": [
        "getCall",
        "postCall",
        "putCall"
    ],
    "policyTemplateInstances": [
      {
        "title": "MyPolicy",
        "templateId": "setqueryparameter",
        "parameters": {
            "x-ms-apimTemplateParameter.name": "queryParameterName",
            "x-ms-apimTemplateParameter.value": "queryParameterValue",
            "x-ms-apimTemplateParameter.existsAction": "override"
        }
      }
    ]    
  }
}
```

More information on each of the properties is given below:

* `properties`: The container for the information.

* `connectionParameters`: Defines the connection parameter for the service.

* `iconBrandColor`: The icon brand color in HTML hex code for the custom connector.

* `scriptOperations`: A list of the operations that are executed with the script file. An empty scriptOperations list indicates that all operations are executed with the script file.

* `capabilities`: Describes the capabilities for the connector, for example, cloud only, on-prem gateway, and so on.

* `policyTemplateInstances`: An optional list of policy template instances and values used in the custom connector.

### Icon File

The icon file is a small image representing the custom connector icon.

### Script File

The script is a CSX script file that's deployed for the custom connector and executed for every call to a subset of the connector's operations.

### Settings File

Instead of providing the arguments in the command line, a `settings.json` file can be used to specify them. A typical `settings.json` file looks like the following sample: 

```json
{
  "connectorId": "CONNECTOR-ID",
  "environment": "ENVIRONMENT-GUID",
  "apiProperties": "apiProperties.json",
  "apiDefinition": "apiDefinition.swagger.json",
  "icon": "icon.png",
  "script": "script.csx",
  "powerAppsApiVersion": "2016-11-01",
  "powerAppsUrl": "https://api.powerapps.com"
}
```

In the settings file, the following items are expected. If an option is missing but required, the console will prompt for the missing information.

* `connectorId`: The connector ID string for the custom connector. This parameter is required for download and update operations, but not for the create or validate operation. A new custom connector with the new ID will be created for create command. If you need to update a custom connector just created using the same settings file, make sure the settings file is correctly updated with the new connector ID from the create operation.

* `environment`: The environment ID string for the custom connector. This parameter is required for all operations, except the validate operation.

* `apiProperties`: The path to the API properties `apiProperties.json` file. It's required for the create and update operation. When this option is present during the download, the file will be downloaded to the given location; otherwise it will be saved as `apiProperties.json`.

* `apiDefinition`: The path to the swagger file. It's required for the create, update, and validate operations. When this option is present during the download operation, the file in the given location will be written to; otherwise it will be saved as `apiDefinition.swagger.json`.

* `icon`: The path to the optional icon file. The create and update operations will use the default icon when this parameter is no specified. When this option is present during the download operation, the file in the given location will be written to; otherwise it will be saved as `icon.png`.

* `script`: The path to the optional script file. The create and update operations will use only the value within the specified parameter. When this option is present during the download operation, the file in the given location will be written to; otherwise it will be saved as `script.csx`.

* `powerAppsUrl`: The API URL for Power Apps. This parameter is optional and set to `https://api.powerapps.com` by default.

* `powerAppsApiVersion`: The API version to use for Power Apps. This parameter is optional and set to `2016-11-01` by default.

## Command-Line Operations

### Login

Log in to Power Platform by running:
   
`paconn login`

This command will ask you to log in using the device code login process. Follow the prompt for the login. Service Principle authentication isn't supported at this point.

### Logout

Logout by running:
   
`paconn logout`

### Download Custom Connector Files

The connector files are always downloaded into a subdirectory with the connector ID as the directory name. When a destination directory is specified, the subdirectory will be created in the specified one. Otherwise, it will be created in the current directory. In addition to the three connector files, the download operation will also write a fourth file called settings.json containing the parameters used to download the files.

Download the custom connector files by running:

`paconn download`

or
   
`paconn download -e [Power Platform Environment GUID] -c [Connector ID]`
   
or
   
`paconn download -s [Path to settings.json]`

When the environment or connector ID isn't specified, the command will prompt for the missing argument(s). The command will output the download location for the connector if it successfully downloads.

All the arguments can be also specified using a [settings.json file](#settings-file).

```
Arguments
   --cid -c       : The custom connector ID.
   --dest -d      : Destination directory.
   --env -e       : Power Platform environment GUID.
   --overwrite -w : Overwrite all the existing connector and settings files.
   --pau -u       : Power Platform URL.
   --pav -v       : Power Platform API version.
   --settings -s  : A settings file containing required parameters.
                    When a settings file is specified some command 
                    line parameters are ignored.
```

### Create a New Custom Connector

A new custom connector can be created from the connectors files by running the `create` operation. Create a connector by running:
   
`paconn create --api-prop [Path to apiProperties.json] --api-def [Path to apiDefinition.swagger.json]`
   
or
   
`paconn create -e [Power Platform Environment GUID] --api-prop [Path to apiProperties.json] --api-def [Path to apiDefinition.swagger.json] --icon [Path to icon.png] --secret [The OAuth2 client secret for the connector]`
   
or
   
`paconn create -s [Path to settings.json] --secret [The OAuth2 client secret for the connector]`

When the environment isn't specified, the command will prompt for it. However, the API definition and API properties file must be provided as part of the command line argument or a settings file. The OAuth2 secret must be provided for a connector using OAuth2. The command will print the connector ID for the newly created custom connector on successful completion. If you're using a settings.json file for the create command, make sure to update it with the new connector ID before you update the newly created connector.

```
Arguments
   --api-def     : Location for the Open API definition JSON document.
   --api-prop    : Location for the API properties JSON document.
   --env -e      : Power Platform environment GUID.
   --icon        : Location for the icon file.
   --script -x   : Location for the script file.
   --pau -u      : Power Platform URL.
   --pav -v      : Power Platform API version.
   --secret -r   : The OAuth2 client secret for the connector.
   --settings -s : A settings file containing required parameters.
                   When a settings file is specified some command 
                   line parameters are ignored.
```
### Update an Existing Custom Connector

Like the `create` operation, an existing custom connector can be updated using the `update` operation. Update a connector by running:
   
`paconn update --api-prop [Path to apiProperties.json] --api-def [Path to apiDefinition.swagger.json]`
   
or
   
`paconn update -e [Power Platform Environment GUID] -c [Connector ID] --api-prop [Path to apiProperties.json] --api-def [Path to apiDefinition.swagger.json] --icon [Path to icon.png] --secret [The OAuth2 client secret for the connector]`
   
or
   
`paconn update -s [Path to settings.json] --secret [The OAuth2 client secret for the connector]`

When environment or connector ID isn't specified, the command will prompt for the missing argument(s). However, the API definition and API properties file must be provided as part of the command-line argument or a settings file. The OAuth2 secret must be provided for a connector using OAuth2. The command will print the updated connector ID on successful completion. If you're using a settings.json file for the update command, make sure the correct environment and connector ID are specified.
  
```
Arguments
   --api-def     : Location for the Open API definition JSON document.
   --api-prop    : Location for the API properties JSON document.
   --cid -c      : The custom connector ID.
   --env -e      : Power Platform environment GUID.
   --icon        : Location for the icon file.
   --script -x   : Location for the script file.
   --pau -u      : Power Platform URL.
   --pav -v      : Power Platform API version.
   --secret -r   : The OAuth2 client secret for the connector.
   --settings -s : A settings file containing required parameters.
                   When a settings file is specified some command 
                   line parameters are ignored.
   ```

### Validate a Swagger JSON

The validate operation takes a swagger file and verifies if it follows all the recommended rules. Validate a swagger file by running:
   
`paconn validate --api-def [Path to apiDefinition.swagger.json]`

or

`paconn validate -s [Path to settings.json]`

The command will print the error, warning, or success message depending on the result of the validation.
  
```
Arguments
   --api-def     : Location for the Open API definition JSON document.
   --pau -u      : Power Platform URL.
   --pav -v      : Power Platform API version.
   --settings -s : A settings file containing required parameters.
                   When a settings file is specified some command 
                   line parameters are ignored.
   ```


### Best Practice

Download all the custom connectors and use git or any other source control system to save the files. If there's an incorrect update, redeploy the connector by rerunning the update command with the correct set of files from the source control system.

Test the custom connector and the settings file in a test environment before deploying in the production environment. Always double check that the environment and connector ID are correct.

## Limitations

The project is limited to creation, update, and download of a custom connector in the Power Automate and Power Apps environment. When an environment isn't specified, only the Power Automate environments are listed to choose from. For a non-custom connector, the swagger file isn't returned.

**Stack Owner Property & apiProperties file:**

Currently, there's a limitation that prevents you from updating your connector's artifacts in your environment using Paconn when the `stackOwner` property is present in your `apiProperties.json` file. As a workaround to this, create two versions of your connector artifacts: The first is the version that's submitted to certification and contains the `stackOwner` property. The second has the `stackOwner` property omitted to enable updating within your environment. We're working to remove the limitation and will update this section once complete. 

## Reporting issues and feedback

If you encounter any bugs with the tool, submit an issue in the [Issues](https://github.com/Microsoft/PowerPlatformConnectors/issues) section of our GitHub repo.

If you believe you have found a security vulnerability that meets [Microsoft's definition of a security vulnerability](/previous-versions/tn-archive/cc751383%28v=technet.10%29), submit a [report to MSRC](https://msrc.microsoft.com/create-report). More information can be found at [MSRC frequently asked questions on reporting](https://www.microsoft.com/msrc/faqs-report-an-issue).

[!INCLUDE[feedback](../includes/feedback.md)]
