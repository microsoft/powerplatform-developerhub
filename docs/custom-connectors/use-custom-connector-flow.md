---
title: Use a custom connector from a flow | Microsoft Docs
description: Call a custom connector from a flow you create with Power Automate.
author: sunaysv
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: sunayv
ms.reviewer: angieandrews
---

# Use a custom connector from a flow

> [!Note]
> This topic is part of a tutorial series on creating and using custom connectors in Azure Logic Apps, Power Automate, and Power Apps. Make sure you read the [custom connector overview](index.md) to understand the process.

In this topic, you build a basic flow that uses the custom connector you created in a previous topic. The flow is triggered when an item is added to a SharePoint list, then the flow uses the custom connector to call the Cognitive Services Text Analytics API. The connector returns the sentiment score (0 to 1) for the text in the list item, and the flow writes the score back to the list. The following image shows the finished flow:

![Finished sentiment analysis flow](./media/use-custom-connector-flow/finished-flow.png)

## Prerequisites

* An [Office 365 Business Premium subscription](https://products.office.com/business/get-office-365-for-your-business-with-latest-2016-apps?&WT.srch=1&wt.mc_id=AID623587_SEM_AhsLj214), or sign up for a [free trial](https://signup.microsoft.com/Signup?OfferId=467eab54-127b-42d3-b046-3844b860bebf&dl=O365_BUSINESS_PREMIUM&ali=1)
* Basic experience building flows. For more information, see [Create a flow from scratch](/flow/get-started-logic-flow).
* The custom connector that you created in one of these topics:
    * [Create a custom connector from an OpenAPI definition](define-openapi-definition.md)
    * [Create a custom connector from a Postman collection](define-postman-collection.md)
    * [Create a custom connector from scratch](define-blank.md)

## Create the SharePoint list

You first create a simple three-column list in SharePoint Online; this list stores movie review data that the flow analyzes for sentiment. For more information about SharePoint lists, see [Introduction to lists](https://support.office.com/article/Introduction-to-lists-0A1C3ACE-DEF0-44AF-B225-CFA8D92C52D7) in the SharePoint documentation.

1. In your SharePoint Online site, choose **New**, then **List**.
   
    ![Create new SharePoint list](./media/use-custom-connector-flow/new-list.png)

2. Enter the name **Movie Reviews**, then choose **Create**.
   
    ![Specify name for new list](./media/use-custom-connector-flow/create-list.png)
   
    The list is created, with the default **Title** field.
   
    ![Project Requests list](./media/use-custom-connector-flow/initial-list.png)

3. Choose ![New item icon](./media/use-custom-connector-flow/icon-new.png), then **Single line of text**.
   
    ![Add single line of text field](./media/use-custom-connector-flow/add-column.png)

4. Enter the name **Review**, then choose **Save**.

5. Repeat steps **3.** and **4.** to add another column to the list: use a data type of **Number**, and the name **Score**.

## Create a flow from the list 

SharePoint Online provides the ability to create flows right from a list, so we'll use that approach. You could create the same flow from [flow.microsoft.com](https://flow.microsoft.com).

1. In the SharePoint list, choose **Flow** then **Create a flow**.

    ![Create a flow](./media/use-custom-connector-flow/create-flow.png)

2. In the right pane, choose **Show more**.

    ![Show more flows](./media/use-custom-connector-flow/show-more.png)

3. Choose the template **When a new item is added in SharePoint, complete a custom action**.

    ![Custom action for SharePoint](./media/use-custom-connector-flow/custom-action.png)

    The custom action in this case is calling the API through the custom connector.

4. Ensure you're signed in to SharePoint with the correct account, then choose **Continue**. 

    ![SharePoint connector permissions](./media/use-custom-connector-flow/sharepoint-permissions.png)

5. In [flow.microsoft.com](https://flow.microsoft.com), choose **Edit**. You should see the SharePoint site and list from which you launched the **Create a flow** process.

    ![Confirm site and list](./media/use-custom-connector-flow/confirm-list.png)

## Add the custom connector

Power Automate created a basic flow with a trigger that fires when an item is added to the SharePoint list. Now you add actions to take based on the item that is added.

1. Choose **New step**, then **Add an action**.

2. Search for the connector you created, then choose the action associated with that connector. 

    ![Choose SentimentDemo action](./media/use-custom-connector-flow/text-analytics-action.png)

    The name and description for the action come from information you provided when you created the connector.

3. Enter values for all the fields.

    ![Connector parameters](./media/use-custom-connector-flow/connector-parameters.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Language** | "en" | 
   | **ID** | "1" |
   | **Text** | The SharePoint **Review** field (from the **Dynamic content** dialog box) |
   ||

    **ID** is required because the connector can handle multiple documents; in these examples, you send one document at a time. In a production flow, **Language** and **ID** values might come from a list or another data source.

4. Choose **New step** then **Add an action**.

5. Add the action **SharePoint - Update item** and enter values for all the fields.

    ![Update list item action](./media/use-custom-connector-flow/update-list-item-action.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Site Address** | The address of the SharePoint Online site from which you launched the **Create a flow** process | 
   | **List name** | The list from which you launched the **Create a flow** process |
   | **Id** | The SharePoint **ID** field |
   | **Title** | The SharePoint **Title** field | 
   | **Review** | The SharePoint **Review** field |
   | **Score** | The custom connector **score** field |
   ||

   When you add the dynamic content for the **Score** field, notice that Power Automate adds an **Apply to each** container because it recognizes that the custom connector accepts multiple documents. Your flow only sends one at a time, but it's cool that the flow matches the capabilities of the connector.

    ![Apply to each](./media/use-custom-connector-flow/apply-to-each.png)

    The finished flow should now look like the following image:

    ![Finished sentiment analysis flow](./media/use-custom-connector-flow/finished-flow.png)

6. Enter a name for your flow, like **Sentiment Analysis**, then choose **Create flow** and **Done**.

## Test the flow

Now that the flow is complete, it's time to test it by adding reviews to the SharePoint list, and seeing how the flow responds.

1. In your SharePoint Online list, choose **Quick Edit**.

    ![SharePoint list quick edit](./media/use-custom-connector-flow/quick-edit.png)

2. Add two reviews to the list (one negative and one positive), then choose **Done**.

    ![Quick edit done](./media/use-custom-connector-flow/quick-edit-done.png)

   | Parameter | Suggested value | 
   | --------- | --------------- | 
   | **Title** (positive) | My Favorite Movie | 
   | **Review** (positive) | "I enjoyed the new movie after a long day" |
   | **Title** (negative) | Some Other Movie | 
   | **Review** (negative) | "The worst movie I've seen in decades" |
   ||

3. In [flow.microsoft.com](https://flow.microsoft.com), choose **My flows**, then choose the flow you created.

    ![Choose new flow](./media/use-custom-connector-flow/choose-new-flow.png)

4. Look at **RUN HISTORY**, and you should see two runs&mdash;one for each review you added to the list.

    ![Run history](./media/use-custom-connector-flow/run-history.png)

5. Back on the SharePoint list page, refresh the browser to see the scores that the flow added.

    ![Scores added from flow](./media/use-custom-connector-flow/scores-added.png)

You're all done! This is a simple flow, but it gains powerful functionality by being able to call Cognitive Services through a custom connector.

## Next steps

Share the connector within your organization and/or get the connector certified so that people outside your organization can use it:

* [Share your connector](share.md)
* [Certify your connector](submit-certification.md)

[!INCLUDE[feedback](../includes/feedback.md)]
