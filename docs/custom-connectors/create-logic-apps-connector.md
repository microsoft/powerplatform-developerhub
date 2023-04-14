---
title: Create a custom connector in Azure Logic Apps | Microsoft Docs
description: Create an Azure Logic Apps custom connector in the Azure portal.
author: divyaswarnkar
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: divswa
ms.reviewer: angieandrews
---

# Create a custom connector in Azure Logic Apps

> [!Note]
> This topic is part of a tutorial series on creating and using custom connectors in Azure Logic Apps, Microsoft Power Automate, and Microsoft Power Apps. Make sure you read the [custom connector overview](index.md) to understand the process.

In Azure Logic Apps, you must first create the custom connector resource (by performing the following steps) before defining the behavior of the
connector by using an OpenAPI definition or a Postman collection.

1. In the Azure portal, on the **Azure services** menu, select **Create a resource**. 

2. In the **New** dialog, enter **logic apps custom connector** in the search box, and then select **Logic Apps Custom Connector** from the dropdown list.

    <kbd>![Search for logic apps custom connector.](./media/create-logic-apps-connector/search-logic-apps-connector.png)

3. In the **Logic Apps Custom Connector** dialog, select **Create**.

    <kbd>![Create a Logic Apps custom connector.](./media/create-logic-apps-connector/create-logic-apps-connector.png)

4. Enter the details for registering your connector as described in the following table, and then select **Review + create**.

    <kbd>![Registering your connector.](./media/create-logic-apps-connector/logic-apps-connector-details.png)

   | Property | Description | Example text | 
   | -------- | ----------- | ------- | 
   | **Subscription** | Azure subscription for the connector | Contoso Azure Subscription #1 | 
   | **Resource group** | Azure resource group for the connector | logic-apps-custom-connectors | 
   | **Name** | Name of your custom connector | SentimentDemo | 
   | **Location** | Same Azure region as your logic app | (US) West US |

5. In the **Create Logic Apps Custom Connector** dialog, review the details you've entered, and then select **Create**.

    <kbd>![Reviewing the Logic Apps custom connector.](./media/create-logic-apps-connector/review-and-create.png)

After you've created the custom connector resource, the custom connector menu should open automatically. If it doesn't, you can go to the subscription and resource group you selected and open it directly.

Now that you've created a custom connector, you can define the connector's behavior.

### See also

[Define a custom connector using an OpenAPI definition](define-openapi-definition.md)  
[Define a custom connector using a Postman collection](define-postman-collection.md)

[!INCLUDE[feedback](../includes/feedback.md)]