---
title: Custom connectors overview
description: Overview about creating custom connectors for supporting and expanding integration scenarios.
author: jopanchal
contributors:
  - jopanchal
  - v-aangie
  - sunaysv
ms.author: jopanchal
ms.reviewer: angieandrews
ms.service: connectors
ms.workload: connectors
ms.topic: overview
ms.date: 01/09/2023
---

# Custom connectors

While [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps), [Microsoft Power Automate](https://flow.microsoft.com), and
[Microsoft Power Apps](https://powerapps.microsoft.com) offer over [900 connectors](/connectors/connector-reference/) to connect to Microsoft and verified services, you may want to communicate with services that aren't available as prebuilt connectors. Custom connectors address this scenario by allowing you to create (and even share) a connector with its own triggers and actions.

<p align="center">
  <img src="./media/index/intro.png" alt="Screenshot of custom connectors - overview.">
</p>

## Lifecycle

<p align="center">
  <img src="./media/index/authoring-steps.png" alt="Screenshot of custom connectors - lifecycle.">
</p>

### 1. Build your API

A custom connector is a wrapper around a REST API (Logic Apps also supports SOAP APIs) that allows Logic Apps,
Power Automate, or Power Apps to communicate with that REST or SOAP API. These APIs can be:

* Public (visible on the public internet) such as [Spotify](https://developer.spotify.com/), [Slack](https://api.slack.com/),
  [Rackspace](http://docs.rackspace.com/), or an API you manage.
* Private (visible only to your network).

For public APIs that you plan to create and manage, consider using one of these Microsoft Azure products:
* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Azure Web Apps](https://azure.microsoft.com/services/app-service/web/)
* [Azure API Apps](https://azure.microsoft.com/services/app-service/api/)

For private APIs, Microsoft offers on-premises data connectivity through an [on-premises data gateway](/flow/gateway-reference).

### 2. Secure your API

Use one of these standard authentication methods for your APIs and connectors ([Azure Active Directory](https://azure.microsoft.com/develop/identity/) is recommended):

   * [Generic OAuth 2.0](https://oauth.net/2/)
   * OAuth 2.0 for specific services, including Azure Active Directory (Azure AD), Dropbox, GitHub, and SalesForce
   * [Basic authentication](https://swagger.io/docs/specification/authentication/basic-authentication/)
   * [API Key](https://swagger.io/docs/specification/authentication/api-keys/)

You can set up Azure AD authentication for your API in the Azure portal so you don't have to implement authentication. Or, you can require and enforce authentication in your API's code. For more information about Azure AD for custom connectors, see [Secure your API and connector with Azure AD](azure-active-directory-authentication.md).

### 3. Describe the API and define the custom connector

Once you have an API with authenticated access, the next thing to do is to describe your API so that Logic Apps, Power Automate, or Power Apps can communicate with your API. The following approaches are supported:

- An OpenAPI definition (formerly known as a Swagger file)
    - [Create a custom connector from an OpenAPI definition](define-openapi-definition.md)
    - [OpenAPI documentation](http://swagger.io/getting-started/)

- A Postman collection
    - [Create a Postman collection](create-postman-collection.md)
    - [Create a custom connector from a Postman collection](define-postman-collection.md)
    - [Postman documentation](https://www.getpostman.com/docs/)

- Start from scratch using the custom connector portal (Power Automate and Power Apps only)
  * [Create a custom connector from scratch](define-blank.md)

OpenAPI definitions and Postman collections use different formats, but both are language-agnostic, machine-readable 
documents that describe your API. You can generate these documents from various tools based on the language and platform used by your API. Behind the scenes, Logic Apps, Power Automate, and Power Apps use OpenAPI to define connectors.

### 4. Use your connector in a Logic App, Power Automate, or Power Apps app

Custom connectors are used the same way Microsoft-managed connectors are used. You'll need to create a connection
to your API in order to use that connection to call any operations that you've exposed in your custom connector.

Connectors created in Power Automate are available in Power Apps. Likewise, connectors created in Power Apps are available
in Power Automate. This isn't true for connectors created in Logic Apps. However, you can reuse the OpenAPI definition or Postman collection to recreate the connector in any of these services. For more information, see the appropriate tutorial:

- [Use a custom connector from a flow](use-custom-connector-flow.md)
- [Use a custom connector from an app](use-custom-connector-powerapps.md)
- [Use a custom connector from a logic app](use-custom-connector-logic-apps.md)

### 5. Share your connector

You can share your connector with users in your organization the same way that you share resources in Logic Apps, Power Automate, or Power Apps. Sharing is optional, but you may have scenarios where you want to share your connectors with other users.

For more information, see [Share custom connectors in your organization](share.md). 

### 6. Certify your connector

If you'd like to share your connector with all users of Logic Apps, Power Automate, and Power Apps, you can submit
your connector for Microsoft certification. Microsoft will review your connector, check for technical and content
compliance, and validate functionality. 

For more information, see [Submit your connectors for Microsoft certification](submit-certification.md).

## Tutorial

The tutorial uses the [Cognitive Services Text Analytics API](https://azure.microsoft.com/services/cognitive-services/text-analytics/).
Microsoft already provides a connector for this API. It is a good example for teaching the custom connector lifecycle and how custom connectors can support unique scenarios.

### Scenario

The connector you'll build exposes the [Text Analytics Sentiment](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9)
operation, which returns the sentiment score (0.000 to 1.000) for the text input.

### Prerequisites

- One of the following subscriptions:
    - An [Azure](https://azure.microsoft.com/get-started/) subscription (Logic Apps)
    - [Power Automate](/flow/sign-up-sign-in)
    - [Power Apps](/powerapps/signup-for-powerapps)

- Basic understanding of how to create Logic Apps, Power Automate flows, or Power Apps.

- API key for the Cognitive Services Text Analytics API.

### Get an API key

The Text Analytics API uses an *API key* to authenticate users. When a user creates a connection to the API through a custom connector, the user specifies the value of this key. To get an API key:

- [Request an API key](https://azure.microsoft.com/try/cognitive-services/?api=text-analytics) to try out the API. This doesn't require an Azure subscription.
- [Add the Text Analytics API](/azure/cognitive-services/cognitive-services-apis-create-account) to your Azure subscription. Once you have the API resource in your subscription, get the API key from the **Keys** section:

    ![Get API key](./media/index/api-key.png)

### Start the tutorial

- If you're using Logic Apps, see:
    - [Create an Azure Logic Apps custom connector](create-logic-apps-connector.md)

- If you're using Power Automate or Power Apps, see:
    - [Create a custom connector from an OpenAPI definition](define-openapi-definition.md)
    - [Create a custom connector from a Postman collection](define-postman-collection.md)
    - [Create a custom connector from scratch](define-blank.md)

## Advanced guidance

The tutorials and video in this section will give you the required insight to leverage Power Platform connectors as part of your implementations.

### Tutorials

The following tutorials provide more detail for specific custom connector scenarios:

- [Extend an OpenAPI definition](openapi-extensions.md)
- [Create a Postman collection for a custom connector](create-postman-collection.md)
- [Authenticate with Azure Active Directory](azure-active-directory-authentication.md)
- [Create a connector for a web API](create-web-api-connector.md)
- [Use a webhook as a trigger](create-webhook-trigger.md)
- [Create a Logic Apps SOAP API connector](create-register-logic-apps-soap-connector.md)

### Video

The following 45 minute video shows you how Power Platform connectors work. It also demonstrates how to create simple and advanced custom connectors.<br/>
<br/>
> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE5cmnM]

### See also

[List of all connectors](/connectors/connector-reference#list-of-connectors)

[!INCLUDE[feedback](../includes/feedback.md)]