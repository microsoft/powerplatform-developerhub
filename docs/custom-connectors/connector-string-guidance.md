---
title: Connector String Guidance
description: Guidelines to follow for some string fields in connector OpenAPI definition.
author: bezhan-msft
services: connectors
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 06/28/2022
ms.author: bezhan
ms.reviewer: angieandrews
---

# Connector String Guidance

The following article contains general guidance for string fields within a connector for Power Automate, Power Apps, and Logic Apps.

## Connector Info

Each connector should have a title, which is the name of the connector, and a description, which describes the connector in general. This information should be specified in the _title_ and _description_ fields under the _info_ section in the OpenAPI definition (in the apiDefinition.swagger.json file).

The following guidelines at a minimum should be followed for connector titles and descriptions:

* There can be a maximum of 30 characters in the connector title.
* The connector title and description can't include the word `API`.
* The connector title and description can't refer to a Power Platform product or a product for which you don't own the backend APIs.

A higher standard of title and description field guidelines enforced for certified connectors can be found [here](certification-submission.md) and should be used as best practices.

## Operations

Each path and verb in the OpenAPI definition corresponds to an operation. Properly describing the operation with each of the strings/tags below helps the end user use it correctly. Some of the string fields for an operation are:

* **_summary_**: This will show up as the name of the operation.
  * Case: Sentence
  * Notes: 
      - There should be no slash ('/') in the name.
      - It shouldn't exceed 80 characters.
      - It shouldn't end with a non-alphanumeric character, including punctuation or space.

* **_description_**: This will show up as the operation description when selecting the information button ![Screenshot that shows the information button.](./media/connector-string-guidance/information.png), as shown in the following image.
   * Case: Sentence.
   * Notes: Keep short to fit in the text box. No period required if there's a single word.

* **_operationId_**: This is the unique ID associated with the operation.
   * Case: Camel (no spaces or punctuation).
   * Notes: Intended to convey the meaning of the operation, such as GetContacts or CreateContact.

   The following image shows how the _summary_&mdash;**Send an email**, and _description_&mdash;**This operation sends an email message** fields will appear while you're creating a workflow.

  ![Screenshot that shows how the summary and description fields will appear.](./media/connector-string-guidance/operations.png "Summary and description fields")

### Triggers versus Actions

A trigger starts a workflow or process. For example, "Start a workflow every Monday at 3 AM", "When an object is created", and so on. 

Trigger summary and description fields should be human readable and have semantic meaning. Trigger _summary_ is usually in the format: "When a __________________". 

Example:

Trigger | Summary
--| --
Create | When a task is created
Update | When a task is updated
Deleted | When a task is deleted

Trigger _description_ is usually in the format: "This operation triggers when _______________" 

Example:

* This operation triggers when a new task is added.

An action is a task to complete within your workflow, such as "Send an email", "Update a row", "Send a notification", and so on. A few examples for action _summary_ are given below:

Action | Summary
--| --
Create | Create new task
Read | Get Task by ID
Update | Update object
Deleted | Delete object
List | List all objects

### Parameters

Each operation (whether a trigger or an action) has parameters that the user gives as input. Some of the important string fields for a parameter are:

* **_x-ms-summary_**: This will show up as the parameter name.
  * Case: Title
  * Notes: It has a limit of 80 characters

* **_description_**: This will show up as the parameter description within the input box.
  * Case: Sentence
  * Notes: Keep short to fit text box. No period required if there's a single word.

  In the image shown below, the parameter highlighted has "Subject" as the value for the _x-ms-summary_ field and "Specify the subject of the mail" as the _description_.

  ![Screenshot that shows the x-ms-summary and description parameter values in the interface.](./media/connector-string-guidance/parameters.png)

### Response

Each operation has a response that can be used later on in the workflow as an input to a subsequent operation. The result schema is made up of multiple properties. Some of the important string fields for each property are:

* **_x-ms-summary_**: This will show up as the result property name.
  * Case: Title
  * Notes: Use a short name.

* **_description_**: This will show up as the description for the result property.
  * Case: Sentence
  * Notes: Should be short and concise, with a period at the end.

  In the image shown below, the result schema from "Manually trigger a flow" operation shows up when you try to add dynamic content in one of the subsequent operations in the workflow. Here, "User email" is the _x-ms-summary_ and the text below it's the _description_ for a property in the response of 
  "Manually trigger a flow" operation.

![response](./media/connector-string-guidance/result.png)

A few important notes to be considered in general for summary/x-ms-summary and description fields are:

* Summary and description text shouldn't be the same.
* Description should be used to provide additional information to the user, such as the output format or the object the property is related to. For example: _summary_ : ID, _description_: User's ID.
* For an object with nested values, the _x-ms-summary_ of the parent name will be appended to the child.

### _x-ms-visibility_

Determines the visibility priority of the entity. If no visibility is specified, that is considered "normal" visibility. The possible values are "important", "advanced" or "internal". Entities marked as ‘internal’ don't show up in the user interface.

Applies to:

*	Operations
*	Parameters
*	Response properties

Example:

In the UI, entities marked as "important" are displayed first, things marked as "advanced" are hidden under a toggle (highlighted), and things marked as "internal" aren't displayed. The following image is an example of parameters marked as "important" being shown by default. It also shows parameters marked as "advanced" being shown after selecting the **Show advanced options** button.

![Screenshot that shows a dropdown list for advanced options.](./media/connector-string-guidance/x-ms-visibility-hidden.png "Show advanced options")

![Screenshot that shows the expanded hidden advanced options.](./media/connector-string-guidance/x-ms-visibility-visible.png "Hide advanced options")

[!INCLUDE[feedback](../includes/feedback.md)]
