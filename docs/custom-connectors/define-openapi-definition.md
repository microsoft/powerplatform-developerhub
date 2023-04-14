---
title: Create a custom connector from an OpenAPI definition | Microsoft Docs
description: Use an OpenAPI definition to create a custom connector for Azure Logic Apps, Power Automate, and Power Apps.
author: sunaysv
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 07/15/2022
ms.author: sunayv
ms.reviewer: angieandrews
---

# Create a custom connector from an OpenAPI definition

> [!Note]
> This topic is part of a tutorial series on creating and using custom connectors in Azure Logic Apps, Microsoft Power Automate, and Microsoft Power Apps. Make sure you read the [custom connector overview](index.md) to understand the process.

To create a custom connector, you must describe the API you want to connect to so that the connector understands the API's operations and data structures. In this topic, you create a custom connector using an OpenAPI definition that describes the Cognitive Services Text Analytics Sentiment API (our example for this series).

For other ways to describe an API, go to the following topics:

* [Create a custom connector from a Postman collection](define-postman-collection.md)
* [Create a custom connector from scratch](define-blank.md)

## Prerequisites

* An [OpenAPI definition](https://procsi.blob.core.windows.net/docs/SentimentDemo.openapi_definition.json) that describes the example API. When creating a custom connector, the OpenAPI definition must be less than 1 MB. The OpenAPI definition needs to be in OpenAPI 2.0 (formerly known as Swagger) format.

    If there are multiple security definitions, the custom connector picks the top security definition. Custom connector creation doesn't support client credentials (for example, application and password) in OAuth security definition.  

* An [API key](index.md#get-an-api-key) for the Cognitive Services Text Analytics API.

* One of the following subscriptions:
    * [Azure](https://azure.microsoft.com/get-started/), if you're using Logic Apps
    * [Power Automate](/flow/sign-up-sign-in.md)
    * [Power Apps](/powerapps/signup-for-powerapps.md)

* If you're using Logic Apps, first [create an Azure Logic Apps custom connector](create-logic-apps-connector.md).

## Import the OpenAPI definition

You're now ready to work with the OpenAPI definition you downloaded. All the required information is contained in the definition, and you can review and update this information as you go through the custom connector wizard.

Start by importing the OpenAPI definition for [Logic Apps](#import-the-openapi-definition-for-logic-apps), or for [Power Automate and Power Apps](#import-the-openapi-definition-for-power-automate-and-power-apps).

> [!Note]
> An OpenAPI definition needs to be in OpenAPI 2.0 (formerly known as Swagger) format. OpenAPI definitions that are in OpenAPI 3.0 format are not supported. 

### Import the OpenAPI definition for Logic Apps

1. Go to the [Azure portal](https://portal.azure.com), and open the Logic Apps connector you created earlier in [Create an Azure Logic Apps custom connector](create-logic-apps-connector.md).

2. In your connector's menu, select **Logic Apps Connector**, and then select **Edit**.

   ![Edit Logic Apps Connector.](./media/define-openapi-definition/logic-apps-edit-connector.png)

3. Under **General**, select **Upload an OpenAPI file**, and then go to the OpenAPI definition that you created.

   ![Upload OpenAPI file.](./media/define-openapi-definition/logic-apps-upload-definition.png)

> [!NOTE]
> This tutorial focuses on a REST API, but you can also [use a SOAP API with Logic Apps](create-register-logic-apps-soap-connector.md).

### Import the OpenAPI definition for Power Automate and Power Apps

1. Go to [make.powerapps.com](https://make.powerapps.com) or [flow.microsoft.com](https://flow.microsoft.com).

2. On the left pane, select **Data** > **Custom connectors**.

   ![Select custom connector.](./media/define-openapi-definition/custom-connector.png)

3. Select **New custom connector**, and then select **Import an OpenAPI file**.

   ![Import an OpenAPI file.](./media/define-openapi-definition/flow-apps-create-connector.png)

4. Enter a name for the custom connector, go to the OpenAPI definition that you downloaded or created, and then select **Continue**.

   ![Upload Postman collection.](./media/define-openapi-definition/flow-apps-upload-definition.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | Custom connector title | SentimentDemo |

## Review general details

From this point, we'll show the Power Automate UI, but the steps are largely the same across all three technologies. We'll point out any differences. In this part of the topic, we'll mostly review the UI and show you how the values correspond to sections of the OpenAPI file.

1. At the top of the wizard, make sure the name is set to **SentimentDemo**, and then select **Create connector**.

2. On the **General** page, review the information that was imported from the OpenAPI definition, including the API host and the base URL for the API. The connector uses the API host and the base URL to determine how to call the API.

    ![Custom connector General page.](./media/define-openapi-definition/page-general.png)

    > [!Note]
    > For more information about connecting to on-premises APIs, go to [Connect to on-premises APIs using the data gateway](https://go.microsoft.com/fwlink/?linkid=857646).

    The following section of the OpenAPI definition contains information for this page of the UI:

    ```JSON
      "info": {
        "version": "1.0.0",
        "title": "SentimentDemo",
        "description": "Uses the Cognitive Services Text Analytics Sentiment API to determine whether text is positive or negative"
      },
      "host": "westus.api.cognitive.microsoft.com",
      "basePath": "/",
      "schemes": [
        "https"
      ]
    ```

## Review authentication type

There are several options available for authentication in custom connectors. The Cognitive Services APIs use API key authentication, so that's what's specified in the OpenAPI definition.

On the **Security** page, review the authentication information for the API key.

![API key parameters.](./media/define-openapi-definition/api-key-parameters.png)

The label is displayed when someone first makes a connection with the custom connector; you can select **Edit** and change this value. The parameter name and location must match what the API expects, in this case **Ocp-Apim-Subscription-Key** and **Header**.

The following section of the OpenAPI definition contains information for this page of the UI:

```JSON
  "securityDefinitions": {
    "api_key": {
      "type": "apiKey",
      "in": "header",
      "name": "Ocp-Apim-Subscription-Key"
    }
  }
```

## Review the connector definition

The **Definition** page of the custom connector wizard gives you many options for defining how your connector functions and how it's exposed in logic apps, flows, and apps. We'll explain the UI and cover a few options in this section, but we also encourage you to explore on your own. For information on defining objects from scratch in this UI, go to [Create the connector definition](define-blank.md#step-3-create-the-connector-definition).

1. The following area displays any actions, triggers (for Logic Apps and Power Automate), and references that are defined for the connector. In this case, the `DetectSentiment` action from the OpenAPI definition is displayed. There are no triggers in this connector, but you can learn about triggers for custom connectors in [Use webhooks with Azure Logic Apps and Power Automate](create-webhook-trigger.md).

    ![Definition page - actions and triggers.](./media/define-openapi-definition/definition-actions-triggers.png)

2. The **General** area displays information about the action or trigger currently selected. You can edit the information here, including the **Visibility** property for operations and parameters in a logic app or flow:

   * **none**: displayed normally in the logic app or flow
   * **advanced**: hidden under an additional menu
   * **internal**: hidden from the user
   * **important**: always shown to the user first

     ![Definition page - general.](./media/define-openapi-definition/definition-general.png)

3. The **Request** area displays information based on the HTTP request that's included in the OpenAPI definition. In this case, you see that the HTTP verb is **POST**, and the URL is **/text/analytics/v2.0/sentiment** (the full URL to the API is `<https://westus.api.cognitive.microsoft.com//text/analytics/v2.0/sentiment>`). We'll look more closely at the **body** parameter shortly. 

    ![Definition page - request.](./media/define-openapi-definition/definition-request.png)

    The following section of the OpenAPI definition contains information for the **General** and **Request** areas of the UI:

    ```JSON
    "paths": {
      "/text/analytics/v2.0/sentiment": {
        "post": {
          "summary": "Returns a numeric score representing the sentiment detected",
          "description": "The API returns a numeric score between 0 and 1. Scores close to 1 indicate positive sentiment, while scores close to 0 indicate negative sentiment.",
          "operationId": "DetectSentiment"
    ```

4. The **Response** area displays information based on the HTTP response that's included in the OpenAPI definition. In this case, the only response defined is for **200** (a successful response), but you can define additional responses.

    ![Definition page - response.](./media/define-openapi-definition/definition-response.png)

    The following section of the OpenAPI definition contains some of the information related to the response:

    ```JSON
   "score": {
     "type": "number",
     "format": "float",
     "description": "score",
     "x-ms-summary": "score"
   },
   "id": {
     "type": "string",
     "description": "id",
     "x-ms-summary": "id"
   }
    ```

    This section shows the two values that are returned by the connector: `id` and `score`. It includes their data types and the field `x-ms-summary`, which is an OpenAPI extension. For more information on this and other extensions, go to [Extend an OpenAPI definition for a custom connector](openapi-extensions.md).

5. The **Validation** area displays any issues that are detected in the API definition. Make sure to check this area before you save a connector.

    ![Definition page - validation.](./media/define-openapi-definition/definition-validation.png)

### Update the definition

The OpenAPI definition you downloaded is a good basic example, but you might work with definitions that require much updating so that the connector is more friendly when someone uses it in a logic app, flow, or app. We'll show you how to make a change to the definition. 

1. In the **Request** area, select **body**, and then select **Edit**.

    ![Edit request body.](./media/define-openapi-definition/request-body.png)

2. In the **Parameter** area, you now see the three parameters that the API expects: `ID`, `Language`, and `Text`. Select **ID**, and then select **Edit**.

    ![Edit request body id.](./media/define-openapi-definition/request-body-id.png)

3. In the **Schema Property** area, update the description for the parameter, and then select **Back**.

    ![Edit schema property.](./media/define-openapi-definition/request-edit-schema.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | Description | A numeric identifier for each document that you submit |
   ||

4. In the **Parameter** area, select **Back** to take you back to the main definition page.

5. In the upper-right corner of the wizard, select **Update connector**. 


## Download the updated OpenAPI file 

You can create a custom connector from an OpenAPI file, a Postman collection, or from scratch (in Power Automate and Power Apps). Regardless of how you create the connector, you can download the OpenAPI definition that the service uses internally.

* In Logic Apps, download from the custom connector.

    ![Download OpenAPI definition for Logic Apps.](./media/define-openapi-definition/download-definition-logic-apps.png)

* In Power Automate or Power Apps, download from the list of custom connectors.

    ![Download OpenAPI definition for Power Automate.](./media/define-openapi-definition/download-definition-flow-apps.png)

## Test the connector

Now that you've created the connector, test it to make sure it's working properly. Testing is currently available only in Power Automate and Power Apps.

> [!Important]
> When using an API key, we recommend against testing the connector immediately after you create it. It can take a few minutes until the connector is ready to connect to the API.

1. On the **Test** page, select **New connection**.

    ![New connection.](./media/define-openapi-definition/new-connection.png)

2. Enter the API key from the Text Analytics API, and then select **Create connection**.

    ![Create connection.](./media/define-openapi-definition/create-connection.png)

3. Return to the **Test page**, and do one of the following:

    * In Power Automate, you're taken back to the **Test** page. Select the refresh icon to make sure the connection information is updated.

        ![Refresh connection.](./media/define-openapi-definition/refresh-connection.png)

    * In Power Apps, you're taken to the list of connections available in the current environment. In the upper-right corner, select the gear icon, and then select **Custom connectors**. Choose the connector you created, and then go back to the **Test** page.

        ![Gear icon in service.](./media/define-openapi-definition/icon-gear.png)

4. On the **Test** page, enter a value for the **text** field (the other fields use the defaults that you set earlier), and then select **Test operation**.

    ![Test operation.](./media/define-openapi-definition/test-operation.png)

5. The connector calls the API, and you can review the response, which includes the sentiment score.

    ![Connector response.](./media/define-openapi-definition/connector-response.png)

## Next steps

Now that you've created a custom connector and defined its behaviors, you can use the connector.

* [Use a custom connector from a flow](use-custom-connector-flow.md)
* [Use a custom connector from an app](use-custom-connector-powerapps.md)
* [Use a custom connector from a logic app](use-custom-connector-logic-apps.md)

You can also share a connector within your organization or get the connector certified so that people outside your organization can use it.

* [Share your connector](share.md)
* [Certify your connector](submit-certification.md)

[!INCLUDE[feedback](../includes/feedback.md)]
