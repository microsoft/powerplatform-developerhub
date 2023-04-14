---
title: Submit your connector to Microsoft for certification | Microsoft Docs
description: Submit your connector for Microsoft certification
author: bezhan-msft
manager: maheshp
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: article
ms.date: 06/08/2022
ms.author: bezhan
ms.reviewer: angieandrews
---

# Submit your connector for Microsoft certification

This topic outlines how to:

1. Publish your connector to the [open-source repository](https://github.com/microsoft/PowerPlatformConnectors).

1. Submit the commit information for certification in [ISV Studio](https://isvstudio.powerapps.com/home). *(This step doesn't apply to independent publishers.)*

In general, we require open-sourcing as part of our certification program. Ask your Microsoft contact if you have any concerns.

## Open-source the connector

This section applies to [verified publishers](submit-certification.md#who-is-eligible-to-get-a-connector-certified), *including* independent publishers.

> [!Important]
> You must open-source the connector before it gets added to the certification portal to reduce inconsistencies. If it’s your first time contributing to GitHub, attend the [Power Platform Samples - First Time Contributor](https://forms.office.com/pages/responsepage.aspx?id=KtIy2vgLW0SOgZbwvQuRaXDXyCl9DkBHq4A2OG7uLpdUMTFJWFFGVUxBNUFZQjZWRUdaOE5BMFkwNS4u) session.

1. Open a pull request in the [open-source repository](https://github.com/microsoft/PowerPlatformConnectors).

1. A Microsoft certification engineer will provide feedback within **1 to 2 weeks** of your initial request. If the feedback requires an update to the connector, you'll need to submit an update to the pull request. Allow an extra **1 to 2 weeks** for this.

    You'll retain ownership of your connector, and can accept or reject any changes to your connector.

1. Microsoft will approve and merge the pull request. As soon as your connector's pull request is merged, you can submit your connector artifacts to ISV Studio. Go to [Submit to ISV Studio](#submit-to-isv-studio) for instructions.

### Benefits to open-sourcing your connector

There are several benefits to open-sourcing your connector, including:

- Easily adding functionality to a previously out-of-the-box released, unchangeable connector.

- Taking advantage of contributions from the developer community for connector enhancements and maintenance.

- Enabling a platform for users to submit and contribute feature requests.

- Providing a richer tracking history for changes made to the connector.

- Enabling collaboration for multiple developers.

 The following points about our program are important:

- You'll only be open-sourcing your connector files, not your API. This data is already accessible to users through the Microsoft Power Platform public APIs.

- Your connector artifacts will be stored and publicly available in our [GitHub repository](https://github.com/Microsoft/PowerPlatformConnectors). You can view our existing open-sourced connectors there.

- No personal identifiable information data or intellectual property will be stored in the repository.
- Microsoft will include you in the [CODEOWNERS](https://docs.github.com/github/creating-cloning-and-archiving-repositories/about-code-owners) file for the connector in GitHub, and any change to your connector will be managed by you.

## Upload from local files

If you've been granted an exception from open-sourcing, you'll need to submit your connector by uploading your local connector files:

1. Format your custom connector files into a .zip archive that you'll be submitting to Microsoft.

1. Only submit archives that have passed validation and contain the required three artifact files:

    - `apiDefinition.swagger.json`
    - `apiProperties.json`
    - The connector icon

   > [!Important]
   > When using macOS, ensure that _only_ the three connector artifact files are included in the .zip archive. By default, macOS includes other folders and files in a .zip archive.

## Submit to ISV Studio

This section applies to [verified publishers](submit-certification.md#who-is-eligible-to-get-a-connector-certified), *excluding* independent publishers.

As soon as your connector's pull request is merged, you can submit your connector artifacts to the **Connector certification** tab in [ISV Studio](https://isvstudio.powerapps.com/ep/connector). 

If your submission was successful, you can expect a reply from a certification engineer within **1 to 2 weeks**. If there are issues with the submission, you'll need to update the submission based on the feedback. The certification engineer will require an extra **1 to 2 weeks** to reply.

If you've been granted an exception from open-sourcing, don't submit your connector until your Microsoft contact has directed you to. Ensure you've followed all the steps in [Certification process](certification-submission.md) and validated your artifacts before submitting.

The **Submit for review** button will be available when all fields contain valid inputs.

1. Ensure you're creating a submission with an active email address.

1. On the **Connector certification** tab in [ISV Studio](https://isvstudio.powerapps.com/home), you'll be prompted to submit your files by either:
   - Entering GitHub commit information from your open-sourced connector. 

      > [!Note]
      >  Ensure you're providing the recent commit ID of your connector's PR.
   - Uploading the .zip archive you created in the previous step.

1. Enter the following information in [ISV Studio](https://isvstudio.powerapps.com/home):

   - Connector testing information and credentials:

      - Provide as much detail as possible to ensure smooth testing. Ensure you're providing valid testing information.

      > ![Screenshot of connector submission details.](media/certification-submission/isv-submit.png "Connector Submission details")

        > [!important]
        > If your connector type is OAuth, ensure you're providing a valid **Client ID** and **Client secret**. This information can't be altered after submission. Once it is received, it will be stored in Azure Key Vault.
        >
        > If you want to change this, connect with your Microsoft contact. 
      
      - If your connector uses OAuth as its authentication type, add these allowed redirect URLs to your app:
      

         `https://global.consent.azure-apim.net/redirect`<br/>
         `https://global-test.consent.azure-apim.net/redirect`<br/>

      - Avoid providing an account that uses multifactor authentication, or provide steps for the certification team to properly access the multifactor authentication&ndash;protected account.

   - A support email for Microsoft to contact if there are any issues.

1. Connect with your Microsoft contact if you face difficulties submitting your connector artifacts.

1. After you've submitted your artifacts, separately attach an `intro.md` to the **Activity Control** or **Functional Documentation** area. This `intro.md` will be included in the public-facing documentation for your connector. This is separate from the `readme.md` that you submitted during the open-sourcing step.

   Don't include information about your connector's actions or triggers within the `intro.md`, because this information will be automatically generated for you during certification.

1. *(Required)* Use the following markdown as an example of a template for an `intro.md` file:

```Markdown
Provide a detailed description here, distinct from your connector's description, of the value that the connector offers users and a high-level overview of functionality that the connector supports. This description should be no more than one paragraph of eight sentences.

## Prerequisites

Provide information about any prerequisites that are required to use this connector. For example, an account on your website or a paid service plan. 

## How to get credentials

Provide detailed information about how a user can get credentials to use the connector. Where possible, this should be step-by-step instructions with links pointing to relevant parts of your website.

If your connector doesn't require authentication, this section can be removed.

## Get started with your connector

Provide users with a step-by-step process for getting started with your connector. This is where you should highlight common use cases, such as your expected popular triggers and actions, and how they can help in automation scenarios. Include images where possible.

## Known issues and limitations

If your connector has any known issues and limitations, include a detailed description of them here. This information should be as robust as possible so users have plenty of information should they run into problems. If any workarounds are known, include them here.

## Common errors and remedies

Highlight any errors that might commonly occur when using the connector (such as HTTP status code errors), and what the user should do to resolve the error.

## FAQ

Provide a breakdown of frequently asked questions and their respective answers here. This can cover FAQs about interacting with the underlying service or about the connector itself.
```

After you submit your connector and `intro.md`, you're ready to prepare for testing your connector. Go to [Test your connector in certification](certification-testing.md) for instructions.

## Update submission prior to certification

This section applies to [verified publishers](submit-certification.md#who-is-eligible-to-get-a-connector-certified), *excluding* independent publishers.

You should only update a connector that's in the process of being certified but hasn't been certified yet, in these situations:

- When Microsoft has requested changes.

- When you've found a critical issue that needs to be addressed before certification.

> [!Important] 
> Any updates during the certification process will reset the process. After submitting an update, please submit a pull request with the updated artifacts in our [open-source repository](https://github.com/microsoft/PowerPlatformConnectors).

To submit an update:

1. Go to the **Connector certification** tab of [ISV Studio](https://isvstudio.powerapps.com/home), and select your existing submission.

1. In the pane that opens on the right, select your most recent version.

    > ![Submitted connectors version list](media/certification-submission/versionlist.png "Select your most recent version")

1. In the lower-left corner of the **Connector Submissions** view, select **Submit an update**.

1. In the **Confirm Edit** prompt, select **Confirm**. This will reset the certification process.

1. Remove your existing connector archive by selecting **Remove**. Do one of the following:

   - Use the GitHub integration to ingest the updated artifacts from the open-source repository 
   - Upload the updated .zip archive

   ![Remove existing .zip archive](media/certification-submission/removezip.png)

1. Fill in the fields in the submission form. Ensure you provide the proper information&mdash;including testing&mdash;as it applies to your latest update.

1. In the lower-left corner of the **Connector Submission** view, select **Submit for Review**. You'll see the confirmation illustrated in the following image, and Microsoft will begin the review process.

   ![Submission received confirmation](media/certification-submission/submissionconf.png)

> [!Important] 
> Don't select **Submit an update** again after completing step 7. Doing so will reset progress once more.

If you'd like to submit an update to a connector that's already been certified, go to [Update your certified connector](certification-updates.md).

## Next step

[Test your connector in certification](certification-testing.md)

[!INCLUDE[feedback](../includes/feedback.md)]