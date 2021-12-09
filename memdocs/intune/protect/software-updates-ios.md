---
# required metadata

title: Configure iOS/iPadOS software update policies in Microsoft Intune
description: In Microsoft Intune, create or add a configuration policy to restrict when software updates automatically install on iOS/iPadOS devices. You can choose the date and time when updates aren't installed. You can also assign this policy to groups, users, or devices, and check for any installation failures. 
keywords:
author: brenduns 
ms.author: brenduns
manager: dougeby
ms.date: 06/15/2021
ms.topic: how-to
ms.service: microsoft-intune
ms.subservice: protect
ms.localizationpriority: high

# optional metadata

#ROBOTS:
#audience:

#ms.reviewer: annovich
#ms.suite: ems
search.appverid: MET150
#ms.tgt_pltfrm:
#ms.custom:
ms.collection: 
  - M365-identity-device-management
  - highpri
---

# Add iOS/iPadOS software update policies in Intune

Software update policies apply to supervised iOS/iPadOS devices to install OS updates. [Supervised devices](../enrollment/device-enrollment-program-enroll-ios.md#what-is-supervised-mode) are devices that enrolled using either Apple Business Manager or Apple School Manager.

When configuring a policy to deploy updates, you can:

- Choose to deploy the *latest update* that's available, or choose to deploy an older update by the update version number if you don't want to deploy the latest update. If you choose to deploy an older update, you must also set a Device Configuration policy to restrict visibility of software updates.
- Specify a schedule that determines when the update installs. Schedules can be as simple as installing updates the next time that the device checks in, or creating date and time ranges during which updates can install or are blocked from installing.

This feature applies to:

- iOS 10.3 and later (supervised)
- iPadOS 13.0 and later (supervised)

By default, devices check in with Intune about every 8 hours. If an update is available through an update policy, the device downloads the update. The device then installs the update upon next check-in within your schedule configuration. Profiles don't prevent users from updating the OS manually. Users can be prevented from updating the OS manually with a Device Configuration policy to restrict visibility of software updates.

> [!NOTE]
> iOS/iPadOS software updates that you send to a [Shared iPad](../enrollment/device-enrollment-shared-ipad.md), can install only when there is no user signed in to a Shared iPad session and the device is charging. The iPad must be signed out of all user accounts and plugged into a power source for the device to update successfully. 

> [!NOTE]
> If using [Autonomous Single App Mode (ASAM)](../configuration/device-restrictions-ios.md#autonomous-single-app-mode-asam), the impact of OS updates should be considered as the resulting behaviour may be undesirable.
Consider testing to assess the impact of OS updates on the app you are running in ASAM.

## Configure the policy

1. Sign in to the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431).
2. Select **Devices** > **Update policies for iOS/iPadOS** > **Create profile**.
3. On the **Basics** tab, specify a name for this policy, specify a description (optional), and then select **Next**.

   :::image type="content" source="./media/software-updates-ios/basics-tab.png" alt-text="Example Update policy settings.":::

4. On the **Update policy settings** tab, configure the following options:

   1. **Select version to install**. You can choose from:

      - *Latest update*: Deploys the most recently released update for iOS/iPadOS.
      - Any previous version that is available in the dropdown box. If you select a previous version, you must also deploy a device configuration policy to delay visibility of software updates.

   2. **Schedule type**: Configure the schedule for this policy:

      - *Update at next check-in*: The update installs on the device the next time it checks in with Intune. This is the simplest option and has no additional configurations.
      - *Update during scheduled time*: You configure one or more windows of time during which the update will install upon check-in.
      - *Update outside of scheduled time*: You configure one or more windows of time during which the updates won't install upon check-in.

   3. **Weekly schedule**: If you choose a schedule type other than *update at next check-in*, configure the following options:

      :::image type="content" source="./media/software-updates-ios/scheduled-time.png" alt-text="Example of selecting an update during scheduled time.":::

      - **Time zone**: Choose a time zone.
      - **Time window**: Define one or more blocks of time that restrict when the updates install. The effect of the following options depends on the Schedule type you selected. By using a start day and end day, overnight blocks are supported. Options include:

        - **Start day**: Choose the day on which the schedule window starts.
        - **Start time**: Choose the time day when the schedule window begins. For example, if you select 5 AM and have a Schedule type of *Update during scheduled time*, 5 AM will be the time that updates can begin to install. If you chose a Schedule type of *Update outside of a scheduled time*, 5 AM will be the start of a period of time that updates can't install.
        - **End day**: Choose the day on which the schedule window ends.
        - **End time**: Choose the time of day when the schedule window stops. For example, if you select 1 AM and have a Schedule type of *Update during scheduled time*, 1 AM will be the time that updates can no longer install. If you chose a Schedule type of *Update outside of a scheduled time*, 1 AM will be the start of a period of time that updates can install.

       If you don't configure times to start or end, the configuration results in no restriction and updates can install at any time.  

       > [!NOTE]
       > You can configure settings in [Device Restrictions](../configuration/device-restrictions-ios.md#general) to hide an update from device users for a period of time on your supervised iOS/iPadOS devices. A restriction period can give you time to test an update before its visible to users to install. After the device restriction period expires, the update becomes visible to users. Users can then choose to install it, or your Software update policies might automatically install it soon after.
       >
       > When you use a device restriction to hide an update, review your software update policies to ensure they wont schedule the install of the update before that restriction period ends. Software update policies install updates based on their own schedule, regardless of the update being hidden or visible to the device user.

   After configuring *Update policy settings*, select **Next**.

5. On the **Scope tags** tab, select **+ Select scope tags** to open the *Select tags* pane if you want to apply them to the update policy.

   - On the **Select tags** pane, choose one or more tags, and then click **Select** to add them to the policy and return to the *Scope tags* pane.

   When ready, select **Next** to continue to *Assignments*.

6. On the **Assignments** tab, choose **+ Select groups to include** and then assign the update policy to one or more groups. Use **+ Select groups to exclude** to fine-tune the assignment. When ready, select **Next** to continue.

   The devices used by the users targeted by the policy are evaluated for update compliance. This policy also supports userless devices.

7. On the **Review + create** tab, review the settings, and then select **Create** when ready to save your iOS/iPadOS update policy. Your new policy is displayed in the list of update policies for iOS/iPadOS.

For guidance from the Intune support team, see [Delay visibility of software updates in Intune for supervised devices](https://techcommunity.microsoft.com/t5/Intune-Customer-Success/Delaying-visibility-of-software-updates-in-Intune-for-supervised/ba-p/345753).

> [!NOTE]
> Apple MDM doesn't allow you to force a device to install updates by a certain time or date. You can't use Intune software update policies to downgrade the OS version on a device.

## Edit a policy

You can edit an existing policy, including changing the restricted times:

1. Select **Devices** > **Update policies for iOS**. Select the policy you want to edit.

2. While viewing the policies **Properties**, select **Edit** for the policy page you want to modify.

   :::image type="content" source="./media/software-updates-ios/edit-policy.png" alt-text="Editing an existing policy.":::

3. After introducing a change, select **Review + save** > **Save** to save your edits, and return to the policies *Properties*.

> [!NOTE]
> If the **Start time** and **End time** are both set to 12 AM, Intune does not check for restrictions on when to install updates. This means than any configurations you have for **Select times to prevent update installations** are ignored, and updates can install at any time.

## Monitor device installation failures

<!-- 1352223 -->
In the Microsoft Endpoint Manager admin center, go to **Devices** > **Monitor** > **Installation failures for iOS devices**.

Intune displays a list of supervised iOS/iPadOS devices that are targeted by an update policy. The list doesn't include devices that are up-to-date and healthy because iOS/iPad devices only return information about installation failures.

For each device on the list, the *Installation Status* displays the error that was returned by the device. To view the list of potential installation status values, on the *Installation failures for iOS devices* page, select **Filters** and then expand the drop-down list for *Installation Status*.

## Next steps

[Monitor device profiles](../configuration/device-profile-monitor.md)
