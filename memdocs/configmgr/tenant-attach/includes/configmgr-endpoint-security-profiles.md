---
author: mestew
ms.author: mstewart
ms.prod: configuration-manager
ms.technology: configmgr-core
ms.topic: include
ms.localizationpriority: high
ms.date: 09/27/2021
---
<!--Don't apply H2/H3 in this include file since they are context driven by article. This file is currently used by endpoint-security-get-started.md and deploy-antivirus-policy.md. -->
The following profiles are supported for devices you manage with Configuration Manager current branch 2006 or later, through the tenant attach scenario:
<!--The following profiles are supported for devices you manage with Configuration Manager Technical Preview 2007 or later, through the tenant attach scenario:-->

- Platform: **Windows 10, Windows 11, and Windows Server (ConfigMgr)**

  - Profile: **Microsoft Defender Antivirus Policy (preview)** - Manage [Antivirus policy settings for Configuration Manager devices](../../../intune/protect/antivirus-microsoft-defender-settings-windows-tenant-attach.md?toc=/mem/configmgr/tenant-attach/toc.json&bc=/mem/configmgr/tenant-attach/breadcrumb/toc.json), when you use tenant attach.

    This profile is supported with devices that are tenant attached and run the following platforms:
    - Windows 10 and later (x86, x64, ARM64)
    - Windows Server 2019 and later (x64)
    - Windows Server 2016 (x64)
    - Windows 8.1 (x86, x64), starting in Configuration Manager version 2010 <!--8763780, 8740844-->
    - Windows Server 2012 R2 (x64), starting in Configuration Manager version 2010 <!--8763780, 8740844-->

  - Profile: **Endpoint detection and response (ConfigMgr)** - Manage [Endpoint detection and response policy settings](../../../intune/protect/endpoint-security-edr-profile-settings.md?toc=/mem/configmgr/tenant-attach/toc.json&bc=/mem/configmgr/tenant-attach/breadcrumb/toc.json), when you use tenant attach.

    This profile is supported with devices that are tenant attached and run the following platforms:

    - Windows 10 and later (x86, x64, ARM64)
    - Windows 8.1 (x84, x64)
    - Windows Server 2019 and later (x64)
    - Windows Server 2016 (x64)
    - Windows Server 2012 R2 (x64)

  - Profile: **Windows Security experience (preview)** - Manage [Windows Security app settings for Configuration Manager devices](../../../intune/protect/antivirus-windows-security-settings-windows-tenant-attach.md?toc=/mem/configmgr/tenant-attach/toc.json&bc=/mem/configmgr/tenant-attach/breadcrumb/toc.json), when you use tenant attach.

    This profile is supported with devices that are tenant attached and run the following platforms:
    - Windows 10 and later (x86, x64, ARM64)
    - Windows Server 2019 and later (x64)
  
  > [!Important]
  > To support managing tamper protection your environment must additionally meet the [prerequisites for managing tamper protection with Intune](/windows/security/threat-protection/microsoft-defender-antivirus/prevent-changes-to-security-settings-with-tamper-protection#turn-tamper-protection-on-or-off-for-your-organization-using-intune) as detailed in the Windows documentation.

- Platform: **Windows 10 and later**

  - Profile: **Microsoft Defender Firewall (ConfigMgr) (preview)** - Manage [firewall policy settings for Configuration Manager devices](../../../intune/protect/endpoint-security-firewall-profile-settings-tenant-attach.md?toc=/mem/configmgr/tenant-attach/toc.json&bc=/mem/configmgr/tenant-attach/breadcrumb/toc.json), when you use tenant attach.

    This profile is supported with devices that are tenant attached and run the following platforms:
    - Windows 10 and later (x86, x64, ARM64)

    > [!Important]
    > To support firewall policies, install [KB4578605](https://support.microsoft.com/help/4578605/) for Configuration Manager version 2006. The update is available in the Configuration Manager console.

  - Profile: **Exploit Protection (ConfigMgr)(preview)** - Manage [Exploit Protection settings for Configuration Manager devices](../../../intune/protect/endpoint-security-asr-profile-settings.md?toc=/mem/configmgr/tenant-attach/toc.json&bc=/mem/configmgr/tenant-attach/breadcrumb/toc.json#attack-surface-reduction-configmgr) as part of Attack surface reduction policy, when you use tenant attach.

    This profile is supported with devices that are tenant attached and run the following platforms:

    - Windows 10 and later (x86, x64, ARM64)


  - Profile: **Web Protection (ConfigMgr)(preview)** - Manage [Web Protection settings for Configuration Manager devices](../../../intune/protect/endpoint-security-asr-profile-settings.md?toc=/mem/configmgr/tenant-attach/toc.json&bc=/mem/configmgr/tenant-attach/breadcrumb/toc.json#attack-surface-reduction-configmgr) as part of Attack surface reduction policy, when you use tenant attach.

    This profile is supported with devices that are tenant attached and run the following platforms:

    - Windows 10 and later (x86, x64, ARM64)
