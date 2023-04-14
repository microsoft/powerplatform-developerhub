---
title: Create custom connectors in solutions | MicrosoftDocs
description: Learn how to use solutions to manage the application lifecycle of custom connectors.
Keywords: connectors, custom connectors, canvas app, flow
author: samrat-gavale
contributors:
  - samrat-gavale
  - v-aangie
ms.author: sagavale
ms.reviewer: angieandrews
ms.date: 10/28/2022
ms.service: connectors
ms.topic: conceptual
---
# Create custom connectors in solutions

By using solutions, you can move your custom connectors&mdash;along with canvas apps, flows, and Microsoft Dataverse customizations&mdash;in a single package. Solutions have the added benefit of managing your customizations in Azure DevOps and automating your CI/CD process.

## Get started

Solutions are accessible both from [Power Apps](https://make.powerapps.com) and [Power Automate](https://flow.microsoft.com).

### Prerequisites

You must have a license for Dataverse or Dynamics 365 Customer Engagement and a valid security role granting create access for connectors.

### Steps

1. Select the **Solutions** tab.

1. Create a new solution or open an existing [unmanaged solution](/powerapps/maker/data-platform/solutions-overview).

1. Select **New** > **Automation** > **Custom connector**.

1. A new tab opens, where you [create a new custom connector](index.md).

1. When you've finished, select **Save**.

1. Close the browser tab. You'll see the new custom connector listed within your solution.

1. To migrate the custom connector and any other customizations, export your solution, and then import it to the target environment.

> [!NOTE]
>
> You'll need to re-enter credentials required by the connector, and then create connections.

## Known limitations

- Adding custom connectors created outside a solution.
- Dependencies aren't logged.
- Managed properties.
- The custom connectors created inside solutions must be shared through the [Dataverse GrantAccess Api](/power-apps/developer/data-platform/webapi/reference/grantaccess). To obtain the `connectorid` to be used in such a request, you can query the connectors table in Dataverse and filter on the `displayname` property. To learn more, go to [Manage solution custom connectors with Dataverse APIs](solution-custom-api.md).
- Backup/restore/copy.
- Custom connectors aren't available in classic solution explorer.
- Custom connectors need to be imported first, before connection references or flows.
  - If your environment doesn’t contain the custom connector in a solution, import a separate solution that only the custom connectors. Do this importation before you import the actual solution because Azure needs to register the custom connector beforehand.
  - If you import a solution that contains custom connectors and flows, Azure won't be able to register the custom connector while it's registering your connection references or flows. This also applies to connection references for the custom connector that wasn't previously imported in a separate solution. If Azure hasn't registered your custom connector, the importation will fail, or you won't be able to start the import.
- Solution custom connectors are created without role assignments by default.
  - The **Power Apps for Admin** connector has the **Get Custom Connectors as Admin** action that doesn't return solution custom connectors without role assignments. Users can add role assignments to custom connectors through sharing or through the **Power Apps for Admin** connector.
  - Role assignments aren't maintained when you import/export custom connectors across environments. This is because role assignments depend on specific users in the environment. Once the solution custom connector is created, configure [role privileges](/power-platform/admin/security-roles-privileges) and [role based security](/power-platform/admin/database-security) as needed.

### See also

[Build and certify custom connectors](/powerapps/maker/canvas-apps/register-custom-api)<br/>
[Use solutions in Power Apps](/powerapps/maker/common-data-service/use-solution-explorer)

[!INCLUDE[feedback](../includes/feedback.md)]
