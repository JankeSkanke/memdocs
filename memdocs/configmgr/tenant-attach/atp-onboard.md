---
title: Tenant attach - Onboard Configuration Manager clients to Microsoft Defender for Endpoint from the Microsoft Endpoint Manager admin center (preview)
titleSuffix: Configuration Manager
description: "Deploy Microsoft Defender for Endpoint Detection and Response (EDR) onboarding policies to Configuration Manager managed clients from the admin center."
ms.date: 09/27/2021
ms.topic: conceptual
ms.prod: configuration-manager
ms.technology: configmgr-core
manager: dougeby
author: mestew
ms.author: mstewart
ms.localizationpriority: high
---

# <a name="bkmk_atp"></a> Tenant attach: Onboard Configuration Manager clients to Microsoft Defender for Endpoint from the admin center (preview)
<!--5691658-->
*Applies to: Configuration Manager (current branch)*

> [!Important]
> This information relates to a preview feature which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.

Microsoft Endpoint Manager is an integrated solution for managing all of your devices. Microsoft brings together Configuration Manager and Intune into a single console called **Microsoft Endpoint Manager admin center**. You can deploy Microsoft Defender for Endpoint onboarding policies to Configuration Manager managed clients. These clients don't require Azure AD or MDM enrollment, and the policy is targeted at ConfigMgr collections rather than Azure AD Groups.

<!--Adding Include for Prerequisites-->

[!INCLUDE [Prerequisites for Configuration Manager tenant attached devices](./includes/configmgr-endpoint-security-prerequisties.md)]
- [Microsoft Intune and Microsoft Defender for Endpoint integration enabled](../../intune/protect/advanced-threat-protection-configure.md#enable-microsoft-defender-for-endpoint-in-intune)
- Client onboarded to [Microsoft Defender for Endpoint](/windows/security/threat-protection/microsoft-defender-atp/minimum-requirements#licensing-requirements).<!--Adding MDE License Requirement-->

## <a name="bkmk_onboard"></a> Create Microsoft Defender for Endpoint policies

1. Sign in to the [Microsoft Endpoint Manager admin center](https://endpoint.microsoft.com).

1. Select **Endpoint security** > **Endpoint detection and response** > **Create Policy**.

1. Select the following platform and profile for your policy:

   - Platform: **Windows 10, Windows 11, and Windows Server (ConfigMgr)**
   - Profile: **Endpoint detection and response (ConfigMgr)**

1. Select **Create**.

1. On the **Basics** page, enter a name and description for the profile, then choose **Next**.

1. On the **Configuration settings** page, configure the settings you want to manage with this profile. The onboarding package is automatically included and isn’t something you can configure.

   When your done configuring settings, select **Next**.

1. On the **Assignments** page, select the collections that will receive this policy. Select collections from Configuration Manager that you’ve synced to Microsoft Endpoint Manager admin center and enabled for Microsoft Defender for Endpoint policy.

   You can choose not to assign collections at this time, and later edit the policy to add an assignment.

   When ready to continue, select **Next**.

1. On the **Review + create** page, when you're done, choose **Create**.

   The new profile is displayed in the list when you select the policy type for the profile you created.

[!INCLUDE [Device status for Configuration Manager tenant attached devices](./includes/configmgr-endpoint-security-device-status.md)]

## Next steps

- [Create and deploy endpoint security Antivirus policy to tenant attached devices](deploy-antivirus-policy.md)
- [Create and deploy endpoint security Firewall policy to tenant attached devices](deploy-firewall-policy.md)
