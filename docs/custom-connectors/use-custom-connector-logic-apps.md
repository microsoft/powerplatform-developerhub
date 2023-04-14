---
title: Use a custom connector from a logic app | Microsoft Docs
description: Call a custom connector from a logic app you create with Azure Logic Apps.
author: divyaswarnkar
manager: maheshp
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: divswa
ms.reviewer: angieandrews
---

# Use a custom connector from a logic app

> [!Note]
> This topic is part of a tutorial series on creating and using custom connectors in Azure Logic Apps, Power Automate, and Power Apps. Make sure you read the [Custom connector overview](index.md) to understand the process.

In this topic, you build a basic logic app that uses the custom connector you created in a previous topic. The logic app is triggered when an item is added to a SharePoint list, then the logic app uses the custom connector to call the Cognitive Services Text Analytics API. The connector returns the sentiment score (0 to 1) for the text in the list item, and the logic app writes the score back to the list. The following image shows the finished logic app:

![Finished sentiment analysis logic app](./media/use-custom-connector-logic-apps/finished-logic-app.png)

## Prerequisites

* An [Office 365 Business Premium subscription](https://products.office.com/business/get-office-365-for-your-business-with-latest-2016-apps?&WT.srch=1&wt.mc_id=AID623587_SEM_AhsLj214), or sign up for a [free trial](https://signup.microsoft.com/Signup?OfferId=467eab54-127b-42d3-b046-3844b860bebf&dl=O365_BUSINESS_PREMIUM&ali=1)

* Basic experience building logic apps. For more information, see [Build your first logic app workflow](/azure/logic-apps/quickstart-create-first-logic-app-workflow).

* The custom connector that you created in one of these topics:

  * [Create a custom connector from an OpenAPI definition](define-openapi-definition.md)
  * [Create a custom connector from a Postman collection](define-postman-collection.md)

* If your custom connector accesses on-premises resources by using the [on-premises data gateway](/azure/logic-apps/logic-apps-gateway-connection), you need to set up the gateway installation to allow access for the corresponding *managed connectors [outbound IP addresses](/azure/logic-apps/logic-apps-limits-and-config#outbound)*. *All* logic apps in the same region use the same IP address ranges. For more information, see [Install on-premises data gateway for Azure Logic Apps - Check or adjust communication settings](/azure/logic-apps/logic-apps-gateway-install#check-or-adjust-communication-settings).

## Create the SharePoint list

You first create a simple three-column list in SharePoint Online; this list stores movie review data that the logic app analyzes for sentiment. For more information about SharePoint lists, see [Introduction to lists](https://support.office.com/article/Introduction-to-lists-0A1C3ACE-DEF0-44AF-B225-CFA8D92C52D7) in the SharePoint documentation.

1. In your SharePoint Online site, choose **New**, then **List**.
   
    ![Create new SharePoint list](./media/use-custom-connector-logic-apps/new-list.png)

2. Enter the name "Movie Reviews", then choose **Create**.
   
    ![Specify name for new list](./media/use-custom-connector-logic-apps/create-list.png)
   
    The list is created, with the default **Title** field.
   
    ![Project Requests list](./media/use-custom-connector-logic-apps/initial-list.png)

3. Choose ![New item icon](./media/use-custom-connector-logic-apps/icon-new.png), then **Single line of text**.
   
    ![Add single line of text field](./media/use-custom-connector-logic-apps/add-column.png)

4. Enter the name "Review", then choose **Save**.

5. Repeat steps **3.** and **4.** to add another column to the list: use a data type of **Number**, and the name "Score".

## Create a logic app

Now that you have a list to work with, you create a logic app in the Azure portal.

1. Sign in to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> 
with your Azure account credentials.

2. From the main Azure menu, choose 
**New** > **Enterprise Integration** > **Logic App**.

   ![Create logic app](./media/use-custom-connector-logic-apps/create-logic-app.png)

3. Under **Create logic app**, provide details about your logic app as shown here. 
After you're done, choose **Pin to dashboard** > **Create**.

   ![Provide logic app details](./media/use-custom-connector-logic-apps/create-logic-app-settings.png)

   | Setting | Value | Description | 
   | ------- | ----- | ----------- | 
   | **Name** | SentimentAnalysis | The name for your logic app | 
   | **Subscription** | <*your-Azure-subscription-name*> | The name for your Azure subscription | 
   | **Resource group** | My-First-LA-RG | The name for the [Azure resource group](/azure/azure-resource-manager/resource-group-overview) used to organize related resources | 
   | **Location** | East US 2 | The region where to store your logic app information <p>**Note**: Your logic app and custom connector must exist in the same region. | 
   | **Log Analytics** | Off | Keep the **Off** setting for diagnostic logging. | 
   |||| 

3. After Azure deploys your app, the Logic Apps Designer opens and shows a page 
with an introduction video and commonly used triggers. Under **Templates**, 
choose **Blank Logic App**.

   ![Choose blank logic app template](./media/use-custom-connector-logic-apps/choose-logic-app-template.png)


## Add the trigger and custom connector

With the logic app created, you add a trigger that fires when an item is added to the SharePoint list. Then you add an action to take based on the added item.

1. In Logic Apps Designer, search for or select **SharePoint**, then the trigger **SharePoint - when an item is created**.

    ![SharePoint create item trigger](./media/use-custom-connector-logic-apps/create-trigger.png)

2. If prompted, sign in with your credentials for SharePoint.

3. Enter values for the SharePoint trigger.

    ![SharePoint create item trigger parameters](./media/use-custom-connector-logic-apps/trigger-parameters.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Site Address** | <*your-SharePoint-site-address*> | 
   | **List Name** | Movie Reviews |
   | **Interval** | 10 |
   | **Frequency** | Second |
   ||

4. Choose **New step**, then **Add an action**.

5. Search for the **SentimentDemo** custom connector you created, then choose the action associated with that connector. 

    ![Choose SentimentDemo action](./media/use-custom-connector-logic-apps/text-analytics-action.png)

    The name and description for the action come from information you provided when you created the connector.

6. Enter a name for the connection and the API key.

    ![Connection name and API key](./media/use-custom-connector-logic-apps/api-key.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Connection Name** | A name such as SentimentDemoConnection. | 
   | **API Key** | The API key for the Text Analytics API. For more information, see [Get an API key](index.md#get-an-api-key). |
   ||

7. Enter values for all the fields.

    ![Connector parameters](./media/use-custom-connector-logic-apps/connector-parameters.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Language** | en | 
   | **ID** | 1 |
   | **Text** | The SharePoint **Review** field (from the **Dynamic content** dialog box) |
   ||

    **ID** is required because the connector can handle multiple documents; in our examples, we send one document at a time. In a production logic app, **Language** and **ID** values might come from a list or another data source.

4. Choose **New step** then **Add an action**.

5. Add the action **SharePoint - Update item** and enter values for all the fields.

    ![Update list item action](./media/use-custom-connector-logic-apps/update-list-item-action.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Site Address** | <*your-SharePoint-site-address*> | 
   | **List name** | Movie Reviews |
   | **Id** | The SharePoint **ID** field |
   | **Title** | The SharePoint **Title** field | 
   | **Review** | The SharePoint **Review** field |
   | **Score** | The custom connector **score** field |
   ||

   When you add the dynamic content for the **score** field, notice that Logic Apps adds a **For each** container because it recognizes that the custom connector accepts multiple documents. Your connector only sends one at a time, but it's cool that the logic app matches the capabilities of the connector.

    ![Apply to each](./media/use-custom-connector-logic-apps/apply-to-each.png)

    The finished logic app should now look like the following image:

    ![Finished sentiment analysis logic app](./media/use-custom-connector-logic-apps/finished-logic-app.png)

6. In the top of the Logic Apps Designer, choose **Run**.

## Test the logic app

Now the logic app is complete, it's time to test it by adding reviews to the SharePoint list, and seeing how the logic app responds.

1. In your SharePoint Online list, choose **Quick Edit**.

    ![SharePoint list quick edit](./media/use-custom-connector-logic-apps/quick-edit.png)

2. Add two reviews to the list (one negative and one positive), then choose **Done**.

    ![Quick edit done](./media/use-custom-connector-logic-apps/quick-edit-done.png)

   | Parameter | Suggested value | 
   | --------- | --------------- | 
   | **Title** (positive) | My Favorite Movie | 
   | **Review** (positive) | I enjoyed the new movie after a long day |
   | **Title** (negative) | Some Other Movie | 
   | **Review** (negative) | The worst movie I've seen in decades |
   ||

4. Back in the Azure portal, choose **Overview** to look at the run history for this logic app. You should see two runs&mdash;one for each review you added to the list. To save Azure resources after you're done with this logic app, choose **Disable**.

    ![Run history](./media/use-custom-connector-logic-apps/run-history.png)

5. Back on the SharePoint list page, refresh the browser to see the scores that the logic app added.

    ![Scores added from logic app](./media/use-custom-connector-logic-apps/scores-added.png)

You're all done! This is a simple logic app, but it gains powerful functionality by being able to call Cognitive Services through a custom connector.

## Next steps

Share the connector within your organization and/or get the connector certified so that people outside your organization can use it:

* [Share your connector](share.md)
* [Certify your connector](submit-certification.md)

[!INCLUDE[feedback](../includes/feedback.md)]