---
title: Authenticate your API and connector with Azure Active Directory - Azure AD | Microsoft Docs
description: Learn how to add security and authentication to your API and connector with Azure Active Directory (Azure AD).
author: sunaysv
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: sunayv
ms.reviewer: angieandrews
---

# Use Azure Active Directory with a custom connector in Power Automate

[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview) enables you to manage the components of a solution on Azure&mdash;components like databases, virtual machines, and web apps. This tutorial demonstrates how to enable authentication in Azure Active Directory (Azure AD), register one of the Resource Manager APIs as a custom connector, and then connect to it in Power Automate. You can also create the custom connector for Power Apps or Azure Logic Apps.

The process you follow in this tutorial can be used to access any RESTful API that's authenticated by using Azure AD.

## Prerequisites

- An [Azure subscription](https://azure.microsoft.com/free/)
- A [Power Automate account](https://flow.microsoft.com)
- The [sample OpenAPI file](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json) used in this tutorial

## Enable authentication in Azure AD

First, you create an Azure AD application that performs the authentication when calling the Resource Manager API endpoint.

1. Sign in to the [Azure portal](https://portal.azure.com). If you have more than one Azure AD tenant, make sure you're signed in to the correct directory by verifying that your user name appears in the upper-right corner.

    ![User name.](./media/azure-active-directory-authentication/current-user.png)

1. On the left pane, select **All services**. In the **Filter** box, enter **Azure Active Directory**, and then select **Azure Active Directory**.

    ![Azure Active Directory.](./media/azure-active-directory-authentication/azureaad.png)

    The **Azure Active Directory** blade opens.  

1. On the left pane of the **Azure Active Directory** blade, select **App registrations**.

    ![App registrations.](./media/azure-active-directory-authentication/azureapplication.png)

1. In the list of registered applications, select **New application registration**.

    ![Add button.](./media/azure-active-directory-authentication/add-app-btn.png)   

1. Enter a name for your application, and leave **Application Type** as **Web app / API**. For **Sign-on URL**, enter an appropriate value for your organization, such as `https://login.windows.net`. Select **Create**.  

    ![Create a new application registration.](./media/azure-active-directory-authentication/newapplication.png)

1. Copy the **Application ID**, because you'll need it later.

1. The **Settings** blade should have opened as well. If it didn't, select **Settings**.

    ![Settings button.](./media/azure-active-directory-authentication/settings-btn.png)

1. On the left pane of the **Settings** blade, select **Required permissions**. On the **Required permissions** blade, select **Add**.

    ![Required permissions.](./media/azure-active-directory-authentication/permissions.png)

    The **Add API access** blade opens.

1. On the **Add API access** blade, select **Select an API**, and then select **Azure Service Management API** > **Select**.

    ![Select an API.](./media/azure-active-directory-authentication/permissions2.png)

1. Under **Delegated permissions**, select **Access Azure Service Management as organization users** > **Select**.

    ![Delegated permissions.](./media/azure-active-directory-authentication/permissions3.png)

1. On the **Add API access** blade, select **Done**.

1. Back on the **Settings** blade, select **Keys**. On the **Keys** blade, enter a description for your key, select an expiration period, and then select **Save**. 

1. Your new key is displayed. Copy the key value, because you'll need it later.

    ![Create a key.](./media/azure-active-directory-authentication/configurekeys.png)

There's one more step to complete in the Azure portal, but first you create a custom connector.

## Create a custom connector

Now that the Azure AD application is configured, you create the custom connector.

1. In the [Power Automate web app](https://flow.microsoft.com/), select **Settings** (the gear icon) in the upper-right corner of the page, and then select **Custom Connectors**.

	![Find custom connectors.](./media/azure-active-directory-authentication/finding-custom-apis.png)  

1. Select **Create custom connector** > **Import an OpenAPI file**.

	![Create a custom connector.](./media/azure-active-directory-authentication/create-custom-connector.png) 

1. Enter a name for the connector, browse to where you downloaded the [sample Resource Manager OpenAPI file](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json), and then select **Continue**.

	![Name and file location.](./media/azure-active-directory-authentication/name-file-location.png) 

1. The **General** page opens. Leave the default values as they appear, and then select the **Security** page.

1. On the **Security** page, enter Azure AD information for the application:

   - Under **Client id**, enter the Azure AD application ID value you copied earlier.

   - For **Client secret**, use the value you copied earlier.

   - For **Resource URL**, enter `https://management.core.windows.net/`. Be sure to include the resource URL exactly as written, including the trailing slash.

   ![OAuth settings.](./media/azure-active-directory-authentication/oauth-settings.png)

	After entering security information, select the check mark (**&#x2713;**) next to the flow name at the top of the page to create the custom connector.

1. On the **Security** page, the **Redirect URL** field is now populated. Copy this URL so you can use it in the next section of this tutorial.

1. Your custom connector is now displayed under **Custom Connectors**.

	![Available APIs.](./media/azure-active-directory-authentication/list-custom-apis.png)  

1. Now that the custom connector has been registered, create a connection to the custom connector so that it can be used in your apps and flows. Select the plus sign (**+**) to the right of the name of your custom connector, and then complete the sign-in screen.

>[!Note]
>The sample OpenAPI doesn't define the full set of Resource Manager operations; currently, it only contains the [List all subscriptions](/rest/api/resources/subscriptions)<!--note from editor: The MSDN link resolves to this page on docs.--> operation. You can edit this OpenAPI file, or create another OpenAPI file by using the [online OpenAPI editor](http://editor.swagger.io/).

## Set the reply URL in Azure

On the **Settings** blade, select **Reply URLs**. In the list of URLs, add the value you copied from the **Redirect URL** field in the custom connector—for example, `https://msmanaged-na.consent.azure-apim.net/redirect`—<!--note from editor: Is this a good, neutral example to use? I can't tell whether it's free from any PII. Is it even necessary to give an example here?-->and then select **Save**.

![Reply URLs.](./media/azure-active-directory-authentication/reply-urls.png)

## Next steps
<!--note from editor: Suggest using this heading because "See also" sections typically are just a list of related articles, they don't include guidance about what to do like these paragraphs do.-->
For more detailed information about using a custom connector, go to [Use custom connectors in Power Automate](use-custom-connector-flow.md).

To ask questions or make comments about custom connectors, [join our community](https://aka.ms/flow-community).

[!INCLUDE[feedback](../includes/feedback.md)]
