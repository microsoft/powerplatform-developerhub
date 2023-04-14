---
title: Create a custom connector from a Postman collection
description: Learn how to use a Postman collection to create a custom connector for Azure Logic Apps, Power Automate, and Power Apps.
author: sunaysv
contributors:
  - samrat-gavale
  - v-aangie
editor: camydee
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 03/29/2023
ms.author: sagavale
ms.reviewer: angieandrews
---

# Create a custom connector from a Postman collection

> [!Note]
> This topic is part of a tutorial series on creating and using custom connectors in Azure Logic Apps, Power Automate, and Power Apps. Make sure you read the [custom connector overview](index.md) to understand the process.

To create a custom connector, you must describe the API you want to connect to so that the connector understands the API's operations and data structures. In this topic, you create a custom connector using a *Postman collection* that describes the Cognitive Services Text Analytics Sentiment API (our example for this series).

For other ways to describe an API, go to the following topics:

* [Create a custom connector from an OpenAPI definition](define-openapi-definition.md)

* [Create a custom connector from scratch](define-blank.md)

## Prerequisites

* A Postman collection that describes the example API:
    * Download the [Postman collection](https://procsi.blob.core.windows.net/docs/SentimentDemo.postman_collection.json) we created<br/>
    or
    * Complete the topic [Create a Postman collection for a custom connector](create-postman-collection.md)
    *Note that when creating a custom connector, the Postman collection must be no larger than 1 MB.*

* An [API key](index.md#get-an-api-key) for the Cognitive Services Text Analytics API

* One of the following subscriptions:
    * [Azure](https://azure.microsoft.com/get-started/), if you're using Logic Apps
    * [Power Automate](/flow/sign-up-sign-in)
    * [Power Apps](/powerapps/signup-for-powerapps)

* If you are using Logic Apps, first [create an Azure Logic Apps custom connector](create-logic-apps-connector.md)

## Import the Postman collection

You are now ready to work with the Postman collection you created or downloaded. A lot of the required information is contained in the collection. You can also review and update this information as you go through the custom connector wizard. Start by importing the Postman collection for [Logic Apps](#import-the-postman-collection-for-logic-apps), or for [Power Automate and Power Apps](#import-the-postman-collection-for-power-automate-and-power-apps).

### Import the Postman collection for Logic Apps

1. Go to the [Azure portal](https://portal.azure.com), and open the Logic Apps connector you created earlier in [Create an Azure Logic Apps custom connector](create-logic-apps-connector.md).

2. In your connector's menu, choose **Logic Apps Connector**, then choose **Edit**.

   ![Edit Logic Apps Connector](./media/define-postman-collection/logic-apps-edit-connector.png)

3. Under **General**, choose **Upload Postman collection V1**, then navigate to the Postman collection that you created.

   ![Screenshot that shows the Upload Postman collection V1 option.](./media/define-postman-collection/logic-apps-upload-collection.png)

   The wizard imports the collection, then converts it to an OpenAPI definition named `generatedApiDefinition.swagger.json`.

> [!Note]
> This tutorial focuses on a REST API, but you can also [use a SOAP API with Logic Apps](create-register-logic-apps-soap-connector.md).

### Import the Postman collection for Power Automate and Power Apps

1. Go to [make.powerapps.com](https://make.powerapps.com) or [flow.microsoft.com](https://flow.microsoft.com).

1. In the navigation pane, select **Data > Custom connectors**.

1. Choose **New custom connector**, then choose **Import a Postman collection**.

1. Enter a name for the custom connector, then navigate to the Postman collection that you downloaded or created, and choose **Continue**.

   ![Screenshot that shows the steps for importing the collection.](./media/define-postman-collection/flow-apps-upload-collection.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Custom connector title** | "SentimentDemo" | 
   || 

   The wizard imports the collection, then converts it to an OpenAPI definition named `generatedApiDefinition.swagger.json`.

## Update general details

From this point, we will show the Power Automate UI, but the steps are largely the same across all three technologies. We will highlight any differences.

1. On the **General** page, review the information that was imported from the Postman collection, including the host and the base URL for the API. The connector uses the host and base URL to determine how to call the API.

   > [!Note]
   > For more information about connecting to on-premises APIs, see [Connect to on-premises APIs using the data gateway](https://go.microsoft.com/fwlink/?linkid=857646).

1. Update the description to something meaningful. This description is displayed in the custom connector's details, and can help other users comprehend how your connector could be useful to them.

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Description** | "Uses the Cognitive Services Text Analytics Sentiment API to determine whether text is positive or negative" | 
   || 

## Specify authentication type

There are several options available for authentication in custom connectors. The Cognitive Services APIs use API key authentication.

1. On the **Security** page, under **Authentication type**, choose **API Key**.

1. Under **API Key**, specify a parameter label, name, and location. Choose an expressive and meaningful label. This text will be displayed to users to direct them in making connections using your custom connector. The parameter name and location must match what the API expects (in this case the header you specified in Postman). Choose **Connect**.

   ![API key parameters](./media/define-postman-collection/api-key-parameters.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Parameter label** | "API key" | 
   | **Parameter name** | "Ocp-Apim-Subscription-Key" |
   | **Parameter location** | "Header" |
   ||

1. At the top of the wizard, make sure the name is set to "SentimentDemo", then choose **Create connector**.

## Review and update the connector definition

The custom connector wizard gives you a lot of options for defining how your connector functions, and how it is exposed in logic apps, flows, and apps. We'll explain the UI and cover a few options in this section, but we also encourage you to explore on your own.

### Review the UI and definition 

Before we get into some specific steps on the **Definition** page, let's first take a look at the UI.

1. This area displays any actions, triggers (for Logic Apps and Power Automate), and references that are defined for the connector. In this case, the `DetectSentiment` action from the Postman collection is displayed. There are no triggers in this connector, but you can learn about triggers for custom connectors in [Use webhooks with Azure Logic Apps and Power Automate](create-webhook-trigger.md).

    ![Definition page - actions and triggers](./media/define-postman-collection/definition-actions-triggers.png)

1. The **General** area displays information about the action or trigger currently selected. This information comes from the Postman collection. You can edit the information here, including the **Visibility** property for operations and parameters in a logic app or flow:
   * **important**: always shown to the user first
   * **none**: displayed normally in the logic app or flow
   * **advanced**: initially hidden under an additional menu
   * **internal**: not shown to the user

1. The **Request** area displays information based on the HTTP request that's included in the Postman collection. In this case, you see that the HTTP *verb* is **POST**, and the URL is "/text/analytics/v2.0/sentiment" (the full URL to the API is `<https://westus.api.cognitive.microsoft.com//text/analytics/v2.0/sentiment>`). We'll look closer at the **body** parameter shortly. 

1. The **Response** area displays information based on the HTTP response that's included in the Postman collection. In this case, the only response defined is for "200" (a successful response), but you can define additional responses.

1. The **Validation** area displays any issues that are detected in the API definition. Make sure to check this area before you save a connector.

### Update the definition

Now let's change a few things so that the connector is more friendly when someone uses it in a Logic App, Power Automate, or Power Apps.

1. In the **General** area, update the summary to "Returns a numeric score representing the sentiment detected".

1. In the **Request** area, choose **body** then **Edit**.

1. In the **Parameter** area, you now see the three parameters that the API expects: `id`, `language`, and `text`. Choose **id** then **Edit**.

1. In the **Schema Property** area, update values for the parameter, then choose **Back**.

    ![Edit schema property](./media/define-postman-collection/request-edit-schema.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Title** | "ID" | 
   | **Description** | "An identifier for each document that you submit" |
   | **Default value** | "1" |
   | **Is required** | "Yes" |
   ||

1. In the **Parameter** area, choose **language** then **Edit**, and repeat the process you used above with the following values.

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Title** | "Language" | 
   | **Description** | "The 2 or 4 character language code for the text" |
   | **Default value** | "en" |
   | **Is required** | "Yes" |
   ||

1. In the **Parameter** area, choose **text** then **Edit**, and repeat the process you used above with the following values.

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Title** | "Text" | 
   | **Description** | "The text to analyze for sentiment" |
   | **Default value** | None |
   | **Is required** | "Yes" |
   ||

1. In the **Parameter** area, choose **Back** to take you back to the main definition page.

1. At the top right of the wizard, choose **Update connector**. 

## Test the connector

Now that you've created the connector, test it to make sure it's working properly. Testing is currently available only in Power Automate and Power Apps.

> [!Important]
> When using an API key, we recommend not testing the connector immediately after you create it. It can take a few minutes until the connector is ready to connect to the API.

1. On the **Test** page, choose **New connection**.

1. Enter the API key from the Text Analytics API, then choose **Create connection**.

1. Return to the **Test page**:

    * In Power Automate, you are taken back to the **Test** page. Choose the refresh icon to make sure the connection information is updated.
    * In Power Apps, you are taken to the list of connections available in the current environment. In the upper right corner, choose the gear icon, then choose **Custom connectors**. Choose the connector you created, then go back to the **Test** page.

 1. On the **Test** page, enter a value for the **text** field (the other fields use the defaults that you set earlier), then choose **Test operation**.

1. The connector calls the API, and you can review the response, which includes the sentiment score.

    ![Connector response](./media/define-postman-collection/connector-response.png)

## Limitations

In Power Automate and Power Apps, if you update an existing custom connector using a Postman collection, you'll need to re-do any previous customizations before saving the connector. For example, you'll need to reconfigure the authentication type, default values of the parameters for the actions, and other. 
     
## Next steps

Now that you've created a custom connector and defined its behaviors, you can use the connector.

* [Use a custom connector from a flow](use-custom-connector-flow.md)
* [Use a custom connector from an app](use-custom-connector-powerapps.md)
* [Use a custom connector from a logic app](use-custom-connector-logic-apps.md)

You can also share a connector within your organization and/or get the connector certified so that people outside your organization can use it.

* [Share your connector](share.md)
* [Certify your connector](submit-certification.md)

[!INCLUDE[feedback](../includes/feedback.md)]
