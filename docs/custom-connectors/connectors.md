---
title: Power Platform connectors overview | Microsoft Docs
description: This topic provides an overview of Power Platform connectors, including connector architecture, components, and the Microsoft products you can use them in. 
author: asavaritayal
ms.custom: intro-internal
ms.author: angieandrews
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 07/25/2022
ms.reviewer: angieandrews
---

# Connectors overview

> [!NOTE]
> Fill this survey to help us understand your Power Platform connectivity needs and scenarios, and improve your connectivity experience: <https://aka.ms/connectorscenarios>.

A *connector* is a proxy or a wrapper around an API that allows the underlying service to talk to [Microsoft Power Automate](https://flow.microsoft.com), [Microsoft Power Apps](https://powerapps.microsoft.com), and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps). It provides a way for users to connect their accounts and leverage a set of prebuilt [*actions*](#actions) and [*triggers*](#triggers) to build their apps and workflows.

Our large ecosystem of software as a service (SaaS) connectors enables you to connect apps, data, and devices in the cloud. Examples of popular connectors include Salesforce, Office 365, Twitter, Dropbox, Google services, and more.

## Architecture

### Runtime flow

![Connector architecture - runtime flow](architecture.png)

### Architecture components

Here are the architecture components and what they do:

- **Credential and metadata store**&mdash;A service to store connector metadata (swagger, connection, ACLs, etc.), and credentials associated with a connection.

- **Connector**
   - Azure APIM (API Manager) to host all the swagger and policies. In addition to being the entry point for all calls that interact with the connector calls, Azure APIM verifies keys, tokens, certificates, and other credentials.
   - App Service Environment to host connector webapps.

## Connector components

Each connector offers a set of operations classified as *actions* and *triggers*. Once you connect to the underlying service, these operations can be easily leveraged within your apps and workflows.

### Actions

Actions are changes directed by a user. For example, you would use an action to look up, write, update, or delete data in a SQL database. All actions directly map to operations defined in the swagger.

### Triggers

Several connectors provide triggers that can notify your app when specific events occur. For example, the FTP connector has the OnUpdatedFile trigger. You can build either a Logic App or a flow that listens to this trigger and performs an action whenever the trigger fires.

There are two types of triggers:

- **Polling Triggers**&mdash;These triggers call your service at a specified frequency to check for new data. When new data is available, it causes a new run of your workflow instance with the data as input. 
	
- **Push Triggers**&mdash;These triggers listen for data on an endpoint, that is, they wait for an event to occur. The occurrence of this event causes a new run of your workflow instance.

>[!Note]
> Triggers are not supported in Power Apps. Learn how to [start a flow in an app](https://powerapps.microsoft.com/tutorials/using-logic-flows/).

## Use connectors

Connectors are available for use in multiple products.

[![Power Automate Logo](flow.png)](https://flow.microsoft.com/)

### Power Automate
Work smarter by building workflows and automating processes across your apps and services. Streamline notifications, sync data between systems, automate approval, and more.

Learn how to [build flows](/power-automate/multi-step-logic-flow) and [manage your connections](/power-automate/add-manage-connections).

[![Power Apps Logo](powerapps.png)](https://powerapps.microsoft.com/)
### Power Apps
Power Apps enables users to build cloud connected and cross platform business apps using clicks and minimal code. Create rich user experiences across the web, phones, and tablets. Assemble forms, add business logic, and take advantage of device capabilities with full creative freedom.

Learn how to [create an app from scratch](https://powerapps.microsoft.com/tutorials/get-started-create-from-blank/), [use the formula builder](https://powerapps.microsoft.com/tutorials/working-with-formulas/), and [manage your connections](https://powerapps.microsoft.com/tutorials/add-manage-connections/).

[![Logic Apps](logicapps.png)](https://azure.microsoft.com/services/logic-apps/)

### Logic Apps

Logic Apps is the workflow engine for Power Automate. It enables pro-developers to visually create or programmatically configure workflows in Azure. A connector in Logic Apps enables users to automate EAI, Business to business (B2B), and Business to consumer (B2C) scenarios while reaping the benefits of source control, testing, support, and operations.

In Logic Apps, you can use [enterprise connectors](/azure/connectors/apis-list) to [create logic app workflows](/azure/logic-apps/logic-apps-create-a-logic-app) and automate processes between cloud apps and cloud services.

## Custom connectors

We offer a wide variety of connectors, but sometimes you might want to call APIs, services, and systems that aren't available as prebuilt connectors. To support more tailored scenarios, you can build *custom connectors* with their own triggers and actions. These connectors are *function-based*&mdash;data is returned based on calling specific functions in the underlying service.

### See also

- [Integrate your service with Microsoft products and services](/connectors/isv/)
- [List of all connectors](/connectors/connector-reference#list-of-connectors)

[!INCLUDE[feedback](includes/feedback.md)]