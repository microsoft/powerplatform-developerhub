---
title: Independent publisher certification process | Microsoft Docs
description: A tutorial for submitting connector artifacts for Microsoft certification by an independent publisher.
author: bezhan-msft
manager: maheshp
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: article
ms.date: 06/28/2022
ms.author: bezhan
ms.reviewer: angieandrews
---

# Independent publisher certification process

This process is for [independent publishers](submit-certification.md#who-is-eligible-to-get-a-connector-certified). If you own the underlying service to your connector, go to [Verified publisher certification process](certification-submission.md).

> [!NOTE]
> This topic provides information for certifying independent publisher connectors in Azure Logic Apps, Power Automate, and Power Apps. *Before* following the steps in this article, read [Get your connector certified](submit-certification.md).

After you’ve identified a custom connector to build to the Independent Publisher program, follow these steps to prepare it for certification and generate the connector files to submit to Microsoft. The submission process is done through the [GitHub repository](https://github.com/microsoft/PowerPlatformConnectors/tree/dev/independent-publisher-connectors) for independent publisher connectors.

## Read the manifesto

The Independent Publisher Connector Group makes it possible for anyone to publish a connector to the official list of Microsoft connectors. This group asks that participants are aware of and agree to the content of its [manifesto](https://github.com/microsoft/PowerPlatformConnectors/wiki/Independent-Publisher-Connector-Group-%22Manifesto%22). **Please read and understand this document** if you plan on contributing to or being a part of this effort.

## Prepare and submit your connector for certification

The process for certifying your connector as an independent publisher is easy. Before submitting it in the [GitHub repository](https://github.com/microsoft/PowerPlatformConnectors/tree/dev/independent-publisher-connectors), make sure you perform the steps in this section.

By submitting your independent publisher connector, your name will be featured within our product as the official publisher and a generic icon will be applied to your connector.

> [!IMPORTANT]
> If you're considering building and publishing a connector to a Microsoft first party service, email [connectorpartnermgmtteam@service.microsoft.com](mailto:connectorpartnermgmtteam@service.microsoft.com) with an explanation of the connector and its endpoints. We'll work with you to find publishing options.

1. **Before you start building a connector, share your connector proposal with Microsoft.** Verify that the connector hasn't already been built by searching for the connector in the Power Platform documentation and in the pull requests of the GitHub repository. The following table gives you options on what you can do with your connect based on its status.

    |If your proposed connector  |Option  |
    |---------|---------|
    |Already exists on the Power Platform.  | You can't build the connector.        |
    |Already exists as an independent publisher connector. | You can add more functionality to the connector.  |
    |Is currently a pull request *and* a proposal. | You can contact the independent publisher to collaborate with them on the connector.  |
    |Is a pull request *and* isn't a proposal.     | Wait until the connector is certified and deployed and then add an update to that connector file.  |

1. After you’ve verified that the connector isn't on the platform, share your connector proposal on the GitHub repository. This will avoid duplicating efforts with your peers who might have the same idea for a new connector.  It might also create an opportunity for you to find others to collaborate with as you build your connector.


    To share your proposal, submit a pull request in the [GitHub repository](https://github.com/microsoft/PowerPlatformConnectors/tree/dev/independent-publisher-connectors) with the following criteria:

    - Title the pull request "**Proposal - *\<Connector Name>* (Independent Publisher)**". For example, Proposal - HubSpotCRM (Independent Publisher). 

    - Commit a **readme.md** file with as many details as you can provide, including your contact email (if you’re open to finding a collaborator).

    > [!NOTE]
    > Please use the same pull request when you’re ready to submit all of the files for certification in step 10.

 1. **Build the connector.** For instructions on how to build a connector, go to [Create a custom connector from scratch](define-blank.md).  

1. **Create a title for your connector that meets Microsoft requirements.** For instructions and examples, go to [Give your connector a title](certification-submission.md#give-your-connector-a-title).

1. **Write a description for your connector.** For instructions, go to [Write a description for your connector](certification-submission.md#write-a-description-for-your-connector).

1. **Define summaries and descriptions.** For instructions, go to [Define operation and parameter summaries and descriptions](certification-submission.md#define-operation-and-parameter-summaries-and-descriptions).

1. **Define exact operation responses.** For instructions, go to [Define exact operation responses](certification-submission.md#define-exact-operation-responses).

1. **Add metadata describing the connector and its end service.** For instructions, go to [Add metadata](certification-submission.md#step-3-add-metadata).

1. **Prepare the connector artifacts.** For instructions, go to [Prepare the connector artifacts](certification-submission.md#step-4-prepare-the-connector-artifacts).

1. **Submit your connector for deployment.**

    1. Submit the connector artifacts to the pull request you created in Step 2, fill out the checklist in the pull request template, and remove "**Proposal -**" from your pull request title.

    1. A Microsoft certification engineer will provide feedback within **1-2 weeks** of your initial request. If the feedback requires an update to the connector, you'll need to submit an update to the pull request. Allow an extra **1-2 weeks** for this. If you need to troubleshoot swagger errors, go to [Fix Swagger Validator errors](certification-swagger-validator-rules.md).
    
        You'll retain ownership of your connector, and can accept or reject any changes to your connector.

    1. Microsoft will approve and merge the pull request.

1. **Your connector will be submitted for certification**. During certification, expect to hear from your Microsoft contact within **1-2 weeks**.

1. **Wait for deployment.** After your connector is validated and prepared for production deployment, we'll deploy it to all production regions.

    > [!IMPORTANT]
    > On average, it takes 15 business days to deploy the connector. This is required regardless of the size or complexity of your connector, whether it's new or an update. In order to protect integrity, the connector will be subjected to the same validation tasks to test functionality and content that are followed in every deployment.

    - **Deployment schedules**: Our connector deployment schedules for production start Friday mornings, PST/PDT. Notify your Microsoft contact when you're ready for production deployment at least 24 hours in advance for us to include your connector in the next scheduled deployment.

    - **Region deployment**: We'll notify you by email with the names of the regions the connector will be deployed to, as deployment to regions is done in steps. If there's a deployment delay or freeze, you'll be notified by email. More information: [Region deployment](certification-submission.md#region-deployment)

   As your connector is finishing certification, we'll engage you about a marketing opportunity for the connector on the [Power Automate blog](https://flow.microsoft.com/en-us/blog/).

1. **Update your connector anytime.** Once your certification ends and your connector is published, you can add new operations and functionalities to your connector in the GitHub repository anytime. We'll recertify your connector as needed.

## Best practices for submission

- You can only submit one connector per pull request. This ensures that our validation process runs smoothly.

- The pull request for your connector should follow the pattern **Connector Name (Independent Publisher)**.

- Add an email to the support email section. This is in case we need to contact you.

- Make sure to fill in the privacy policy parameter with the privacy policy for the end service.

- Make sure that your operation descriptions are detailed. This ensures that the user can understand your operation.

- If your connector uses OAuth, make sure you provide detailed steps on how to create an app in your readme.md. Failure to do so will result in delays in certification. For an example of documentation to include, go to the [Readme.md example](https://github.com/microsoft/PowerPlatformConnectors/blob/dev/custom-connectors/AzureKeyVault/Readme.md).

- Make sure that you add response schemas to your actions, unless the response schema is dynamic. This will ensure that your connector gets more usage.

- Review the [Checklist before submitting](certification-submission.md#checklist-before-submitting).

## Microsoft guarantees

Microsoft commits to satisfying the following guarantees:

- If there’s an update to the connector, we'll run the breaking change tool and all other validation tools over again.​

- If there’s no update to a connector, we guarantee that it works unless there's an API change or update, or a platform issue.​

- Microsoft will investigate platform and security issues as they arise and deprecate broken independent publisher connectors.​

## Next step

[Test your connector in certification](certification-testing.md)

[!INCLUDE[feedback](../includes/feedback.md)]
