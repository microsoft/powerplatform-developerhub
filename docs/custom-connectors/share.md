---
title: Share a custom connector in your organization | Microsoft Docs
description: Make connectors available to users in your organization.
author: sunaysv
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: sunayv
ms.reviewer: angieandrews
---

# Share a custom connector in your organization

> [!Note]
> This topic is part of a tutorial series on creating and using custom connectors in Azure Logic Apps, Power Automate, and Power Apps. Make sure you read the [custom connector overview](index.md) to understand the process.

Now that you have a custom connector, you might want to enable other people to use it. People within your organization can use the custom connector just like they use other Microsoft-managed connectors. We cover the details in this topic.

To share your connector with people outside your organization, such as all Logic Apps, Power Automate, and Power Apps users, [certify your connector](submit-certification.md).

> [!IMPORTANT]
> If you share a connector, others might start to depend on that connector. ***Deleting your connector deletes all connections to that connector.***

## Sharing in Logic Apps

Custom connectors in Logic Apps are visible and available to the connector's author and users who have the same Azure Active Directory tenant and Azure subscription for logic apps in the region where those apps are deployed.

## Sharing in Power Apps and Power Automate

By default, custom connectors in Power Apps and Power Automate aren't available to other users. You can share them directly, and they are also shared if you [share an app](/powerapps/share-app) or create a [team flow](/flow/create-team-flows) that uses the connector.

1. Go to [make.powerapps.com](https://make.powerapps.com) or [flow.microsoft.com](https://flow.microsoft.com).

2. In the navigation pane, select **Data > Custom connectors**.

   <kbd>![Select custom connector](./media/share/custom-connector.png)

3. Choose the ellipsis button (**. . .**) for your connector, then choose **Invite another user**.  

   ![Invite another user](./media/share/invite-user.png)

4. On the **Share** tab, under **Add people**, enter the users or groups you want to share with. Choose a permission of **Can view** or **Can edit**, then choose **Save**.

   ![Share custom connector](./media/share/sharecustomapi.png)

### More information

If you want to share a custom connector with users outside your organization, [submit your connectors for Microsoft certification](submit-certification.md).

[!INCLUDE[feedback](../includes/feedback.md)]