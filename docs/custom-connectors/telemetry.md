---
title: View connector telemetry | Microsoft Docs
description: This topic describes available connector telemetry and provides error codes and troubleshooting suggestions.
author: amjed-ayoub
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: article
ms.date: 08/25/2022
ms.author: v-amjedayoub
ms.reviewer: angieandrews
---

# View connector telemetry

You can gain key insights of your certified connector by viewing telemetry in the ISV Studio portal. After you select **Home** and then the connector name, you'll be able to monitor the health and usage of your connector.

In the following example, the ISV Studio portal shows the monthly success rate in terms of API responses, monthly active connections, and the monthly request count for your connector. In this case, *monthly* refers to the last 30 days.

>[!div class="mx-imgBorder"]
>![Screenshot of the monthly success rate in the ISV Studio portal.](media/telemetry/isv.png "Monthly success rate in the ISV Studio portal")


## Improve connector integrity

The integrity of your connector is protected by meeting certain standards. One of these standards is usage criteria, which includes the success rate. This is calculated by the percentage of API responses in the 100s, 200s, 300s, 400s, and 500s. For example, 100 covers responses between 100 below 200. The calculation doesn't include failures due to throttling.

Following is the formula to calculate the success rate:

`Success Rate = ((responseCode 100s + responseCode 200s + responseCode 300s - responseCode 400s - responseCode 500s) * (1/ all-responseCode-count)) * 100`

## Error codes

The following table lists the most common errors experienced by users and partners, and their descriptions. It includes error codes in the 400s and 500s. It doesn't include error codes in the 100s, 200s, or 300s, as they're less common. Error codes are available in the ISV Studio portal.

To help you troubleshoot errors quickly and effectively, follow the suggestions later in this article. If you need further guidance or your error code doesn't appear in the table, send an email to [ConnectorPartnerMgmtTeam@service.microsoft.com](mailto:connectorpartnermgmtteam@service.microsoft.com).

| Error Code    | Description  |
|:-------------:| -------------|
|     400     | Indicates that the server can't or won't process the request due to something that's perceived to be a user error (Bad Request). The user entered some invalid inputs.   |
|     401     | Indicates that the authentication process failed. That means the page that the user is trying to access can't be loaded until they first log in with valid authentication credentials.  |
|     403     | When a user goes past the limit provided by the API.  |
|     404     | User is calling an operation that was deprecated or no longer exists.  |
|     409     | The request can't be completed due to a conflict with the current state of the target resource. This code is used when the user might be able to resolve the conflict and resubmit the request.  |
|     415     | Indicates that the request entity has a media type that the server or resource doesn't support (Unsupported Media Type). The response code may still be returned if the contents of the request body weren't   supported by the server. |
|     426     | Indicates that the server refuses to perform the request using the current protocol. It might be willing to do so after the client upgrades to a different protocol (Upgrade Required).  |
|     429     | Rate limit is exceeded (Too Many Requests). Reaching beyond the rate limit of an API.  |
|     500     | Indicates that the server encountered an unexpected condition that prevented it from fulfilling the request (Internal Server Error).  |
|     502     | Indicates an issue with the hosting web server (Server Side Error). For example, the underlying connection was closed: A connection was closed by the server.      |
|     504     | Indicates that the server is taking so long that it's "timing out," is probably down, or not working properly. This error is given when the server can't get a response in time. |
|     522     | Connection timed out.  |
|     523     | Indicates a problem with the connection.    |
|     524     | Indicates that the connection to the server has been closed due to a timeout (Timeout Occurred).   |

## Troubleshoot 400 errors

Error codes in the 400s indicate a system error due to a problem with the request.

- Update the connector documentation page to account for all the common errors that users may experience.

- Add more information to the **Known issues and limitations** section. You can use the intro.md file to add these common errors and limitations. For an example, go to the `intro.md` screenshot and look for the heading, [Known issues and limitations](submit-for-certification.md#submit-to-isv-studio) in step 6.

- To guide your users to be successful and help them reduce mistakes, add tooltips directly into the UI. The following screenshot shows an example of tooltips.

    >[!div class="mx-imgBorder"]
    >![Screenshot of tooltips in the UI](media/telemetry/tooltips.png "Example of tooltips in the UI")

## Troubleshoot 500 errors

Error codes in the 500s indicate an error due to a problem with the server.

- Add the API known issues and limitations to the connector **Known issues and limitations** section. You can use the intro.md file to add these common errors and limitations. For an example, go to the `intro.md` screenshot and look for the heading, [Known issues and limitations](submit-for-certification.md#submit-to-isv-studio) in step 6.

- Make sure that the API and the hosting web server are working properly.

- Make sure that all actions and triggers are using the correct http or https requests with the appropriate responses.

## More information

[Update your certified connector](certification-updates.md)<br/>
[Submit your connector for Microsoft certification](submit-for-certification.md)

[!INCLUDE[feedback](../includes/feedback.md)]