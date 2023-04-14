---
title: Create a custom connector for a web API | Microsoft Docs
description: Build custom connectors from Web APIs for Logic Apps, Power Automate, and Power Apps.
author: sunaysv
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: sunayv
ms.reviewer: angieandrews
---

# Create a custom connector for a web API
<!--note from editor: I don't think we've used "Web API, "web API," "Web App," and "web app" in a perfectly consistent way. There are some cases where it seems obvious that lowercase "web app" is right, but other cases I just don't know. Please see if you can make sense of https://styleguides.azurewebsites.net/Styleguide/Read?id=2696&topicid=27541 and https://styleguides.azurewebsites.net/Styleguide/Read?id=2696&topicid=25375?-->
This tutorial shows you how to start building an ASP.NET Web API, host it on the Azure Web Apps feature of Azure App Service, enable Azure Active Directory (Azure AD) authentication, and then register the ASP.NET Web API in Power Automate. After the API is registered, you can connect to it and call it from your flow. You can also register and call the API from Power Apps or Azure Logic Apps.

## Prerequisites

* [Visual Studio 2013 or later](https://www.visualstudio.com/vs/). This tutorial uses Visual Studio 2015.

* Code for your Web API. If you don't have any, try this tutorial: [Getting Started with ASP.NET Web API 2 (C#)](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).

* An Azure subscription. If you don't have a subscription, you can start with a [free Azure account](https://azure.microsoft.com/free/). Otherwise, sign up for a [Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).

## Create and deploy an ASP.NET web app to Azure

For this tutorial, create a Visual C# ASP.NET web app.

1. Open Visual Studio, and then select **File** > **New Project**.

   1. Expand **Installed**, go to **Templates** > **Visual C#** > **Web**, and then select **ASP.NET Web Application**.

   2. Enter a project name, location, and solution name for your app, and then select **OK**.

   ![Screenshot showing a new Visual C# ASP.NET web application.](./media/create-web-api-connector/visual-studio-new-project-aspnet-web-app.png)

2. In the **New ASP.NET Web Application** box, select the **Web API** template, make sure the **Host in the cloud** checkbox is selected, and then select **Change Authentication**.

   ![Screenshot showing the New ASP.NET Web Application dialog.](./media/create-web-api-connector/visual-studio-web-api-template.png)

3. Select **No Authentication**, and then select **OK**. You can set up authentication later.

   ![Select No Authentication.](./media/create-web-api-connector/visual-studio-change-authentication.png)

4. When the **New ASP.NET Web Application** box reappears, select **OK**. 

5. In the **Create App Service** box, review the hosting settings described in the following table, make the changes you want, and then select **Create**. 

   An [App Service plan](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) represents a collection of physical resources used for hosting apps in your Azure subscription. [Learn about App Service](/azure/app-service/app-service-web-overview).

   ![Create App Service.](./media/create-web-api-connector/visual-studio-create-app-service.png)


   |                Setting               |          Suggested value       |         Description       |
   |--------------------------------------|--------------------------------|---------------------------|
   | Your Azure work or school account, or your personal Microsoft account |              <em>your-user-account</em>              |   Select your user account.    |
   |                     <strong>Web App Name</strong>                     | <em>custom-web-api-app-name</em> or the default name |   Enter the name for your Web API app, which is used in your app's URL, for example: http://<em>web-api-app-name</em>.        |
   |                     <strong>Subscription</strong>                     |           <em>Azure-subscription-name</em>           |    Select the Azure subscription that you want to use.      |
   |                    <strong>Resource Group</strong>                    |          <em>Azure-resource-group-name</em>          | Select an existing Azure resource group, or—if you haven't already—create a resource group. <p><strong>Note</strong>: An Azure resource group organizes Azure resources in your Azure subscription. |
   |                   <strong>App Service Plan</strong>                   |            <em>App-Service-plan-name</em>            |                                                            Select an existing App Service plan, or—if you haven't already—create a plan.          |

   If you create an App Service Plan, specify the following.

   | Setting | Suggested value | Description | 
   | ------- | --------------- | ----------- | 
   | **Location** | *deployment-region* | Select the region for deploying your app. | 
   | **Size** | *App-Service-plan-size* | Select your plan size, which determines the cost and computing resource capacity for your service plan. | 

   To set up any other resources required by your app, select **Explore additional Azure services**.


   |            Setting             |       Suggested value        |                           Description                            |
   |--------------------------------|------------------------------|------------------------------------------------------------------|
   | <strong>Resource Type</strong> | <em>Azure-resource-type</em> | Select and set up any additional resources required by your app. |


6. After Visual Studio deploys your project, build the code for your app.

## Create an OpenAPI (Swagger) file that describes your Web API

To connect your Web API app to Power Automate, Power Apps, or Logic Apps, you need an [OpenAPI (formerly Swagger) file](http://swagger.io/) that describes your API's operations. You can write your own OpenAPI definition for your API with the [Swagger online editor](http://editor.swagger.io/), but this tutorial uses an open-source tool named [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle/blob/master/README.md).

1. If you haven't already, install the Swashbuckle Nuget package in your Visual Studio project:

   1. In Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**.

   2. In the **Package Manager Console**, go to your app's project directory if you're not there already (run `Set-Location "project-path"`), and run this PowerShell cmdlet: 

      `Install-Package Swashbuckle`

      ![Screenshot showing Swashbuckle installed by using the Package Manager Console.](./media/create-web-api-connector/visual-studio-package-manager-install-swashbuckle.png)

   > [!TIP]
   > If you run your app after installing Swashbuckle, Swashbuckle generates an OpenAPI file at this URL:
   >
   > &nbsp;&nbsp;http://<em>{your-web-api-app-root-URL}</em>/swagger/docs/v1
   > 
   > Swashbuckle also generates a user interface at this URL:<br>
   > 
   > &nbsp;&nbsp;http://<em>{your-web-api-app-root-URL}</em>/swagger

2. When you're ready, publish your Web API app to Azure. To publish from Visual Studio, right-click your web project in Solution Explorer, select **Publish**, and follow the prompts.

   > [!IMPORTANT]
   > If an OpenAPI document contains duplicate operation IDs, it will be invalid. The sample C# template repeats the operation ID, `Values_Get`. 
   > 
   > If you used the sample C# template, you can fix this problem by changing one operation ID instance to `Value_Get`, and republishing.

3. Get the OpenAPI document by browsing to this location: 

   http://<em>{your-web-api-app-root-URL}</em>/swagger/docs/v1

   You can also download a [sample OpenAPI document](https://procsi.blob.core.windows.net/docs/webAPI.json) from this tutorial. Make sure that you remove the comments, which start with `//`<!--note from editor: Edit okay? I assume the quotation marks aren't part of the comment string.-->, before you use the document.

4. Save the content as a JSON file. Depending on your browser, you might have to copy and paste the text into an empty text file.

## Set up Azure AD authentication

You'll now create two Azure AD applications in Azure. For more information on how to do this, go to [Integrating applications with Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).

>[!IMPORTANT] 
>Both apps must be in the same directory.

### First Azure AD application: Securing the Web API

The first Azure AD application is used to secure the Web API. Name it **webAPI**. You can enable Azure AD authentication on your web API by following [these steps](/azure/app-service/app-service-mobile-how-to-configure-active-directory-authentication) with the following values:

- Sign-on URL: `https://login.windows.net`
- Reply URL: `https://<your-root-url>/.auth/login/aad/callback`
- You don't need a client key.
- You don't need to delegate any permissions.
- Copy the application ID, because you'll need it later.

### Second Azure AD application: Securing the custom connector and delegated access

The second Azure AD application is used to secure the custom connector registration and acquire delegated access to the Web API protected by the first application. Name this one **webAPI-customAPI** .

- Sign-on URL: `https://login.windows.net`
- Reply URL: `https://msmanaged-na.consent.azure-apim.net/redirect`
- Add permissions to have delegated access to Web API.
- Copy the application ID, because you'll need it later.
- Generate a client key and copy it, because you'll need it later.

## Add authentication to your Azure web app

1. Sign in to the [Azure portal](https://portal.azure.com), and then find the web app that you deployed in the first section.

2. Select **Settings**, and then select **Authentication / Authorization**.

3. Turn on **App Service Authentication** and then select **Azure Active Directory**. On the next blade, select **Express**.  

4. Select **Select Existing AD App**, and then select the **webAPI** Azure AD application you created earlier.

You should now be able to use Azure AD to authenticate your web app.

## Add the custom connector to Power Automate

1. Modify your OpenAPI to add the `securityDefintions` object and Azure AD authentication used for the web app. The section of your OpenAPI with the **host** property should look like this:

```javascript
// File header should be above here...

"host": "<your-root-url>",
"schemes": [
    "https"		 //Make sure this is https!
],
"securityDefinitions": {
    "AAD": {
        "type": "oauth2",
        "flow": "accessCode",
        "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
        "tokenUrl" : "https://login.windows.net/common/oauth2/token",
        "scopes": {}
    }
},

// The rest of the OpenAPI follows...
```

2. Browse to [Power Automate](https://powerapps.microsoft.com) and add a custom connector as described in [Use custom connectors in Power Automate](use-custom-connector-flow.md).

3. After you've uploaded your OpenAPI, the wizard auto-detects that you're using Azure AD authentication for your Web API.

4. Configure the Azure AD authentication for the custom connector.  

  - **Client ID**: *Client ID of webAPI-CustomAPI*
  - **Secret**: *Client key of webAPI-CustomAPI*
  - **Login URL**: `https://login.windows.net`
  - **ResourceUri**: *Client ID of webAPI*

5. Select **Create** to create a connection to the custom connector.

### See also

[Learn more about Azure Active Directory authentication](azure-active-directory-authentication.md)

[!INCLUDE[feedback](../includes/feedback.md)]