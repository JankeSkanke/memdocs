---
# required metadata

title: Get started with cloud native Windows endpoints 
titleSuffix: Microsoft Endpoint Manager
description: Set up secure cloud native Windows endpoints that are Azure AD joined, enrolled in Intune, and then deploy at scale with Autopilot.
keywords:
author: scottbreenmsft
  
ms.author: brenduns
manager: dougeby
ms.date: 09/01/2021
ms.topic: conceptual
ms.service: mem
ms.subservice: fundamentals
ms.localizationpriority: high
ms.technology:
ms.assetid: 
# optional metadata
 
#audience:
#ms.devlang:
ms.reviewer: scbree;rogerso
ms.suite: ems
search.appverid: MET150
#ms.tgt_pltfrm:
ms.custom: intune-azure
ms.collection:
  - M365-identity-device-management
  - highpri
---

# Get started with cloud native Windows endpoints with Microsoft Endpoint Manager

Increasing demand for remote work is accelerating adoption of Zero Trust security models, enabled by cloud- powered solutions. The shifting of device management to the cloud provides a better end-user experience and simplifies IT operations, while reducing reliance on on-premises infrastructure. This guide walks you through the steps to create a cloud native Windows endpoint configuration for your organization.

> [!TIP]
> If you’re looking for a Microsoft recommended, standardized solution to build on top of, you might be interested in *Windows in cloud configuration* which can easily be configured using a [Guided Scenario](./intune/fundamentals/guided-scenarios-overview.md) in Intune. See [Windows in cloud configuration](https://www.microsoft.com/microsoft-365/windows/cloud-configuration).

The table below describes the key difference between this guide and *Windows in cloud configuration*.

---
| Solution | Objective |
| --- | --- |
| Get started with cloud native Windows endpoints (this guide) | Guides you through creating your own configuration for your environment, based on Microsoft recommended settings, and helps you start testing |
| [Windows in cloud configuration](https://www.microsoft.com/microsoft-365/windows/cloud-configuration) | A guided scenario experience that creates and applies pre-built configuration based on Microsoft best practices for frontline, remote, and other workers with more focused needs |

---

You can use this guide in combination with *Windows in cloud configuration* to further customize the pre-built experience.

## Overview

### What is a cloud native Windows endpoint?

A cloud native Windows endpoint is joined to [Azure AD](/azure/active-directory/devices/concept-azure-ad-join) (AADJ) and managed by a Mobile Device Management (MDM) solution. Unlike traditional domain join or [Hybrid Azure AD joined](/azure/active-directory/devices/concept-azure-ad-join-hybrid) endpoints, it has no dependencies on on-premises Active Directory. This document focuses on Microsoft Endpoint Manager (MEM) as the MDM solution. Cloud native endpoints can be easily deployed by using [Autopilot](./autopilot/windows-autopilot.md) to transform the pre-installed Windows operating system.

:::image type="content" source="./media/cloud-native-windows-endpoints/cloud-native-endpoint-graphic.png" alt-text="Graphic representation of a cloud native Windows endpoint.":::

### Benefits of cloud native Windows endpoints

- **Best for remote work**  
  Cloud native endpoints don't require line of sight to an on-premises domain controller, which significantly simplifies the user experience especially in remote work situations. For instance, a VPN connection isn't required for the initial device sign-in or to update the local device password after a network password change.
- **Reduced reliance on on-premises infrastructure**  
  Reducing your reliance on on-premises infrastructure will allow devices to be provisioned on any network, simplifying requirements and management.

- **Unified cross platform endpoint management**  
  You can step away from complex Group Policy management and modernize endpoint management by using a unified MDM solution that handles multiple platforms, like iOS, Android, macOS, and Windows all together in one place.

- **Preserve Single-Sign-On (SSO) to on-premises applications**  
  Cloud native endpoints don't compromise the user experience while on the corporate network. They natively provide SSO to on-premises infrastructure such as file servers, print servers, and web applications. For more information, see [SSO to on-premises resources](/azure/active-directory/devices/azuread-join-sso).

- **Personalization preservation**  
  Cloud native endpoints can easily take advantage of Microsoft 365 services to provide a synchronized and seamless experience across endpoints, including:
  - Windows wallpaper
  - Automatic sync of documents and desktop files to OneDrive
  - Settings for Office
  - Outlook signatures
  - Microsoft Edge settings

### How to get started

Use the five ordered phases in this guide, which build on each other to help you prepare your cloud native Windows endpoint configuration. By completing these phases in order, you'll see tangible progress along the way and to be ready to provision new devices at the end of this guide.

**Phases**:

:::image type="content" source="./media/cloud-native-windows-endpoints/phases.png" alt-text="Five phases for setting up cloud native Windows endpoints.":::

- Phase 1 – Set up your environment
- Phase 2 – Build your first cloud native Windows endpoint
- Phase 3 – Secure your cloud native Windows endpoint
- Phase 4 – Apply your custom settings and applications
- Phase 5 – Deploy at scale with Autopilot

At the end of this guide, you’ll have a cloud native Windows endpoint ready to start testing in your environment.
Before you get started, you might want to check out the Azure AD join planning guide at [How to plan your Azure Active Directory join implementation](/azure/active-directory/devices/azureadjoin-plan).

## Phase 1 – Set up your environment

:::image type="content" source="./media/cloud-native-windows-endpoints/phase-1.png" alt-text="Phase 1.":::

Before you build your first cloud native Windows endpoint, there are some key requirements and configuration that need to be checked. This phase will walk you through checking the requirements, configuring [Autopilot](./autopilot/windows-autopilot.md), and creating some settings and applications.

### Step 1 - Network requirements

Your cloud native Windows endpoint will need access to several internet services. Start your testing on an open network or use your corporate network after providing access to all the endpoints that are listed at [Windows Autopilot networking requirements](./autopilot/networking-requirements.md).

If your wireless network requires certificates, you can start with an Ethernet connection during testing while you determine the best approach for wireless connections for device provisioning.

### Step 2 - Enrollment and Licensing

Before you can join Azure AD and enroll in Intune, there are a few things you need to check. You could create a new Azure AD group (for example, with the name **Intune MDM Users**), and add specific test user accounts and target each of the following configurations at that group to limit who can enroll devices while you set up your configuration. To create an Azure AD group, see [Create a basic group and add members](/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).

- **Enrollment restrictions**  
Enrollment restrictions allow you to control what types of devices can enroll into management with Intune. For this guide to be successful, make sure *Windows (MDM)* enrollment is allowed, which is the default configuration.

  For information on configuring Enrollment Restrictions, see [Set enrollment restrictions in Microsoft Intune](./intune/enrollment/enrollment-restrictions-set.md).

- **Azure AD Device MDM settings**  
  When you join a Windows device to Azure AD, Azure AD can be configured to tell your devices to automatically enroll with an MDM. This configuration is required for Autopilot to work.
  
  To check your Azure AD Device MDM settings are enabled properly, see [Quickstart - Set up automatic enrollment in Intune](./intune/enrollment/quickstart-setup-auto-enrollment.md).
  
- **Azure AD company branding**  
  Adding your corporate logo and images to Azure Active Directory ensures that users see a familiar and consistent look-and-feel when they sign-in to Microsoft 365. This configuration is required for Autopilot to work.

  For information on configuring custom branding in Azure AD, see [Add branding to your organization's Azure Active Directory sign-in page](/azure/active-directory/fundamentals/customize-branding).

- **Licensing**  
  Users enrolling Windows devices from the Out Of Box Experience (OOBE) into Intune will require two key capabilities.

  Users will require the following licenses:
  - A **Microsoft Intune** or **Microsoft Intune for Education** license
  - A license like one of the following options that allows for automatic MDM enrollment:
    - **Azure AD Premium P1**
    - **Microsoft Intune for Education**

  To assign licenses, see [Assign Microsoft Intune licenses](./intune/fundamentals/licenses-assign.md).

  > [!NOTE]
  > Both types of licenses are typically included with licensing bundles like Microsoft 365 E3 (or A3) and above. View comparisons of M365 licensing [here](https://www.microsoft.com/microsoft-365/compare-microsoft-365-enterprise-plans).

### Step 3 - Import your test device

To test the Cloud Native Windows endpoint, we need to start by getting a virtual machine or physical device ready for testing. The following steps will gather the device details and upload them into the Autopilot service for use later in this document.

> [!NOTE]
> While the following steps provide a way to import a device for testing, Partners and OEMs can import devices into Autopilot on your behalf as part of purchasing. There is more information about Autopilot in [Phase 5](#phase-5--deploy-at-scale-with-autopilot).

1. Install Windows (preferably 20H2 or later) in a virtual machine or reset physical device so that it’s waiting at the OOBE setup screen. For a virtual machine, you can optionally create a checkpoint.

2. Complete the necessary steps to connect to the Internet.

3. Open a command prompt by using the keyboard combination **Shift+F10**.

4. Verify that you have internet access by pinging bing.com:
   - `ping bing.com`

5. Switch into PowerShell by running the command:
   - `powershell.exe`

6. Download the *Get-WindowsAutoPilotInfo* script by running the commands:
   - `Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process`
   - `Install-Script Get-WindowsAutoPilotInfo`

7. When prompted, enter **Y** to accept.

8. Type the command:
   - `Get-WindowsAutoPilotInfo.ps1 -GroupTag CloudNative -Online`

   > [!NOTE]
   > Group Tags allow you to create dynamic Azure AD groups based on a subset of devices. Group Tags can be set when importing devices or changed later in the Microsoft Endpoint Manager admin console. We'll use the Group Tag *CloudNative* in Step 4. You can set the tag name to something different for your testing.

9. Log into the credential prompt with your Intune Administrator account.

10. Leave the computer at the out of box experience until [Phase 2](#phase-2---build-a-cloud-native-windows-endpoint).

### Step 4 - Create Azure AD dynamic group for the device

To limit the configurations from this guide to the test devices that you import to Autopilot, create a dynamic Azure AD group that automatically includes the devices that import to Autopilot and have the Group Tag *CloudNative*. You can then target all your configurations and applications at this group.

1. Open the [Microsoft Endpoint Manager admin center](https://endpoint.microsoft.com).

2. Select **Groups**.

3. Select **New Group** and enter the following details:
   1. Group type: **Security**
   2. Group Name: **Autopilot Cloud Native Windows Endpoints**
   3. Membership type: **Dynamic Device**

4. Select **Add dynamic query**.

5. In the **Rule Syntax** section, select **Edit.**

6. Paste the following text:
   - `(device.devicePhysicalIds -any (_ -eq "[OrderID]:CloudNative"))`

7. Select **OK**, then **Save**, then **Create**.

> [!TIP]
> Dynamic groups take a few minutes to populate after changes occur. In large organizations, it [can take much longer](/azure/active-directory/enterprise-users/groups-troubleshooting#troubleshooting-dynamic-memberships-for-groups). After creating a new group, wait a few minutes before you check to confirm the device is now a member of the group.

> [!NOTE]
> For more information about dynamic groups for devices, see [Rules for devices](/azure/active-directory/enterprise-users/groups-dynamic-membership#rules-for-devices).

### Step 5 - Configure the Enrollment Status Page

The enrollment status page (ESP) is the mechanism an IT pro uses to control the end-user experience during endpoint provisioning. See [Set up the Enrollment Status Page](./intune/enrollment/windows-enrollment-status.md). To limit the scope of the enrollment status page, you can create a new profile and target the **Autopilot Cloud Native Windows Endpoints** group created in the previous step, *Create Azure AD dynamic group for the device*.

- For the purposes of testing, we recommend the following settings, but feel free to adjust them as required:
  - **Show app and profile configuration progress** - Yes
  - **Only show page to devices provisioned by out-of-box experience (OOBE)** – Yes (*default*)

### Step 6 - Create and assign Autopilot profile

Now we can create an Autopilot profile and assign it to our test device. This profile tells your device to join Azure AD and what settings to apply during OOBE.

1. Open the [Microsoft Endpoint Manager admin center](https://endpoint.microsoft.com).

2. Select **Devices** > **Enroll devices** > **Deployment profiles** (under **Windows Autopilot Deployment Program**).

3. Select **Create profile** > **Windows PC**.

4. Enter the name **Autopilot Cloud Native Windows Endpoint**, and then select **Next**.

5. Review and leave the default settings and select **Next**.

6. Leave the scope tags and select **Next**.

7. Assign the profile to the Azure AD group you created called **Autopilot Cloud Native Windows Endpoint**, select **Next**, and then select **Create**.

### Step 7 - Sync Autopilot devices

The Autopilot service will synchronize several times a day. You can also trigger a sync immediately so that your device is ready to test. To synchronize immediately:

1. Open the [Microsoft Endpoint Manager admin center](https://endpoint.microsoft.com).

2. Select **Devices** > **Enroll devices** > **Devices**.

3. Select **Sync**.

The sync takes several minutes and will continue in the background. When the sync is complete, the profile status for the imported device displays **Assigned**.

### Step 8 - Configure settings for an optimal Microsoft 365 experience

We’ve selected a few settings to configure that will demonstrate an optimal Microsoft 365 end-user experience on your Windows cloud native device. These settings are configured using a device configuration settings catalog profile. For more information, see [Create a policy using the settings catalog in Microsoft Intune](./intune/configuration/settings-catalog.md). After you’ve created the profile and added your settings, assign the profile to the **Autopilot Cloud Native Windows Endpoints** group created previously.

- **Microsoft Outlook**  
  To improve the first run experience for Microsoft Outlook, the following setting will automatically configure a profile when Outlook is opened for the first time.

  - Microsoft Outlook 2016\Account Settings\Exchange (User setting)
    - Automatically configure only the first profile based on Active Directory primary SMTP address - **Enabled**

- **Microsoft Edge**  
  To improve the first run experience for Microsoft Edge, the following settings will configure Edge to sync the user’s settings and skip the first run experience.

  - Microsoft Edge
    - Hide the first-run experience and splash screen - **Enabled**
    - Force synchronization of browser data and do not show the sync consent prompt - **Enabled**

- **Microsoft OneDrive**  
  To improve the first sign-in experience, the following settings will configure Microsoft OneDrive to automatically sign in and redirect Desktop, Pictures, and Documents to OneDrive. Files On-Demand (FOD) is also recommended, however as it's enabled by default, it's not included in the list below. For more information on the recommended configuration for the Onedrive sync app, see [here](/onedrive/ideal-state-configuration).

  - OneDrive
    - Silently sign in users to the OneDrive sync app with their Windows credentials - **Enabled**
    - Silently move Windows known folders to OneDrive – **Enabled**

    > [!NOTE]
    > For more information, see [Redirect Known Folders](/onedrive/redirect-known-folders).

The following screenshot shows an example of a settings catalog profile with each of the suggested settings configured:
:::image type="content" source="./media/cloud-native-windows-endpoints/settings-catalog-example.png" alt-text="Example of a settings catalog profile.":::

### Step 9 - Set up Microsoft Store for Business (or Education)

Before you enable synchronization in the Intune console, you must configure your store account to use Intune as a management tool.

1. Ensure that you sign into the [**Microsoft Store for Business (or Education)**](https://www.microsoft.com/business-store) using the same tenant account you use to sign into Intune.

2. In the [**Business Store**](https://www.microsoft.com/business-store), choose the **Manage** tab, select **Settings**, and choose the **Distribute** tab.

3. Activate **Microsoft Intune**
    - If you don't specifically have **Microsoft Intune** available as a mobile device management tool, choose **Add management tool** to add **Microsoft Intune**.
    - If you don't have **Microsoft Intune** activated as your mobile device management tool, click **Activate** next to **Microsoft Intune**. 
      
        > **[!NOTE]**
        > Pay careful attention to the exact names used - you should activate **Microsoft Intune** rather than **Microsoft Intune Enrollment**.

### Step 10 - Create and assign some applications

Your cloud native endpoint will need some applications. To get started, we recommend configuring the following applications and targeting them at the **Autopilot Cloud Native Windows Endpoints** group created previously.

- **Microsoft 365 Apps** (formerly Office 365 ProPlus)  
  Microsoft 365 Apps such as Word, Excel, and Outlook can easily be deployed to devices using the built-in *Microsoft 365 apps for Windows* app profile in Intune.

  - Select **configuration designer** for the settings format, as opposed to XML.
  - Select **Current Channel** for the update channel.
  - Ensure that you de-select (uncheck) the option for **OneDrive (Groove)** as this app is the legacy OneDrive. Because OneDrive is included with Windows, it isn't mandatory to install it. Remove other applications you don't want installed by unchecking them.

  To deploy Microsoft 365 Apps, see [Add Microsoft 365 apps to Windows devices using Microsoft Intune](./intune/apps/apps-add-office365.md)

- **Company Portal**  
  Deploying the Intune *Company Portal* app to all devices as a required application is recommended. Company Portal is the self-service hub for users that they use to install applications from multiple sources, like Intune, Microsoft Store, and Configuration Manager. Users also use the portal to sync their device with Intune, check compliance status, and so on.

  You can set up a relationship between Intune and the Microsoft Store for Business (MSfB) and get the Company Portal app as an offline app that can be synched to Intune and targeted at devices. For more information, see [Add and assign the Windows Company Portal app for Autopilot provisioned devices](./intune/apps/store-apps-company-portal-autopilot.md).

- **Microsoft Store App** (Whiteboard)  
  While Intune can deploy a wide variety of apps, we'll deploy a store app (Microsoft Whiteboard) to help keep things simple for this guide. Follow the steps from [Get the offline Company Portal app from the store](./intune/apps/store-apps-company-portal-autopilot.md#get-the-offline-company-portal-app-from-the-store) but select *Microsoft Whiteboard* as the app. Then, set the app to be *Online* and deploy it as *available* instead of as a required app. The app should later appear within Company Portal for manual installation by the user.

## Phase 2 - Build a cloud native Windows endpoint

:::image type="content" source="./media/cloud-native-windows-endpoints/phase-2.png" alt-text="Phase 2.":::

To build your first cloud native Windows endpoint, use the same virtual machine or physical device from which you gathered and then uploaded the hardware hash to the Autopilot service in [step 3 of Phase 1](#phase-1--set-up-your-environment). With this device, go through the Autopilot process.

1. Resume (or reset if necessary) your Windows PC to the Out of Box Experience (OOBE).
   > [!NOTE]
   > If you're prompted to choose setup for personal or an organization, then the Autopilot process hasn’t triggered. In that situation, restart the device and ensure it has internet access. If it still doesn’t work, try resetting the PC or reinstalling Windows.

2. Sign in with Azure AD credentials (*UPN* or *AzureAD\username*).
3. The enrollment status page will appear and provide the status of the device configuration.

**Congratulations!** You’ve provisioned your first cloud native Windows endpoint!

Some things to check out on your new cloud native Windows endpoint:

- Notice that OneDrive folders are redirected and when Outlook opens, it's configured automatically to connect to Office 365.
- Open **Company Portal** from the **Start Menu** and notice that **Microsoft Whiteboard** is available for installation.
- Consider testing access from the device to on-premises resources like file shares, printers, and intranet sites.

  > [!NOTE]
  > If you haven’t set up [Windows Hello for Business Hybrid](/windows/security/identity-protection/hello-for-business/hello-identity-verification#hybrid-deployments), you might be prompted on Windows Hello logons to enter passwords to access on-premises resources. To continue testing single sign on access you can configure Windows Hello for Business Hybrid or logon to the device with username and password rather than Windows Hello. To do so, select the key shaped icon on the logon screen.

## Phase 3 – Secure your cloud native Windows endpoint

:::image type="content" source="./media/cloud-native-windows-endpoints/phase-3.png" alt-text="Phase 3.":::

This phase is designed to help you build out security settings for your organization. This section draws your attention to the various Endpoint Security components in Microsoft Endpoint Manager including:

- [Microsoft Defender Antivirus (MDAV)](#microsoft-defender-antivirus-mdav)
- [Microsoft Defender Firewall](#microsoft-defender-firewall)
- [BitLocker Encryption](#bitlocker-encryption)
- [Security baselines](#security-baselines)
- [Windows Update for Business](#windows-update-for-business)

### Microsoft Defender Antivirus (MDAV)

The following settings are recommended as a minimum configuration for Microsoft Defender Antivirus, a built-in OS component of Windows. These settings don't require any specific licensing agreement such as E3 or E5, and can be enabled in the [Microsoft Endpoint Manager admin center](https://endpoint.microsoft.com) by going to **Endpoint Security** > **Antivirus** > **Create Policy** > **Windows and later** > **Profile type** = **Microsoft Defender Antivirus**.

**Cloud Protection**:

- Turn on cloud-delivered protection: **Yes**
- Cloud-delivered protection level: **Not configured**
- Defender Cloud Extended Timeout In Seconds: **50**

**Real-time protection**:

- Turn on real-time protection: **Yes**
- Enable on access protection: **Yes**
- Monitoring for incoming and outgoing files: **Monitor all files**
- Turn on behavior monitoring: **Yes**
- Turn on intrusion prevention: **Yes**
- Enable network protection: **Enable**
- Scan all downloaded file and attachments: **Yes**
- Scan scripts that are used in Microsoft browsers: **Yes**
- Scan network files: **Not Configured**
- Scan emails: **Yes**

**Remediation**:

- Number of days (0-90) to keep quarantined malware: **30**
- Submit samples consent: **Send safe samples automatically**
- Action to take on potentially unwanted apps: **Enable**
- Actions for detected threats: **Configure**
  - Low threat: **Quarantine**
  - Moderate threat: **Quarantine**
  - High threat: **Quarantine**
  - Severe threat: **Quarantine**

Settings configured in the MDAV profile within Endpoint Security:
:::image type="content" source="./media/cloud-native-windows-endpoints/defender-antivirus-policy.png" alt-text=" Example of a Microsoft Defender Antivirus profile.":::

For more information on Windows Defender configuration, including Microsoft Defender for Endpoint for customer’s licensed for E3 and E5, see:

- [Next-generation protection in Windows, Windows Server 2016, and Windows Server 2019](/windows/security/threat-protection/microsoft-defender-antivirus/microsoft-defender-antivirus-in-windows-10)
- [Evaluate Microsoft Defender Antivirus](/windows/security/threat-protection/microsoft-defender-antivirus/evaluate-microsoft-defender-antivirus)

### Microsoft Defender Firewall

Use Endpoint Security in Microsoft Endpoint Manager to configure the firewall and firewall rules. For more information, see [Firewall policy for endpoint security in Intune](./intune/protect/endpoint-security-firewall-policy.md).

> [!NOTE]
> Azure AD joined devices cannot use the *domain* network profile as they cannot leverage LDAP to detect a domain connection in the same way that domain-joined devices do. Microsoft is aware of this issue and is investigating alternatives that can help to improve this experience in future Windows releases.
>
> As a workaround, consider leveraging a PowerShell script to detect whether a device is on the corporate network, and if so, switch the current network profile to *private*. The PowerShell cmdlet to change network profiles is: [Set-NetConnectionProfile](/powershell/module/netconnection/set-netconnectionprofile).
>
> Using *private* as a replacement for *domain* allows you to separate rules based on your domain network and all other networks between the *private* and *public* profiles. However, users with administrative privileges will occasionally be prompted at run time to create new firewall rules and apply them to either public or private profiles. If these users are not aware of the use of the private network profile as a stand-in for the domain profile, unexpected firewall rule configuration may result.

### BitLocker Encryption

Use Endpoint Security in Microsoft Endpoint Manager to configure encryption with BitLocker.

- For more information about managing BitLocker, see [Encrypt Windows 10/11 devices with BitLocker in Intune](./intune/protect/encrypt-devices.md).
- Check out our blog series on BitLocker at [Enabling BitLocker with Microsoft Endpoint Manager](https://techcommunity.microsoft.com/t5/intune-customer-success/enabling-bitlocker-with-microsoft-endpoint-manager-microsoft/ba-p/2149784).

These settings can be enabled in the [Microsoft Endpoint Manager admin center](https://endpoint.microsoft.com)  by going to **Endpoint Security** > **Disk encryption** > **Create Policy** > **Windows and later** > **Profile** = **BitLocker**.

**BitLocker – Base Settings**:

- Enable full disk encryption for OS and fixed data drives: **Yes**
- Require storage cards to be encrypted (mobile only): **Not configured**
- Hide prompt about third-party encryption: **Yes**
  - Allow standard users to enable encryption during Autopilot: **Yes**
- Configure client-driven recovery password rotation: **Enable rotation on Azure AD-joined devices**

**BitLocker – Fixed Drive Settings**:

- BitLocker fixed drive policy: **Configure**
- Fixed drive recovery: **Configure**
  - Recovery key file creation: **Blocked**
  - Configure BitLocker recovery package: **Password and key**
  - Require device to back up recovery information to Azure AD: **Yes**
  - Recovery password creation: **Allowed**
  - Hide recovery options during BitLocker setup: **Not configured**
  - Enable BitLocker after recovery information to store: **Not configured**
  - Block the use of certificate-based data recovery agent (DRA): **Not configured**
  - Block write access to fixed data-drives not protected by BitLocker: **Not configured**
  - Configure encryption method for fixed data-drives: **Not configured**

**BitLocker – OS Drive Settings**:

- BitLocker system drive policy: **Configure**
  - Startup authentication required: **Yes**
  - Compatible TPM startup: **Required**
  - Compatible TPM startup PIN: **Not configured**
  - Compatible TPM startup key: **Not configured**
  - Compatible TPM startup key and PIN: **Not configured**
  - Disable BitLocker on devices where TPM is incompatible: **Not configured**
  - Enable preboot recovery message and url: **Not configured**
- System drive recovery: **Configure**
  - Recovery key file creation: **Blocked**
  - Configure BitLocker recovery package: **Password and key**
  - Require device to back up recovery information to Azure AD: **Yes**
  - Recovery password creation: **Allowed**
  - Hide recovery options during BitLocker setup: **Not configured**
  - Enable BitLocker after recovery information to store: **Not configured**
  - Block the use of certificate-based data recovery agent (DRA): **Not configured**
  - Minimum PIN length: *leave blank*
  - Configure encryption method for Operating System drives: **Not configured**

**BitLocker – Removable Drive Settings**:

- BitLocker removable drive policy: **Configure**
  - Configure encryption method for removable data-drives: **Not configured**
  - Block write access to removable data-drives not protected by BitLocker: **Not configured**
  - Block write access to devices configured in another organization: **Not configured**

### Security Baselines

You can use security baselines to apply a set of configurations that are known to increase the security of a Windows endpoint. For more information about security baselines, see [Windows MDM security baseline settings for Intune](./intune/protect/security-baseline-settings-mdm-all.md).

Baselines can be applied using the suggested settings and customized as per your requirements. Some settings within baselines might cause unexpected results or be incompatible with apps and services running on your Windows endpoints. As a result, baselines should be tested in isolation by applying only the baseline to a selective group of test endpoints without any other configuration profiles or settings.

#### Security Baselines Known Issues

The following settings in the **Windows security baseline** can cause issues with Windows Autopilot or attempting to install apps as a standard user:

- Local Policies Security Options\Administrator elevation prompt behavior (default = Prompt for consent on the secure desktop)
- Standard user elevation prompt behavior (default = Automatically deny elevation requests)

For more information, see [Windows Autopilot policy conflicts](./autopilot/policy-conflicts.md).

### Windows Update for Business

*Windows Updates for Business* is the cloud technology for controlling how and when updates are installed on devices. In Intune, Windows Update for Business can be configured using:

- [Windows update rings](./intune/protect/windows-10-update-rings.md)
- [Windows Feature Updates](./intune/protect/windows-10-feature-updates.md)

For more information, see:

- [Learn about using Windows Update for Business in Microsoft Intune](./intune/protect/windows-update-for-business-configure.md)
- [Module 4.2 - Windows Update for Business Fundamentals](https://www.youtube.com/watch?v=TXwp-jLDcg0&list=PLMuDtq95SdKsEc_BmAbvwI5l6RPQ2Y2ak&index=6&t=5s) from the Intune for Education Deployment Workshop video series

If you’d like more granular control for Windows Updates and you use Configuration Manager, consider [co-management](./configmgr/comanage/overview.md).

## Phase 4 – Apply customizations and review your on-premises configuration

:::image type="content" source="./media/cloud-native-windows-endpoints/phase-4.png" alt-text="Phase 4.":::

In this phase, you'll apply organization-specific settings, apps, and review your on-premises configuration. The phase is designed to help you build out customizations specific to your organization. We also draw your attention to the various components of Windows and how you can review existing configurations from an on-premises Active Directory Group Policy environment and apply them to cloud native endpoints. There are sections for each of the following:

- [Microsoft Edge](#microsoft-edge)
- [Start and Taskbar layout](#start-and-taskbar-layout)
- [Settings catalog](#settings-catalog)
- [Device Restrictions](#device-restrictions)
- [Delivery Optimization](#delivery-optimization)
- [Local Administrators](#local-administrators)
- [Group Policy to MDM Setting Migration](#group-policy-to-mdm-setting-migration)
- [Scripts](#scripts)
- [Mapping Network Drives and Printers](#mapping-network-drives-and-printers)
- [Applications](#applications)

### Microsoft Edge
#### Microsoft Edge Deployment
Microsoft Edge is included on devices that run:
 - Windows 11.
 - Windows 10 20H2 or later.
 - Windows 10 1803 or later, with the May 2021 or later cumulative monthly security update.

Microsoft Edge will update automatically post user logon. To trigger an update for Microsoft Edge during deployment you could run the following command:
```powershell
Start-Process -FilePath "C:\Program Files (x86)\Microsoft\EdgeUpdate\MicrosoftEdgeUpdate.exe" -argumentlist "/silent /install appguid={56EB18F8-B008-4CBD-B6D2-8C97FE7E9062}&appname=Microsoft%20Edge&needsadmin=True"
```

To deploy Microsoft Edge to previous versions of Windows, see [Add Microsoft Edge for Windows to Microsoft Intune](./intune/apps/apps-windows-edge.md).

#### Microsoft Edge Configuration
Two components of the Microsoft Edge experience, which apply when users sign in with their Microsoft 365 credentials, can be configured from the Microsoft 365 Admin Center.

- The start page logo in Microsoft Edge can be customized by configuring the *Your organization* section within the Microsoft 365 admin center. For more information, see [Customize ‎Office 365‎ for your organization](/microsoft-365/admin/setup/customize-your-organization-theme).
- The default new tab page experience in Edge includes Office 365 information and personalized news. How this page is displayed can be customized from the Microsoft 365 admin center under **Settings** > **Org settings** > **News** > **Microsoft Edge new tab page**.

You can also set other settings for Microsoft Edge using settings catalog profiles. For example, you might want to configure specific sync settings for your organization.

- **Microsoft Edge**
  - Configure the list of types that are excluded from synchronization - **passwords**

### Start and Taskbar layout

You can customize and set a standard start and taskbar layout using Intune.

- **For Windows 10**:
  - For more information about start and taskbar customization, see [Manage Windows Start and taskbar layout (Windows)](/windows/configuration/windows-10-start-layout-options-and-policies).
  - To create a start and taskbar layout, see [Customize and export Start layout (Windows)](/windows/configuration/customize-and-export-start-layout).

  After the layout has been created, it can be uploaded to Intune by configuring a [Device Restrictions](./intune/configuration/device-restrictions-configure.md) profile. The setting is under the *Start* category.

- **For Windows 11**:
  - To create and apply a Start menu layout, see [Customize the Start menu layout on Windows 11](/windows/configuration/customize-start-menu-layout-windows-11) for more information.
  - To create and apply a Taskbar layout, see [Customize the Taskbar on Windows 11](/windows/configuration/customize-taskbar-windows-11) for more information.

### Settings catalog

The settings catalog is a single location where all configurable Windows settings will be listed. This feature simplifies how you create a policy, and how you see all the available settings. For more information, see [Create a policy using the settings catalog in Microsoft Intune](./intune/configuration/settings-catalog.md).

> [!NOTE]
>
> - While the settings catalog is in preview, some settings might not be available in the catalog but are available in Templates for Intune device configuration profiles.
>
> - Many of the settings you're familiar with from group policy are already available in the settings catalog. For more information, see [The latest in Group Policy settings parity in Mobile Device Management](https://techcommunity.microsoft.com/t5/intune-customer-success/the-latest-in-group-policy-settings-parity-in-mobile-device/ba-p/2269167).
>
> - If you intend to leverage either ADMX templates or settings catalog (recommended), be sure to update your devices with the September 2021 ‘patch Tuesday’ update ([KB5005565](https://support.microsoft.com/topic/september-14-2021-kb5005565-os-builds-19041-1237-19042-1237-and-19043-1237-292cf8ed-f97b-4cd8-9883-32b71e3e6b44)) for Windows 10 versions 2004 and later. This monthly update includes [KB5005101](https://support.microsoft.com/topic/september-1-2021-kb5005101-os-builds-19041-1202-19042-1202-and-19043-1202-preview-82a50f27-a56f-4212-96ce-1554e8058dc1) which which brings [1,400+ group policy settings to MDM](https://blogs.windows.com/windows-insider/2021/08/18/releasing-windows-10-build-19043-1198-21h1-to-release-preview-channel/#:~:text=We%20enabled%20over,new%20MDM%20policies.). Failure to apply this update will result in a 'Not Applicable' message alongside the setting in the MEM portal. These policies only work on Windows Enterprise and Education versions. Windows 11 includes the necessary updates to make these policies work.

Following are some settings available in the settings catalog that might be relevant to your organization:

- **Azure Active Directory preferred tenant domain**  
  This setting configures the preferred tenant domain name to be appended to a user’s username. A preferred tenant domain allows users to sign in to Azure AD endpoints with only their username rather than their whole UPN so long as the user’s domain name matches the preferred tenant domain. For users that have different domain names, they can type their whole UPN.

  The setting can be found in:
  - Authentication
    - Preferred AAD Tenant Domain Name - Specify domain name, like contoso.onmicrosoft.com.

- **Windows Spotlight**  
  By default, several consumer features of Windows are enabled which results in selected Store apps installing and third-party suggestions on the lock screen. You can control this using the Experience section of the settings catalog.

  - Experience
    - Allow Windows Consumer Features - **Block**
    - Allow Third-Party Suggestions in Windows Spotlight (user) - **Block**

- **Microsoft Store**  
  Organizations typically want to restrict the applications that can install on endpoints. Use this setting if your organization wants to control which applications can install from the Microsoft Store. This setting will prevent users from installing applications unless they're approved.
  - Microsoft App Store
    - Require Private Store Only - **Only Private store is enabled**

- **Block Gaming**  
  Organizations might prefer that corporate endpoints cannot be used to play games. The Gaming page within the Settings app can be hidden entirely using the following setting.
  For additional information on the settings page visibility, refer to the [CSP documentation](/windows/client-management/mdm/policy-csp-settings#settings-pagevisibilitylist) and the ms-settings [URI scheme reference](/windows/uwp/launch-resume/launch-settings-app#ms-settings-uri-scheme-reference).
  - Settings
    - Page Visibility List – **hide:gaming-gamebar;gaming-gamedvr;gaming-broadcasting;gaming-gamemode;gaming-trueplay;gaming-xboxnetworking;quietmomentsgame**

- **Control Chat Icon Visbility in Taskbar**
  The visiblity of the Chat icon in the Windows 11 taskbar can be controlled using the [Policy CSP](/windows/client-management/mdm/policy-csp-Experience#experience-configurechaticonvisibilityonthetaskbar).
  
  - Experience
    - Configure Chat Icon - **Disabled**

- **Control which tenants the Teams desktop client can sign in to**
  When this policy is configured on a device, users can only sign in with accounts homed in an Azure AD tenant that is included in the "Tenant Allow List" defined in this policy. The "Tenant Allow List" is a comma seperated list of Azure AD tenant IDs. By specifing this policy and defining an Azure AD tenant you also block sign in to Teams for personal use. For more information see [How to restrict sign in on desktop devices](/microsoftteams/sign-in-teams#how-to-restrict-sign-in-on-desktop-devices).
  
  - Administrative Templates \ Microsoft Teams
    - Restrict sign in to Teams to accounts in specific tenants (User) - **Enabled**

### Device Restrictions

Windows Device restrictions templates contain many of the settings required to secure and manage a Windows endpoint using Windows Configuration Service Providers (CSPs). More of these settings will be made available in the settings catalog over time. For more information, see [Device Restrictions](./intune/configuration/device-restrictions-configure.md).

To create a profile that uses the Device restrictions template, in the [Microsoft Endpoint Manager admin center](https://endpoint.microsoft.com) go to **Devices** > **Configuration profile** > **Create  profile** > Select **Windows 10 and later** for or *Platform* and **Templates** for *Profile type* > **Device restrictions**.

- **Desktop background picture URL (Desktop only)**  
  Use this setting to set a wallpaper on Windows Enterprise or Windows Education SKUs. You’ll either host the file online or reference a file that has been copied locally. To configure this setting, on the *Configuration settings* tab in the *Device restrictions* profile, expand *Personalization* and configure **Desktop background picture URL (Desktop only)**.

- **Require users to connect to a network during device setup**  
  This setting reduces the risk that a device can skip Autopilot if the computer is reset. This setting requires devices to have a network connection during the out of box experience phase. To configure this setting, on the *Configuration settings* tab in the *Device restrictions* profile, expand *General* and configure **Require users to connect to network during device setup**.

  > [!NOTE]
  > The setting becomes effective the next time the device is wiped or reset.

### Delivery Optimization

Delivery Optimization is used to reduce bandwidth consumption by sharing the work of downloading supported packages among multiple endpoints. Delivery Optimization is a self-organizing distributed cache that enables clients to download those packages from alternate sources, like peers on the network. These peer sources supplement the traditional Internet-based servers. You can find out about all the settings available for Delivery Optimization and what types of downloads are supported at [Delivery Optimization for Windows updates](/windows/deployment/update/waas-delivery-optimization).

To apply Delivery Optimization settings, create an Intune [Delivery Optimization profile](./intune/configuration/delivery-optimization-settings.md) or a settings catalog profile.

Some settings that are commonly considered by organizations are:

- **Restrict peer selection – Subnet**. This setting restricts peer caching to computers on the same subnet.
- **Group ID**. Delivery Optimization clients can be configured to only share content with devices in the same group. Group IDs can be configured directly by sending a GUID through policy or using DHCP options in DHCP scopes.

Customers using Microsoft Endpoint Configuration Manager can deploy connected cache servers that can be used to host Delivery Optimization content. For more information, see [Microsoft Connected Cache in Configuration Manager](./configmgr/core/plan-design/hierarchy/microsoft-connected-cache.md).

### Local Administrators

If you have only one group of people that need local administrator access to all Windows Azure AD joined devices, you can add them to the [Azure AD Joined Device Local Administrator](/azure/active-directory/roles/permissions-reference#azure-ad-joined-device-local-administrator).

You might have a requirement for IT helpdesk or other support staff to have local admin rights on a select group of devices. With Windows 2004 or later, you can meet this requirement by using the following Configuration Service Providers (CSPs).

- Ideally use [Local Users and Groups CSP](/windows/client-management/mdm/policy-csp-localusersandgroups), which requires Windows 10 20H2 or later.
- If you have Windows 10 20H1 (2004) use the [Restricted Groups CSP](/windows/client-management/mdm/policy-csp-restrictedgroups) (no update action, only replace).
- Windows versions prior to Windows 10 20H1 (2004) can't use groups, only individual accounts.

For more information, see [How to manage the local administrators group on Azure AD joined devices](/azure/active-directory/devices/assign-local-admin)

### Group Policy to MDM Setting Migration

There are several options for creating your device configuration when considering a migration from Group Policy to cloud native device management:

- Start fresh and apply custom settings as required.
- Review existing Group Policies and apply required settings. You can use tools to help, like [Group Policy Analytics](./intune/configuration/group-policy-analytics.md).
- Use Group Policy Analytics to create Device Configuration profiles directly for supported settings.

The transition to a cloud native Windows endpoint represents an opportunity to review your end-user computing requirements and establish a new configuration for the future. Wherever possible, start fresh with a minimal set of policies. Try to avoid carrying forward unnecessary or legacy settings from a domain-joined environment or older operating systems, like Windows 7 or Windows XP.

To start fresh, review your current requirements and implement a minimal collection of settings to meet these requirements. Requirements might include regulatory or mandatory security settings and settings to enhance the end-user experience. The list of requirements should be driven by the business and not by IT. Every setting should be documented, understood, and should serve a purpose.

Migrating settings from existing Group Policies to MDM (Microsoft Endpoint Manager and Intune) isn't the preferred approach. When transitioning to cloud native Windows, the intention shouldn't be to lift and shift existing group policy settings. Instead, consider the target audience and what settings they require. It's time consuming and likely impractical to review each group policy setting in your environment to determine its relevance and compatibility with a modern managed device. Avoid trying to assess every group policy and individual setting. Instead, focus on assessing those common policies that cover most devices and scenarios.

Instead, identify the group policy settings that are mandatory and review those settings against available MDM settings. Any gaps would represent blockers that can prevent you from moving forward with a cloud native device if unresolved. Tools such as [Group Policy Analytics](./intune/configuration/group-policy-analytics.md) can be used to analyze group policy settings and determine if they can be migrated to MDM policies or not.

### Scripts

You can use PowerShell scripts for any settings or customizations that you need to configure outside of the in-built configuration profiles. For more information, see [Add PowerShell scripts to Windows devices in Microsoft Intune](./intune/apps/intune-management-extension.md).

### Mapping Network Drives and Printers

Cloud native scenarios have no built-in solution for mapped network drives. Instead, we recommend that users migrate to Teams, SharePoint, and OneDrive for Business. If migration isn't possible, consider the use of scripts if necessary.

For personal storage, in [Step 8 - Configure settings for an optimal Microsoft 365 experience](#step-8---configure-settings-for-an-optimal-microsoft-365-experience), we configured OneDrive Known Folder move. For more information, see [Redirect Known Folders](/onedrive/redirect-known-folders).

For document storage, users can also benefit from SharePoint integration with File Explorer and the ability to sync libraries locally, as referenced here: [Sync SharePoint and Teams files with your computer](https://support.microsoft.com/office/sync-sharepoint-and-teams-files-with-your-computer-6de9ede8-5b6e-4503-80b2-6190f3354a88).

If you use corporate Office document templates, which are typically on internal servers, consider the newer cloud-based equivalent that allows users to access the templates from anywhere.

- [Create an organization assets library](/sharepoint/organization-assets-library)

For printing solutions, consider Universal Print. For more information, see the following links:

- [What is Universal Print?](/universal-print/fundamentals/universal-print-whatis)
- [Announcing general availability of Universal Print](https://techcommunity.microsoft.com/t5/universal-print-blog/announcing-general-availability-of-universal-print/ba-p/2176759)

### Applications

Intune supports the deployment of many different Windows application types.

- Windows Installer (MSI) – [Add a Windows line-of-business app to Microsoft Intune](./intune/apps/lob-apps-windows.md)
- MSIX – [Add a Windows line-of-business app to Microsoft Intune](./intune/apps/lob-apps-windows.md)
- Win32 apps (MSI, EXE, script installers) – [Win32 app management in Microsoft Intune](./intune/apps/apps-win32-app-management.md)
- Store apps – [Add Microsoft Store apps to Microsoft Intune](./intune/apps/store-apps-windows.md)
- Web links – [Add web apps to Microsoft Intune](./intune/apps/web-app.md)

If you have applications that use MSI, EXE, or script installers, you can deploy all of these applications using *Win32 app management in Microsoft Intune*. Wrapping these installers in the Win32 format provides more flexibility and benefits that include notifications, delivery optimization, dependencies, detection rules, and support for the Enrollment Status Page in Autopilot.

> [!NOTE]
> To prevent conflicts during installation, we recommend that you stick to using the Windows line-of-business apps or Win32 apps features exclusively. If you have applications that are packaged as .msi or .exe, those can be converted to Win32 apps (.intunewin) by using the *Microsoft Win32 Content Prep Tool*, which is [available from GitHub](https://github.com/microsoft/Microsoft-Win32-Content-Prep-Tool).

## Phase 5 – Deploy at scale with Autopilot

:::image type="content" source="./media/cloud-native-windows-endpoints/phase-5.png" alt-text="Phase 5.":::

Now that you’ve configured your cloud native Windows endpoint and provisioned it with Autopilot, consider how you can import more devices. Also consider how you can work with your partner or hardware supplier to start provisioning new endpoints from the cloud. Review the following resources to determine the best approach for your organization.

- [Overview of Windows Autopilot](./autopilot/windows-autopilot.md)
- [Module 6.4 - Windows Autopilot Fundamentals - YouTube](https://www.youtube.com/watch?v=wNmLvqZ21AE)

If for some reason Autopilot isn’t the right option for you, there are other enrollment methods for Windows detailed here – [Intune enrollment methods for Windows devices](./intune/enrollment/windows-enrollment-methods.md).

## Next steps

Learn more about the following subjects:

- [Co-management for Windows devices](./configmgr/comanage/overview.md)
- [Windows Subscription Activation](/windows/deployment/windows-10-subscription-activation)
- Configure an Intune [device compliance policy](./intune/protect/compliance-policy-create-windows.md) that can allow or deny access to resources based on an Azure AD [Conditional Access policy](/azure/active-directory/conditional-access/howto-conditional-access-policy-compliant-device)
- Add [Store Apps](./intune/apps/windows-store-for-business.md)
- Add [Win32 apps](./intune/apps/apps-win32-app-management.md)
- [Use certificates for authentication in Intune](./intune/protect/certificates-configure.md)
- Deploy network profiles, including [VPN](./intune/configuration/vpn-settings-windows-10.md) and [Wi-Fi](./intune/configuration/wi-fi-settings-windows.md)
- Deploy [Multi-Factor Authentication](/azure/active-directory/authentication/concept-mfa-howitworks)
- Security baseline for [Edge](./intune/protect/security-baseline-settings-edge.md)
