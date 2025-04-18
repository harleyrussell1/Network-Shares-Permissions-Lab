# Network File Shares and Permissions Lab

## Overview

This lab demonstrates how to configure shared folders with various permission levels in a Windows Active Directory environment. Testing was done between a domain controller (DC-1) and a domain-joined client (Client-1) in Azure.

## Technologies Used

- Microsoft Azure (VM deployment)
- Windows Server (Domain Controller)
- Windows File Sharing (SMB)
- NTFS & Share Permissions
- Active Directory Security Groups
- Network Pathing (\\hostname)

---

## 1. Shared Folder Creation and Permissions

Logged into:

- **DC-1** as `mydomain.com\jane_admin`
- **Client-1** as a standard domain user (`mydomain\<someuser>`)

On **DC-1**, created four folders on the C: drive:

- `read-access`
- `write-access`
- `no-access`
- `accounting`

Applied share permissions as follows:

- `read-access`: Group = **Domain Users**, Permission = **Read**
- `write-access`: Group = **Domain Users**, Permission = **Read/Write**
- `no-access`: Group = **Domain Admins**, Permission = **Read/Write**
- Skipped permissions on `accounting` initially

---

## 2. Testing Share Access from Client-1

From **Client-1**, navigated to `\\dc-1` using File Explorer.

- **read-access**: Accessible, but no write permissions
- **write-access**: Readable and writable
- **no-access**: Access denied as expected due to permission restrictions
- **accounting**: Access denied (no permissions yet assigned)

Results aligned with the expected permission configuration.

---

## 3. Group-Based Access Control

On **DC-1**:

- Created a new **Security Group**: `ACCOUNTANTS`
- Assigned the following share permission:
  - Folder: `accounting`
  - Group: `ACCOUNTANTS`
  - Permission: **Read/Write**

Back on **Client-1**:

- Attempted to access the `accounting` share as `<someuser>` → Access denied
- Logged out

On **DC-1**:

- Added `<someuser>` to the `ACCOUNTANTS` security group

Back on **Client-1**:

- Logged in again as `<someuser>`
- Accessed `\\dc-1\accounting` → Access granted and verified read/write capability
