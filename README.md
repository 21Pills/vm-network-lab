# vm-network-lab
Testing my abilities on setting up DHCP, DNS AD, and FTS. 
# Home Lab – Windows Server VM Network

A hands-on home lab project where I built a virtualized network using two VMs communicating with each other. The Windows Server VM acts as the backbone of the network, providing **DHCP**, **DNS**, **Active Directory (AD)**, and **File Sharing Services (FSS)**.

---

##  Table of Contents

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

*Screenshot: DNS Manager showing forward lookup zone*
<img width="1707" height="912" alt="image" src="https://github.com/user-attachments/assets/5a913b68-52db-4ead-b4d4-9ad03e6fb24f" />

*Screenshot: nslookup test result on client*
 <img width="824" height="504" alt="image" src="https://github.com/user-attachments/assets/6b7e6c45-3a9c-4e4b-9cfb-3d8a29a8ed0c" />

---

### 3. Active Directory (AD)

> Centralizes user authentication and manages access to network resources.

**Steps taken:**
1. Installed **Active Directory Domain Services (AD DS)** role
2. Promoted the server to a **Domain Controller**
   - Created a new forest: *21Pills*
   - Set the **DSRM password**
3. Server rebooted into the domain
4. Created **Organizational Units (OUs):**
   - `OU=Users`
   - `OU=Computers`
5. Created a test domain user account *(e.g., jdoe@homelab.local)*
6. Joined the **Client VM** to the domain
7. Logged into the client using the domain user account

 *Screenshot:
<img width="747" height="395" alt="image" src="https://github.com/user-attachments/assets/a4bbc7cd-2d3a-4763-8786-b842a8add5a1" />

 *Screenshot: 
<img width="1609" height="945" alt="image" src="https://github.com/user-attachments/assets/43606702-9546-41d7-97bf-95f65de94a99" />

---

### 4. File Sharing Services (FSS)

> Allows files to be shared across the network and accessed from the client VM.

**Steps taken:**
1. Installed the **File and Storage Services** role
2. Created a shared folder *(Sales)*
3. Configured **share permissions:**
   - Domain Users — Read
   - Administrators — Full Control
4. Configured **NTFS permissions** on the folder
5. Accessed the shared folder from the client VM via `\\[21Pills]\SharedFiles`

 *Screenshot: 
<img width="762" height="682" alt="image" src="https://github.com/user-attachments/assets/cd0619ef-9c53-4fd5-b746-73410b253c89" />

*Screenshot:
<img width="735" height="558" alt="image" src="https://github.com/user-attachments/assets/f86f81c6-8ef3-4fbc-88db-4d7364583422" />


---

## VM Communication Test

Verified that both VMs can communicate with each other.

**Tests performed:**

| Test | Command | Result |
|------|---------|--------|
| Ping server from client | `ping 192.168.1.129` |  Success |
| Ping client from server | `ping 192.168.1.2` |  Success |
| DNS resolution | `nslookup 21Pills.com` |  Resolved |
| Domain join | System Properties → Domain |  Joined |
| File share access | `\\server\SharedFiles` |  Accessible |

*Screenshot: Ping test results*
<img width="1025" height="774" alt="image" src="https://github.com/user-attachments/assets/ade1002d-f82a-4fd5-a310-be5e7accb9f1" />
<img width="1290" height="780" alt="image" src="https://github.com/user-attachments/assets/0ae97ac5-0103-410a-9ff1-0abe46d3e997" />
<img width="1288" height="785" alt="image" src="https://github.com/user-attachments/assets/0e2c05a5-4702-4153-9997-4a2d17c0abc4" />
<img width="1288" height="785" alt="image" src="https://github.com/user-attachments/assets/73268ad1-b747-41ce-a639-8ee0275ff8a7" />
<img width="1023" height="763" alt="image" src="https://github.com/user-attachments/assets/e3dc679d-93e1-47b5-b96d-a1999c56c70a" />
<img width="1019" height="766" alt="image" src="https://github.com/user-attachments/assets/90fcea32-5751-4d10-b3b0-9e877513ee30" />

---



---

## What I Learned

- How to configure a **Windows Server** as a Domain Controller
- How **DHCP** dynamically assigns IP addresses within a local network
- How **DNS** resolves names inside a private domain
- How **Active Directory** manages users, computers, and domain authentication
- How to set up and secure **file shares** with NTFS and share-level permissions
- How to connect client machines to a Windows domain and authenticate with domain credentials
- Networking fundamentals: subnets, IP ranges, gateways, and internal VM networking
