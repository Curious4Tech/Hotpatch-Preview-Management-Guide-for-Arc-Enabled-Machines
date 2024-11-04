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

You can install hotpatch (preview) updates by creating a user-defined schedule or performing a one-time update. There are two options:

1. Install all available update classifications or limit the installation to security updates only.
2. Specify individual hotpatch knowledge base IDs (KB IDs) if you need to install specific updates. You can enter multiple KB IDs for this process.

This process ensures that the hotpatch update, which does not require a reboot, is installed as part of your defined schedule or in a one-time update, making patch installation predictable.

## Viewing Update History

You can view the history of hotpatch deployments on your VM:

1. Navigate to the **History** option in the Azure portal.
2. The **Update history** section shows patch installation details for the past 30 days, including reboot status for each update.

---

By following this guide, you can effectively manage hotpatch (preview) updates on your Arc-enabled Windows 2025 machines without the need for frequent reboots, ensuring better uptime and security.

