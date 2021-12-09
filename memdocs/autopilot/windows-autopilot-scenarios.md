---
title: Windows Autopilot scenarios and capabilities
description: Follow along with several typical Windows Autopilot deployment scenarios, such as redeploying a device in a business-ready state.
keywords: mdm, setup, windows, windows 10, oobe, manage, deploy, autopilot, ztd, zero-touch, partner, msfb, intune, white glove, pre-provision
ms.prod: w10
ms.mktglfcycl: deploy
ms.localizationpriority: medium
ms.sitesec: library
ms.pagetype: deploy
audience: itpro
author: greg-lindsay
ms.author: greglin
ms.reviewer: jubaptis
manager: dougeby
ms.date: 12/16/2020
ms.collection: M365-modern-desktop
ms.topic: conceptual
---


# Windows Autopilot scenarios and capabilities

**Applies to**

- Windows 11
- Windows 10
- Windows Holographic, version 2004 or later

## Scenarios

Windows Autopilot supports a growing list of scenarios that organizations commonly need. These needs vary based on:
- Organization type.
- Progress moving to Windows 10/11.
- How far they've [transitioned to modern management](/windows/client-management/manage-windows-10-in-your-organization-modern-management).

The following Windows Autopilot scenarios are described in this guide:

| Scenario | More information |
| --- | --- |
| Deploy and configure devices so that an end user can set it up for themselves | [Windows Autopilot user-driven mode](user-driven.md) |
| Deploy devices to be automatically configured for shared use, as a kiosk, or as a digital signage device.| [Windows Autopilot self-deploying mode](self-deploying.md) |
| Redeploy a device in a business-ready state.| [Windows Autopilot Reset](windows-autopilot-reset.md) |
| Pre-provision a device with up-to-date applications, policies, and settings.| [Pre-provisioning](pre-provision.md) |
| Deploy Windows 10/11 on an existing Windows 7 or 8.1 device | [Windows Autopilot for existing devices](existing-devices.md) |

These scenarios are summarized in the following video.

&nbsp;

> [!video https://www.microsoft.com/videoplayer/embed/RE4Ci1b?autoplay=false]

## Windows Autopilot capabilities

### Windows Autopilot is self-updating during OOBE

For Windows 10, version 1903 and later devices, Autopilot functional and critical updates automatically download during OOBE after:
- The device is connected to a network.
- The [critical driver and Windows zero-day patch (ZDP) updates](/windows-hardware/customize/desktop/windows-updates-during-oobe) are complete.

You can't opt out of these Autopilot updates because they're required for Windows Autopilot deployment. Windows alerts the user that the device is checking for, downloading, and installing the updates.

### Cortana voiceover and speech recognition during OOBE

In Windows 10, version 1903 and later, Cortana voiceover and speech recognition during OOBE is DISABLED by default. This default applies to all Windows Pro, Education, and Enterprise editions.

You can also enable Cortana voiceover and speech recognition during OOBE by creating the following registry key. This key doesn't exist by default:

HKLM\Software\Microsoft\Windows\CurrentVersion\OOBE\EnableVoiceForAllEditions

The key value is a DWORD with **0** = disabled and **1** = enabled.

| Value | Description |
| --- | --- |
| 0 | Cortana voiceover is disabled |
| 1 | Cortana voiceover is enabled |
| No value | Device will fall back to default behavior of the edition |

To change this key value, use WCD tool to create as PPKG as documented [here](/windows/configuration/wcd/wcd-oobe#nforce).

### BitLocker encryption

With Windows Autopilot, you can configure the BitLocker encryption settings to be applied before automatic encryption is started. For more information, see [Setting the BitLocker encryption algorithm for Autopilot devices](bitlocker.md)

## Related topics

[Windows Autopilot: What's new](windows-autopilot-whats-new.md)
