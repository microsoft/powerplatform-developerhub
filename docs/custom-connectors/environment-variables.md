---
title: Use environment variables in solution custom connectors (preview) | MicrosoftDocs
description: Use environment variables in solution custom connectors to update key custom connector properties. 
Keywords: connectors, custom connectors, canvas app, flow
author: schabungbam
ms.author: sameerch
contributors:
  - sameerch
  - v-aangie
ms.date: 06/08/2022
ms.service: connectors
ms.topic: conceptual
ms.reviewer: angieandrews
---

# Use environment variables in solution custom connectors (preview)

Applications often require different configuration settings or input parameters when deployed to different environments. Environment variables store the parameter keys and values, which can then serve as input to various other application objects. You can also use environment variables in solution custom connectors to update key custom connector properties such as `Host`, `Base URL`, `Client ID`, `Client Secret`, `Login Url`, and `Refresh Url`. For a detailed tutorial, you can read [this blog](https://powerautomate.microsoft.com/blog/environment-variables-in-custom-connectors/).

## Use an environment variable in a custom connector

When a custom connector is created or updated, the values of environment variables will be used to create the custom connector. The custom connector uses the value of the environment variables during save time. When an environment variable is updated, custom connectors need to be resaved to use the updated environment variable value.

1. Sign in to [Power Apps](https://make.powerapps.com) or [Power Automate](https://flow.microsoft.com).

1. Select a solution from the list. 

   or

   If you need to create a new custom connector in a solution, go to [Create custom connectors in solutions](CustomConnectorsSolutions.md).

1. Select the newly created or existing solution from the list.

1. Select **Environment variables**.

1. Select **New** > **More** > **Environment variable**.

    > [!div class="mx-imgBorder"]
    > ![Screenshot of the new environment variable menu.](media/environment-variables/new-env-var.png "New environment variable menu")

1. Enter the environment variable **Name**, which contains the publisher Id prefix. (Don't use the name in the **Display name** field.)

    The following example uses **SharePoint Site URL**. You can create other environment variable values for other settings like OAUTH Client ID, Resource, and others.

    > [!div class="mx-imgBorder"]
    > ![Screenshot of the environment variable name.](media/environment-variables/display-name.png "Environment variable name")

    Environment variables can use the following syntax in custom connector fields:<br/> `@environmentVariables("{{environmentVariableName}}")`

    **Example**<br/>`@environmentVariables("cr49f_SharePointSiteURL_7weem")`

    > [!div class="mx-imgBorder"]
    > ![Screenshot of the Environment variables screen.](media/environment-variables/env-variables.png "Environment variables screen")

1. (Optional) To use the values from environment variables in the **Host** and **Base URL** fields, do the following:

    1. Select **New** > **Automation** > **Custom connector**.

        > [!div class="mx-imgBorder"]
        > ![Screenshot of the new custom connector menu.](media/environment-variables/new-automation.png "New custom connector menu")

    1. On the **General** tab, enter the environment variable syntax to refer to an environment variable.

        > [!div class="mx-imgBorder"]
        > ![Screenshot of the General tab.](media/environment-variables/host-base.png "General tab")

1. (Optional) To use the values from environment variables in any of the fields on the **Security** tab, do the following:

    1. Select **New** > **Automation** > **Custom connector**.
    
    1. On the **Security** tab, enter the environment variable syntax to refer to an environment variable.

    To learn more, go to [Specify authentication type](define-blank.md#step-2-specify-authentication-type):

    > [!div class="mx-imgBorder"]
    > ![Screenshot of the Security tab.](media/environment-variables/security-tab.png "Security tab")

    >[!Note]
    >Environment variables with the data type **Secret** can now be used in custom connectors. You need to configure the Azure Key Vault by using the [steps outlined here](/powerapps/maker/data-platform/environmentvariables#use-azure-key-vault-secrets). In the security configuration UI, the value is masked. You'll need to use the following syntax: `@environmentVariables("{{environmentVariableName}}")`

    > [!IMPORTANT]
    > An environment variable created for Client Secret with the **Text** data type isn't secure. These values aren't encrypted. The recommendation is to use Azure Key Vault.

1. (Optional) On the **Definition** tab, add any necessary actions, triggers, or policies. Currently, environment variables aren't supported in actions, triggers, or policies.

## Use new values for environment variables while importing solutions

If you'd like to use new values for environment variables while importing solutions, you can remove the value from your solution before exporting the solution. This ensures that the existing value will remain in your development environment, but won't get exported in the solution. This approach allows a new value to be provided while importing the solution into other environments.

**To use new values for environment variables**

1. [Export the solution](/powerapps/maker/data-platform/export-solutions). This step is where you'll remove the value, as mentioned in the previous paragraph.

1. [Import the solution](/powerapps/maker/data-platform/import-update-export-solutions) to a new environment.

   You won't be prompted for new values during solution import if the environment variables already have a default value or any value is present. This happens when values are part of your solution or are already present in the target environment.

   To learn more, go to [How do I remove a value from an environment variable?](/powerapps/maker/data-platform/EnvironmentVariables#how-do-i-remove-a-value-from-an-environment-variable)

[!INCLUDE[feedback](../includes/feedback.md)]