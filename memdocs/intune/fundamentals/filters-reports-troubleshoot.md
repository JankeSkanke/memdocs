---
# required metadata

title: Filter reports and troubleshooting in Microsoft Intune
description: Use the device and app filter reports to get more information on successfully applied filters. Learn the impact of include and exclude filters in Microsoft Intune and Endpoint Manager.
keywords:
author: MandiOhlinger
ms.author: mandia
manager: dougeby
ms.date: 10/11/2021
ms.topic: conceptual
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

# Filter reports and troubleshooting in Microsoft Endpoint Manager

When you create an app, compliance policy, or configuration profile, you assign the policy to groups (users or devices). When you assign the app or policy, you can also use filters. For example, you can assign policies to Windows 10 devices running a specific OS version. For more information, see [Use filters when assigning your apps, policies, and profiles](filters.md).

Devices are evaluated against these filters to meet the rules you configure. The results of the filter evaluations are logged, and reported in the Microsoft Endpoint Manager admin center.

Use this article to learn more about the reporting features, and to help troubleshoot filters and conflicts.

> [!IMPORTANT]
> 
> - From evaluation time, the filter evaluation results can take up to 30 minutes to show in Endpoint Manager admin center.
> - This feature is in [public preview](public-preview.md). The entry point to filter evaluation results will change, and become more integrated into policy and app reports.

## Reports

The Endpoint Manager admin center has per-device and per-app reporting information. Use this information to help troubleshoot filter evaluation, and determine why a policy applied or didn’t apply.

You can use the following reports to get more information on your filters:

- [Filter evaluation report for devices](#filter-evaluation-report-for-devices) (in this article)
- [App filter evaluation report](#app-filter-evaluation-report) (in this article)

### Filter evaluation report for devices

This report shows every app or policy with a filter that's been applied. For each evaluated app or policy, you can see the applied filters, and get more detailed information.

1. Sign in to the [Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431).
2. Select **Devices** > **All Devices** > select a device > **Filter evaluation (preview)**. The following information is shown:

    - The filters that were evaluated.
    - The date and time the evaluation occurred.
    - The evaluation results: **Match** or **No match**
    - If the filter was using Include or Exclude mode
    - The filter name, description, and rules
    - The properties that were evaluated, such as `deviceName`.  
    - The available apps that can be assigned to the device.  

    The **Filter information** section is populated with the currently configured filter name, description, and rules. The information isn't populated from log data. The filter name, syntax, and any other metadata may have changed since the last evaluation time. When troubleshooting, be sure to look at the **Evaluation time** and **Last modified** timestamps.

In the following example, you can see this information for the **TestDevice**:

:::image type="content" source="./media/filters-reports-troubleshoot/filter-properties-single-device.png" alt-text="See the date, time, evaluation results, and other device filter assignment properties in Microsoft Endpoint Manager and Microsoft Intune.":::

### App filter evaluation report

This report shows filter information for each device that was evaluated in an app assignment. For each device, you can see the device's overall applicability for an app, and get more detailed information about the filter evaluation.

1. Sign in to the [Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431).
2. Select **Apps** > **All apps** > select an app > **Device install status**.
3. Select the **Filter** column > **Filters evaluated**. The following information is shown:

    - The filters that were evaluated.
    - The date and time the evaluation occurred.
    - The evaluation results: **Match** or **No match**, and **App assignment applied** or **App assignment not applied**
    - If the filter was using Include or Exclude mode
    - The filter name, description, and rules
    - The properties that were evaluated, such as `deviceCategory`.

In the following example, you can see this information for the **Microsoft Word** store app:

:::image type="content" source="./media/filters-reports-troubleshoot/filter-properties-single-app.png" alt-text="See the date, time, evaluation results, and other app filter properties in Microsoft Endpoint Manager and Microsoft Intune.":::

> [!IMPORTANT]
> In the **Device install status** report, apps deployed as "Available" aren't shown. To troubleshoot if a user/device is filtered in or out of an Available assignment, use the **Filter evaluation report for devices**. To generate filter evaluation results, the end user must go to the list of apps in the Company portal app or website.

## Include vs. Exclude

When you create a filter, you choose to include or exclude devices based on some properties, such as `device.model -equals “Surface pro”`, or `device.model -notEquals “Surface pro”`. It can be difficult to understand the evaluation results, especially when including or excluding devices.

Use the following table to help understand when you include or exclude devices:

| Filter mode | Filter result | Overall result |
| --- | --- | --- |
| Include | Match | Apply policy or app assignment |
| Include | Not Match | Don’t apply policy or app assignment |
| Exclude | Match | Don’t apply policy or app assignment |
| Exclude | Not Match | Apply policy or app assignment |

### What you need to know

- A **Not evaluated** filter result may show when a policy has a conflicting assignment on the device. For more information, see [Filters and assignment conflict resolution](#filters-and-assignment-conflict-resolution) (in this article).
- Filters are evaluated at enrollment and device check-in. The evaluation can also run at other times, such as a compliance check.
- The latest filter evaluation results are stored for 30 days. If the logs are expired, you may see a **We were not able to retrieve any filter evaluation results** message.

## Filters and assignment conflict resolution

When you assign a policy to a group (users or devices), it's possible to overlap assignments. It's not recommended, as the overlapping can cause conflicts.

Intune helps avoid conflicts. It prevents you from creating multiple assignments to the same Azure AD group. It's not recommended to assign apps or policies to the same target user or device with more than one intent. For example, when you deploy an app, you can't select a group for an **Available** assignment, and then the same group for a **Required** assignment.

An overlap can occur when a user or device is in multiple targeted groups. Conflicting assignments aren't recommended. For more information, see [conflicts between app intents](../apps/apps-deploy.md#how-conflicts-between-app-intents-are-resolved).

:::image type="content" source="./media/filters-reports-troubleshoot/device-multiple-groups.png" alt-text="Conflicts can occur when a device is in multiple groups in Microsoft Endpoint Manager and Microsoft Intune.":::

When you use filters, conflict resolution is handled using the following methods:

- [Filter mode](#filter-mode) (in this article)
- [Use "OR" logic when filter modes are the same](#use-or-logic-when-filter-modes-are-the-same) (in this article)
- [App intent](#app-intent) (in this article)

### Filter mode

When there's a device with conflicting assignments for the same policy, the follow precedence applies:

1. **Exclude** mode applies. **Exclude** wins over **No filter**, and wins over **Include** mode.
2. **No filter** mode applies. **No filter** wins over **Include** mode.
3. **Include** mode applies.

When you assign the app or policy, you choose to apply a filter:

:::image type="content" source="./media/filters-reports-troubleshoot/assignment-filter-precedence.png" alt-text="Filter precedence is exclude, no filter, and then include when assigning policies in Microsoft Endpoint Manager and Microsoft Intune.":::

For example:

- PolicyA is assigned to three device groups: GroupA, GroupB, and GroupC.
- The GroupA assignment uses FilterA. FilterA uses Include mode.
- The GroupB assignment uses FilterB. FilterB uses Exclude mode.
- The GroupC assignment isn't using any filters.
- DeviceA is a member of all three groups: GroupA, GroupB, and GroupC.

In this scenario, the exclude assignment wins because of filter mode precedence. DeviceA evaluates FilterB. If DeviceA matches the rules, then DeviceA is excluded from PolicyA. If DeviceA doesn't match FilterB, then PolicyA applies. DeviceA doesn't do any more evaluations against GroupB and GroupC assignments and filters.

### Use "OR" logic when filter modes are the same

If multiple filters using the same mode are applied, such as Include, then **OR** logic is used. The device only needs to match the rules in one of the filters to be included (or excluded) from the policy assignment.

For example:

- PolicyA is assigned to two groups: GroupA and GroupB.
- The GroupA assignment uses FilterA. FilterA uses Include mode.
- The GroupB assignment uses FilterB. FilterB uses Include mode.
- DeviceA is a member of both groups: GroupA and GroupB.

In this scenario, both filters use the same mode. So, the filters resolve the conflict with **OR** logic. DeviceA evaluates FilterA and FilterB. If DeviceA matches the rules in either filter, then DeviceA receives PolicyA. If DeviceA doesn't match either filter, then PolicyA isn't applied.

### App intent

Apps use conflict resolution based on "intent". The intent is evaluated before filters are evaluated. For example, apps evaluate if a device is targeted with the **Available**, **Required**, or **Uninstall** assignment intent. The winning intent is then passed to the filtering engine to determine applicability.

For example:

- AppA is assigned to two groups: GroupA and GroupB.
- The GroupA assignment uses the **Required** intent. The GroupA assignment uses FilterA, which uses Include mode.
- The GroupB assignment uses the **Uninstall** intent. The GroupB assignment uses FilterB, which uses Include mode.
- DeviceA is a member of both groups: GroupA and GroupB.

In this scenario, the winning app intent is **Required**. For more information, see [conflicts between app intents](../apps/apps-deploy.md#how-conflicts-between-app-intents-are-resolved). So, DeviceA must only evaluate FilterA. If DeviceA matches the rules in FilterA, DeviceA receives AppA as a required app.

Apps use special behavior when resolving conflicts between **Required** and **Available** assignments. If a user or device is targeted with both **Available** and **Required** assignments, then it receives a merged intent called **Required and Available**. The device must evaluate filters used in both assignments. When evaluating both filters, the device implements the same conflict resolution: [Filter mode](#filter-mode) and ["OR" logic when filter modes are the same](#use-or-logic-when-filter-modes-are-the-same).

## Conflict resolution matrix

In the following example, there's a conflict between assignments because the same user/device is in both assignments:

:::image type="content" source="./media/filters-reports-troubleshoot/example-conflict-same-group-user-policy-assignment.png" alt-text="Example assignment conflict when using filters in Microsoft Endpoint Manager and Microsoft Intune.":::

The following matrix explains the impact, depending on the conflict scenario:

:::image type="content" source="./media/filters-reports-troubleshoot/conflict-matrix.png" alt-text="Conflict impact depend on the setting configured when using filters in Microsoft Endpoint Manager and Microsoft Intune.":::

## Next steps

- [Use filters when assigning your apps, policies, and profiles](filters.md)
- [Supported device properties when creating filters](filters-device-properties.md)
- [Supported workloads when creating filters](filters-supported-workloads.md)
