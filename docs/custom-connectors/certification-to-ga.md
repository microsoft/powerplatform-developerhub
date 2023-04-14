---
title: Move your connector from preview to general availability | Microsoft Docs
description: This topic describes how to move your connector from public preview to general availability (production-ready).
author: jopanchal
contributors:
  - jopanchal
  - v-aangie
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: jopanchal
ms.reviewer: angieandrews
---

# Move your connector from preview to general availability (GA)

A new connector is deployed in *preview*. This means it's available to customers for evaluation purposes and at reduced or different service terms. The term *preview* distinguishes services and features that aren’t yet generally available from those that have been released.

## Benefits of moving your connector to GA

Many enterprises won't use your connector unless it's in production. You'll want to move your connector from preview to GA to meet this requirement. This will distinguish it as a stable and production-ready product or service that's publicly available to all customers and backed by service level agreement (SLA) guarantees.

## Release your connector to other clouds

You can increase your connector’s visibility and usage by releasing it in other clouds such as GCC, GCC-H, DoD, Azure Government, and/or Mooncake. Doing this at the same time you move from preview to GA can help grow usage by enterprises.

If you’d like your connector to be released in other clouds, let us know. Also, tell us if you have cloud specific endpoints that you want us to use for those clouds.

To get started, make sure your connector meets the standards in the next section, [Criteria for moving to GA](#criteria-for-moving-to-ga). Then, submit your request with the specifics listed in this section to the email in the [Request the move to GA](#request-the-move-to-ga) section at the end of this topic.

## Criteria for moving to GA

The integrity of your connector is protected by meeting the standards set in the following criteria. Most criteria are [required](#required-criteria) to be eligible to move to GA. Other criteria are strongly [recommended](#recommended-criteria). There are also exceptions that will let you move your connector to GA even if you don't meet the usage criteria. This is explained in the blue **Important** note in the [Recommended criteria](#recommended-criteria) section towards the bottom of this topic.

### Required criteria

Your connector is required to meet the following usage criteria to be eligible to move to GA:

- Backend service APIs that this connector is using need to be in production.

- Connector has 99.95% availability. Connector availability consists of issues related to the connector. Backend service issues aren't used in this calculation.

- Connector has 99.5% service level objective (SLO). This is the percentage of connections meeting the service level agreement bar mentioned previously.

- Connector has greater than an 80% success rate. This is calculated by the percentage of responses in the 100s, responses in the 200s, responses in the 300s, etc. For example, 100 covers responses between 100 below 200. The calculation doesn't include failures due to throttling.

- Your availability to support the connector should match your support model for your backend service. For example, if your support for the service is 5 days a week from 8am-5pm PT, then that is what we expect should be the support for the connector.

### Recommended criteria

Your connector isn't required to meet the following usage criteria to be eligible to move to GA. However, it's *strongly recommended* as an added safeguard to ensure your connector will work:

- Connector has at least 500 weekly active users over the last three weeks.

- Usage is 50 monthly active connections (MAC) at any given time.

> [!Important]
> You can still move your connector to GA even if you don't meet the usage criteria. Here's how:
>
>- If your connector:
>    - Doesn’t have 50 MAC usage *and*
>    - Has been in preview for 6 months *and*
>    - Meets all required criteria.</br>
>   *or*
>- If your customer requires the connector to be in production in order to use it.

## Find your connector criteria

You can find your connector's MAC and success rate using Connector Telemetry in the [ISV portal](https://isvstudio.powerapps.com/home). Reliability and SLO aren't available in the portal. If you need guidance, ask your Microsoft contact.

## Request the move to GA

If your connector meets the criteria listed in this topic, send an email to [ConnectorPartnerMgmtTeam@service.microsoft.com](mailto:ConnectorPartnerMgmtTeam@service.microsoft.com) to remove the preview tag and assign the GA tag.

The process for moving from preview to GA takes about a month. This assumes the connector has met the criteria.

[!INCLUDE[feedback](../includes/feedback.md)]
