---
title: Migrate Microsoft Tunnel to the Microsoft Defender for Endpoint app for Microsoft Intune
description: Migrate your Microsoft Tunnel configuration from using the standalone tunnel client app to use Microsoft Defender for Endpoint.
keywords:
author: brenduns
ms.author: brenduns
manager: dougeby
ms.date: 09/09/2021
ms.topic: how-to
ms.service: microsoft-intune
ms.subservice: protect
ms.localizationpriority: high
ms.technology:

# optional metadata

#ROBOTS:
#audience:
 
ms.reviewer: tycast
ms.suite: ems
search.appverid: MET150
#ms.tgt_pltfrm:
ms.custom: intune-azure
ms.collection: M365-identity-device-management
---

# Migrate to Microsoft Defender for Endpoint for the Microsoft Tunnel in Intune

On June 14 2021, Microsoft Defender for Endpoint became generally available as the Microsoft Tunnel client app for Android for use with the Microsoft Tunnel Gateway in Microsoft Intune.

If you've previously configured Microsoft Tunnel for Android using the standalone Microsoft Tunnel client app, you must migrate your devices to use Microsoft Defender for Endpoint as the tunnel client app before support for the Android standalone tunnel client app ends on January 31, 2022.

The following device platforms support Microsoft Defender for Endpoint as the tunnel client app:

- Android Enterprise:
  - Fully managed
  - Corporate-owned work profile
  - Personally-owned work Profile

When using Microsoft Defender for Endpoint to connect to Tunnel, use [custom settings](../protect/microsoft-tunnel-configure.md#use-custom-settings-for-microsoft-defender-for-endpoint) in the VPN profile to manage Defender for Endpoint instead of using a separate app configuration profile. If you do not intend to use any Defender functionality, including web protection, use [custom settings](../protect/microsoft-tunnel-configure.md#use-custom-settings-for-microsoft-defender-for-endpoint) in the VPN profile and set the **defendertoggle** setting to **0**.

<!-- Hiding the following info box, but keeping it for historical context and in case these issues resurface in the future >
  > [!IMPORTANT]
  > If you are using per-app VPN and also have Defender web protection enabled, you may experience connectivity issues for apps outside your per-app VPN list in the following scenarios, which may prevent devices from communicating with Intune:
  > - You are using an internal proxy. In this case, you must disable web protection in the VPN profile by adding the **antiphishing** setting in the [custom settings](../protect/microsoft-tunnel-configure.md#use-custom-settings-for-microsoft-defender-for-endpoint) section and entering a value of **0**.
  > - You are using internal DNS servers. You must include the IP address of at least one publicly-accessible DNS server, like 1.1.1.1, in your Tunnel Gateway [server configurations](../protect/microsoft-tunnel-configure.md#create-a-server-configuration).
  >
-->
<!-- The following is retained for future use should iOS receive the same style of preview  >

Unlike most public previews for Intune, you must opt in before you can use  this preview. When you opt in, Microsoft grants your tenant access to the preview build of Microsoft Defender for Endpoint that supports the tunnel app functionality. After you opt in:

- Your tenant receives access to the app, and we'll send you an email with instructions for deploying it.
- You’ll deploy the Microsoft Defender for Endpoint app to devices.
- Replace your existing VPN profile for Microsoft Tunnel with a new VPN profile that directs devices to use the Microsoft Defender for Endpoint app.

## Changes introduced with this preview
-->

## Changes introduced to support Defender for Endpoint

The introduction of Microsoft Defender for Endpoint as the tunnel client app brings the following changes.

**Renamed connection type for VPN profiles for all tenants**:

To support Defender for Endpoint, all VPN profiles created before March 2, 2021 that have a connection type of **Microsoft Tunnel** were updated to a connection type of **Microsoft Tunnel (standalone client)**.

This change:  

- Applies to all tenants.
- Applies to both the Android and iOS/iPadOS platforms, even though there's no active preview or support by iOS/iPadOS for Defender for Endpoint as the tunnel client app.
- Has no effect on the functionality of those existing profiles other than the change of connection type name.
- Supports the change to use Microsoft Defender for Endpoint to support Microsoft Tunnel functionality now or at a future time.
- Cannot be reversed. You can’t edit existing profiles to change their connection type.

The following connection types are now available in VPN profiles:

- **Android**:
  - **Microsoft Tunnel**
    - A VPN profile with this connection type directs devices to use the Microsoft Defender for Endpoint app to connect to Microsoft Tunnel Gateway.
    - Use this connection type with VPN profiles for devices that run Android Enterprise.
    - A connection type of *Microsoft Tunnel (standalone client)* can no longer be created for Android. Existing VPN profiles with this connection type should be migrated to *Microsoft Tunnel* and use of Defender for Endpoint as the tunnel client app.

- **iOS/iPadOS**:
  - **Microsoft Tunnel (standalone client) (preview)**
    - A VPN profile with a connection type of *Microsoft Tunnel* and use of Defender for Endpoint isn't supported for iOS/iPad OS VPN profiles.  

**End-user changes**:

For Android, the Microsoft Defender for Endpoint app you use as the tunnel client app includes a new tab for the Microsoft Tunnel functionality.

## Licensing

The Microsoft Defender for Endpoint app combines functionality of Microsoft Defender for Endpoint with the functionality of the Microsoft Tunnel app. The new app doesn't require any change to your existing licenses.

A license for Microsoft Intune grants access to the following tab of the app:

- **Tunnel** is where users connect to the Tunnel Gateway and can view connection statistics and client configuration settings.

A license for Microsoft Defender for Endpoint grants access to the following tabs:

- **Dashboard** displays a summary of the device’s overall health, app security status, web protection status, and Tunnel status.
- **App security** is where users can view the status of automatic scans on the device. Users can also uninstall the apps identified as threats and run a manual scan.
- **Web Protection** shows the status of the feature enabled or disabled by administrators, and details of the feature described in the flip cards.

With both licenses, all four tabs are available.

:::image type="content" source="./media/microsoft-tunnel-migrate-app/defender-app-tabs.png" alt-text="Screenshot of the Defender for Endpoint tabs.":::

For information about license requirements for Microsoft Defender for Endpoint, see [Get Microsoft Defender for Endpoint](/windows/security/threat-protection/microsoft-defender-atp/switch-to-microsoft-defender-prepare#get-microsoft-defender-for-endpoint).

## Migrate devices to Microsoft Defender for Endpoint

When you're ready to use Microsoft Defender for Endpoint, migrate supported devices from the standalone tunnel client app to the new app. You can also deploy the new app to other devices that haven't previously used the Microsoft Tunnel.

Migrating to Microsoft Defender for Endpoint requires the following broad actions, which are described in the following sections:

<!-- Retained for use should iOS receive a preview > 
1. Join the public preview.
-->
1. Review and record your current Tunnel configurations.
1. Deploy Microsoft Defender for Endpoint to supported devices.
1. Create new VPN profiles.
1. Clean up your previous deployments.

<!-- Retained for use should iOS receive a preview > 
### Join the Microsoft Defender for Endpoint public preview for Microsoft Tunnel

To gain access to the Microsoft Defender for Endpoint app that includes support for Microsoft Tunnel, you must opt into the public preview. That’s because the version of the app that’s available from the regular Google Play store doesn’t support the Microsoft Tunnel functionality. Only tenants that sign-up for the preview will have access to the preview app.

**To opt into the preview**:

Sign up at https://aka.ms/VPNpreview where you provide your Managed Google Play Organization ID and contact email.

After you sign up, you’ll be alerted by email when your tenant has access to the preview app. The email includes instructions for deploying the preview Microsoft Defender for Endpoint app from the Managed Google Play store.
-->

<!-- No longer needed due to fix made in early September, 2021, but retaining for history and in case issues arise again >
> [!IMPORTANT]
>
> If you use *Always-on VPN* with the standalone Tunnel client app today, during migration to Microsoft Defender for Endpoint:
>
> - Set *Always-on VPN* to **Not configured** in profiles for **Microsoft Tunnel (standalone client)**, which is the old client app.
> - Set *Always-on VPN* to **Enable** in profiles for **Microsoft Tunnel**, which is the new Microsoft Defender for Endpoint client app.
-->

### Review and record your current Tunnel configurations

Before you begin your migration to Defender for Endpoint, take the time to review and record the settings you currently use for the following Intune configurations:

- VPN profiles for Microsoft Tunnel
- App deployments of the Microsoft Tunnel

You'll use this information when you deploy new VPN profiles and the Microsoft Defender for Endpoint app, to mirror your existing deployments.

1. Sign in to [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431) > **Devices** > **Configuration profiles**. Locate the VPN profiles you use for Microsoft Tunnel for your Android devices. They display a connection type of *Microsoft Tunnel (standalone client)*. You’ll replace these profiles with new profiles that use the Defender for Endpoint app.

   1. Select each profile and then **Properties**.

   2. From Properties, record the available values. This information will help you create new VPN profiles that mirror your current configurations.

1. Next, record details for your Tunnel app deployments. In the admin center, go to **Apps**. Locate your deployments of Microsoft Tunnel to Android Enterprise devices.

   1. Select each applicable deployment and review its **Properties**.

   2. From Properties, record the available values. This information will help you to create similar deployments for the Microsoft Defender for Endpoint app.

### Deploy Microsoft Defender for Endpoint
<!-- Retained for use should iOS receive a preview > 
After receiving confirmation for the public preview, you can access and deploy the preview version of the Microsoft Defender for Endpoint app to your devices. This app is available from the Managed Google Play store.
-->
Microsoft Defender for Endpoint with support for Microsoft Tunnel on Android, is available from the Managed Google Play store.

1. Locate and **Approve** the app in the Managed Google Play store for your tenant, and then **Sync** it. For information on this process, see [Managed Google Play store apps](../apps/apps-add-android-for-work.md#managed-google-play-store-apps).

2. **Assign** the app to groups.
<!-- Retained for use should iOS receive a preview>  While assigning the app, under App settings set **Tracks** to **SuperApp Public Preview**. 

   Only **Available** deployments are supported for the public preview. -->
3. Complete the assignment, and then ask users to install the Microsoft Defender for Endpoint app.

### Create new VPN profiles

To enable devices to use Microsoft Defender for Endpoint to connect to Microsoft Tunnel Gateway, deploy new VPN profiles with the Microsoft Tunnel connection type. Editing the connection type of an existing profile isn’t supported.

1. Use the information from [Create a VPN Profile](../protect/microsoft-tunnel-configure.md#create-a-vpn-profile) to create and deploy new VPN profiles for your Android Enterprise devices.

2. During configuration, reference the settings you recorded from your existing profiles, but use a *connection type* of **Microsoft Tunnel**.

  > [!NOTE]
  > If you are using the Microsoft Defender for Endpoint app for Android, have web protection enabled, and are using per-app VPN, web protection will only apply to the apps in the per-app VPN list. On devices with a work profile, in this scenario we recommend adding all web browsers in the work profile to the per-app VPN list to ensure all work profile web traffic is protected.

### Clean up previous deployments

After devices install the Microsoft Defender for Endpoint app and receive new VPN profiles, you can remove configurations for the original deployments.

For deployments of the original Microsoft Tunnel app:

1. Remove **Required** and **Available for enrolled devices**.

2. Add **Uninstall** to trigger removal of the application.

## Next Steps

[Use Conditional Access with the Microsoft Tunnel](microsoft-tunnel-conditional-access.md)  
[Monitor Microsoft Tunnel](microsoft-tunnel-monitor.md)
