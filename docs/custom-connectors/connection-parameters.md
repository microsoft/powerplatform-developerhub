---
title: Specifying connection parameters | Microsoft Docs
description: Specify how the connection will be created.
author: revanth1996
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: angieandrews
ms.reviewer: angieandrews
---

# Connection Parameters

Before using any connector in Azure Logic Apps, Microsoft Power Automate, or Microsoft Power Apps, the user needs to create a connection by authenticating to the backend service. This information on how the authentication with the backend service happens needs to be defined while creating the connector, as part of Connection Parameters. When creating a custom connector via the User Interface on the Power Automate/Power Apps portal, in the **Security** tab, you can specify which type of authentication you want the use when creating the connection.

## **Authentication types**

The different types of authentication that are currently supported are:
* No authentication
* Basic authentication
* Api Key based authentication
* Oauth 2.0

### **No authentication**

The user will not need any authentication to create a connection to the connector. Any anonymous user can use your connector in this case.

### **Basic authentication**

This is the simplest type of authentication, where the user just has to provide the username and password to create the connection.

![Screenshot that shows the fields for basic authentication.](./media/connection-parameters/basic-auth.png)

The values you enter under *Parameter label* will be the names for the "username" and "password" fields that the user will see while creating the connection. For the example shown above, this is what the user will see when creating a connection:

![Screenshot that shows hidden values in the authentication fields.](./media/connection-parameters/basic-auth-connectionUI.png)

### **Api Key based authentication**

The user will need to provide the API key while creating the connection. The *Parameter location* field gives you the option to send the API key to your service in headers or a query string when the request is made.

![API Key Authentication](./media/connection-parameters/api-key-auth.png)

The value you enter under *Parameter label* will be the name of the field the user will see. For example, the following image will be shown to the user at connection creation time. When a request is made to your service, a header with name 'ApiKey' and value as entered by the user will be added to the request.
 
![Api Key Authentication](./media/connection-parameters/api-key-auth-connectionUI.png)

### **Oauth 2.0**

This is the most frequently used type, which uses the Oauth 2 authentication framework to authenticate with the service. Before using this authentication type, you'll need to register your application with the service so that it can receive access tokens for the users. For example, [Register the application in Azure AD](/azure/active-directory/develop/v1-oauth2-on-behalf-of-flow#register-the-application-and-service-in-azure-ad) shows how to register an application with the Azure Active Directory service. The following information needs to be provided:

* **Identity Provider**: The user interface currently supports multiple identity providers. The general ones are *Generic Oauth 2*, which can be used for any service, and *Azure Active Directory*, which can be used for all Azure services. A few specific identity providers like Facebook, GitHub, Google, and so on, are also supported.

* **Client id**: The client ID of the application you have registered with the service.

* **Client secret**: The client secret of the application you have registered with the service.

* **Authorization Url**: The API authorization endpoint to authenticate with the service.

* **Token Url**: The API endpoint to get the access token after the authorization has been done.

* **Refresh Url**: The API endpoint to refresh the access token once it has expired.

> [!Note] 
> Currently, client credentials grant type is not supported by custom connectors.

The following example demonstrates using the Generic Oauth 2 identity provider to authenticate with the Active Directory service.

![Oauth 2.0](./media/connection-parameters/oauth.png)

During the connection creation process, the user will be asked to enter the credentials for login to the service. These credentials will be used by the application to get an authorization token. For every request, this authorization token will be sent to your service through the Authorization header.

[!INCLUDE[feedback](../includes/feedback.md)]