---
title: Verified publisher certification process | Microsoft Docs
description: This topic describes the steps to achieve Microsoft certification for a connector by a verified publisher.
author: bezhan-msft
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: article
ms.date: 06/08/2022
ms.author: bezhan
ms.reviewer: angieandrews
---

# Verified publisher certification process

This process is for [verified publishers](submit-certification.md#who-is-eligible-to-get-a-connector-certified) (excluding independent publishers). If you're an independent publisher, go to [Independent publisher certification process](certification-submission-ip.md).

After you've finished developing your custom connector, follow these steps to prepare it for certification and generate the connector files to submit to Microsoft.

> [!NOTE]
> This topic provides information for certifying custom connectors in Azure Logic Apps, Power Automate, and Power Apps. Before following the steps in this article, read [Get your connector certified](submit-certification.md) and [register](https://aka.ms/ConnectorRegistration) your custom connector with Microsoft.

## Basic certification process workflow

The following flowchart shows the basic certification process workflow. The numbered steps in this article correspond to the workflow. They should provide the details you need to complete the certification process.

For an expanded view of the flowchart, select the magnifying glass icon at the bottom-right.

[ ![Connector certification process workflow.](media/certification-submission/certification-basic.png)](media/certification-submission/certification-basic.png#lightbox)

For an in-depth visual of this flowchart, go to [In-depth connector certification process workflow](certification-workflow.md).

## Step 1: Register your connector

You don't need to have finished development on your custom connector to apply for certification. To begin the certification process, register your connector for certification by filling out our [registration form](https://aka.ms/ConnectorRegistration).

Expect an email within two business days from a Microsoft contact, who will:

- Understand your custom connector.
- Learn about your development progress.
- Guide you through the certification process.

## Step 2: Meet submission requirements

To maintain a high standard of quality and consistency among our certified connectors, Microsoft has a set of requirements and guidelines that your custom connector must adhere to for certification.

### Give your connector a title

- Must exist and be written in English.
- Must be unique and distinguishable from any existing connector title.
- Should be the name of your product or organization.
- Should follow existing naming patterns for certified connectors. For independent publishers, the connector name should follow the pattern, ***Connector Name* (Independent Publisher)**.
- Can't be longer than 30 characters.
- Can't contain the words "API", "Connector", or any of our Power Platform product names (for example, "Power Apps").
- Can't end in a non-alphanumeric character, including carriage return, new line, or blank space.

**Examples**

- Good connector titles: "Azure Sentinel", "Office 365 Outlook"
- Poor connector titles: "Azure Sentinel's Power Apps Connector", "Office 365 Outlook API"

### Write a description for your connector

- Must exist and be written in English.
- Must be free of grammatical and spelling errors.
- Should describe concisely the main purpose and value offered by your connector.
- Can't be shorter than 30 characters or longer than 500 characters.
- Can't contain any Power Platform product names (for example, "Power Apps").

### Design an icon for your connector

**This section doesn't apply to independent publishers.**

- Create a logo of approximately 160 &times; 160 pixels inside a square of approximately 230 &times; 230 pixels (no rounded edges).
- Must contain a non-transparent, non-white color (#ffffff) background and not-default color (#007ee5) that matches your specified icon background color.
- Must be unique to any other certified connector icon.
- Must be submitted in PNG format as `icon.png`.

### Define operation and parameter summaries and descriptions

- Must exist and be written in English.
- Must be free of grammatical and spelling errors.
- Operation and parameter summaries should be phrases of 80 characters or shorter, and contain only alphanumeric characters or parentheses.
- Operation and parameter descriptions should be full, descriptive sentences, and end in punctuation.
- Can't contain any of the Microsoft Power Platform product names (for example, "Power Apps").

### Define exact operation responses

- Define operation responses with an exact schema only with expected responses.
- Don't use default responses with an exact schema definition.
- Provide valid response schema definitions for all operations in the swagger.
- Empty response schemas aren't allowed except in special cases where the response schema is dynamic. This means no dynamic content will be shown in the output and makers must use JSON to parse the response.
- Empty operations aren't allowed.
- Remove empty properties unless they're required.

### Create quality English language strings

Connectors are localized as part of Power Automate localization; therefore, when you're developing a connector, the quality of the English language strings is key to the translation quality. Here are some major areas to focus on as you create the values of the strings you'll provide.

- Make sure to run a spell check program to ensure all string values are free of typographical errors. If there's any incomplete English language string, the translation result will be incomplete or incorrect in context.

- Make sure the sentence is in complete form. If the sentence isn't complete, that can also generate lower quality translations.

- Make sure the meaning of the sentence is clear. If the meaning of the sentence is ambiguous, that can also generate lower quality or incorrect translations.

- Make sure summaries, x-ms-summaries, and descriptions are grammatically correct. Don't copy and paste them. To learn how they're shown within the product, go to [Connector string guidance](connector-string-guidance.md).

- Avoid runtime composite strings, if possible. Use fully formed sentences instead. Concatenated strings or sentences make it difficult to translate, or can cause a wrong translation.

- If you use abbreviations, make sure to capitalize them to make it clear. This will reduce the chance of it being mistaken for a typographical error.

- Strings in CaMel form (ex. minimizeHighways or MinimizeHighways) are typically considered non-translatable. If you want to localize the string value, you should fix the CaMel form string.

## Step 3: Add metadata

Your connector artifacts (files) must contain specific metadata describing the connector and its end service. The information provided in metadata will be published in our connector documentation and is publicly accessible by all users. Don't provide any private or confidential information, and let us know through your Microsoft contact if there are any issues with providing us this information. To learn how the metadata will be documented, visit any one of the connector-specific documentation pages under [Connector reference](https://aka.ms/listofconnectors).

### Step 3a: publisher and stackOwner properties

- **"publisher"** is the name of your company or organization. Provide the full company name (for example, "Contoso Corporation"). This must be in alphanumeric format.

- **"stackOwner"** is the owning company or organization of the back-end service stack that the connector is connecting to. This must be in alphanumeric format. 


|Publisher  |Description  |Example |
|---------|---------|---------|
|Verified     | The publisher and stackOwner will be the same, unless the ISV is building a connector on behalf of a stackOwner.  | "publisher": "Tesla",<br/>"stackOwner": "Tesla"         |
|Independent     |  You must provide stack owner and publisher owner.       | "publisher": "Nirmal Kumar",<br/>"stackOwner": "ITGlue"        |

**File Location:** apiProperties.json <br>
To learn more, go to [API Properties file](paconn-cli.md#api-properties-file).<br/>

**Syntax:** The **publisher** and **stackOwner** properties exist as top-level properties within the apiProperties.json file. Add the following highlighted lines as shown. Ensure that you enter the property name and schema exactly as shown.

:::image type="complex" source="media/certification-submission/metadata-publisher.png" alt-text="Screenshot showing publisher and stackOwner properties, which are available in Sample code snippets.":::
   Code showing two lines highlighted in red. The two lines are for publisher and stackOwner, and are located directly after the closing square bracket in "capabilities":[ "actions" ]
:::image-end:::

### Step 3b: Product or end service metadata

- **"contact"** describes how users can contact the product or end service support resources for assistance or troubleshooting. Provide one value for each of the following:

  - Name of your support team
  - URL of your support website
  - Support email

- **"Website"** is the product or end service website. It gives users information about the product or end service they're using with the connector. This should be a URL.

- **"Privacy policy"** refers to the public privacy policy of the product or end service, or its company or organization. This should be a URL.

- **"Categories"** refer to a logical classification of your connector using a maximum of two of the following categories: AI, Business Management, Business Intelligence, Collaboration, Commerce, Communication, Content and Files, Finance, Data, Human Resources, Internet of Things, IT Operations, Lifestyle and Entertainment, Marketing, Productivity, Sales and CRM, Security, Social Media, Website.

**File Location:** apiDefinition.swagger.json

**Syntax:** The **contact** object is a standard field defined by the OpenAPI contract under the top-level **info** property. **Website**, **Privacy policy**, and **Categories** will be defined in a custom top-level extension called `x-ms-connector-metadata`. The **Categories** property value is a semicolon-delimited string. Add the red-lined code snippets as shown. Ensure you enter the schema exactly as shown; don't change the **propertyName**.

:::image type="complex" source="media/certification-submission/metadata-swagger.png" alt-text="Screenshot showing x-ms-connector-metadata, which is available as a usable example in Sample code snippets":::
   Code showing the block defining the contact object highlighted in red. This block must be located directly under the description. Another block, x-ms-connector-metadata, is also highlighted in red. This block must be located directly under paths: {}.
:::image-end:::

### Step 3c: Sample code snippets

You can use the following code snippets to copy and enter your information. Ensure that you add the snippets to the correct files in the correct locations as described in the preceding section.

```JSON
    "publisher": "_____",
    "stackOwner": "_____"
```
```JSON
    "contact": {
      "name": "_____",
      "url": "_____",
      "email": "_____"
    }
```
```JSON
    "x-ms-connector-metadata": [
      {
        "propertyName": "Website",
        "propertyValue": "_____"
      },
      {
        "propertyName": "Privacy policy",
        "propertyValue": "_____"
      },
      {
        "propertyName": "Categories",
        "propertyValue": "_____;_____"
      }
    ]
```

> [!Note]
> There is a current limitation in the use of the **stackOwner** property and the Paconn CLI tool. For more information, go to [Limitations](https://github.com/microsoft/PowerPlatformConnectors/tree/master/tools/paconn-cli#limitations) in the README file.

### Step 3d: JSON file formatting and limitations

- Make sure your properties are correctly aligned.
- Paste your JSON into Visual Studio Code. Feel free to use extensions such as spell checkers, and plugins such as JSON plugins.  
- Swagger files shouldn't be greater than 1 MB.
    - Consider the design of your connector before your start building it. Evaluate if the connector should be broken down into two (2) or more connectors.
    - Larger swagger files may cause a delay when using the connector.

    For example, there are three (3) different HubSpot connectors on the platform.

    > [!div class="mx-imgBorder"]
    > ![Screenshot of folders for the three HubSpot connectors.](media/certification-submission/hubspot-connectors.png "HubSpot connectors")

### Step 3e: Validate your custom connector files
Run `paconn validate --api-def [Location of apiDefinition.swagger.json]`. This tool will validate your connector definition and let you know of any errors you'll need to fix before submission.

 **If your connector uses OAuth as its authentication type, add these allowed redirect URLs to your app:**

- ```https://global.consent.azure-apim.net/redirect```

- ```https://global-test.consent.azure-apim.net/redirect```

## Step 4: Prepare the connector artifacts

This step should take you about one week to complete.

> [!Note]
> - Ensure that you've followed the specifications and have ensured the quality of your connector before certification. Failure to do so will result in delays in certification because you'll be asked to make changes.<br/>
> - Provide a production version of the host URL. Staging, dev, and test host URLs aren't allowed.


You'll be submitting to Microsoft a set of files called *connector artifacts*, downloaded by using a [command-line interface](paconn-cli.md) (CLI) tool provided by Microsoft. This tool will validate your connector for any breaking errors.

Follow these steps to get started:

1. Install the Microsoft Power Platform Connectors CLI tool by following the [installation instructions](paconn-cli.md#installing).

1. Sign in to Microsoft Power Platform using your command line by running `paconn login`. Follow the instructions to sign in using Microsoft's Device Code process.

1. After you're authenticated, download your custom connector files:

   - Run `paconn download`. Select the environment your custom connector is located in by specifying its number in the command line, and then select the custom connector name.
  
    The tool will download your connector artifacts in a folder to the file system location where you ran `paconn`. Depending on the type of publisher, you'll find various artifacts:

  |Publisher  |Artifact  |
  |---------|---------|
  |Verified     | `apiDefinition.swagger.json`<br/>`apiProperties.json`<br/>`settings.json`<br/>The connector icon        |
  |Independent     | `apiDefinition.swagger.json`<br/>`apiProperties.json` |

Both verified publishers and independent publishers will download `apiProperties.json` in their artifacts. You need to set the IconBrandColor in this file.
- **Verified publishers:** Set iconBrandColor to your brand color in the apiProperties file.
- **Independent publishers:** Set iconBrandColor to "#da3b01" in the apiProperties file.
    <br/>![Screenshot of a vivid orange (da3b01) icon.](media/certification-submission/vivid-orange.png "Vivid orange (da3b01) icon")

### Create a Readme file artifact

A Readme.md file is necessary for both independent publishers and verified publishers. You need to create a Readme.md file to document your connector's features and functionality. For an example of documentation to include, go to the [Readme.md example](https://github.com/microsoft/PowerPlatformConnectors/blob/dev/custom-connectors/AzureKeyVault/Readme.md).Look at other Readme.md files in our [GitHub repository](https://github.com/microsoft/PowerPlatformConnectors/tree/dev/certified-connectors) to learn about writing a Readme.md file.

If you’re an independent publisher and your connector uses OAuth, make sure you include instructions for how to obtain credentials.
> [!TIP]
> **Known Issues and Limitations** is a great section to maintain to keep your users up-to-date.

## Step 5: Submit your connector for deployment

> [!Note]
> During the submission process, you'll be open-sourcing your connector to our [Microsoft Power Platform Connectors repository](https://github.com/microsoft/PowerPlatformConnectors).

1. Follow the instructions in [Submit your connector for Microsoft certification](submit-for-certification.md) to submit to GitHub and the certification portal.

    If you're a verified publisher, you'll need to submit a script.csx file if you're using custom code.

1. Once you submit a pull request to the [open-source repository](https://github.com/microsoft/PowerPlatformConnectors), Microsoft will deploy and validate your connector within 1 to 2 weeks. If updates are required, allow an extra 1 to 2 weeks.

    As part of submission, Microsoft will validate your connector by using the CLA-bot, Swagger Validator, and Breaking Change Detector tools. If you need to troubleshoot swagger errors, go to [Fix Swagger Validator errors](certification-swagger-validator-rules.md).

## Step 6: Expectations for testing done by verified publishers

After we validate your connector, we'll ask you to perform thorough testing.

1. Follow the instructions in [Test your connector in certification](certification-testing.md) to create an environment in the Preview region in preparation for your testing.

1. Within one week, notify your Microsoft contact that you've completed testing so we can begin deployment.

1. After your connector's functionality and content are validated by both Microsoft and you, we'll stage the connector for deployment in the Preview region for testing.

## Step 7: Wait for deployment

After your connector is validated for testing, we'll deploy it across all products and regions. 

> [!Important]
> On average, it takes 3 to 4 weeks to deploy the connector. This is required regardless of the size or complexity of your connector, whether it's new or an update. In order to protect integrity, the connector will be subjected to the same validation tasks to test functionality and content that are followed in every deployment.

We'll notify you by email with the names of the regions the connector will be deployed to, as deployment to regions is done in steps. If there's a deployment delay or freeze, verified publishers can find the status in the **Activity Control** in the [ISV portal](https://isvstudio.powerapps.com/home). Independent publishers will be notified by email.

### Production deployment

Our connector deployment schedules for production start Friday mornings, PST/PDT. You'll need to let Microsoft know that you're ready for production deployment at least 24 hours in advance for us to include your connector in the next scheduled deployment. Verified publishers can notify us in the **Activity Control** of [ISV portal](https://isvstudio.powerapps.com/home). Independent publishers can notify their Microsoft contact.

### Region deployment

Deployment to various regions takes place in a pre-determined daily sequence. The regions are:

- Testing.
- US preview.
- Asia, except Japan and India.
- Europe, except UK.
- Brazil, Canada, Japan, and India.
- Australia, UK, and US.

For example, if your connector is scheduled to deploy on Monday, it will deploy to the Testing region on day 1. Then, it will deploy to the US preview region on Day 2. Deployment will continue daily until the connector is deployed to all six regions.

We don't deploy on Saturdays, Sundays, and US holidays.

As your connector is finishing certification, we'll engage you about a marketing opportunity for the connector on the [Power Automate blog](https://flow.microsoft.com/en-us/blog/).

## Step 8: Explore post-deployment options

Here are some options you can explore after your connector is deployed:

- View connector telemetry anytime in the [ISV portal](https://isvstudio.powerapps.com/home). To get health and usage of your connector, go to [Gain key insights on your certified connector in the ISV portal](https://flow.microsoft.com/blog/gain-key-insights-on-your-certified-connector-in-the-isv-portal/).

- Submit updates to your connector. For more information, go to [Update your certified connector](certification-updates.md).

- Monitor your connector on the [community discussion forum](https://powerusers.microsoft.com/t5/General-Power-Automate/bd-p/MPAForum) to discover if customers encounter any issues or have feature requests for your connector.

- Request removal of the *preview* tag. After the connector has been publicly available for some time and meets certain requirements, it can qualify to be reassigned the *general availability* tag. This tag will show that the connector is a production-ready product. For details, go to [Move your connector from preview to general availability](certification-to-ga.md).

## Checklist before submitting

Before moving on to [Submit your connector for Microsoft certification](submit-for-certification.md), ensure that:

- Your connector satisfies all standards set in [Step 2: Meet submission requirements](#step-2-meet-submission-requirements) and [Step 3: Add metadata](#step-3-add-metadata).

- No operations are missing a [summary](openapi-extensions.md#summary), [description](openapi-extensions.md#description), or [visibility information](openapi-extensions.md#visibility).

- You've tested your custom connector to ensure the operations work as expected (at least 10 successful calls per operation).

- No runtime or schema validation errors appear in the [test section](define-blank.md#step-5-test-the-connector) of the custom connector wizard.

> [!TIP]
>- Create YouTube videos, blogs, or other content to share samples or screenshots for how to get started with the connector.<br/> - Include the links in the [Readme.md](certification-submission.md#create-a-readme-file-artifact) file so that we can add to our docs.
> - Add [tooltips](/windows/apps/design/controls/tooltips) to your swagger file to help your users be more successful.

If you're a verified publisher (and not an independent publisher), you'll be asked to agree to our Partner Agreement and non-disclosure agreement when you submit for Microsoft certification.
If you'd like to review these terms and language before submission, connect with your Microsoft contact.

### Next step

[Submit your connector for Microsoft certification](submit-for-certification.md)

[!INCLUDE[feedback](../includes/feedback.md)]
