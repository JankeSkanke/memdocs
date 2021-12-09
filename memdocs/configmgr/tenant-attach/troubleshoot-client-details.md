---
title: Troubleshoot client details
titleSuffix: Configuration Manager
description: Troubleshoot client details for Configuration Manager tenant attach
ms.date: 09/13/2021
ms.topic: troubleshooting
ms.prod: configuration-manager
ms.technology: configmgr-core
manager: dougeby
author: mestew
ms.author: mstewart
ms.localizationpriority: high
---

# Troubleshoot ConfigMgr client details in the admin center (preview)
<!--6374854, 6521921-->
*Applies to: Configuration Manager (current branch)*

When viewing the ConfigMgr client details, you may run across a common error. Use the following information of common error messages to troubleshoot ConfigMgr client details in the Microsoft Endpoint Manager admin center:

> [!Important]
> This information relates to a preview feature which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.

## <a name="bkmk_intune"></a> You don’t have access to view this information
<!--7980141-->
**Error message:** You don’t have access to view this information. Make sure a proper user role is assigned from Intune.

**Possible cause:** The user account needs an [Intune role](../../intune/fundamentals/role-based-access-control.md) assigned. In some cases, this error may also occur during replication of information and it resolves without intervention after a few minutes.

## <a name="bkmk_noinfo"></a> Unable to get device information

### Error message 1: Device can't be found or you don't have permission to access the device

**Possible causes**:

- Verify that Configuration Manager's [role based access control](../core/understand/fundamentals-of-role-based-administration.md) for the admin user has the device in scope.
- The machine account of [SMS Provider role](../core/plan-design/hierarchy/plan-for-the-sms-provider.md) of the primary site (or standalone site) isn't a member of either the **Pre-Windows 2000 Compatible Access** or **Windows Authorization Access** (WAA) groups in on-premises Active Directory. For more information, see [Some applications and APIs require access to authorization information on account objects](/troubleshoot/windows-server/identity/apps-apis-require-access).

### Error message 2: Unable to get client details (or collection) information. Make sure Azure AD and AD user discovery are configured and the user is discovered by both. Verify that the user has proper permissions in Configuration Manager

**Possible causes for Configuration Manager versions 2103 and later:**

Typically, this error is caused by an issue with the admin account. Below are the most common issues with the administrative user account:

1. Use the same account to sign in to the admin center. The on-premises identity must be synchronized with and match the cloud identity.
1. Make sure that Configuration Manager has discovered the administrative user account you're using to access the tenant attach features within Microsoft Endpoint Manager admin center. In the Configuration Manager console, go to the **Assets and Compliance** workspace. Select the **Users** node, and find your user account.

    If your account isn't listed in the **Users** node, check the configuration of the site's [Active Directory User discovery](../core/servers/deploy/configure/about-discovery-methods.md#bkmk_aboutUser).

**Possible causes for Configuration Manager versions 2010 and earlier:**

Typically, this error is caused by an issue with the admin account. Below are the most common issues with the administrative user account:

1. Use the same account to sign in to the admin center. The on-premises identity must be synchronized with and match the cloud identity.
1. Make sure that Configuration Manager has discovered the administrative user account you're using to access the tenant attach features within Microsoft Endpoint Manager admin center. In the Configuration Manager console, go to the **Assets and Compliance** workspace. Select the **Users** node, and find your user account.

    If your account isn't listed in the **Users** node, check the configuration of the site's [Active Directory User discovery](../core/servers/deploy/configure/about-discovery-methods.md#bkmk_aboutUser).

1. Verify the discovery data. Select your user account. In the ribbon, on the **Home** tab select **Properties**. In the properties window, confirm the following discovery data:

    - **Azure Active Directory Tenant ID**: This value should be a GUID for the Azure AD tenant.
    - **Azure Active Directory User ID**: This value should be a GUID for this account in Azure AD.
    - **User Principal Name**: The format of this value is user@domain. For example, `jqpublic@contoso.com`.

    If the Azure AD properties are empty, check the configuration of the site's [Azure AD user discovery](../core/servers/deploy/configure/about-discovery-methods.md#azureaddisc).

## <a name="bkmk_timeout"></a> Error loading your content

**Error message:** Getting results timed out. Make sure the Configuration Manager service connection point is operational and has a connection to the cloud. <!-- 8974697 -->

**Possible causes**:

- Make sure the hierarchy is still tenant-attached and connected. For more information, see the **CMGatewayNotificationWorker.log** file.
- If the service connection point or site server were recently rebooted, this error occurs temporarily.
- A site upgrade or a transient network error can cause this message to occur temporarily.
- For Configuration Manager versions 2103 and earlier, it's possible that the cache has expired and the SQL connection is stale. Restart **SMS_Executive** service on the machine running the [service connection point (SCP)](../core/servers/deploy/configure/about-the-service-connection-point.md) role if you see errors similar to the following in the SCP's **CMGatewayNotificationWorker.log**: <!--10380979-->
      
    ```text
    [Critical][CMGatewayNotificationWorker][0][System.InvalidOperationException][0x80131509]
    ExecuteReader requires an open and available Connection. The connection's current state is closed.
    ```

## <a name="bkmk_firewall"></a> Error validating request

**Error message:** Error validating request. Verify that the Configuration Manager service connection point can reach the internet endpoints required for tenant attach.

**Possible causes:** Typically this error is seen when URLs that are needed by tenant attach are blocked. If the service connection point can't access the needed internet endpoints, a validation error will occur. For more information, see [Internet endpoints](device-sync-actions.md#internet-endpoints).

## <a name="bkmk_1603"></a> Unexpected error occurred

**Error message:** Unexpected error occurred

**Possible causes:** Unexpected errors are typically caused by either [service connection point](../core/servers/deploy/configure/about-the-service-connection-point.md), [administration service](../develop/adminservice/overview.md), or connectivity issues.

1. Verify the service connection point has connectivity to the cloud using the **CMGatewayNotificationWorker.log**.
1. Verify the administrative service is healthy by reviewing the SMS_REST_PROVIDER component from site component monitoring on both the central site and primary site that owns the device.
1. IIS must be installed on provider machine. For more information, see [Prerequisites for the administration service](../develop/adminservice/overview.md#prerequisites).
1. For Configuration Manager version 2002, verify the clock on the service connection point is in sync. If the service connection point's clock is slightly behind, apply [KB4563473 - Update rollup for Configuration Manager version 2002 tenant attach issues](https://support.microsoft.com/help/4563473). Check **AdminService.log** on the provider machine for any errors.
1. For Configuration Manager version 2002, verify the device is in the security scope for the administrator's security role. For more information, see [Fundamentals of role-based administration](../core/understand/fundamentals-of-role-based-administration.md).

## Known issues

### Boundary groups list is empty

**Error message**: No boundary groups found or the user may not have permissions to view boundary group information.

The empty list is a known issue for Configuration Manager version 2002 when you have a hierarchy of Configuration Manager sites.

:::image type="content" source="media/6024387-known-issue-device-details.png" alt-text="Boundary group list is empty" lightbox="media/6024387-known-issue-device-details.png":::

[!INCLUDE [Known issues shared across tenant attach features](includes/known-issues-shared.md)]

## Next steps

[Troubleshoot tenant attach](troubleshoot.md)
