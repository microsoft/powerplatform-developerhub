---
title: Update your certified connector for use in Azure Logic Apps, Power Automate, and Power Apps | Microsoft Docs
description: A tutorial to update your Microsoft certified connector for use in Azure Logic Apps, Power Automate, and Power Apps.
author: bezhan-msft
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: bezhan
ms.reviewer: angieandrews
---

# Update your certified connector

> [!Note]
> This topic contains information for certifying custom connectors in Azure Logic Apps, Power Automate, and Power Apps. Make sure you have a certified, released connector with Microsoft before following these steps.

To update your certified connector, you'll need to submit a new set of connector artifacts for certification. Follow the same instructions you used when first certifying your connector.

- If you're a verified publisher, go to [Verified publisher certification process](certification-submission.md) for instructions.
- If you're an independent publisher, go to [Independent publisher certification process](certification-submission.md) for instructions.

Also, ensure that your new connector version still adheres to our [guidelines and requirements](certification-submission.md#step-2-meet-submission-requirements).

The time it takes to complete the certification update process for your connector depends on if it has *major* or *minor* updates. A major update will take about 6-10 weeks for deployment. A minor update will take only 4-5 weeks. Microsoft engineers will evaluate your updates and make the final decision. This article describes the criteria for minor updates. If your connector doesn't meet any of the criteria, it will be considered a major update.

## Submit an update

> [!Important]
> You must open-source the connector before it gets added to the certification portal to reduce inconsistencies.
>
> For instructions, go to [Submit your connector for Microsoft certification](submit-for-certification.md#open-source-the-connector).

1. Ensure that you've [validated your connector files](certification-submission.md#step-4-prepare-the-connector-artifacts) before proceeding with your update submission. *Failure to do so can result in delays in the update process.*

1. For verified publishers excluding independent publishers: After you've gotten the new connector artifacts ready to submit, go back to the **Connector certification** tab in [ISV Studio](https://isvstudio.powerapps.com/home).

    You might have previously submitted artifacts to the Connector Certification Portal; the experience is now in [ISV Studio](https://isvstudio.powerapps.com/home).

1. Don't create a new submission for an update to an existing certified connector.

    Instead, select your existing submission from the list and then select **Submit a new version**. This will bring you back to the connector artifact submission area, where you can upload the new version of your connector artifacts.

1. After you submit your artifacts, we'll deploy and test your connector as we did for the first version. For more information, go to [Test your connector in certification](certification-testing.md).

    > [!Important]
    > Fifteen days are required for deployment regardless of the size or complexity of your connector, whether it's new or an update. In order to protect integrity, the connector will be subjected to the same validation tasks to test functionality and content that are followed in every deployment.
    >
    > For more information, go to [production schedule](certification-submission.md#step-7-wait-for-deployment).

1. Ensure that your testing environment in Preview is still accessible. If your environment was of Trial type, it might have expired, in which case you'll need to create a new environment.

### Notify your customer of breaking changes

If your new connector version changes the functionality of the operations within your connector&mdash;for example, changing an operation path&mdash;the update will introduce *breaking changes*. The connector will need special handling.

You'll need to provide customers with the breaking changes you're going to use in the new update. This will ensure customers who are using your currently published connector version will receive updates for the new updated version.

Generally, we suggest [operational versioning](operational-versioning.md) as a solution to breaking changes. This allows existing workflows to continue to work and introduces new versions of operations as needed. There are some cases of breaking changes that might not require operational versioning. Connect with your Microsoft contact to discuss these cases.

> [!Note]
> Changing the authentication type of your connector in an update isn't allowed.

Connect with your Microsoft contact with questions or if you experience any unexpected errors when trying to submit an update.

## Criteria of a minor update

Microsoft might consider your connector request a minor update. This would qualify it for an expedited completion time of 4-5 weeks for the certification process. A minor update will be considered for existing certified connectors only. It doesn't apply to new connector certification requests.

Changes to one or more of the following artifacts will qualify your connector as a minor update.


|Artifact  |Field  |Definition  |
|---------|---------|---------|
|ApiDefinition.swagger.json   | Connector information  | </li><li>Description<br/></li><li>Contact |
|ApiDefinition.swagger.json     |  Actions       | </li><li>Description<br/></li><li>Summary        |
|ApiDefinition.swagger.json     | Responses   | </li><li>Description<br/></li><li>Message’s description        |
|ApiProperties.json     | &mdash;   | </li><li>Description<br/></li><li>Tooltip<br/></li><li>iconBrandColor<br/></li><li>Publisher<br/></li><li>StackOwner        |  
|Readme file     |   &mdash;      | Content changes        |
|Icon   |  &mdash;       |  Replacement       |

[!INCLUDE[feedback](../includes/feedback.md)]
