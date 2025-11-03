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

## Environment Setup
- Windows Server 2019 Domain Controller
- Client system (joined to the domain)
- Active Directory Domain Services (AD DS)
- Group Policy Management Console (GPMC)
- File & Storage Services
- iSCSI Target Server role

Domain Example:
```
FinalNI.com
```

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
- Software deployment menu
- Personalization restriction
- iSCSI disk creation wizard
- Results summary
- iSCSI target login success
- Disk Management initialized volumes

---

## Credits
- Windows Server 2019
- Group Policy Management Console
- File and Storage Services
- Microsoft iSCSI Initiator
- WIN700 Course Labs
