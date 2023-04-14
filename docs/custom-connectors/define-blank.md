---
title: Create a custom connector from scratch | Microsoft Docs
description: Use the "create from blank" option to create a custom connector for Power Automate and Power Apps
author: sunaysv
manager: maheshp
services: connectors
documentationcenter: 
ms.assetid: 
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 07/13/2022
ms.author: sunayv
ms.reviewer: angieandrews
---

# Create a custom connector from scratch

> [!Note]
> This topic is part of a tutorial series on creating and using custom connectors in Azure Logic Apps, Microsoft Power Automate, and Microsoft Power Apps. Make sure you read the [custom connector overview](index.md) to understand the process.

To create a custom connector, you must describe the API you want to connect to so that the connector understands the API's operations and data structures. In this topic, you create a custom connector from scratch, without using a [Postman collection](define-postman-collection.md) or an [OpenAPI definition](define-openapi-definition.md) to describe the Azure Cognitive Services Text Analytics API sentiment operation (our example for this series). Instead, you describe the connector completely in the custom connector wizard.

For other ways to describe an API, go to the following topics:

* [Create a custom connector from an OpenAPI definition](define-openapi-definition.md)
* [Create a custom connector from a Postman collection](define-postman-collection.md)

> [!Note]
> You can currently create a custom connector from scratch in Power Automate and Power Apps. For Logic Apps, you must start with at least a basic OpenAPI definition or Postman collection.

## Prerequisites

* An [API key](index.md#get-an-api-key) for the Cognitive Services Text Analytics API

* One of the following subscriptions:
    * [Power Automate](/flow/sign-up-sign-in)
    * [Power Apps](/powerapps/signup-for-powerapps)

## Start the custom connector wizard

1. Sign in to [Power Apps](https://make.powerapps.com) or [Power Automate](https://flow.microsoft.com).

2. On the left pane, select **Data** > **Custom connectors**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of Select custom connectors.](./media/define-blank/custom-connector.png "Select custom connectors")

3. Select **New custom connector**, and then select **Create from blank**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of Create from blank.](./media/define-blank/flow-apps-create-connector.png "Create from blank")

4. Enter a name for the custom connector, and then select **Continue**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of the custom connector name.](./media/define-blank/flow-apps-blank.png "Enter the custom connector name")

   | Parameter | Value |
   | --------- | --------------- |
   | Custom connector title | SentimentDemo |

## Step 1: Update general details

From this point, we'll show the Power Automate UI, but the steps are largely the same across the technologies. We'll point out any differences.

1. On the **General** tab, do the following:

    * In the **Description** field, enter a meaningful value. This description will appear in the custom connector's details, and it can help others decide whether the connector might be useful to them.

    * Update **Host** to the address for the Text Analytics API. The connector uses the API host and the base URL to determine how to call the API.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of the custom connector General tab.](./media/define-blank/page-general.png "Custom connector General tab")

   | Parameter | Value | 
   | --------- | --------------- | 
   | Description | Uses the Cognitive Services Text Analytics Sentiment API to determine whether text is positive or negative |
   | Host | westus.api.cognitive.microsoft.com |  

    > [!Note]
    > For more information about the option **Connect via on-premises data gateway**, go to [Connect to on-premises APIs by using the data gateway](https://go.microsoft.com/fwlink/?linkid=857646).

## Step 2: Specify authentication type

There are several options available for authentication in custom connectors. The Cognitive Services APIs use API key authentication, so that's what you specify for this tutorial.

1. On the **Security** tab, under **Authentication type**, select **API Key**.

   > [!div class="mx-imgBorder"]
   >![Screenshot of Authentication type.](./media/define-blank/authentication-type.png "Authentication type")

2. Under **API Key**, specify a parameter label, name, and location. Specify a meaningful label, because this is displayed when someone first makes a connection with the custom connector. The parameter name and location must match what the API expects. Select **Connect**.

      > [!div class="mx-imgBorder"]
      > ![Screenshot of API key parameters.](./media/define-blank/api-key-parameters.png "API key parameters")

   | Parameter | Value | 
   | --------- | --------------- | 
   | Parameter label | API key | 
   | Parameter name | Ocp-Apim-Subscription-Key |
   | Parameter location | Header |

3. At the top of the wizard, make sure the name is set to **SentimentDemo**, and then select **Create connector**.

## Step 3: Create the connector definition

The custom connector wizard gives you many options for defining how your connector functions, and how it's exposed in logic apps, flows, and apps. We'll explain the UI and cover a few options in this section, but we also encourage you to explore on your own.

### Create an action

The first thing to do is create an action that calls the Text Analytics API sentiment operation.

1. On the **Definition** tab, the left pane displays any actions, triggers (for Logic Apps and Power Automate), and references that are defined for the connector. Select **New action**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of Definition tab - actions and triggers.](./media/define-blank/definition-actions-triggers.png "Definition tab - actions and triggers")

    There are no triggers in this connector. You can learn about triggers for custom connectors in [Use webhooks with Azure Logic Apps and Power Automate](create-webhook-trigger.md).

2. The **General** area displays information about the action or trigger that's currently selected. Add a summary, description, and operation ID for this action.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of Definition tab - general.](./media/define-blank/definition-general.png "Definition tab - general")

   | Parameter | Value | 
   | --------- | --------------- | 
   | Summary | Returns a numeric score representing the sentiment detected | 
   | Description | The API returns a numeric score between 0 and 1. Scores close to 1 indicate positive sentiment, while scores close to 0 indicate negative sentiment. |
   | Operation ID | DetectSentiment |

    Leave the  **Visibility** property set to **none**. This property for operations and parameters in a logic app or flow has the following options:
    * **none**: displayed normally in the logic app or flow
    * **advanced**: hidden under another menu
    * **internal**: hidden from the user
    * **important**: always shown to the user first

3. The **Request** area displays information based on the HTTP request for the action. Select **Import from sample**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of Definition tab - import from sample in the Request area.](./media/define-blank/import-sample.png "Definition tab - import from sample in the Request area")

4. Specify the information necessary to connect to the API, specify the request body (provided after the following image), and then select **Import**. We provide this information for you, but for a public API, you typically get this information from documentation such as [Text Analytics API (v2.0)](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9).

   > [!div class="mx-imgBorder"]
   >![Screenshot of Definition tab - import from sample.](./media/define-blank/import-sample-UI.png "Definition tab - import from sample")


   |       Parameter       |                                    Value                                     |
   |-----------------------|------------------------------------------------------------------------------|
   | Verb |                                    POST                                    |
   | URL  | `<https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment>` |
   | Body |                                  Use the following JSON code                 |
   |                       |                                                                              |

   ```JSON
   {
     "documents": [
       {
         "language": "string",
         "id": "string",
         "text": "string"
       }
     ]
   }
   ```

5. The **Response** area displays information based on the HTTP response for the action. Select **Add default response**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of Definition tab - Add default response.](./media/define-blank/definition-response.png "Definition tab - Add default response")

6. Specify the response body, and then select **Import**. As we did for the request body, we provide this information for you following the image, but it's typically provided in the API documentation. 

   > [!div class="mx-imgBorder"]
   >![Sreenshot of Definition tab - response.](./media/define-blank/definition-response-UI.png "Definition tab - response")

    ```JSON
   {
     "documents": [
       {
         "score": 0.0,
         "id": "string"
       }
     ],
     "errors": [
       {
         "id": "string",
         "message": "string"
       }
     ]
   }
    ```

7. The **Validation** area displays any issues that are detected in the API definition. Check the status, and then in the upper-right corner of wizard, select **Update connector**.

   > [!div class="mx-imgBorder"]
   >![Screenshot of Definition tab - validation.](./media/define-blank/definition-validation.png "Definition tab - validation")

### Update the definition

Now let's change a few things so that the connector is more friendly when someone uses it in a logic app, flow, or app.

1. In the **Request** area, select **body**, and then select **Edit**.

      > [!div class="mx-imgBorder"]
      > ![Screenshot of Edit request body.](./media/define-blank/request-body.png "Edit request body")

2. In the **Parameter** area, you now see the three parameters that the API expects: `id`, `language`, and `text`. Select **id**, and then select **Edit**.

      > [!div class="mx-imgBorder"]
      >![Screenshot of Edit request body id.](./media/define-blank/request-body-id.png "Edit request body id")

3. In the **Schema Property** area, update values for the parameter, and then select **Back**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of Edit schema property.](./media/define-blank/request-edit-schema.png "Edit schema property")

<br/>

   | Parameter | Value | 
   | --------- | --------------- | 
   | Title | ID | 
   | Description | An identifier for each document that you submit |
   | Default value | 1 |
   | Is required | Yes |

4. In the **Parameter** area, select **language**, select **Edit**, and then repeat the process that you used for `id` in steps 2 and 3 of this procedure, with the following values.

   | Parameter | Value | 
   | --------- | --------------- | 
   | Title | Language | 
   | Description | The 2 or 4 character language code for the text |
   | Default value | en |
   | Is required | Yes |

5. In the **Parameter** area, select **text**, select **Edit**, and then repeat the process that you used for `id` in steps 2 and 3 of this procedure, with the following values.

   | Parameter | Value | 
   | --------- | --------------- | 
   | Title | Text | 
   | Description | The text to analyze for sentiment |
   | Default value | None |
   | Is required | Yes |

6. In the **Parameter** area, select **Back** to take you back to the main **Definition** tab.

7. In the upper-right corner of the wizard, select **Update connector**.

## Step 4: (Optional) Use custom code support

> [!NOTE]
> - *This step is optional*. You can complete the codeless experience for creating your connector by ignoring this step and going to [Step 5: Test the connector](#step-5-test-the-connector).
>
>  - Custom code support is available in public preview. 

Custom code transforms request and response payloads beyond the scope of existing policy templates. Transformations include sending external requests to fetch additional data. When code is used, it will take precedence over the codeless definition. This means that the code will execute, and we won't send the request to the back end.

You can either paste in your code or upload a file with your code. Your code must:

- Be written in C#.
- Have a maximum execution time of five seconds.
- Have a file size no larger than 1 MB.

For instructions and samples of writing code, go to [Write code in custom connectors](write-code.md).

For frequently asked questions about custom code, go to [Custom code FAQ](write-code.md#custom-code-faq).

1. On the **Code** tab, insert your custom code by using one of the following options:

   - Copy/paste 
   - Select the **Upload** button.  

   If you choose to upload your custom code, only files with a .cs or .csx extension will be available.

    > [!div class="mx-imgBorder"]
    > ![Screenshot of Upload your custom code.](./media/define-blank/custom-code-crop.png "Upload your custom code")
<br/>

   > [!IMPORTANT]
   >Currently, we only support syntax highlighting in the code editor. Make sure to test your code locally.

2. After you paste or upload your code, select the toggle next to **Code Disabled** to enable your code. The toggle name changes to **Code Enabled**.

   You can enable or disable your code anytime. If the toggle is **Code Disabled**, your code will be deleted.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of Disable code message.](./media/define-blank/disable-code.png "Disable code message")

3. Select the actions and triggers to apply to your custom code by selecting an option in the dropdown list. If no operation is selected, the actions and triggers<!--note from editor: Is this what "it's" means here?--> are applied to *all* operations.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of Select actions and triggers.](./media/define-blank/custom-code-actiontrig-crop.png "Select actions and triggers")

## Step 5: Test the connector

Now that you've created the connector, test it to make sure it's working properly. Testing is currently available only in Power Automate and Power Apps.

> [!Important]
> When using an API key, we recommend against testing the connector immediately after you create it. It can take a few minutes until the connector is ready to connect to the API.

1. On the **Test** tab, select **New connection**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of new connection.](./media/define-blank/new-connection.png "New connection")

2. Enter the API key from the Text Analytics API, and then select **Create connection**. 

   > [!div class="mx-imgBorder"]
   >![Screenshot of create connection.](./media/define-blank/create-connection.png "Create connection")

   > [!NOTE]
   > For APIs that require bearer authentication, add **Bearer** and one space before the API key. 

3. Return to the **Test** tab, and do one of the following:

    * In Power Automate, you're taken back to the **Test** tab. Select the refresh icon to make sure the connection information is updated.

      > [!div class="mx-imgBorder"]
      > ![Screenshot of refresh connection.](./media/define-blank/refresh-connection.png "Refresh connection")

    * In Power Apps, you're taken to the list of connections available in the current environment. On the left pane, select **Data** > **Custom connectors**. Choose the connector you created, and then go back to the **Test** tab.

      > [!div class="mx-imgBorder"]
      > ![Screenshot of select custom connector.](./media/define-blank/custom-connector.png "Select custom connector")
       
4. On the **Test** tab, enter a value for the **text** field (the other fields use the defaults that you set earlier), and then select **Test operation**.

   > [!div class="mx-imgBorder"]
   >![Screenshot of test operation.](./media/define-blank/test-operation.png "Test operation")

5. The connector calls the API, and you can review the response, which includes the sentiment score.

   > [!div class="mx-imgBorder"] 
   >![Screenshot of connector response.](./media/define-blank/connector-response.png "Connector response")

## (For CLI users) Best practices

- Download all your connectors, and use Git or any source code management system to save the files. 

- If there's an incorrect update, redeploy the connector by rerunning the update command with the correct set of files from the source code management system.

- Test the custom connector and the settings file in a test environment before deploying in the production environment.

- Always double-check that the environment and connector ID are correct.

## Next steps

Now that you've created a custom connector and defined its behaviors, you can use the connector from:

* [A flow](use-custom-connector-flow.md)
* [An app](use-custom-connector-powerapps.md)
* [A logic app](use-custom-connector-logic-apps.md)

You can also share a connector within your organization or get the connector certified so that people outside your organization can use it.

* [Share your connector](share.md)
* [Certify your connector](submit-certification.md)

[!INCLUDE[feedback](../includes/feedback.md)]
