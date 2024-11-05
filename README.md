# ure-Hotpatch-Preview-Management-Guide-for-Arc-Enabled-Machines
A comprehensive guide to managing hotpatch (preview) updates on Azure Arc-enabled virtual machines. Learn how to view hotpatch statuses, perform update assessments, install patches without reboots, and track update history to improve machine uptime and security.

# Hotpatch (Preview) Management for Windows 2025 Using Azure Arc

This guide provides instructions on how to view, check, and install hotpatch (preview) updates on Windows 2025 using Azure Arc.

## Table of Contents
- [Overview](#overview)
- [Viewing Hotpatch Status](#viewing-hotpatch-status)
- [Checking Hotpatch Updates](#checking-hotpatch-updates)
- [Installing Hotpatch Updates](#installing-hotpatch-updates)
- [Viewing Update History](#viewing-update-history)

## Overview
Hotpatch (preview) allows you to apply security updates to your Windows 2025 machines without requiring a reboot. This feature is available for Arc-enabled machines through the Azure Update Manager in the Azure portal.

## Viewing Hotpatch Status

To view the hotpatch (preview) status of a single machine, follow these steps:

1. Sign in to the [Azure portal](https://portal.azure.com).
2. Navigate to **Azure Update Manager**.

    ![image](https://github.com/user-attachments/assets/191199c5-5916-4148-88f5-6f695189473f)

  
3. Under **Resources**, select **Machines**, then choose the specific Arc-enabled machine.

   ![image](https://github.com/user-attachments/assets/ab144491-7d8a-49a7-a103-892513c9e16b)

4. In the **Arc-enabled machine | Updates** page, under the **Recommended updates** section, you will see the Hotpatch status for your virtual machine (VM).

### Hotpatch Statuses
| Status       | Meaning                                                                 |
|--------------|-------------------------------------------------------------------------|
| **Not enrolled** | License is available but not enrolled on this machine.                |
| **Enabled**      | License is enrolled, and the machine is enabled for receiving updates.|
| **Canceled**     | License has been canceled on the machine.                            |
| **Disabled**     | License is enrolled, but the machine is disabled for updates.         |
| **Pending**      | Interim status while enrollment is in progress.                      |


 ![image](https://github.com/user-attachments/assets/68aa57b3-6301-4d9e-8637-4ac9105dc6a9)


## Checking Hotpatch Updates

To check for the latest hotpatch updates, you can either enable **Periodic Assessment** or manually trigger an on-demand assessment:

1. **Periodic Assessment**: Automatically checks for available updates, ensuring that all patches are detected. The results can be viewed on the **Recommended updates** tab, along with the time of the last assessment.
2. **On-Demand Patch Assessment**: Trigger a manual check using the **Check for updates** option. You can view the reboot status of the updates in the assessment result under the **Reboot required** column.

## Installing Hotpatch Updates

**Hotpatch Requirements**

Hotpatching requires certain preconditions to be met, including the activation of Virtualization-based security (VBS).

1. VBS must be enabled for hotpatching to work. In the image shown above, VBS is currently disabled, and hotpatching will remain unavailable until VBS is turned on.
    
2. The VBS Status updates approximately every 5 minutes. Use the refresh option to check the latest status updates.

    
  ![image](https://github.com/user-attachments/assets/388f1340-fefd-4dcd-9a37-9db4f2c3629f)

You can verify these features are supported on your machine by running the following command in PowerShell:

```
    Get-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform
```


  ![image](https://github.com/user-attachments/assets/f294190d-14eb-4bdb-8101-60b2b1f87259)

   
 The Windows Hypervisor Platform feature is currently disabled on my system.
 
To enable the Windows Hypervisor Platform and proceed with enabling Virtualization-Based Security (VBS) for hotpatching, open PowerShell with elevated privileges and type the following command in PowerShell to enable the Hypervisor Platform:

   
```
   Enable-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform -All
```


 ![image](https://github.com/user-attachments/assets/b6ed7b64-d69f-4a29-8d25-d023dd9b83e8)

 

You can verify again if enabled by running the previous command in PowerShell:

```
    Get-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform
```


![image](https://github.com/user-attachments/assets/f610e0d0-d153-4019-8c1f-10c724d002d3)



The Hyper-V is already installed, but the Host Guardian Service is available but not installed. 

## Create and configure DeviceGuard registry settings

Let's try installing the Host Guardian Service first, as it's related to VBS and security features:

```
Install-WindowsFeature -Name HostGuardianServiceRole
```

Then let's install the Host Guardian Hyper-V Support:

```
Install-WindowsFeature -Name HostGuardian
```

After those are installed, let's proceed with the registry modifications for VBS:

```
New-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard" -Force
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard" -Name "EnableVirtualizationBasedSecurity" -Value 1 -PropertyType DWORD -Force
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard" -Name "RequirePlatformSecurityFeatures" -Value 1 -PropertyType DWORD -Force
```

![image](https://github.com/user-attachments/assets/fb890d09-26fa-4f57-9943-dca6911181cc)


Let's verify your system's virtualization capabilities now:

```
systeminfo | findstr /i "virtualization"
Get-ComputerInfo -Property "HyperV*"
```

![image](https://github.com/user-attachments/assets/11c8a9b6-4a4b-4a6c-9b53-a3bcfd82906f)



Restart your computer after making these changes.

Now, return to the Azure Portal and locate the Hotpatch option. Click on **Change** next to Hotpatch. A new window will appear—review the details, and then click on **Confirm** to activate Hotpatch.


 ![image](https://github.com/user-attachments/assets/9b6bc1a4-39b5-4b04-bcef-04317075dcfc)
 

 Please wait approximately **10 minutes** for the changes to take effect.

 ![image](https://github.com/user-attachments/assets/da009411-1bed-49ac-84ae-6d95268c6a5e)


After successful enrollment, you will see "Enabled" as shown in the image below:


![image](https://github.com/user-attachments/assets/f6022388-1899-4af1-b7ae-d2fdf0bcf5c7)


You can now install hotpatch (preview) updates by creating a **user-defined schedule** or performing a **one-time update** . There are two options:

1. Install all available update classifications or limit the installation to security updates only.
2. Specify individual hotpatch knowledge base IDs (KB IDs) if you need to install specific updates. You can enter multiple KB IDs for this process.

This process ensures that the hotpatch update, which does not require a reboot, is installed as part of your defined schedule or in a one-time update, making patch installation predictable.


![image](https://github.com/user-attachments/assets/04947051-24eb-434b-8620-81264cc83f03)


## Viewing Update History

You can view the history of hotpatch deployments on your VM:

1. Navigate to the **History** option in the Azure portal.
2. The **Update history** section shows patch installation details for the past 30 days, including reboot status for each update.

---

By following this guide, you can effectively manage hotpatch (preview) updates on your Arc-enabled Windows 2025 machines without the need for frequent reboots, ensuring better uptime and security.

