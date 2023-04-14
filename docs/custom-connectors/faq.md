---
title: Custom connector frequently asked questions for Azure Logic Apps, Power Automate, and Power Apps
description: Learn FAQ about requirements, triggers, and more for custom  connectors.
author: jopanchal
contributors:
  - jopanchal
  - v-aangie
editor: camydee
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 02/13/2023
ms.author: jopanchal
ms.reviewer: angieandrews
---

# Custom connector FAQ for Azure Logic Apps, Power Automate, and Power Apps

> [!Note]
> This topic is part of a tutorial series on creating and using custom connectors in Azure Logic Apps, Power Automate, and Power Apps. Make sure you read the [Custom connector overview](index.md) to understand the process.

## API and OpenAPI

**Q: What are the limits for connectors and API requests?**</br>
**A:** The following tables show the limits:

| Azure Logic Apps | Limit | 
| ---- | ----- | 
| Number of custom connectors that you can create | 1,000 per Azure subscription | 
| Number of requests per minute for each connection created by a custom connector | 500 requests for each connection created by the connector |
| Maximum schema count per body allowed in a Swagger file |512 |
| Maximum number of operations allowed in a Swagger file |256 |
| Maximum number of schemas per operation allowed in a Swagger file | 16,384 |
| Maximum number of schemas allowed in a Swagger file | Maximum number of operations allowed in a Swagger file * Maximum number of schemas per operation allowed in a Swagger file |
| Maximum request content-length that can be sent to a gateway |3,182,218 |

| Power Automate and Power Apps | Limit | 
| ---- | ----- | 
| Number of custom connectors that you can create | Free plan: one<sup>1</sup> </br>Office 365 and Dynamics 365 plans: one </br> Per user plan: 50 | 
| Number of requests per minute for each connection created by a custom connector | 500 requests for each connection created by the connector |
| Maximum request content-length that can be sent to a gateway | 3,182,218 |

<sup>1</sup> The free plan doesn’t allow you to share connections or custom connectors with other people in your company.

If you use an OpenAPI definition or Postman collection to describe a custom connector, the file must be less than 1 MB.

**Q: What are the size limits for connectors and operations through a gateway?**</br>
**A:** The limits can be found in the [gateway documentation](/data-integration/gateway/service-gateway-onprem#considerations).

**Q: My APIs use a dynamic host. How do I implement them with OpenAPI?**</br>
**A:** Policy templates can be used to dynamically modify the host at runtime. For more information, go to the [Set Host URL](./policy-templates/dynamichosturl/dynamichosturl.md).

## Certification

**Q: I don't own the API and I don't have explicit rights to use the API. Can I still create connectors?** </br>
**A:** Yes, you can register and share these connectors for internal use within your organization, or you can publish it as an independent publisher connector. To certify and publicly release a connector as an independent publisher, you *don't* need to own the underlying service or present explicit rights to use the API. 

**Q: How long does certification take?**</br>
**A:** The time for certification significantly depends on the quality of the submission and the speed of your testing. Barring problems with the process, it typically takes three to five business days. After certification, deployment to all regions will take another 7 to 10 business days.

**Q: Where can I get help if I'm having trouble understanding or executing the submission steps?**</br>
**A:** Connect with your Microsoft contact for assistance. You're also encouraged to provide any feedback on our documentation so we can improve it.

**Q: Can my connector be certified as a Standard connector instead of Premium?**</br>
**A:** No. Unfortunately, we currently only release certified connectors as Premium. Any user with a Power Automate license can use both Standard and Premium connectors.

**Q: Can the Preview tag be removed from my connector?**</br>
**A:** Your connector needs to meet certain requirements before the Preview tag can be removed. Ensure that your connector has significant usage and high reliability before contacting your Microsoft contact to ask this.

**Q: Can the throttling limits for my connector be adjusted?**</br>
**A:** We handle throttling limit adjustments on a case-by-case basis. Connect with your Microsoft contact with justification if you'd like your connector's limits adjusted.

**Q: Does my connector have to be finished to register for certification?**</br>
**A:** *(Registration doesn't apply to independent publishers.)* No, you should register for certification as soon as you know you want to certify. As soon as you register, a Microsoft contact will get in touch and offer assistance throughout development, if necessary.

**Q: Am I able to add more connector owners to my submission to help manage the connector on the **Connector certification** tab in ISV Studio?**</br>
**A:** *(IVS Studio doesn't apply to independent publishers.)* Yes, multiple owners are allowed. Connect with your Microsoft contact for assistance to have owners added.

For more information, go to [Get your connector certified](submit-certification.md).

## Requirements

**Q: Can I build a connector without REST APIs?**</br>
**A:** For Power Apps and Power Automate, you must support stable HTTP REST APIs for your service. For Logic Apps, you also have the option of using a SOAP API.

**Q: What tools can I use to expose an API?**</br>
**A:** Azure has capabilities and services that you can use for exposing any service as an API, such as Azure App Service for hosting, API Management, and more.

**Q: What authentication types are supported?**</br>
**A:** You can use these supported authentication standards:

   - OAuth 2.0 for specific services, including 
[Azure Active Directory](https://azure.microsoft.com/develop/identity/), Facebook, Dropbox, GitHub, and SalesForce
   - [Generic OAuth 2.0](https://oauth.net/2/)
   - [Basic authentication](https://swagger.io/docs/specification/authentication/basic-authentication/)
   - [API Key](https://swagger.io/docs/specification/authentication/api-keys/) (Except on-premises data gateway)

> [!Note]
> Client credentials grant type is not supported.

## Support

**Q: I have a technical question on connector development. Where should I ask for help?**</br>
**A:** Add your question on [Power Automate Forum Connector Development Forum](https://powerusers.microsoft.com/t5/Connector-Development/bd-p/ConnectorDevelopment). Our team or a community member will provide an answer. You can also search for a similar issue. Posting your question will help you get answers from a wider community, and will improve the forum's knowledge base.

**Q: Do you support Postman Collection V2?**</br>
**A:** Yes, support for Postman Collection v2 is available as of Oct 2021.

**Q: Do you support OpenAPI 3.0?**</br>
**A:** No, only OpenAPI 2.0 is currently supported. Support for OpenAPI 3 is in the backlog.

**Q: Is a developer plan available to use for my connector development efforts?**</br>
**A:** Yes, the [Power Apps Community Plan](https://powerapps.microsoft.com/communityplan/) is available for use and is free for Power Apps and Power Automate.

## Triggers

**Q: Can I build triggers without webhooks?**</br>
**A:** Custom connectors for Azure Logic Apps and Power Automate support webhook-based and polling triggers. Webhook-based triggers wait for an event to occur, while polling triggers call your service at a specified frequency to check for new data. If you want to request other patterns for implementation, contact [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com)
with more details about your API.

### See also

[Connectors overview](../connectors.md)<br/>
[Get your connector certified](submit-certification.md)<br/>

[!INCLUDE[feedback](../includes/feedback.md)]
