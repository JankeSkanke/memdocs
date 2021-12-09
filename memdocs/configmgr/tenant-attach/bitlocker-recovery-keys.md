---
title: Tenant attach - BitLocker recovery keys (preview)
titleSuffix: Configuration Manager
description: View BitLocker recovery keys for tenant-attached devices from the Microsoft Endpoint Manager admin center.
ms.date: 10/25/2021
ms.topic: conceptual
ms.prod: configuration-manager
ms.technology: configmgr-core
manager: dougeby
author: aczechowski
ms.author: aaroncz
---

# Tenant attach: BitLocker recovery keys (preview)

*Applies to: Configuration Manager (current branch)*

<!--6979225-->

> [!IMPORTANT]
> This information relates to a preview feature which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.

You can get BitLocker recovery keys for a tenant-attached device from the Microsoft Endpoint Manager admin center. For example, a help desk technician who doesn't have access to Configuration Manager could use the web-based admin center to help an end user get a recovery key for their device.

## Prerequisites

- Configuration Manager site version 2107 or later

    To support devices that are joined to Azure Active Directory (Azure AD), install the [update rollup](../hotfix/2107/11121541.md) for Configuration Manager version 2107.<!-- 11285470 -->

- Apply a Configuration Manager [BitLocker management](../protect/deploy-use/bitlocker/deploy-management-agent.md) policy to the device.

## Permissions

The administrative user needs the following permissions:

- On the **Collection** object that's scoped to a collection that includes the device:

  - **Read**

  - **Read BitLocker Recovery Key**

- An [Intune role](../../intune/fundamentals/role-based-access-control.md) assigned to the user

## View recovery keys

1. In a browser, go to [https://endpoint.microsoft.com](https://endpoint.microsoft.com).

1. In the admin center, select **Devices** and then **All Devices**.

1. Select a device that's synced from Configuration Manager via [tenant attach](device-sync-actions.md).

1. Select **Recovery keys** in the device menu. You'll see the list of encrypted drives on the device.

1. To display a recovery key for a drive, select **Show recovery key**. This action reveals the recovery key, which causes the device to rotate its recovery key. Select **Yes** to continue and view the key.

1. A pane to the right displays the device information, including the BitLocker recovery key. Select the copy icon to copy the key to the clipboard. This action makes it easier to share with a user.

:::image type="content" source="media/6979225-bitlocker-recovery-key.png" alt-text="Recovery Keys pane in the Microsoft Endpoint Manager admin center.":::

## Next steps

[Deploy BitLocker management](../protect/deploy-use/bitlocker/deploy-management-agent.md)
