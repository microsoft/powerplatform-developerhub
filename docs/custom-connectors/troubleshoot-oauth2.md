---
title: Troubleshoot OAuth 2.0 | MicrosoftDocs
description: How to troubleshoot when your services aren't implementing OAuth correctly. 
Keywords: connectors, custom connectors, OAuth, troubleshoot
author: jopanchal
contributors:
  - jopanchal
  - v-aangie
ms.author: jopanchal
ms.date: 06/08/2022
ms.service: connectors
ms.topic: conceptual
ms.reviewer: angieandrews
---

# Troubleshoot OAuth 2.0

OAuth 2.0 is a secure but complicated authentication pattern. Many customers report OAuth issues with their custom connectors because their services aren't implementing it correctly. Troubleshooting these issues is technical and it might help to have some background in how OAuth works.

To learn more, go to the [OAuth documentation](https://auth0.com/docs/authorization/flows/authorization-code-flow).

## Limitations

- APIHub only supports the "access code" method of OAuth 2.0 configuration. To learn more, go to [Authorization code request](https://www.oauth.com/oauth2-servers/access-tokens/authorization-code-request/).

- Gateways don't support AAD or OAuth.

## Symptoms

- A connection is failing after *X* amount of time (where *X* is consistent).

- **401 unauthorized** is returned when the custom connector is using OAuth.

## Troubleshoot the OAuth flow

The problem almost always lies within the configuration of the custom connector or the third party service you're using. The first step is to walk through the OAuth flow with the third party service through Postman:

1. Call the token endpoint using the same client ID, client secret, and redirect URI (if used) as the custom connector. To learn more about this request, go to [Authorization code request](https://www.oauth.com/oauth2-servers/access-tokens/authorization-code-request/).

    - Verify the endpoint returns an access token.

    - Does the endpoint return a refresh token? If not, verify the access token doesn't expire.

1. Call the API action using the access token.

    - Verify the token is inside the **Authorization** header prefixed by 'bearer'. Following is an example from Postman:

        ![Screenshot of the Authorization header in Postman.](./media/troubleshoot-oauth2/authorization.png "Authorization header in Postman")

    - Verify the response is successful and the action succeeds.

1. If you have an expiring access token, call the refresh URL with the refresh token (or access token if you don't have the refresh token). To learn more about the refresh request, go to [Refreshing access tokens](https://www.oauth.com/oauth2-servers/access-tokens/refreshing-access-tokens/).

    - Verify a new access token is returned.

    - If the refresh token expires, verify a new refresh token is returned.

1. Call the API with the new access token like in step 2.

    - Verify the response is successful.

1. If you've verified steps 1 through 4, then check the custom connector:

    - Verify the client Id is the same as the client Id used in step 1.

    - Verify the client secret is the same.

    - Verify the token URL is the same.

    - Verify the refresh URL is the same or, if not used, make sure it's the same as the token URL in this step.

   [!INCLUDE[feedback](../includes/feedback.md)]
