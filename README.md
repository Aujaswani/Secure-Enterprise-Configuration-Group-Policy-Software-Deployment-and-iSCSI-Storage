# Secure Enterprise Configuration — Group Policy, Software Deployment, and iSCSI Storage

> **An enterprise-grade Windows Server implementation** demonstrating Group Policy management, software deployment automation, user restriction policies, and centralized storage using iSCSI.  
> This project simulates real-world IT administration tasks in a domain environment.

---

## Table of Contents
1. [Overview](#overview)
2. [Objectives](#objectives)
3. [Environment Setup](#environment-setup)
4. [Active Directory Structure](#active-directory-structure)
5. [Group Policy Configuration](#group-policy-configuration)
6. [Software Deployment Policy](#software-deployment-policy)
7. [Wallpaper Restriction Policy](#wallpaper-restriction-policy)
8. [iSCSI Target Server Setup](#iscsi-target-server-setup)
9. [iSCSI Virtual Disk Creation](#iscsi-virtual-disk-creation)
10. [Client Initiator Connection](#client-initiator-connection)
11. [Disk Initialization & Formatting](#disk-initialization--formatting)
12. [Verification](#verification)
13. [Benefits](#benefits)
14. [Troubleshooting](#troubleshooting)
15. [Conclusion](#conclusion)
16. [Screenshots](#screenshots)
17. [Credits](#credits)

---

## Overview
This project implements enterprise-level Windows Server administration by configuring domain-based Group Policies, deploying software remotely, restricting user personalization, and attaching cloud-style block storage through iSCSI targets.

---

## Objectives
- Deploy software via Group Policy
- Prevent user wallpaper customization
- Enforce uniform domain policy compliance
- Centralize storage using iSCSI virtual disks
- Connect clients to shared storage
- Format and mount disks on Windows clients

---


## 🖧 Network Architecture Diagram 

```

DC[Domain Controller (AD DS)] --> GPMC[Group Policy Management]
DC --> iSCSI[iSCSI Target Server]
GPMC --> OU[Accounting OU]
OU --> Client[Client (SWIN700)]
iSCSI --> VDisks[Virtual Disks (VHDX)]
Client --> Initiator[iSCSI Initiator Login]
Client --> Wallpaper[Wallpaper GPO Applied]
Client --> Software[Software Installed via MSI]
```

---

## 🏗️  Infrastructure Layout (ASCII)
```
                   +-----------------------------+
                   |     Windows Server 2019     |
                   |   Domain Controller (AD DS) |
                   +--------------+--------------+
                                  |
                    Group Policy  |   iSCSI Virtual Disks
                                  |
           +----------------------+----------------------+
           |                                             |
   +-------v--------+                            +-------v--------+
   |  Accounting OU |                            |  Target Server |
   +-------+--------+                            +--------+-------+
           |                                                |
+----------v----------+                    +----------------v----------------+
| Domain User SWIN700 |                    | Virtual Disks: HARD1 / HARD2    |
+----------+----------+                    | Exposed as block devices        |
           |                                +-------------------------------+
           |
+----------v----------+
|   Client Workstation|
|  Wallpaper Enforced |
| Software Auto-Install|
| iSCSI Disks Mounted |
+---------------------+
```



---

Domain Example:
```
FinalNI.com
```
<img width="799" height="430" alt="image" src="https://github.com/user-attachments/assets/ec2c255b-79c4-4823-88b0-7325be9907de" />

---

## Active Directory Structure
```
FinalNI.com
 ├─ Domain Controllers
 ├─ Users
 ├─ Computers
 └─ Organizational Units (OUs)
```

Users and machines are placed inside designated OUs to receive specific GPOs.

---

## Group Policy Configuration
The following tasks were performed inside **Group Policy Management**:

- Created new domain-linked GPO
- Configured user restrictions
- Applied GPO to specific OU containers
- Forced updates using `gpupdate /force`

---

## Software Deployment Policy
1. Created a shared folder with read permissions
2. Added MSI package through:
```
User Configuration > Policies > Software Settings > Software Installation
```
3. Assigned software to users
4. New software installed automatically at login

---

## Wallpaper Restriction Policy
To prevent personalization:
```
User Configuration > Administrative Templates > Control Panel > Personalization > Prevent changing wallpaper
```

Enabled to enforce enterprise branding and compliance.

---

## iSCSI Target Server Setup
1. Installed the `File and Storage Services` role
2. Enabled `iSCSI Target Server`
3. Created virtual disks for centralized storage

---

## iSCSI Virtual Disk Creation
Virtual disks were provisioned through:
```
File and Storage Services > iSCSI > New Virtual Disk
```

Virtual disks created:
- HARD1.vhdx (2 GB)
- HARD2.vhdx (5 GB)

Access control restricted using client IP (initiator ID).

---

## Client Initiator Connection
On the client machine, configured:
```
iSCSI Initiator > Discovery > Add Portal
```

Connected to the target and confirmed **Login Succeeded**.

---

## Disk Initialization & Formatting
In **Disk Management**:
- Initialized newly discovered disks
- Created primary partitions
- Assigned drive letters (E:, F:)
- Formatted as NTFS

---

## Verification
Successful deployment confirmed by:
- Software installing automatically for domain users
- Wallpaper option greyed-out
- iSCSI disks visible and mounted
- Drives usable across reboots

---

## Benefits
- Centralized software control
- Reduced administrative overhead
- Improved security compliance
- Scalable shared storage
- Realistic enterprise workflow experience

---

## Troubleshooting
- `gpupdate /force` if policy does not apply
- Ensure SMB share has Read permissions
- Check iSCSI service is running
- Disable firewalls if discovery fails
- Refresh Disk Management for new disks

---

## Conclusion
This WIN700 project successfully demonstrates domain-based policy enforcement, automated software deployment, user restriction governance, and enterprise-grade storage provisioning through iSCSI — replicating real IT administration scenarios.

---

## Screenshots
- Group Policy creation
  <img width="869" height="555" alt="Screenshot 2025-11-03 at 15 34 13" src="https://github.com/user-attachments/assets/b98180b8-6295-48aa-9a79-2bef2e0823de" />
---
## Software deployment menu
  <img width="869" height="555" alt="Screenshot 2025-11-03 at 15 33 01" src="https://github.com/user-attachments/assets/c5b6a2f7-6053-4a53-a8f0-ef2f712d97a1" />
  
  <img width="1738" height="1110" alt="image" src="https://github.com/user-attachments/assets/8ed96d03-4061-4b41-869c-6a9a4516a677" />

<img width="1738" height="1110" alt="image" src="https://github.com/user-attachments/assets/02da272a-110f-439d-8de0-805c60ef95c7" />

---

## Personalization restriction
<img width="1682" height="1178" alt="image" src="https://github.com/user-attachments/assets/bf9ec67b-4427-4c5c-be84-f5bed8bbdf2e" />


  <img width="1388" height="1189" alt="image" src="https://github.com/user-attachments/assets/638dc532-1f16-438c-8df3-b77c8e15a6d3" />

---
 
## iSCSI disk creation wizard
<img width="1738" height="1110" alt="image" src="https://github.com/user-attachments/assets/458fe38c-2392-47c3-8d91-819ef0c222f7" />



---

## iSCSI target login success
  <img width="869" height="528" alt="Screenshot 2025-11-03 at 15 38 53" src="https://github.com/user-attachments/assets/a7ad74dc-25e4-4003-a417-6de8cd74d244" />

  <img width="869" height="528" alt="Screenshot 2025-11-03 at 15 39 06" src="https://github.com/user-attachments/assets/d64f472f-6570-4a21-a2b9-4a7b119463df" />
  <img width="869" height="555" alt="Screenshot 2025-11-03 at 15 37 42" src="https://github.com/user-attachments/assets/0d1ce77e-0243-4a15-ac7f-0bc6e52baed4" />

  <img width="869" height="555" alt="Screenshot 2025-11-03 at 15 37 58" src="https://github.com/user-attachments/assets/10415a1e-07b9-4bfc-809f-31f8932528d1" />

  <img width="869" height="528" alt="Screenshot 2025-11-03 at 15 39 47" src="https://github.com/user-attachments/assets/551f1125-e620-4433-b6ce-78c52faf8f78" />

 <img width="869" height="595" alt="Screenshot 2025-11-03 at 15 40 36" src="https://github.com/user-attachments/assets/ee7cea5a-89d2-40c6-abd5-34732db520dd" />


---


## Disk Management initialized volumes
<img width="1198" height="595" alt="Screenshot 2025-11-03 at 15 41 43" src="https://github.com/user-attachments/assets/df4edb11-be79-4ca2-87b5-e3703eea03c8" />

<img width="1229" height="706" alt="Screenshot 2025-11-03 at 15 43 06" src="https://github.com/user-attachments/assets/29802b74-5e85-4c6f-91fe-05dff409a621" />

---
## 📁 Repository Structure
```
Secure-Enterprise-Configuration-Group-Policy-Software-Deployment-and-iSCSI-Storage/
├─ screenshots/
│  ├─ gpo_wallpaper.png
│  ├─ software_deployment.png
│  ├─ iscsi_virtual_disk.png
│  ├─ initiator_success.png
│  ├─ disk_management.png
├─ README.md
└─ LICENSE
```
---

---

## 🔧 Skills Demonstrated
- Active Directory administration
- Group Policy Object (GPO) configuration
- Organizational Unit (OU) targeting
- Enterprise-level software deployment (MSI)
- Desktop environment hardening
- iSCSI Target Server provisioning
- Client iSCSI session authentication
- NTFS disk formatting and provisioning
- Network share permissions
- Troubleshooting via `gpupdate /force`

---

## 📚 What I Learned
- How Group Policy can centrally enforce user restrictions across a domain.
- The difference between Assigned vs. Published software packages.
- How UNC paths are used for enterprise deployment and resource access.
- How to back up GPOs for disaster recovery and migration.
- How iSCSI enables block-level remote storage similar to SAN environments.
- How to initialize, format, and mount remotely provisioned drives.
- How domain login triggers policy processing and software application.

---

## 🔍 Possible Enhancements
- Loopback processing for user settings on machines
- Drive mapping via GPP (Group Policy Preferences)
- Login banners for compliance
- Folder redirection for user data

---

## ✅ Proof of Successful Implementation
- Wallpaper Personalization greyed out
- MSI installed during login automatically
- iSCSI target connection status: **Login Succeeded**
- Remote volumes appear in Disk Management
- Partitions formatted and accessible (E:, F:)

---

## Credits
- Windows Server 2019
- Group Policy Management Console
- File and Storage Services
- Microsoft iSCSI Initiator
- WIN700 Course Labs
