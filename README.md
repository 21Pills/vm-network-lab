# vm-network-lab
Testing my abilities on setting up DHCP, DNS AD, and FTS. 
# 🖥️ Home Lab – Windows Server VM Network

A hands-on home lab project where I built a virtualized network using two VMs communicating with each other. The Windows Server VM acts as the backbone of the network, providing **DHCP**, **DNS**, **Active Directory (AD)**, and **File Sharing Services (FSS)**.

---

## 📋 Table of Contents

- [Lab Overview](#lab-overview)
- [Environment Setup](#environment-setup)
- [VM Configuration](#vm-configuration)
- [Services Configured](#services-configured)
  - [DHCP](#1-dhcp--dynamic-host-configuration-protocol)
  - [DNS](#2-dns--domain-name-system)
  - [Active Directory](#3-active-directory-ad)
  - [File Sharing Services](#4-file-sharing-services-fss)
- [VM Communication Test](#vm-communication-test)
- [Screenshots](#screenshots)
- [What I Learned](#what-i-learned)

---

## Lab Overview

| Component | Details |
|-----------|---------|
| Hypervisor | VMware  Workstation|
| VM 1 | Windows Server 2025 — Server / Domain Controller |
| VM 2 | Windows 11 Buisness — Client |
| VM 3 | Windows 11 Pro — Client |
| Network Mode | Host only |
| Domain Name | 21Pills.com |

---

## Environment Setup

> Steps taken to install and configure the virtualization environment before setting up the VMs.

1. Downloaded and installed **[VMware Workstation]**
2. Created a virtual network / adapter for the VMs to communicate
3. Downloaded the **Windows Server ISO** from Microsoft Evaluation Center
4. Downloaded the **Client OS ISO**

<img width="1915" height="1024" alt="image" src="https://github.com/user-attachments/assets/f4010a6d-6ef2-49a5-93c8-df80d2e836f9" />


---

## VM Configuration


### Windows Server VM
- **RAM:** *(6 GB)*
- **CPU:** *(4 cores)*
- **Storage:** *(60 GB)*
- **Network Adapter:** *(Internal Network)*
- **Static IP assigned:** *(e.g., 192.168.1.1)*

### Client VM
- **RAM:** *(e.g., 2 GB)*
- **CPU:** *(e.g., 2 cores)*
- **Storage:** *(e.g., 30 GB)*
- **Network Adapter:** *(e.g., Internal Network — "intnet")*
- **IP:** 192.168.1.2

<img width="748" height="720" alt="image" src="https://github.com/user-attachments/assets/e64a808c-5382-4212-8115-4f6ed9532734" />

---

## Services Configured

### 1. DHCP – Dynamic Host Configuration Protocol

> Allows the server to automatically assign IP addresses to devices on the network.

**Steps taken:**
1. Opened **Server Manager → Add Roles and Features**
2. Selected **DHCP Server** role
3. Configured a new DHCP scope:
   - **Scope Name:** *(Persona)*
   - **IP Range:** *(192.168.1.1 – 192.168.1.254)*
   - **Subnet Mask:** *(255.255.255.0)*
   - **Default Gateway:** *(8.8.8.8)* 
   - **Lease Duration:** *(8 days)*
4. Authorized the DHCP server in Active Directory
5. Activated the scope

📸 *Screenshot: DHCP scope configuration 
<img width="800" height="325" alt="image" src="https://github.com/user-attachments/assets/9db74944-2393-44f4-b49d-f77869705ab5" />

📸 *Screenshot: Client receiving IP address from server*

---

### 2. DNS – Domain Name System

> Resolves hostnames to IP addresses within the network.

**Steps taken:**
1. Installed the **DNS Server** role via Server Manager
2. Created a **Forward Lookup Zone** for the domain 
3. Added an **A record** for the server
4. Verified DNS resolution from the client VM using `nslookup`

📸 *Screenshot: DNS Manager showing forward lookup zone*
<img width="1707" height="912" alt="image" src="https://github.com/user-attachments/assets/5a913b68-52db-4ead-b4d4-9ad03e6fb24f" />

📸 *Screenshot: nslookup test result on client*
 <img width="824" height="504" alt="image" src="https://github.com/user-attachments/assets/6b7e6c45-3a9c-4e4b-9cfb-3d8a29a8ed0c" />

---

### 3. Active Directory (AD)

> Centralizes user authentication and manages access to network resources.

**Steps taken:**
1. Installed **Active Directory Domain Services (AD DS)** role
2. Promoted the server to a **Domain Controller**
   - Created a new forest: *(e.g., homelab.local)*
   - Set the **DSRM password**
3. Server rebooted into the domain
4. Created **Organizational Units (OUs):**
   - `OU=Users`
   - `OU=Computers`
5. Created a test domain user account *(e.g., jdoe@homelab.local)*
6. Joined the **Client VM** to the domain
7. Logged into the client using the domain user account

📸 *Screenshot: Active Directory Users and Computers panel*
📸 *Screenshot: Client successfully joined to domain*
📸 *Screenshot: Domain user login on client VM*

---

### 4. File Sharing Services (FSS)

> Allows files to be shared across the network and accessed from the client VM.

**Steps taken:**
1. Installed the **File and Storage Services** role
2. Created a shared folder *(e.g., `C:\SharedFiles`)*
3. Configured **share permissions:**
   - Domain Users — Read
   - Administrators — Full Control
4. Configured **NTFS permissions** on the folder
5. Accessed the shared folder from the client VM via `\\[ServerName]\SharedFiles`

📸 *Screenshot: Share permissions configuration*
📸 *Screenshot: Client accessing the shared folder*

---

## VM Communication Test

Verified that both VMs can communicate with each other.

**Tests performed:**

| Test | Command | Result |
|------|---------|--------|
| Ping server from client | `ping 192.168.1.1` | ✅ Success |
| Ping client from server | `ping [client IP]` | ✅ Success |
| DNS resolution | `nslookup homelab.local` | ✅ Resolved |
| Domain join | System Properties → Domain | ✅ Joined |
| File share access | `\\server\SharedFiles` | ✅ Accessible |

📸 *Screenshot: Ping test results*

---

## Screenshots

| # | Description | File |
|---|-------------|------|
| 1 | Hypervisor main screen | `screenshots/01-hypervisor.png` |
| 2 | VM network settings | `screenshots/02-vm-network.png` |
| 3 | DHCP scope setup | `screenshots/03-dhcp-scope.png` |
| 4 | Client IP from DHCP | `screenshots/04-dhcp-client.png` |
| 5 | DNS forward lookup zone | `screenshots/05-dns-zone.png` |
| 6 | nslookup test | `screenshots/06-dns-nslookup.png` |
| 7 | AD Users and Computers | `screenshots/07-ad-users.png` |
| 8 | Client joined to domain | `screenshots/08-domain-join.png` |
| 9 | Domain user login | `screenshots/09-domain-login.png` |
| 10 | File share permissions | `screenshots/10-fss-permissions.png` |
| 11 | Client accessing file share | `screenshots/11-fss-access.png` |
| 12 | Ping test | `screenshots/12-ping-test.png` |

---

## What I Learned

- How to configure a **Windows Server** as a Domain Controller
- How **DHCP** dynamically assigns IP addresses within a local network
- How **DNS** resolves names inside a private domain
- How **Active Directory** manages users, computers, and domain authentication
- How to set up and secure **file shares** with NTFS and share-level permissions
- How to connect client machines to a Windows domain and authenticate with domain credentials
- Networking fundamentals: subnets, IP ranges, gateways, and internal VM networking

---

*Project completed as part of my IT/Networking home lab practice.*# 🖥️ Home Lab – Windows Server VM Network

A hands-on home lab project where I built a virtualized network using two VMs communicating with each other. The Windows Server VM acts as the backbone of the network, providing **DHCP**, **DNS**, **Active Directory (AD)**, and **File Sharing Services (FSS)**.

---

## 📋 Table of Contents

- [Lab Overview](#lab-overview)
- [Environment Setup](#environment-setup)
- [VM Configuration](#vm-configuration)
- [Services Configured](#services-configured)
  - [DHCP](#1-dhcp--dynamic-host-configuration-protocol)
  - [DNS](#2-dns--domain-name-system)
  - [Active Directory](#3-active-directory-ad)
  - [File Sharing Services](#4-file-sharing-services-fss)
- [VM Communication Test](#vm-communication-test)
- [Screenshots](#screenshots)
- [What I Learned](#what-i-learned)

---

## Lab Overview

| Component | Details |
|-----------|---------|
| Hypervisor | *(e.g., VirtualBox / VMware Workstation / Hyper-V)* |
| VM 1 | Windows Server *(version)* — Server / Domain Controller |
| VM 2 | *(e.g., Windows 10 / Windows 11 / Ubuntu)* — Client |
| Network Mode | *(e.g., Internal Network / NAT Network / Host-Only)* |
| Domain Name | *(e.g., homelab.local)* |

---

## Environment Setup

> Steps taken to install and configure the virtualization environment before setting up the VMs.

1. Downloaded and installed **[Hypervisor Name]**
2. Created a virtual network / adapter for the VMs to communicate
3. Downloaded the **Windows Server ISO** from Microsoft Evaluation Center
4. Downloaded the **Client OS ISO**

*(Add screenshot here — hypervisor main screen)*

---

## VM Configuration

### Windows Server VM
- **RAM:** *(e.g., 2 GB)*
- **CPU:** *(e.g., 2 cores)*
- **Storage:** *(e.g., 50 GB)*
- **Network Adapter:** *(e.g., Internal Network — "intnet")*
- **Static IP assigned:** *(e.g., 192.168.1.1)*

### Client VM
- **RAM:** *(e.g., 2 GB)*
- **CPU:** *(e.g., 2 cores)*
- **Storage:** *(e.g., 30 GB)*
- **Network Adapter:** *(e.g., Internal Network — "intnet")*
- **IP:** Assigned via DHCP from the Server

*(Add screenshot here — VM settings panel)*

---

## Services Configured

### 1. DHCP – Dynamic Host Configuration Protocol

> Allows the server to automatically assign IP addresses to devices on the network.

**Steps taken:**
1. Opened **Server Manager → Add Roles and Features**
2. Selected **DHCP Server** role
3. Configured a new DHCP scope:
   - **Scope Name:** *(e.g., HomeLabScope)*
   - **IP Range:** *(e.g., 192.168.1.100 – 192.168.1.200)*
   - **Subnet Mask:** *(e.g., 255.255.255.0)*
   - **Default Gateway:** *(e.g., 192.168.1.1)*
   - **Lease Duration:** *(e.g., 8 days)*
4. Authorized the DHCP server in Active Directory
5. Activated the scope

📸 *Screenshot: DHCP scope configuration*
📸 *Screenshot: Client receiving IP address from server*

---

### 2. DNS – Domain Name System

> Resolves hostnames to IP addresses within the network.

**Steps taken:**
1. Installed the **DNS Server** role via Server Manager
2. Created a **Forward Lookup Zone** for the domain *(e.g., homelab.local)*
3. Added an **A record** for the server
4. Verified DNS resolution from the client VM using `nslookup`

📸 *Screenshot: DNS Manager showing forward lookup zone*
📸 *Screenshot: nslookup test result on client*

---

### 3. Active Directory (AD)

> Centralizes user authentication and manages access to network resources.

**Steps taken:**
1. Installed **Active Directory Domain Services (AD DS)** role
2. Promoted the server to a **Domain Controller**
   - Created a new forest: *(e.g., homelab.local)*
   - Set the **DSRM password**
3. Server rebooted into the domain
4. Created **Organizational Units (OUs):**
   - `OU=Users`
   - `OU=Computers`
5. Created a test domain user account *(e.g., jdoe@homelab.local)*
6. Joined the **Client VM** to the domain
7. Logged into the client using the domain user account

📸 *Screenshot: Active Directory Users and Computers panel*
📸 *Screenshot: Client successfully joined to domain*
📸 *Screenshot: Domain user login on client VM*

---

### 4. File Sharing Services (FSS)

> Allows files to be shared across the network and accessed from the client VM.

**Steps taken:**
1. Installed the **File and Storage Services** role
2. Created a shared folder *(e.g., `C:\SharedFiles`)*
3. Configured **share permissions:**
   - Domain Users — Read
   - Administrators — Full Control
4. Configured **NTFS permissions** on the folder
5. Accessed the shared folder from the client VM via `\\[ServerName]\SharedFiles`

📸 *Screenshot: Share permissions configuration*
📸 *Screenshot: Client accessing the shared folder*

---

## VM Communication Test

Verified that both VMs can communicate with each other.

**Tests performed:**

| Test | Command | Result |
|------|---------|--------|
| Ping server from client | `ping 192.168.1.1` | ✅ Success |
| Ping client from server | `ping [client IP]` | ✅ Success |
| DNS resolution | `nslookup homelab.local` | ✅ Resolved |
| Domain join | System Properties → Domain | ✅ Joined |
| File share access | `\\server\SharedFiles` | ✅ Accessible |

📸 *Screenshot: Ping test results*

---

## Screenshots

| # | Description | File |
|---|-------------|------|
| 1 | Hypervisor main screen | `screenshots/01-hypervisor.png` |
| 2 | VM network settings | `screenshots/02-vm-network.png` |
| 3 | DHCP scope setup | `screenshots/03-dhcp-scope.png` |
| 4 | Client IP from DHCP | `screenshots/04-dhcp-client.png` |
| 5 | DNS forward lookup zone | `screenshots/05-dns-zone.png` |
| 6 | nslookup test | `screenshots/06-dns-nslookup.png` |
| 7 | AD Users and Computers | `screenshots/07-ad-users.png` |
| 8 | Client joined to domain | `screenshots/08-domain-join.png` |
| 9 | Domain user login | `screenshots/09-domain-login.png` |
| 10 | File share permissions | `screenshots/10-fss-permissions.png` |
| 11 | Client accessing file share | `screenshots/11-fss-access.png` |
| 12 | Ping test | `screenshots/12-ping-test.png` |

---

## What I Learned

- How to configure a **Windows Server** as a Domain Controller
- How **DHCP** dynamically assigns IP addresses within a local network
- How **DNS** resolves names inside a private domain
- How **Active Directory** manages users, computers, and domain authentication
- How to set up and secure **file shares** with NTFS and share-level permissions
- How to connect client machines to a Windows domain and authenticate with domain credentials
- Networking fundamentals: subnets, IP ranges, gateways, and internal VM networking

---

*Project completed as part of my IT/Networking home lab practice.*
