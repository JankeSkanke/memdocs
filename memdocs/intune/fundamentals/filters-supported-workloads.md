---
# required metadata

title: Platforms and policy types supported by filters in Microsoft Intune
description: Supported apps, compliance policies, and device configuration profiles that support filters in Microsoft Endpoint Manager and Microsoft Intune.
keywords:
author: MandiOhlinger
ms.author: mandia
manager: dougeby
ms.date: 09/13/2021
ms.topic: reference
ms.service: microsoft-intune
ms.subservice: fundamentals
ms.localizationpriority: high
ms.technology:

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: scottduf
ms.suite: ems
search.appverid: MET150
#ms.tgt_pltfrm:
ms.custom:
ms.collection: M365-identity-device-management
---

# List of platforms, policies, and app types supported by filters in Microsoft Endpoint Manager

When you create an app, compliance policy, or configuration profile, you assign the policy to groups (users or devices). When you assign the policy, you can also use filters. For example, you can assign policies to Windows 10 devices running a specific OS version. For more information, see [Use filters when assigning your apps, policies, and profiles](filters.md).

Filters support some of the different workloads available in Microsoft Intune. This article lists the app types, compliance policies, and device configuration profiles that support filters. It also lists the workloads that aren't supported.

## Before you begin

- This article assume you're familiar with filters. If not, you can learn more at [Use filters when assigning your apps, policies, and profiles](filters.md).
- ✔️: Supports filters.
- ❌: Doesn't support filters
- N/A: Doesn't apply to the platform.

## App types

You can use filters for some common app policies on the following platforms. For a list of what's not supported, see [not supported](#not-supported) (in this article).

### Android device administrator

| App type | Supported |
| --- | --- |
| Store app | ✔️ |
| Microsoft 365 apps | N/A |
| Microsoft Edge version 77 and newer | N/A |
| Microsoft Defender for Endpoint | N/A |
| Web link | ❌ |
| Line-of-business apps | ✔️ |

### Android Enterprise

| App type | Supported |
| --- | --- |
| Store app | N/A |
| Microsoft 365 apps | N/A |
| Microsoft Edge version 77 and newer | N/A |
| Microsoft Defender for Endpoint | N/A |
| Web link | N/A |
| Line-of-business apps | N/A |
| Android Enterprise system app  | ✔️ |
| Managed Google Play store app | ✔️ |
| Managed Google Play web link | ✔️ |
| Managed Android line-of-business app | ✔️ |

> [!NOTE]
> Filters aren't supported on Android Enterprise personally-owned devices with work profile (BYOD) when used in "Available" app assignments. If users are targeted with an "Available" app intent, then the app continues to show as available to install from the Google managed play store. Any include or exclude filtering is ignored.

### iOS/iPadOS

| App type | Supported |
| --- | --- |
| Store app | ✔️ |
| Microsoft 365 apps | N/A |
| Microsoft Edge version 77 and newer | N/A |
| Microsoft Defender for Endpoint | N/A |
| Web link | ❌ |
| Line-of-business apps | ✔️ |
| iOS/iPadOS volume purchase program (VPP) app | ✔️ |

### macOS

| App type | Supported |
| --- | --- |
| Store app | N/A |
| Microsoft 365 apps | ✔️ |
| Microsoft Edge version 77 and newer | ✔️ |
| Microsoft Defender for Endpoint | ✔️ |
| Web link | ❌ |
| Line-of-business apps | ✔️ |

### Windows 10 and newer

| App type | Supported |
| --- | --- |
| Store app | ✔️ |
| Microsoft 365 apps | ✔️ |
| Microsoft Edge version 77 and newer | ✔️ |
| Microsoft Defender for Endpoint | N/A |
| Web link | ❌ |
| Line-of-business apps | ✔️ |
| Windows app (Win32) | ✔️ |
| Microsoft Store for Business | ✔️ |

## Compliance policies

You can use filters for all compliance policies on the following platforms:

- Android device administrator
- Android Enterprise
- iOS/iPadOS
- macOS
- Windows 10 and newer

## Device configuration profiles and Endpoint security

You can use filters for some common device configuration policies on the following platforms. For a list of what's not supported, see [not supported](#not-supported) (in this article).

> [!NOTE]
> Some profile types are only available for specific platforms. For example, the **Device features** profile type includes settings that are only available for iOS/iPadOS and macOS devices. 
>
> For a list of all device configuration profiles, and the platforms they apply to, see [Apply features and settings on your devices](../configuration/device-profiles.md).

### Android device administrator

| Profile type | Supported |
| --- | --- |
| **Device configuration profile** | &nbsp; |
| Custom | ✔️ |
| Derived credential | N/A |
| Device restrictions | ✔️ |
| Device restrictions (Windows 10 Team) | N/A |
| Device features | N/A |
| Email | N/A |
| Email (Samsung KNOX only) | ✔️ |
| Endpoint Protection | N/A |
| MX profile (Zebra only) | ✔️ |
| PKCS certificate | ✔️ |
| PKCS imported certificate | ✔️ |
| SCEP certificate | ✔️ |
| Settings catalog | N/A |
| Trusted certificate | ✔️ |
| VPN | ✔️ |
| Wi-Fi | ✔️ |
| **Endpoint Security profile** | &nbsp; |
| Account protection | N/A |
| Antivirus | N/A |
| Attack surface reduction | N/A |
| Disk encryption | N/A |
| Endpoint detection and response | N/A |
| Firewall | N/A |
| Security Baselines | N/A |

### Android Enterprise

| Profile type | Supported |
| --- | --- |
| **Device configuration profile** | &nbsp; |
| Custom | ✔️ |
| Derived credential | ✔️ |
| Device restrictions | ✔️ |
| Device Restrictions (Windows 10 Team) | N/A |
| Device Features | N/A |
| Email | ✔️ |
| Endpoint Protection | N/A |
| OEMConfig | ❌ |
| PKCS certificate | ✔️ |
| PKCS imported certificate | ✔️ |
| SCEP certificate | ✔️ |
| Settings catalog | N/A |
| Trusted certificate | ✔️ |
| VPN | ✔️ |
| Wi-Fi | ✔️ |
| **Endpoint Security profile** | &nbsp; |
| Account protection | N/A |
| Antivirus | N/A |
| Attack surface reduction | N/A |
| Disk encryption | N/A |
| Endpoint detection and response | N/A |
| Firewall | N/A |
| Security Baselines | N/A |

### iOS/iPadOS

| Profile type | Supported |
| --- | --- |
| **Device configuration profile** | &nbsp; |
| Custom | ✔️ |
| Derived credential | ✔️ |
| Device restrictions | ✔️ |
| Device Restrictions (Windows 10 Team) | N/A |
| Device Features | ✔️ |
| Email | ✔️ |
| Endpoint Protection | N/A |
| PKCS certificate | ✔️ |
| PKCS imported certificate | ✔️ |
| SCEP certificate | ✔️ |
| Settings catalog | N/A |
| Trusted certificate | ✔️ |
| VPN | ✔️ |
| Wi-Fi | ✔️ |
| **Endpoint Security profile** | &nbsp; |
| Account protection | N/A |
| Antivirus | N/A |
| Attack surface reduction | N/A |
| Disk encryption | N/A |
| Endpoint detection and response | N/A |
| Firewall | N/A  |
| Security Baselines | N/A |

### macOS

| Profile type | Supported |
| --- | --- |
| **Device configuration profile** | &nbsp; |
| Custom | ✔️ |
| Derived credential | N/A |
| Device restrictions | ✔️ |
| Device restrictions (Windows 10 Team) | N/A |
| Device features | ✔️ |
| Email | N/A |
| Endpoint Protection | ✔️ |
| Extensions | ✔️ |
| PKCS certificate | ✔️ |
| PKCS imported certificate | ✔️ |
| Preference file | ✔️ |
| SCEP certificate | ✔️ |
| Settings catalog | ✔️ |
| Trusted certificate | ✔️ |
| VPN | ✔️ |
| Wi-Fi | ✔️ |
| Wired network | ✔️ |
| **Endpoint Security profile** | &nbsp; |
| Account protection | N/A |
| Antivirus | ❌ |
| Attack surface reduction | N/A |
| Disk encryption | ❌ |
| Endpoint detection and response | N/A |
| Firewall | ❌ |
| Security Baselines | N/A |

### Windows 10 and newer

| Profile type | Supported |
| --- | --- |
| Update rings for Windows 10 and later | ✔️ |
| **Device configuration profile** | &nbsp; |
| Administrative Templates | ✔️ |
| Custom | ✔️ |
| Derived credential | N/A |
| Delivery optimization | ✔️ |
| Device restrictions | ✔️ |
| Device Restrictions (Windows 10 Team) | ✔️ |
| Device Features | N/A |
| Device Firmware Configuration Interface (DFCI) on Windows 10 RS5 (1809) and newer on supported UEFI | ✔️ |
| Domain Join | ✔️ |
| Edition upgrade and S mode switch | ✔️ |
| Email | ✔️ |
| Endpoint Protection | ✔️ |
| Identity Protection | ✔️ |
| Kiosk | ✔️ |
| Microsoft Defender for Endpoint (Windows 10 Desktop) | ✔️ |
| Network boundary | ✔️ |
| PKCS certificate | ✔️ |
| PKCS imported certificate | ✔️ |
| SCEP certificate | ✔️ |
| Secure assessment (Education) | ✔️ |
| Settings catalog | ✔️ |
| Shared multi-user device | ✔️ |
| Trusted certificate | ✔️ |
| VPN | ✔️ |
| Wi-Fi | ✔️ |
| Windows health monitoring | ✔️ |
| **Endpoint Security profile** | &nbsp; |
| Account protection | ❌ |
| Antivirus | ❌ |
| Attack surface reduction | ❌ |
| Disk encryption | ❌ |
| Endpoint detection and response | ❌ |
| Firewall | ❌ |
| Security Baselines | ❌ |

## Not supported

The following features don't support using filters:

- App configuration policies for Android and iOS/iPadOS
- App protection policies for Android, iOS/iPadOS, and Windows 10
- End user experiences customization policies
- Enrollment restrictions
- iOS/iPadOS app provisioning profiles
- Partner device management
- Policies for Office apps
- Policy sets
- PowerShell scripts for Windows 10
- S mode supplemental policies for Windows 10
- Shell scripts for macOS
- Terms and conditions
- Update policies for iOS/iPadOS
- Feature updates for Windows 10 and later

## Next steps

- [Use filters when assigning your apps, policies, and profiles](filters.md)
- [Supported device properties when creating filters](filters-device-properties.md)
