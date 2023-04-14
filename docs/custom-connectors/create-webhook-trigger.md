---
title: Use a webhook as a trigger for Azure Logic Apps and Power Automate | Microsoft Docs
description: Learn how to create flows that interact with webhooks in Azure Logic Apps and Power Automate
author: sunaysv
ms.service: connectors
ms.topic: conceptual
ms.date: 06/08/2022
ms.author: sunayv
ms.reviewer: angieandrews
---

# Use a webhook as a trigger for Azure Logic Apps and Power Automate

[Webhooks](http://www.webhooks.org/) are simple HTTP callbacks used to provide event notifications. Azure Logic Apps and Power Automate both allow you to use webhooks as *triggers*. A logic app or flow listens for this trigger and performs an action whenever the trigger fires. This tutorial demonstrates how to use a webhook as a trigger.

> [!NOTE]
> We'll use GitHub as an example of a service that can send notifications via webhooks, but the techniques demonstrated here can be extended to any service that uses webhooks.

## Prerequisites

* One of the following subscriptions:
    * [Azure](https://azure.microsoft.com/get-started/), if you're using Logic Apps
    * [Power Automate](/flow/sign-up-sign-in)
* Basic experience building [logic apps](/azure/logic-apps/logic-apps-create-a-logic-app) or [flows](/flow/get-started-logic-flow), and [custom connectors](define-openapi-definition.md).
* If you're using Logic Apps, first [create an Azure Logic Apps custom connector](create-logic-apps-connector.md).
* Basic understanding of [webhooks](http://www.webhooks.org/).
* Basic understanding of the [OpenAPI Specification](http://swagger.io/specification/) (formerly known as Swagger).
* A [GitHub](https://www.github.com) account.
* The [sample OpenAPI definition](https://procsi.blob.core.windows.net/docs/githubWebhookSample.json) for this tutorial.

## The OpenAPI definition

Webhooks are implemented in Logic Apps and Power Automate as part of a [custom connector](index.md), so you need to provide an OpenAPI definition that defines the shape of the webhook. If you want to create a trigger but you don't have an OpenAPI definition, you can use the [triggers UI](#create-webhook-triggers-from-the-ui) in the custom connector wizard to define webhook triggers.

The OpenAPI definition contains three parts that are critical to making the webhook work:

* Creating the webhook
* Defining the incoming hook request from the API (in this case, GitHub)
* Deleting the webhook

### Creating the webhook

The webhook is created on the GitHub side by an HTTP POST to `/repos/{owner}/{repo}/hooks`. When a new logic app or flow is created, it posts to this URL using the trigger defined in the OpenAPI definition. It also posts to the URL if the trigger is modified. In the sample below, the `post` property contains the schema of the request that is posted to GitHub.

```json
"/repos/{owner}/{repo}/hooks": {
    "x-ms-notification-content": {
    "description": "Details for Webhook",
    "schema": {
        "$ref": "#/definitions/WebhookPushResponse"
    }
    },
    "post": {
    "description": "Creates a Github webhook",
    "summary": "Triggers when a PUSH event occurs",
    "operationId": "webhook-trigger",
    "x-ms-trigger": "single",
    "parameters": [
        {
        "name": "owner",
        "in": "path",
        "description": "Name of the owner of targeted repository",
        "required": true,
        "type": "string"
        },
        {
        "name": "repo",
        "in": "path",
        "description": "Name of the repository",
        "required": true,
        "type": "string"
        },
        {
        "name": "Request body of webhook",
        "in": "body",
        "description": "This is the request body of the Webhook",
        "schema": {
            "$ref": "#/definitions/WebhookRequestBody"
        }
        }
    ],
    "responses": {
        "201": {
        "description": "Created",
        "schema": {
            "$ref": "#/definitions/WebhookCreationResponse"
        }
        }
    }
    }
},
```

> [!IMPORTANT]
> The `"x-ms-trigger": "single"` property is a schema extension that tells Logic Apps and Power Automate to display this webhook in the list of available triggers in the designer, so be sure to include it.

### Define the incoming hook request from the API
The shape of the incoming hook request (the notification from GitHub to Logic Apps or Power Automate) is defined in the custom `x-ms-notification-content` property, as shown in the sample above. It doesn't need to contain the entire contents of the request, just the portions you want to use in your logic app or flow.

### Delete the webhook
The OpenAPI definition **must** include a definition for how to delete the webhook. Logic Apps and Power Automate attempt to delete the webhook if you update the trigger, and if you delete the logic app or flow.

```json
"/repos/{owner}/{repo}/hooks/{hook_Id}": {
    "delete": {
    "description": "Deletes a Github webhook",
    "operationId": "DeleteTrigger",
    "parameters": [
        {
        "name": "owner",
        "in": "path",
        "description": "Name of the owner of targeted repository",
        "required": true,
        "type": "string"
        },
        {
        "name": "repo",
        "in": "path",
        "description": "Name of the repository",
        "required": true,
        "type": "string"
        },
        {
        "name": "hook_Id",
        "in": "path",
        "description": "ID of the Hook being deleted",
        "required": true,
        "type": "string"
        }
    ]
    }
},
```

No aditional header is included for the delete webhook call. The same connection used in the connector is also used for the delete webhook call.

> [!IMPORTANT]
> In order for Logic Apps or Power Automate to delete a webhook, the API **must** include a `Location` HTTP header in the 201 response at the time the webhook is created. The `Location` header should contain the path to the webhook that is used with the HTTP DELETE. For example, the `Location` included with GitHub's response follows this format: `https://api.github.com/repos/<user name>/<repo name>/hooks/<hook ID>`. 


## Enable authentication in GitHub

The API that sends the webhook request to Logic Apps or Power Automate typically uses some form of authentication, and GitHub is no exception. GitHub supports several types of authentication; we'll use GitHub personal access tokens for this tutorial.

1. Navigate to [GitHub](https://www.github.com) and sign in if you haven't already.

2. In the upper right, select your **profile picture**, and then, in the menu, choose **Settings**.
   
    ![Settings](./media/create-webhook-trigger/github-settings.png)

3. In the menu on the left, choose **Developer settings**, then choose **Personal access tokens**.

4. Choose the **Generate new token** button, then confirm your password if requested.
   
    ![Generate new token](./media/create-webhook-trigger/generate-new-token.png)

5. In the **Token description** box, enter a description.

6. Select the **admin:repo_hook** checkbox.
   
    ![admin:repo_hook](./media/create-webhook-trigger/repo-hook.png)

7. Choose the **Generate token** button.

8. Make note of your new token.
   
    ![New token](./media/create-webhook-trigger/new-token.png)
   
   > [!IMPORTANT]
   > You won't be able to access this token again. You should copy and paste it somewhere to use later in the tutorial.

## Import the OpenAPI definition

Start by importing the OpenAPI definition for [Logic Apps](#import-the-openapi-definition-for-logic-apps), or for [Power Automate](#import-the-openapi-definition-for-power-automate).

### Import the OpenAPI definition for Logic Apps

1. Go to the [Azure portal](https://portal.azure.com) and open the Logic Apps connector you created earlier in [Create an Azure Logic Apps custom connector](create-logic-apps-connector.md).

2. In your connector's menu, choose **Logic Apps Connector**, then choose **Edit**.

   ![Edit Logic Apps Connector](./media/create-webhook-trigger/logic-apps-edit-connector.png)

3. Under **General**, choose **Upload an OpenAPI file**, then navigate to the OpenAPI file that you downloaded.

   ![Screenshot that shows the Upload an OpenAPI file option.](./media/create-webhook-trigger/logic-apps-upload-definition.png)

### Import the OpenAPI definition for Power Automate

1. Go to [flow.microsoft.com](https://flow.microsoft.com).

2. In the upper right corner, choose the gear icon, then choose **Custom connectors**.

    ![Custom connectors](./media/create-webhook-trigger/custom-apis.png)

3. Choose **Create custom connector**, then choose **Import a Postman collection**.

   ![Create custom connector](./media/create-webhook-trigger/flow-apps-create-connector.png)

4. Enter a name for the custom connector, then navigate to the OpenAPI file that you downloaded, and choose **Connect**.

   ![Screenshot that shows the field for entering a name.](./media/create-webhook-trigger/flow-apps-upload-definition.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Custom connector title** | "GitHubDemo" | 
   || 

## Finish creating the custom connector

1. On the **General** page, choose **Continue**.

2. On the **Security** page, under **Authentication type**, select **Basic authentication**.

3. In the **Basic authentication** section, for the label fields, enter the text **User name** and **Password**. These are only labels that will be displayed when the trigger is used in a logic app or flow.
   
    ![Basic auth](./media/create-webhook-trigger/basic-auth.png)

4. At the top of the wizard, make sure the name is set to "GitHubDemo", then choose **Create connector**.

You're now ready to [use the trigger in a logic app or flow](#use-the-webhook-as-a-trigger), or you can read on about how to create triggers from the UI.

## Create webhook triggers from the UI

In this section, we'll show you how to create a trigger in the UI without having any trigger definitions in your OpenAPI definition. Start with a baseline OpenAPI definition, or start from scratch in the custom connector wizard.

1. On the **General page**, make sure you specify a description and URL.

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Description** | "GitHub is a social source code repository." | 
   | **URL** | "api.github.com" | 
   || 

2. On the **Security** page, configure basic authentication like you did in the previous section. 

3. On the **Definition** page, choose **+ New trigger**, and fill out the description for your trigger. In this example, you are creating a trigger that fires when a pull request is made to a repository.
   
    ![Create Trigger-1](./media/create-webhook-trigger/create-new-trigger-1.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Summary** | "Triggers when a pull request is made to a selected repository" | 
   | **Description** | "Triggers when a pull request is made to a selected repository" | 
   | **Operation ID** | "webhook-PR-trigger" | 
   | **Visibility** | "none" (see below for more information) | 
   | **Trigger type** | "Webhook" | 
   || 

    The **Visibility** property for operations and parameters in a logic app or flow has the following options:
    * **none**: displayed normally in the logic app or flow
    * **advanced**: hidden under an additional menu
    * **internal**: hidden from the user
    * **important**: always shown to the user first

4. The **Request** area displays information based on the HTTP request for the action. Choose **Import from sample**.

    ![Definition page - import from sample](./media/create-webhook-trigger/import-sample.png)

5. Define the request for the webhook trigger, then choose **Import**. We provide a sample for you to import (below the image). For more information, see the [GitHub API reference](https://developer.github.com/v3/repos/hooks/#create-a-hook). Logic Apps and Power Automate automatically add standard `content-type` and security headers, so you don’t need to define those while importing from a sample.
   
    ![Create Trigger-2](./media/create-webhook-trigger/create-new-trigger-2.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Verb** | "POST" | 
   | **URL** | "https://api.github.com/repos/{owner}/{repo}/hooks" 
   | **Body** | See below |
   ||

   ```JSON
   {
     "name": "web",
     "active": true,
     "events": [
       "pull_request"
     ],
     "config": {
       "url": "http://example.com/webhook"
     }
   }
   ```

6. The **Response** area displays information based on the HTTP response for the action. Choose **Add default response**.

    ![Definition page - response](./media/create-webhook-trigger/definition-response.png) 


7. Define the response for the webhook trigger, then choose **Import**. Again, we provide a sample for you to import. For more information, see the [GitHub API reference](https://developer.github.com/v3/activity/events/types/#pullrequestevent). 

    ![Create Trigger-3](./media/create-webhook-trigger/create-new-trigger-3.png)

   ```JSON
   {
     "action": "opened",
     "number": 1,
     "pull_request": {
       "html_url": "https://github.com/baxterthehacker/public-repo/pull/1",
       "state": "open",
       "locked": false,
       "title": "Update the README with new information",
       "user": {
         "login": "baxterthehacker",
         "type": "User"
       }
     }
   }
   ```

8. In the **Trigger configuration** area, select the parameter that should receive the callback URL value from GitHub. This is the `url` property in the `config` object.
   
    ![Create Trigger-4](./media/create-webhook-trigger/create-new-trigger-4.png)

9. At the top of the wizard, enter a name, then choose **Create connector**.

## Use the webhook as a trigger

Now that you've got everything configured, you can use the webhook in a logic app or flow. Next, create a flow that will send a push notification to the Power Automate mobile app whenever your GitHub repo receives a git push.

1. In [flow.microsoft.com](https://flow.microsoft.com), at the top of the page, choose **My flows**.

2. Choose **Create from blank**, then on the next page, choose **Search hundreds of connectors and triggers**.

    ![Search connectors](./media/create-webhook-trigger/search-connectors.png)

3. In the designer for Power Automate, search for the custom connector you registered earlier.
   
    ![New trigger](./media/create-webhook-trigger/new-trigger.png)
   
    Choose the item in the list to use it as a trigger.

4. Since this is the first time you've used this custom connector, you have to connect to it. Enter connection information, then choose **Create**.
   
    ![New connection](./media/create-webhook-trigger/new-connection.png)
   
   | Parameter | Value | 
   | --------- | --------------- | 
   | **Connection name** | A descriptive name | 
   | **User name** | Your GitHub username 
   | **Password** | The personal access token you created earlier |
   ||

5. Enter details about the repo you want to monitor. You might recognize the fields from the **WebhookRequestBody** object in the OpenAPI file.
   
    ![Repo info](./media/create-webhook-trigger/repo-info.png)
   
   | Parameter | Value | 
   | --------- | --------------- | 
   | **owner** | The owner of the repo to monitor | 
   | **repo** | The repo to monitor |
   ||

   > [!IMPORTANT]
   > You should use a repo that your account has rights to. The easiest way to do this would be to use your own repo.

6. Choose **+ New step**, and then choose **Add an action**.

7. Search for and select the **Push notification** action.
   
    ![Push notification](./media/create-webhook-trigger/push-notification.png)

8. Enter some text in the **Text** field and the other fields, using values from the dynamic content dialog box. Note that these values come from the **WebhookPushResponse** object in the OpenAPI file.
   
    ![Push notification details](./media/create-webhook-trigger/push-details.png)

   | Parameter | Value | 
   | --------- | --------------- | 
   | **Connection name** | A descriptive name | 
   | **User name** | Your GitHub username 
   | **Password** | The personal access token you created earlier |
   ||

9. At the top of the page, give the flow a name and choose **Create flow**.
   
    ![Flow name](./media/create-webhook-trigger/flow-name.png)

## Verification and troubleshooting

To verify everything is set up correctly, choose **My flows**, then choose the **information icon** next to the new flow to view the run history:

* You should already see at least one "Succeeded" run from the webhook creation. This indicates that the webhook was created successfully on the GitHub side. 

* If the run failed, you can drill into the run details to see why it failed. If the failure was due to a "404 Not Found" response, it's likely your GitHub account doesn't have the correct permissions to create a webhook on the repo you used.

## Summary
If everything is correctly configured, you'll now receive push notifications in the Power Automate mobile app whenever a git push occurs on the GitHub repository you selected. Using the process above, you can use any webhook-capable service as a trigger in your flows.

## Next steps

* [Create a custom connector for a web API](create-web-api-connector.md)
* [Authenticate your API and connector with Azure Active Directory](azure-active-directory-authentication.md)

[!INCLUDE[feedback](../includes/feedback.md)]
