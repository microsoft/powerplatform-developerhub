---
title: Data protection in connectors | Microsoft Docs
description: Describes how data used by Power Platform and Azure Logic Apps connectors is protected. 
author: asavaritayal
ms.author: angieandrews
ms.service: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.reviewer: angieandrews
---

# Data protection in connectors

This article addresses the most important topics you need to understand to ensure a successful and safe experience with a Power Platform connector using an external service. It may simplify your connector vetting process, which could save you time.

After reading this article, you should be able to answer these questions:

- What [validation checks](#connector-validation) are performed for a connector using an external company's service?
- When is [Microsoft's privacy policy](#microsoft-privacy-policy) applied versus an [external company's privacy policy](#external-company-privacy-policy)?
- How is General Data Protection Regulation (GDPR) compliance handled by [Microsoft](#microsoft-gdpr-compliance) versus an [external publisher](#external-company-gdpr-compliance)?

## Connector validation

Connectors that are published by Microsoft may connect to internal or external services. For example, connectors such as SharePoint or Excel will always connect to services internally.

However, Microsoft also publishes connectors that use external services. For example, the Salesforce connector uses the Salesforce service. Whenever a connector leaves the Microsoft infrastructure, Microsoft runs a series of validation checks to ensure data protection.

> [!IMPORTANT]
> It's the *service*, not the connector itself, that dictates who is responsible for protecting data. The reason is that it's the service you are connected to that stores data, not the connector.
>
> Remember that if your data resides on Microsoft infrastructure, then Microsoft is in control. As soon as it moves to an external infrastructure, the verified publisher is in control.

Knowing the difference will enable you to take appropriate action and can ensure the safety of your data throughout the connection. Details are in the sections below.


|If connector is<br/> published by  |And service<br/>infrastructure is  |<br/>Microsoft does this: | <br/>Example |
|---------|---------|---------|---------|
|[Microsoft](connector-reference/connector-reference-microsoft-connectors.md)     |  External       | - [Verify the endpoint](#verify-the-endpoint)<br/>- [Swagger validation](#swagger-validation)<br/>- [Breaking change validation](#breaking-change-validation)<br/>- [Quality and verification checks](#quality-and-verification-checks)        | Salesforce        |
|Microsoft   | Internal     | All validation checks for data protection        |   SharePoint, Excel   | 
|[Verified ](connector-reference/connector-reference-partners-connectors.md)<br/>(includes independent publishers)    |  External       |  - [Verify the endpoint](#verify-the-endpoint)<br/>- [Swagger validation](#swagger-validation)<br/>- [Breaking change validation](#breaking-change-validation)<br/>- [Quality and verification checks](#quality-and-verification-checks)         | Adobe Creative Cloud          |


## Microsoft privacy policy

Microsoft applies its protection policies for all data that resides on Microsoft infrastructure. As soon as the data moves to an external infrastructure, as in the case of using a verified connector service, the policies set by that company take over.

For details on how Microsoft addresses issues such as security, privacy, and compliance, explore the [Microsoft Trust Center](https://www.microsoft.com/trust-center).

## External company privacy policy

> [!NOTE]
> There are limitations on enforcing Microsoft's policy when connector services reside on external infrastructure. Once you connect to their service, Microsoft has no control over where your data is going; this is up to the external company. It's *your responsibility* to research their terms and conditions, or to work with them directly to provide guidance.

Examples of questions you might have for an external company's connector service include:
- Is the company service storing your data?
   - If so, how does the company determine when to delete the data?
- Does the connector have any vulnerabilities or an opportunity for an attacker to control or intercept data?
- Does the company's service apply single sign-on or two-factor sign-on?

You can find the link to the privacy policy of each verified connector by selecting the connector link in [List of all verified connectors published by partners](connector-reference/connector-reference-partners-connectors.md).

## Microsoft GDPR compliance

While a connector service is running on Microsoft infrastructure, Microsoft governs the rules for GDPR compliance. Once data moves to a company's external infrastructure, Microsoft won't violate their GDPR rules.

> [!IMPORTANT]
> The Microsoft connector platform is GDPR compliant. It only *processes* your data without *storing* it. While processing, it adheres to Microsoft privacy and GDPR policies.

For example, if you create a flow in Europe, Microsoft will not send a request to the United States, process the data there, and then send it back to an external company's service. In this case, Microsoft will honor the geographical boundaries.

## External company GDPR compliance

While a connector service is running on an external company's infrastructure, that company governs the rules for GDPR compliance.

> [!NOTE]
> When service is running on an external company's infrastructure, it's *your responsibility* to understand their GDPR requirements. Microsoft has no control. External company connectors may or may not be GDPR compliant.

If you are using an external company's connector, it's a good idea for you to perform your own research, modeling, vendor audit, and strategy steps at your company to ensure you understand GDPR as it applies to your business. You might want to start with their terms and conditions documentation.

Examples of questions that can be answered only by the external company might involve data traveling or being processed geographically, such as:

- For EU based companies:
  - Is the data leaving the EU at any point?
  - Is the data being sent to the EU at any point?
- For US-based companies:
  - Is the data being sent to the EU at any point?
 
## Verify the endpoint

When your data is being processed on the Microsoft infrastructure, Microsoft verifies the endpoint to ensure that certain quality bars are met. Azure Logic Apps enforces *regional* boundaries but Power Automate and Power Apps enforce *geographical* boundaries.

For example, West US is a region, but United States is a geography. In Logic Apps, if a user selects West US, Microsoft ensures that data doesn’t go outside of the West US datacenter boundary, not even to Central US or East US. However, for Power Automate and Power Apps, Microsoft only guarantees the geographical boundary. This means that if the user selects United States, data can be processed anywhere in the United States, including West US or East US. What Microsoft will guarantee is that data doesn’t go outside of the United States, such as to Europe or Asia.

Once data leaves Microsoft and moves to the geography selected by the user, the usage and storage of data is then governed by the policy set by the external company. Microsoft will act only as a proxy to the company's platform and will send requests to the endpoint.

If you are using a verified external connector, you need to understand the terms and conditions offered by the company to understand how and where your data is being processed.

## Swagger validation

Swagger validation measures the equality bar for connectors. Microsoft runs this validation to ensure the connector adheres to Open API standards, meets connector requirements and guidelines, and includes proper custom Microsoft extensions (x-ms).  

## Breaking change validation

When a connector is updated, Microsoft enforces a validation test to ensure that nothing is breaking. The validation rules will catch any breaking changes by comparing the current swagger version to previous swagger versions. For example, a parameter that a company marked as optional and is now required.

## Quality and verification checks

Microsoft performs quality checks on all connectors. Quality checks include functional verification after deployment, and testing to ensure that functionality is working as expected. Microsoft also performs verification checks. One type is to ensure the publisher is allowed to build a connector. Another type is to scan all connector code for malware or viruses.

## Communication and authentication methods

At runtime, a validation takes place to check whether the user has permission to make the call, and then the token/secrets are fetched for the call to the back end.

Microsoft places no restriction on external companies for the type of authentication to use. It is defined by the authentication supported by the API provided by the underlying connector service. Connectors will support different types of authentication (for example, Azure Active Directory, Basic authentication, or OAuth).

### Examples

If the authentication type used is:

- **Azure Active Directory (Azure AD)**&mdash;The user will be asked to sign in to Azure AD (and if corporate policy enforces a user to have multifactor authentication, certificates, or a smart card, this also can be enforced here). This enforcement takes place between the user and Azure AD, which is independent of the connector itself. Many services, especially those provided by Microsoft, use Azure AD.

- **Basic authentication**&mdash;The username and password are sent in the API request. These secrets are stored and encrypted in an internal token store that's accessible only by Microsoft Power Platform.

- **OAuth**&mdash;The redirect URL of the connector is retrieved from the settings and sent back to the user to sign in to the service directly and grant consent. User credentials are not stored. Once sign-in is successful and the user has granted consent to access data on their behalf, an authorization code is sent back to Microsoft Power Platform. With that authorization code, an access token is then retrieved and stored internally. This access token is accessible only by Microsoft Power Platform.

### See also

[Connectors overview](connectors.md)

[!INCLUDE[feedback](includes/feedback.md)]