---
title: Create a custom connector for Azure AD–protected Azure function apps
description: Shows how to use Azure Functions to build a REST API, enable Azure AD authentication, and then make it available to Power Apps as a custom connector.
author: devkeydet
services: connectors
documentationcenter:
ms.service: connectors
ms.workload: connectors
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: marcsc
ms.reviewer: angieandrews
---

# Create a custom connector for Azure AD–protected Azure function apps

A key principle with Microsoft Power Apps connectors that use Azure Active Directory (Azure AD) for authentication is that they don't provide users with access to any data that the user doesn't already have access to. This is because the API call to the Azure AD&ndash;protected service executes under the user identity used to sign in to the connector. Therefore, the target service maintains responsibility for enforcing what's permitted for the authenticated user.

This tutorial shows you how to use [Azure Functions](/azure/azure-functions) to build a REST API, enable Azure AD authentication, and then make it available to Power Apps as a [custom connector](/connectors/custom-connectors).

## Create Azure function apps by using Visual Studio Code

With Azure Functions, you have many options to consider, including [hosting options](/azure/azure-functions/functions-scale), [language choice](/azure/azure-functions/supported-languages), and authoring options such as using the [Azure portal](/azure/azure-functions/functions-create-first-azure-function), [Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-csharp), or [Visual Studio](/azure/azure-functions/functions-create-your-first-function-visual-studio). For this tutorial, we'll use C# and Visual Studio Code. To complete this tutorial, you must first complete the [Quickstart: Create a C# function in Azure using Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-csharp) tutorial. Make note of the name you provided for your function app in the [Create the function app in Azure](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-csharp#publish-the-project-to-azure) step.

>[!IMPORTANT]
> Don't complete the [Clean up resources](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-csharp#clean-up-resources) section in the Quickstart. You'll need to keep all your resources in place to build on what you've already done.

## Protect calls to Azure function apps by using Azure AD

1. Find your function app in the [Azure portal](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Web%2Fsites/kind/functionapp). Select the name of your function app in the list.

    <kbd>![List of function apps in the Azure portal.](media/create-custom-connector-aad-protected-azure-functions/function-app-list.png)

1. Select the **Platform features** tab.

    <kbd>![The Platform features tab highlighted on the Function Apps blade in the Azure portal.](media/create-custom-connector-aad-protected-azure-functions/use-aadhttpclient-enterpriseapi-secure-function-platform-features.png)

1. From the **Networking** group, select the **Authentication / Authorization** link.

    <kbd>![The Authentication / Authorization link highlighted on the Function Apps blade in the Azure portal.](media/create-custom-connector-aad-protected-azure-functions/use-aadhttpclient-enterpriseapi-secure-function-authn-authz.png)

1. On the **Authentication / Authorization** blade, turn on the **App Service Authentication** toggle.

    <kbd>![The On option for the App Service Authentication toggle highlighted in the function app authentication settings in the Azure portal.](media/create-custom-connector-aad-protected-azure-functions/use-aadhttpclient-enterpriseapi-secure-function-authn-on.png)

1. In the **Action to take when request not authenticated** dropdown, change the value to **Log in with Azure Active Directory**. This setting ensures that anonymous requests to the API won't be allowed.

    <kbd>![The 'Log in with Azure Active Directory' option highlighted in the 'Action to take when request not authenticated' dropdown list on the function app authentication settings blade.](media/create-custom-connector-aad-protected-azure-functions/use-aadhttpclient-enterpriseapi-secure-function-aad-login.png)

1. Next, in the list of authentication providers, select **Azure Active Directory**.

    <kbd>!['Azure Active Directory' highlighted in the list of authentication providers for a function app.](media/create-custom-connector-aad-protected-azure-functions/use-aadhttpclient-enterpriseapi-secure-function-aad.png)

    <a name="create-new-app"></a>

1. On the **Azure Active Directory Settings** blade, set the **Management mode** option to **Express**. Set the second **Management mode** option to **Create New AD App**.

    <kbd>![Azure Active Directory Settings blade opened for a function app in the Azure portal.](media/create-custom-connector-aad-protected-azure-functions/use-aadhttpclient-enterpriseapi-secure-function-aad-settings.png)

    >[!IMPORTANT]
    >Before you continue, copy the value in the **Create App** field and paste it somewhere for later. This value represents the name of the Azure AD application that you'll use to secure the API. You'll use this value later when you configure your custom connector.

1. Select **OK** to confirm your selection.

1. Back on the **Authentication / Authorization** blade, select **Save** to update the function app authentication and authorization settings.

    <kbd>![The Save button highlighted on the Authentication / Authorization blade for a function app in the Azure portal.](media/create-custom-connector-aad-protected-azure-functions/use-aadhttpclient-enterpriseapi-secure-function-save.png)

1. After saving, select **Azure Active Directory** in the **Authentication Providers** section.

    <kbd>![Azure Active Directory Provider.](media/create-custom-connector-aad-protected-azure-functions/aad-provider.png)

1. Select the **Azure AD App**, and then copy the **Client Id** value and paste it somewhere for use later.

     <kbd>![Client Id page.](media/create-custom-connector-aad-protected-azure-functions/azfunc-app-client-id.png)

1. Confirm that the API is correctly secured by opening a new browser window in private mode and going to the API. The URL for your function app can be found in the **Overview** section of the **Function Apps** blade. If the authentication settings have been applied correctly, you should be redirected to the Azure AD sign-in page.

    <kbd>![Azure AD sign-in page.](media/create-custom-connector-aad-protected-azure-functions/use-aadhttpclient-enterpriseapi-secure-function-aad-login-page.png)

## Create a custom connector for your Azure function app

To create a custom connector that uses Azure AD authentication, you must create an Azure AD app registration to secure the custom connector and acquire delegated access to the Azure function app protected by the Azure AD app registration created in the [Protect calls to Azure function apps by using Azure AD](#protect-calls-to-azure-function-apps-by-using-azure-ad) section.

### Create an app registration for your custom connector in Azure AD

First, create an Azure AD application for your custom connector. This is required to grant the custom connector permission to call your Azure function app.

1. Go to the [App registrations](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) page in the Azure portal.

    <kbd>![Azure AD registrations page in the Azure portal.](media/create-custom-connector-aad-protected-azure-functions/app-registrations-blade.png)

1. In the list of registered applications, select **New registration**.

    <kbd>![New registration button.](media/create-custom-connector-aad-protected-azure-functions/azure-active-directory-authenticationadd-app-btn.png)

1. Enter a name for your application, select the supported account types and platform configuration (optional), and then select **Register**.

    <kbd>![Under Supported account types, select Accounts in this organizational directory only. Under Platform configuration, select Web API.](media/create-custom-connector-aad-protected-azure-functions/newapplication.png)

    > [!NOTE]
    > To learn more about app registration options, go to [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app).

1. After selecting **Register**, you'll be shown **API permissions** for your **App registration**.

    <kbd>![API permissions screen.](media/create-custom-connector-aad-protected-azure-functions/api-permissions.png)

1. Select **Add a permission**.

    <kbd>![Add a permission button.](media/create-custom-connector-aad-protected-azure-functions/add-permission.png)

1. Select the **APIs my organization uses** tab, and then search for the app name from [step 7](#create-new-app) in the [Protect calls to Azure function apps by using Azure AD](#protect-calls-to-azure-function-apps-by-using-azure-ad) section. When you find it, select the name of the app.

    > [!IMPORTANT]
    > Your app name will be different than what's in the screenshot.

    <kbd>![Azure function app name.](media/create-custom-connector-aad-protected-azure-functions/azfunc-app.png)

1. Select the **user_impersonation** checkbox, and then select **Add permissions**.

    <kbd>![Add user_impersonation permission.](media/create-custom-connector-aad-protected-azure-functions/user-impersonation.png)

    > [!NOTE]
    > To learn more about permissions, go to [Permissions and consent in the Microsoft identity platform endpoint](/azure/active-directory/develop/v2-permissions-and-consent).

1. Select **Certificates & secrets**, and then select **New client secret**.

    <kbd>![Certificates & secrets.](media/create-custom-connector-aad-protected-azure-functions/certificates-and-secrets.png)

1. Enter a description for your secret, select an expiration period, and then select **Add**.

    <kbd>![Add a client secret.](media/create-custom-connector-aad-protected-azure-functions/add-client-secret.png)

1. Your new secret is displayed. Copy and paste the value somewhere. You'll need it later.

    <kbd>![Screenshot that shows the new secret in the Client secrets section.](media/create-custom-connector-aad-protected-azure-functions/secret.png)

    <a name="conn-clientid"></a>

1. Select **Overview**, and then copy and paste the value of the **Application (client) ID** somewhere. You'll need it later.

    <kbd>![Screenshot that shows the Application (client) I D value.](media/create-custom-connector-aad-protected-azure-functions/copy-conn-client-id.png)

Keep this page open so you can come back to it. There is one more step in the Azure portal, but first you create a custom connector.

### Create a custom connector

Now that the Azure AD application is configured, you create the custom connector. To create a custom connector, you must describe the API you want to connect to so that the connector understands the API's operations and data structures. When building Azure function apps for custom connectors, you'll often iteratively build your connector by repeating the following steps:

1. Create, test, and debug the Azure function app locally by using Visual Studio Code.
1. Deploy your function app to Azure.
1. Update the custom connector configuration to reflect changes in your function app.

<a name="postman-links"></a>

[Postman](https://www.getpostman.com/apps) is a popular tool to test web APIs. It's often used to test Azure function apps. In this topic, you'll create a custom connector from a Postman collection. The Postman collection will be provided for you.

>[!NOTE]
>Go to the [Create a Postman collection for a custom connector](/connectors/custom-connectors/create-postman-collection) topic to learn how to create your own. Using Postman to create a custom connector is just one option. Go to the [Describe the API and define the custom connector](/connectors/custom-connectors/#3-describe-the-api-and-define-the-custom-connector) topic to learn about others.

1. In a new browser tab, sign in to [Power Apps](https://make.powerapps.com) or [Power Automate](https://us.flow.microsoft.com/).

    <a name="copy-connector-json"></a>

1. Copy the code below. Using your favorite text editor, create a new file, and paste the code as the contents of the file. Save the file. You can name it whatever you prefer, but remember the name (for example, **tutorial-custom-connector.json**). You'll need to use this file in the next steps.

    ```json
    {
        "id": "c4b5deba-f97b-47d0-82a5-a2b32561fb01",
        "name": "Custom Connector",
        "description": null,
        "auth": null,
        "events": null,
        "variables": [],
        "order": [
            "374365a1-ede5-4ead-8068-d878085dad26"
        ],
        "folders_order": [],
        "protocolProfileBehavior": {},
        "folders": [],
        "requests": [
            {
                "id": "374365a1-ede5-4ead-8068-d878085dad26",
                "name": "Hello",
                "url": "http://localhost:7071/api/Hello",
                "description": "",
                "data": [],
                "dataOptions": {
                    "raw": {
                        "language": "json"
                    }
                },
                "dataMode": "raw",
                "headerData": [
                    {
                        "key": "Content-Type",
                        "name": "Content-Type",
                        "value": "application/json",
                        "description": "",
                        "type": "text"
                    }
                ],
                "method": "POST",
                "pathVariableData": [],
                "queryParams": [],
                "auth": null,
                "events": null,
                "folder": null,
                "responses": [
                    {
                        "id": "46baba58-7b85-4a2e-8c7d-303080e08ba9",
                        "name": "Hello",
                        "status": null,
                        "mime": null,
                        "language": "plain",
                        "text": "Hello, Marc. This HTTP triggered function executed successfully.",
                        "responseCode": {
                            "code": 200,
                            "name": "OK"
                        },
                        "requestObject": {
                            "data": [],
                            "dataMode": "raw",
                            "dataOptions": {
                                "raw": {
                                    "language": "json"
                                }
                            },
                            "headerData": [
                                {
                                    "key": "Content-Type",
                                    "name": "Content-Type",
                                    "value": "application/json",
                                    "description": "",
                                    "type": "text"
                                }
                            ],
                            "method": "POST",
                            "pathVariableData": [],
                            "queryParams": [],
                            "url": "http://localhost:7071/api/Hello",
                            "rawModeData": "{\n\t\"name\": \"Marc\"\n}"
                        },
                        "headers": [
                            {
                                "key": "Date",
                                "value": "Wed, 04 Mar 2020 22:32:06 GMT"
                            },
                            {
                                "key": "Content-Type",
                                "value": "text/plain; charset=utf-8"
                            },
                            {
                                "key": "Server",
                                "value": "Kestrel"
                            },
                            {
                                "key": "Transfer-Encoding",
                                "value": "chunked"
                            }
                        ],
                        "cookies": null,
                        "request": "374365a1-ede5-4ead-8068-d878085dad26",
                        "collection": "c4b5deba-f97b-47d0-82a5-a2b32561fb01"
                    }
                ],
                "rawModeData": "{\n\t\"name\": \"Marc\"\n}",
                "headers": "Content-Type: application/json\n",
                "pathVariables": {}
            }
        ]
    }
    ```

1. On the left pane, expand **Data**, and then select **Custom Connectors**.

    <kbd>![Screenshot that shows Data and Custom Connectors on the left pane.](media/create-custom-connector-aad-protected-azure-functions/data-custom-connectors.png)

1. In the upper-right corner, select **New custom connector**, and then select **Import a Postman collection**.

    <kbd>![Import a Postman collection.](media/create-custom-connector-aad-protected-azure-functions/import-postman-collection.png)

1. Enter a name for the connector, select **Import**, browse to the file you created in [step 2](#copy-connector-json), and then select **Continue**.

    <kbd>![Name and file location.](media/create-custom-connector-aad-protected-azure-functions/name-file-location.png)

1. The **General** page opens. Change the **Scheme** to **HTTPS**, replace the **Host** value with the domain of your function app, and then advance the wizard to the **Security** page.

    <kbd>![General information settings.](media/create-custom-connector-aad-protected-azure-functions/connector-general-page.png)

1. On the **Security** page, provide Azure AD information for the application:

    - For **Client id**, enter the **Application (client) ID** value you copied in [step 11](#conn-clientid) in the [Create an app registration for your custom connector in Azure AD](#create-an-app-registration-for-your-custom-connector-in-azure-ad).
    - For **Client secret**, enter the **secret** value you copied in [step 11](#conn-clientid) in the [Create an app registration for your custom connector in Azure AD](#create-an-app-registration-for-your-custom-connector-in-azure-ad).

    - For **Resource URL**, enter the URL of your function app. It will be in the format of `https://[function-app-name].azurewebsites.net`.

    <kbd>![OAuth settings.](media/create-custom-connector-aad-protected-azure-functions/oauth-settings.png)

    After entering security information, select **Update connector** to create the custom connector.

1. On the **Security** page, the **Redirect URL** field is now populated. Copy this URL so you can use it in the next section of this tutorial.

    >[!NOTE]
    >You might need to scroll down to see the redirect URL.

    <kbd>![Screenshot that shows the Redirect U R L field.](media/create-custom-connector-aad-protected-azure-functions/copy-resource-url.png)

1. In another browser tab, return to the app registration you created in the [Create an app registration for your custom connector in Azure AD](#create-an-app-registration-for-your-custom-connector-in-azure-ad) section. Make sure you're in the **Overview** section, and then select **Add a Redirect URI**.

    <kbd>![Screenshot that shows the Add a Redirect U R I button.](media/create-custom-connector-aad-protected-azure-functions/add-redirect-uri.png)

1. Select **Add a platform**.

    <kbd>![Add a platform button.](media/create-custom-connector-aad-protected-azure-functions/add-a-platform.png)

1. On the **Configure platforms** pane, select **Web**.

    <kbd>![Screenshot that shows Web on the Configure platforms pane.](media/create-custom-connector-aad-protected-azure-functions/configure-platforms.png)

1. On the **Configure Web** pane, paste the **Redirect URL** value from step *7*, and then select **Configure**.

    <kbd>![Screenshot that shows the Configure Web pane.](media/create-custom-connector-aad-protected-azure-functions/configure-web.png)

1. Return to the browser tab containing the custom connector configuration. You should still be on the **Security** page in the wizard.

1. Advance to the **Definition** page in the wizard by selecting the word **Definition**, and review.

     <kbd>![Screenshot that shows the Definition button.](media/create-custom-connector-aad-protected-azure-functions/connector-definition.png)

    >[!NOTE]
    >The definition comes from the Postman collection you imported. If you changed any of the code in the [Quickstart: Create a C# function in Azure using Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-csharp) tutorial, the import file might not match the shape of your API. You'll either need to create a new Postman collection or manually adjust the action in the custom connector. To create a new Postman collection, use the links shared earlier in this tutorial [here](#postman-links). To manually adjust the action, you can review the [Create the connector definition](/connectors/custom-connectors/define-blank#create-the-connector-definition) section of the [Create a custom connector from scratch](/connectors/custom-connectors/define-blank) tutorial.

## Test the connector

Now that you've created the connector, test it to make sure it's working properly. Testing is currently available only in Power Automate and Power Apps.

1. Advance to the **Test** page in the wizard by selecting the word **Test**.

     <kbd>![Test page.](media/create-custom-connector-aad-protected-azure-functions/connector-test.png)

1. On the **Test** page, select **New connection**.

    <kbd>![Screenshot that shows the New connection button.](media/create-custom-connector-aad-protected-azure-functions/new-connection.png)

1. Select **Create**, and then sign in with your Azure AD user.

    <kbd>![Screenshot that shows the Create button and the sign-in screen.](media/create-custom-connector-aad-protected-azure-functions/create-connection.png)

1. Return to the **Test** page, and do one of the following:.

    * In Power Automate, you're taken back to the **Test** page. Select the refresh icon to make sure the connection information is updated.

        <kbd>![Refresh connection.](media/create-custom-connector-aad-protected-azure-functions/refresh-connection.png)

    * In Power Apps, you're taken to the list of connections available in the current environment. On the left pane, select **Custom Connectors**. Find your custom connector, and then select the edit icon.

        <kbd>![Screenshot that shows the list of custom connectors.](media/create-custom-connector-aad-protected-azure-functions/edit-connector.png)

1. Go back to the **Test** page, enter a value for the **name** field, and then select **Test operation**.

    <kbd>![Test operation.](media/create-custom-connector-aad-protected-azure-functions/test-operation.png)

1. Review the **Request** and **Response**.

    <kbd>![Screenshot that shows the request fields of Url, Method, Headers, ane Body.](media/create-custom-connector-aad-protected-azure-functions/test-request.png)

    <kbd>![Screenshot that shows the response fields of Status, Headers, and Body.](media/create-custom-connector-aad-protected-azure-functions/test-response.png)

## Next steps

Now that you've created a custom connector and defined its behaviors, you can use the connector.

* [Use a custom connector from a flow](use-custom-connector-flow.md)
* [Use a custom connector from an app](use-custom-connector-powerapps.md)
* [Use a custom connector from a logic app](use-custom-connector-logic-apps.md)

[!INCLUDE[feedback](../includes/feedback.md)]
