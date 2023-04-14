---
title:  What is policy and where is it used? | Microsoft Docs
description: Enable a customer connector to run transformations on request and response payload
author: shyamsu
services: connectors
ms.topic: conceptual
ms.service: connectors
ms.workload: connectors
ms.date: 06/08/2022
ms.author: shyamsu
ms.reviewer: angieandrews
---

# What is policy and where is it used?

Policies can be used to modify the behavior of connectors at runtime. For example, policies are used to enforce throttling limits on API calls to route calls to different endpoints, and so on. Policies are used extensively by many of the out-of-box connectors we have today, however, they haven't been available to custom connectors. We're now making it available for custom connectors through easy to use templates.

## Prerequisites

* Basic experience building [flows](/flow/get-started-logic-flow), and [custom connectors](define-openapi-definition.md).
* Basic understanding of the [OpenAPI Specification](http://swagger.io/specification/) (formerly known as Swagger).
* The [sample OpenAPI definition](https://procsi.blob.core.windows.net/docs/githubWebhookSample.json) for this tutorial.

## How to use a policy template

Policy template are available only for custom connectors. To use a policy template, open Power Automate portal and either create a new custom connector or edit an existing one. 

1. In the custom connector wizard, select the **Definition** page.  

    ![Select the Definition page](media/policy-template-overview/UX1.png)

2. From the **Definition** page, select **New Policy**.   

    ![Select new policy](media/policy-template-overview/UX2.png)

   This will open the policy templates page on the right.   
 
    ![Opening the policy templates page](media/policy-template-overview/UX3.png)

3. Select a policy template from the **Template** dropdown.

    ![Select a policy template](media/policy-template-overview/UX4.png)

4. Provide the parameter values required by the policy template. Repeat steps 2 through 4 for adding additional policy templates.  

    ![Provide the parameter values](media/policy-template-overview/UX5.png)

5. At the top right corner of the wizard, select **Create connector** (for new connector) or **Update connector** (for existing connector) to save the policy templates to your connector.

    ![Save the policy templates](media/policy-template-overview/UX6.png)

[!INCLUDE[feedback](../includes/feedback.md)]