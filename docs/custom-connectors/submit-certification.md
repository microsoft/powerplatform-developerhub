---
title: Get your connector certified | Microsoft Docs
description: Learn the benefits of certifying your Power Platform connector and certification criteria to make it available to all users in Azure Logic Apps, Power Automate, and Power Apps.
author: schabungbam
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: article
ms.date: 06/08/2022
ms.author: sameerch
ms.reviewer: angieandrews
---

# Get your connector certified

> [!Note]
> This topic is part of a tutorial series on creating and using custom connectors in Azure Logic Apps, Power Automate, and Power Apps. Make sure you read [Verified publisher certification process](certification-submission.md) or [Independent publisher certification process](certification-submission-ip.md) to understand the process.

To make a custom connector publicly available for all users in Logic Apps, Power Automate, and Power Apps, submit your connector to Microsoft for certification. Microsoft will review the connector and, if it meets certification criteria, approve it for publishing. For the full list of publicly available connectors, go to [Connector reference overview](/connectors/connector-reference/).

## What are the benefits of certifying a connector?

There are many benefits to building and certifying a connector for Power Automate, Power Apps, and Logic Apps. These include the ability to expand the reach of your API to users of Microsoft Power Platform, provide low-friction options for your users to interact with your service, and more.

Both independent publishers and verified publishers share many benefits although there are some differences. For a description of the types of publishers, go to [Who is eligible to get a connector certified?](#who-is-eligible-to-get-a-connector-certified) later in this article.

### For independent publishers

- **Certify for free.** There's no cost to register, go through the certification process, or update your connector after certification—no matter how many times you update it.

- **Belong to the official list of Microsoft connectors.** In addition to being listed in the public connector documentation, the connector will be showcased on certified connector pages on the Power Automate and Power Apps websites. Each connector will receive its own page on the site.

- **Gain visibility through having your name across Power Platform products and documentation.** You and/or your company will be listed as the official publisher of the connectors you submit.

- **Unlock many marketing benefits.** In addition to being featured in the new connectors blog post, you'll be featured in YouTube videos, monthly demos, and social media platforms.

- **Obtain thorough review and technical feedback.** Our team of engineers gained insights based on launching over 400 connectors to date. They'll help unblock you through technical support and perform a technical review before the connector goes public. This will ensure its success and reliability.

> [!IMPORTANT]
> If you want to certify your connector as an *independent publisher*, go to [Independent publisher certification process](certification-submission-ip.md).

### For verified publishers

- **Certify for free.** There's no cost to register, go through the certification process, or update your connector after certification—no matter how many times you update it.

- **Integrate your API with Microsoft Power Platform.** Have your customers asked for integrations for Power Automate, Power Apps, or Logic Apps?  Microsoft Power Platform provides citizen developers with the capabilities to build low code or no code apps and powerful automation with our growing family of connectors.

- **Belong to the official list of Microsoft connectors.** In addition to being listed in the public connector documentation, the connector will be showcased on certified connector pages on the Power Automate and Power Apps websites. Each connector will receive its own page on the site.

- **Expand the reach of your API.** Enable Microsoft Power Platform users anywhere in the world to use your APIs and extend your solution without having to write a single line of code. Simply by clicking, a business user can create and share a multitude of solutions involving several connectors at the same time.

- **Drive the usage of your API through templates.** Templates are a combination of connectors that offer predefined automation to target specific scenarios. By publishing a template, you'll streamline the experience for your users and inspire more ways to use your connector on the platform. This will simultaneously increase the reach, discoverability, and usage of your service.

- **Receive dedicated support throughout the certification process.** A dedicated Microsoft contact will personally review your submission, provide feedback, and help guide you through the certification process every step of the way.

- **Obtain thorough review and technical feedback.** Our team of engineers gained insights based on launching over 400 connectors to date. They'll help unblock you through technical support and perform a technical review before the connector goes public. This will ensure its success and reliability.

- ***(Doesn't apply to independent publishers.)*** **View connector health and usage telemetry any time.** In [ISV Studio](https://isvstudio.powerapps.com/home), you can view the monthly success rate in terms of API responses, the monthly active connections, and the monthly request count for your certified connector. For more information, go to [Gain key insights on your certified connector in the ISV portal](https://flow.microsoft.com/en-us/blog/gain-key-insights-on-your-certified-connector-in-the-isv-portal/).

- **Market your product to the Microsoft community.** After the connector is certified, we'll highlight it in our monthly Power Automate new connector blog post. This will drive visibility of the launch to users. To view our blog, go to the [Power Automate blog](https://flow.microsoft.com/en-us/blog/).

- **Get a social media shoutout.** We'll provide more exposure of your launch by sharing the blog post. In addition, we'll share an image that highlights your brand on our Power Automate Twitter account.

## Who is eligible to get a connector certified?

Each of the two types of connector publishers has a different path toward certification:

- **Verified publisher**: Owns the underlying service behind their connector, among other differences. An example is Adobe, who has certified the Adobe Sign connector.

- **Independent publisher**: Does *not* own the underlying service behind their connector. An example is Jon Smith, a member of the public, who can submit the HubSpot connector for certification. This type allows those in the community to be able to partake in the Power Platform connector ecosystem​.

> [!IMPORTANT]
> Regardless of the type of publisher, connectors to an *external infrastructure* are automatically assigned as *Premium tier connectors*.

### Publisher experience

The following image shows the main tasks in the publisher experience for certifying a connector.

![Publisher experience](media/submit-certification/publisher-experience.png "Publisher experience")

Make sure you follow the links in the **Next step(s)** section toward the end of each article for details on how to complete the next task. You can also find the links in the **Table of contents** under the **Certify your connector** heading.

## What are the requirements for certification?

To qualify for certification regardless of the type of publisher you are, you must meet the following requirements:

| Capability | Details | Required or recommended |
|------------|---------|-------------------------|
| Software as a service (SaaS) app |  *(Doesn't apply to independent publishers.)* You must either own the underlying service, or present explicit rights to use the API and provide a user scenario that fits well with our products. | Required   |
| Authentication type | Your API must support one of: OAuth2, anonymous authentication, API key, or basic authentication. | Required |
| Support | You must provide a support contact so that customers can get help. | Required |
| Availability and uptime | Your app has a minimum of least 99.9% uptime. | Recommended |

If you have any questions on certification development, go to the [community discussion forum](https://powerusers.microsoft.com/t5/General-Power-Automate/bd-p/MPAForum).

### Next steps

- (Independent publishers) [Independent publisher certification process](certification-submission-ip.md)<br/>
- (Verified publishers) [Verified publisher certification process](certification-submission.md)

[!INCLUDE[feedback](../includes/feedback.md)]