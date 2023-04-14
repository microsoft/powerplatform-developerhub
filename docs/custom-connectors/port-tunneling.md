---
title: Use dev tunnels in Visual Studio to debug your web APIs
description: Learn how to use dev tunnels in Visual Studio to debug your web APIs for Power Automate and Power Apps.
author: jopanchal
contributors:
  - jopanchal
  - juliajuju93
  - v-aangie
ms.service: connectors
ms.topic: conceptual
ms.reviewer: angieandrews
ms.date: 02/16/2023
ms.author: jukasper
---

# Use dev tunnels in Visual Studio to debug your web APIs

To quickly debug and test your web APIs within Microsoft Power Automate or Power Apps, use *dev tunnels* in Visual Studio. Dev tunnels enables ad-hoc connections between machines that can't directly connect to each other. Once you enable this feature, you'll see that debugging (F5) automatically creates a dev tunnel URL that you can use to connect to Power Apps or Power Automate.

## Prerequisites

- Download [Visual Studio 2022 Preview](https://visualstudio.microsoft.com/vs/preview/) version 17.5 Preview 3 or later with the **ASP.NET and web development workload** installed. You need to sign in to Visual Studio to create and use dev tunnels. The feature isn't available in Visual Studio for Mac.

- One of the following Power Platform environments:
    - [Power Automate](/flow/sign-up-sign-in)
    - [Power Apps](/powerapps/signup-for-powerapps)

    > [!NOTE]
    >
    > If you need help getting started with Microsoft Power Platform, go to [Create a developer environment](/power-platform/developer/create-developer-environment).

## Step 1: Configure your ASP.NET Core project in Visual Studio

1. Enable the dev tunnels feature in Visual Studio by selecting **Tools** > **Options** > **Environment** > **Preview Features** > **Enable dev tunnels for Web Applications**.

1. In the debug dropdown menu, select **Dev Tunnels** > **Create A Tunnel**.

    :::image type="content" source="media/port-tunneling/devtunnelspicture.png" alt-text="Screenshot of creating a tunnel.":::

1. The tunnel creation dialog opens and you can configure dev tunnels. Make sure to set authentication type to **Public**.

    To learn more, go to [How to use dev tunnels in Visual Studio 2022 with ASP.NET Core apps](/aspnet/core/test/dev-tunnels?).

1. Select **OK**. Visual Studio displays confirmation of tunnel creation. The tunnel is now enabled and appears in the debug dropdown **Dev Tunnels** flyout.

1. Select **F5** (**Debug** > **Start Debugging**), or the **Start Debugging** button to see the dev tunnel URL.

### URL with and without dev tunnels

To learn more, go to [Use a tunnel](/aspnet/core/test/dev-tunnels?view=aspnetcore-7.0#use-a-tunnel&preserve-view=true).

- **Before debugging:** `https://localhost:7223/swagger/index.html`

- **After debugging:** `https://50tt58xr-7223.usw2.devtunnels.ms/swagger/indexf.html`

## Step 2: Create a custom connector for your web API using the dev tunnel URL

A custom connector is a wrapper around a REST API and allows Power Automate or Power Apps solutions to communicate with your web API. There are many ways to create a custom connector. The following sections explain how to use the dev tunnel URL and create a custom connector from scratch, or with API Management.

### Create a custom connector from scratch

1. On the **General** tab, post the dev tunnel URL into the **Host** field.

    :::image type="content" source="./media/port-tunneling/powerappsgeneral.png" alt-text="Screenshot of the General tab.":::

1. On the **Security** tab, select **No authentication** from the dropdown menu.

    :::image type="content" source="./media/port-tunneling/powerappssecurity.png" alt-text="Screenshot of the Security tab.":::

1. On the **Definition** tab, define your HTTP methods by adding actions. For the URL action, use the dev tunnel base URL + /actionName. For an example, go to [How to use dev tunnels](/aspnet/core/test/dev-tunnels?view=aspnetcore-7.0&preserve-view=true).

    :::image type="content" source="./media/port-tunneling/powerappsdefinition.png" alt-text="Screenshot of the Definition tab.":::

1. You can now test your custom connector. To do this, select the **Test** tab. After adding your connection, you can test your web API.

    For instructions, go to [Create a custom connector from scratch](define-blank.md).

### Create a custom connector with API Management

1. Go to your Azure API Management instance in the Azure portal.

1. Modify the runtime URL of your API in the menu under **Backends** and select your API instance.

1. On the **Properties** tab, replace the **Runtime URL** with the dev tunnel URL and select **Save**.

    :::image type="content" source="./media/port-tunneling/apimsettings.png" alt-text="Screenshot of the Settings tab.":::

1. On the **Power Platform** tab, you can now create a custom connector. For instructions, go to [Export APIs from Azure API Management to the Power Platform](/azure/api-management/export-api-power-platform).

## Step 3: Add the custom connector to Power Apps or Power Automate

To debug your web API, use a custom connector from a [Power Apps app](use-custom-connector-powerapps.md) or a [Power Automate flow](use-custom-connector-flow.md).

When your custom connector is integrated in your Power Platform solution, you can set a *breakpoint*, and debug your Power Apps app or Power Automate flow.

> [!NOTE]
> Breakpoints are the most basic and essential feature of reliable debugging. A breakpoint indicates where Visual Studio should suspend your running code so you review the values of variables, behavior of memory, or if a branch of code is getting run.

:::image type="content" source="./media/port-tunneling/PortTunnelingClip.gif" alt-text="Animation demo of debugging.":::

[!INCLUDE[feedback](../includes/feedback.md)]
