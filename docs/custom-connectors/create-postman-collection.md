---
title: Create a Postman collection for a custom connector | Microsoft Docs
description: Create a Postman collection for a custom connector in Azure Logic Apps, Power Automate, and Power Apps.
author: sunaysv
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: sunayv
ms.reviewer: angieandrews
---

# Create a Postman collection for a custom connector

> [!Note]
> This topic is part of a tutorial series on creating and using custom connectors in Azure Logic Apps, Power Automate, and Power Apps. Make sure you read the [custom connector overview](index.md) to understand the process.

Postman is an app for making HTTP requests, and Postman *collections* help you organize and group related API requests. Collections can make custom connector development faster and easier if you don't already have an OpenAPI definition for your API. For more information about collections, go to [Creating collections](https://www.getpostman.com/docs/postman/collections/creating_collections) in the Postman documentation. 

In this topic, you create a collection that includes a request and response from the Azure Cognitive Services Text Analytics API. In a related topic, you [create a connector by using this collection](define-postman-collection.md). 

## Prerequisites

* The [Postman](https://www.getpostman.com/apps) app
* An [API key](index.md#get-an-api-key) for the Cognitive Services Text Analytics API

## Create an HTTP request for the API

1. In Postman, on the **Builder** tab, select the HTTP method, enter the request URL for the API endpoint, and select an authorization protocol, if any.

   ![Create request: "HTTP method", "Request URL", "Authorization".](./media/create-postman-collection/01-create-api-http-request.png)


   | Parameter     | Value    |
   |---------------|----------------|
   |  HTTP method  |    POST    |
   |  Request URL  | `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment` |
   | Authorization|  No Auth (you specify an API key in the next step) |

1. Enter key-value pairs for the request header. For common HTTP headers, you can select from the dropdown list.

   ![Request continued: Headers.](./media/create-postman-collection/03-create-api-http-request-header.png)


   | Key   | Value                                               |
   |-------|----------|
   | Ocp-Apim-Subscription-Key | *Your-API-subscription-key*, which you can find in your Cognitive Services account.
   | Content-Type | application/json |
  
3. Enter content that you want to send in the request body. To check that the request works by getting a response back, select **Send**.

   ![Request continued: "Body".](./media/create-postman-collection/04-create-api-http-request-body.png)

    ```JSON
    {
        "documents": [{
            "language": "en-us",
            "id": "1", 
            "text": "I enjoyed the new movie after a long day."
        }]
    }
    ```

   The response field contains the full response from the API, which includes the result or an error, if one occurred.

   ![Get request response.](./media/create-postman-collection/05-create-api-http-request-response.png)

For more information about HTTP requests, go to [Requests](https://www.getpostman.com/docs/postman/sending_api_requests/requests) in the Postman documentation.

## Save the collection

1. Select **Save**. 

   ![Select Save.](./media/create-postman-collection/06a-save-request.png)

2. On the **Save request** dialog, enter a request name and description. The custom connector uses these values for the API operation summary and description.

   ![Screenshot that shows the Save Request window.](./media/create-postman-collection/06b-save-request.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | Request name | DetectSentiment | 
   | Request description | The API returns a numeric score between 0 and 1. Scores close to 1 indicate positive sentiment, while scores close to 0 indicate negative sentiment. |


3. Select **+ Create Collection**, and enter name for the collection. The custom connector uses this value when you call the API. Select the check mark (**&#x2713;**), which creates a collection folder, and then select **Save to SentimentDemo**.

   ![Screenshot that shows the steps for creating a collection.](./media/create-postman-collection/06c-save-request.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | Collection name | SentimentDemo | 


## Save the request response

Now that you've saved your request, you can save the response. That way, the response will appear as an example when you load the request later.

1. Above the response window, select **Save Response**. 

   ![Screenshot that shows the Save Response button.](./media/create-postman-collection/07a-create-api-http-request-save-response.png)

   Custom connectors support only one response per request. If you save multiple responses per request, only the first one will be used.

2. At the top of the app, enter a name for your example response, and then select **Save Example**.

   ![Screenshot that shows how to save an example.](./media/create-postman-collection/07b-create-api-http-request-save-response.png)

If your API has additional features, you can<!--note from editor: Edit okay? "You would" doesn't say at which point in time this step should happen.--> continue to build your Postman collection with any additional requests and responses.

## Export the Postman collection

You now export the collection as a JSON file, which you import by using the custom connector wizard. Before you export the collection, remove the content type and security headers&mdash;these were required to make API requests, but they're handled differently in the custom connector. 

1. On the **Headers** tab, hover over each header and select the **X** next to the header to remove it. Select **Save** to save the collection again.

   ![Remove headers.](./media/create-postman-collection/remove-headers.png)

2. Select the ellipsis (**&hellip;**) next to the collection, and then select **Export**.

   ![Export collection.](./media/create-postman-collection/08-export-http-request.png)

3. Select the **Collection v1** export format, select **Export**, and then browse to the location where you want to save the JSON file.

   ![Choose export format: "Collection v1".](./media/create-postman-collection/09-export-format.png)

   > [!Note]
   > Currently, you can use only v1 collections for custom connectors.

## Next steps

You're now ready to define a custom connector based on the Postman collection you created:

* [Create a custom connector from a Postman collection](define-postman-collection.md)

[!INCLUDE[feedback](../includes/feedback.md)]
