---
title: Use a custom connector from a Power Apps app | Microsoft Docs
description: Call a custom connector from an app you create with Power Apps.
author: sunaysv
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: sunayv
ms.reviewer: angieandrews
---

# Use a custom connector from a Power Apps app

> [!Note]
> This topic is part of a tutorial series on creating and using custom connectors in Azure Logic Apps, Power Automate, and Power Apps. Make sure you read the [custom connector overview](index.md) to understand the process.

In this topic, you build a basic app that uses the custom connector you created in a previous topic. The app takes text input, then uses the custom connector to call the Cognitive Services Text Analytics API. The connector returns the sentiment score (0 to 1) for the text, and the app displays it as a percentage. The following image shows the finished app:

![Finished sentiment analysis app](./media/use-custom-connector-powerapps/finished-app.png)

## Prerequisites

* A [Power Apps subscription](/powerapps/signup-for-powerapps).
* Basic experience building apps in Power Apps. For more information, see [Create an app from scratch](/powerapps/get-started-create-from-blank).
* The custom connector that you created in one of these topics:
    * [Create a custom connector from an OpenAPI definition](define-openapi-definition.md)
    * [Create a custom connector from a Postman collection](define-postman-collection.md)
    * [Create a custom connector from scratch](define-blank.md)

## Create the app and add the custom connector

The first thing you do is create an app from blank, then connect to the custom connector that you created in a previous topic.

1. In [make.powerapps.com](https://make.powerapps.com), choose **Start from blank** > ![Phone app icon](./media/use-custom-connector-powerapps/icon-phone-app.png) (phone) > **Make this app**.

    ![Start from blank](./media/use-custom-connector-powerapps/start-from-blank.png)

2. On the app canvas, choose **connect to data**.

3. On the **Data** panel, choose the connection you created in a previous topic (such as "SentimentDemo").

4. Save the app with the name `Sentiment Analysis`.

## Add controls to the app

You now build out the UI for the app, so that you can enter text, submit that text to the API, and get a response.

1. Add a rectangle icon as a title bar, then add the label "Sentiment Analysis".

    ![Add a title bar](./media/use-custom-connector-powerapps/add-title-bar.png)

2. Add the label "Enter your text, then click Get score", then add a text input control.

    ![Add a label and text input](./media/use-custom-connector-powerapps/add-text-input.png)

3. Add a button with the text "Get score".

    ![Add a button](./media/use-custom-connector-powerapps/add-button.png)

4. Add the label "The sentiment score is". In the next section, you add a formula to complete this label.

    ![Add a label](./media/use-custom-connector-powerapps/add-label.png)

## Add formulas to drive behavior

With the data connection and UI in place, now you add Power Apps formulas that drive the behavior of the app. The formulas call the API through the custom connector, store the result in a *collection* (a tabular variable), then display the formatted result in the app.

1. Choose the button you created, then set the **OnSelect** property of the button to the name of the connector (including the period).

    ```
    SentimentDemo.
    ```
    Power Apps gives you an auto-complete option of `DetectSentiment` because the custom connector makes this available.

2. Now set the **OnSelect** property of the button to the following formula.

    ```
    ClearCollect(sentimentCollection, SentimentDemo.DetectSentiment(
        {id:"1", language:"en", text:TextInput1.Text}).documents.score)
    ```

    This formula gets the sentiment score from the API, and stores it in a collection:

    1. The formula calls the `DetectSentiment` function with the three parameters exposed by the custom connector: `id`, `language`, and `text`. We specify values for the first two right in the formula, and get the value for `Text` from the text input control (you could also pull the first two values from somewhere else in an app). 

    2. The function returns a `score` for each document that you send; in our examples, we send one document at a time. The score ranges from 0 (negative) to 1 (positive).

    3. The formula then calls the `ClearCollect` function to remove any existing values from the `sentimentCollection` and add the value from `score`.

2. Choose the label that you created, then set the **Text** property of the label to the following formula.

    ```
    "The sentiment score is " & Round(First(sentimentCollection).score, 3) * 100 & "%"
    ```

    This formula gets the sentiment score from the collection, and formats and displays it: 

    1. The `First()` function returns the first (and in this case only) record in `sentimentCollect`, and displays the `score` field (the only field) associated with that record.
    
    2. The `Round()` function rounds the score to 3 places; the rest of the formula formats the result as a percentage and adds some information for context.

## Test the app

Now run the completed app to make sure it works as expected.

1. Choose ![Run app](./media/use-custom-connector-powerapps/icon-run-app.png) in the top right to run the app. 

2. Enter a phrase in the text input control, and choose **Get score**. The sentiment score should be displayed within a few seconds.

The finished app looks like the following image:

![Finished sentiment analysis app](./media/use-custom-connector-powerapps/finished-app.png)

It's a simple app, but it gains powerful functionality by being able to call Cognitive Services through a custom connector.

## Next steps

Share the connector within your organization and/or get the connector certified so that people outside your organization can use it:

* [Share your connector](share.md)
* [Certify your connector](submit-certification.md)

[!INCLUDE[feedback](../includes/feedback.md)]