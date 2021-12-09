---
title: Troubleshoot CMPivot for devices uploaded to the admin center
titleSuffix: Configuration Manager
description: Troubleshooting CMPivot for Configuration Manager tenant attach
ms.date: 12/03/2020
ms.topic: troubleshooting
ms.prod: configuration-manager
ms.technology: configmgr-core
manager: dougeby
author: mestew
ms.author: mstewart
ms.localizationpriority: high
---

# Troubleshoot CMPivot (preview) for devices uploaded to the admin center
<!--6024392-->
*Applies to: Configuration Manager (current branch)*

Use the following to troubleshoot CMPivot in the Microsoft Endpoint Manager admin center:

> [!Important]
> This information relates to a preview feature which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.

## Common issues

### <a name="bkmk_intune"></a> You don’t have access to view this information
<!--7980141-->
**Error message:** You don’t have access to view this information. Make sure a proper user role is assigned from Intune.

**Possible cause:** The user account needs an [Intune role](../../intune/fundamentals/role-based-access-control.md) assigned. In some cases, this error may also occur during replication of information and it resolves without intervention after a few minutes.

### <a name="bkmk_noinfo"></a> Unable to get device information

**Error message 1:** Unable to get device information. Make sure Azure AD and AD user discovery are configured and the user is discovered by both. Verify that the user has proper permissions in Configuration Manager.

**Possible causes:** Typically, this error is caused by an issue with the admin account. Below are the most common issues with the administrative user account:

1. Use the same account to sign in to the admin center. The on-premises identity must be synchronized with and match the cloud identity.
1. Verify the account has **Read** permission for the device's **Collection** in Configuration Manager.
1. Make sure that Configuration Manager has discovered the administrative user account you're using to access the tenant attach features within Microsoft Endpoint Manager admin center. In the Configuration Manager console, go to the **Assets and Compliance** workspace. Select the **Users** node, and find your user account.

    If your account isn't listed in the **Users** node, check the configuration of the site's [Active Directory User discovery](../core/servers/deploy/configure/about-discovery-methods.md#bkmk_aboutUser).

1. Verify the discovery data. Select your user account. In the ribbon, on the **Home** tab select **Properties**. In the properties window, confirm the following discovery data:

    - **Azure Active Directory Tenant ID**: This value should be a GUID for the Azure AD tenant.
    - **Azure Active Directory User ID**: This value should be a GUID for this account in Azure AD.
    - **User Principal Name**: The format of this value is user@domain. For example, `jqpublic@contoso.com`.

    If the Azure AD properties are empty, check the configuration of the site's [Azure AD user discovery](../core/servers/deploy/configure/about-discovery-methods.md#azureaddisc).


### <a name="bkmk_rbac"></a> Not authorized to view query results

**Error message:** Not authorized to view query results. Verify that you've been given permissions for CMPivot in Configuration Manager

**Possible causes:** Verify the user account has permissions for CMPivot. For more information see [Permissions for CMPivot](cmpivot-start.md#permissions).

#### <a name="bkmk_other"></a> Other possible causes of unexpected errors

Unexpected errors are typically caused by either [service connection point](../core/servers/deploy/configure/about-the-service-connection-point.md), [administration service](../develop/adminservice/overview.md), or connectivity issues.

1. Verify the service connection point has connectivity to the cloud using the **CMGatewayNotificationWorker.log**.
1. Verify the administrative service is healthy by reviewing the SMS_REST_PROVIDER component from site component monitoring on the central site.
1. IIS must be installed on provider machine. For more information, see [Prerequisites for the administration service](../develop/adminservice/overview.md#prerequisites).

## Known issues

### Inconsistent results for some operators with Configuration Manager version 2002
<!--7784718, 7884272-->
When using CMPivot from the Microsoft Endpoint Manager admin center with Configuration Manager version 2002, you may get inconsistent results for the following operators:

- Summarize by
- Take
- Order by
- Top
- Count
- Distinct

**Resolution**: Install [KB4578123 - CMPivot queries return unexpected results in Configuration Manager current branch, version 2002](https://support.microsoft.com/help/4578123).

### <a name="bkmk_dblhop"></a> When the SMS provider is remote from the CAS, you may encounter an internal server error from the admin console

**Error message:** On-prem error code: 500 internal server error

**Scenario 1:** When running Configuration Manager version 2002 and there is a remote provider for the CAS, then you may encounter an internal server error from the admin console.

**Scenario 2:** When running Configuration Manager version 2006, you may also encounter this error if the service connection point fails to connect to the provider on the primary site and falls back to the provider for the CAS. 

**Scenario 3:** If the CAS has been upgraded to version 2006 but the primary site hasn't been upgraded yet, then the requests will be routed through the CAS provider. If the provider is remote, you may encounter an internal server error from the admin console. 

**Workaround:** Follow the instructions for the [CAS has a remote provider](../core/servers/manage/cmpivot-changes.md#cas-has-a-remote-provider) scenario in the CMPivot article to work around this "double hop" scenario.


[!INCLUDE [Known issues shared across tenant attach features](includes/known-issues-shared.md)]

## Next steps

[Troubleshoot tenant attach](troubleshoot.md)
