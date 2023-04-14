---
title: Test your connector in certification | Microsoft Docs
description: Learn how to test connector artifacts during Microsoft certification.
author: Amjed-Ayoub
manager: maheshp
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: article
ms.date: 08/16/2022
ms.author: v-amjedayoub
ms.reviewer: angieandrews
---

# Test your connector in certification

> [!Note]
> This topic contains information for certifying custom connectors in Azure Logic Apps, Power Automate, and Power Apps. Make sure you read [Verified publisher certification process](certification-submission.md) or [Independent publisher certification process](certification-submission-ip.md) to understand the process.

During certification, we'll deploy your custom connector to our Preview region, where you'll need to validate its functionality and content.

## Create an environment in the Preview region

You'll need to create an environment in the **Preview (United States)** region to conduct detailed testing before we deploy your connector to all public regions.

> [!Important]
> If you're an internal Microsoft publisher, you *don't* need to create an environment. Use **MS Personal Productivity (msdefault)** as the environment. Then, go to [Test your connector](#test-your-connector).

To create an environment:

1. Make sure you have a Power Automate or Power Apps license, trial license, or a subscription to the Developer Plan.

    If you don't already have a license, you can use the Power Apps trial by going to the [Power Apps trial page](https://make.powerapps.com/trial). Use a work or school Microsoft account to sign in and sign up. Trial environments expire after 30 days. You'll need to re-create the environment with a new license in the future.

    As an alternative, you can use the [Developer Plan](https://powerapps.microsoft.com/en-us/developerplan/) to build and test apps to validate prior to production. This service is a free lifetime subscription with no expiration date.

1. Go to the [Power Platform admin center](https://admin.powerplatform.microsoft.com/environments).

1. In the upper-left corner, select **Environments** > **New**.

1. Enter a name for your test environment.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of naming your new environment.](media/certification-testing/new-env-1.png "Name your new environment")

1. If you have a trial license, select **Trial** as the environment type. If you hold any other Power Automate license, select **Production**. Don't select **Sandbox**.

    If you want to find out your license type, go to [Check app license designation from app details](/powerapps/maker/canvas-apps/license-designation#check-app-license-designation-from-app-details).

   > [!div class="mx-imgBorder"]
   > ![Screenshot of environment types.](media/certification-testing/new-env-2.png "Environment types")

1. Select **Preview (United States) - Default** as the region, regardless of the region you're in.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of the Preview (United States) default region selection.](media/certification-testing/new-env-3.png "Preview (United States) default region")

1. *(Optional)* In the **Purpose** field, enter the purpose of the new environment.

1. Ensure that **Create a database for this environment?** is set to **No**. You're not required to create a database for a test environment.

   > [!div class="mx-imgBorder"]
   > ![Screenshot of Create a database for this environment set to no.](media/certification-testing/new-env-4.png "Create a database for this environment")

1. Select **Save**.

If you're unable to create an environment, contact your tenant administrator.

## Test your connector

After your connector passes certification review, you'll be notified that your connector has been deployed to the Preview region for testing.

1. Sign in to [Power Automate](https://us.flow.microsoft.com/).

1. In the left side menu, select **Connectors**.
 
1. In the right side panel, find and select your connector.

    If you don't see your connector, make sure you're in the environment you created in the **Preview (United States)** region.

    > [!Important]
    > If you're testing in Logic Apps, use the **West Central US** location when you create the resource group for your test.

1. If your connector uses OAuth2 authentication, be sure that you've provided a valid production **ClientID** and **ClientSecret**. If you need to edit this information, notify your Microsoft contact.

1. Ensure that you're testing the correct connector during this process. This must be the certified version we've deployed in preview, not your custom connector.

1. Conduct extensive testing to ensure that your connector is ready to be deployed to all public regions:

    1. Create new flows in the environment.

    1. Test all connector fields and operations.

    The **See documentation** link won't function until the connector has been fully deployed in production.

1. After testing is complete, notify your Microsoft contact.

## After you test your connector

After you notify Microsoft that you've completed testing, Microsoft will do the following:

 - Deploy your connector on the Premium tier to all public regions.
    - This process is expected to take 5 to 6 weeks.
    - Microsoft deploys incrementally in regions around the world.
    - The Premium tier can't be removed or changed at any time.

- If there are any issues, we'll contact you, so please visit your connector regularly.
    - We may need to contact you multiple times. 

- We'll send you email with deployment notifications when there's a change.

When deployment is complete, any user with a Power Automate license will be able to use your certified connector.

### See also

If you'd like to make updates to your connector, go to [Update your certified connector](certification-updates.md).

[!INCLUDE[feedback](../includes/feedback.md)]