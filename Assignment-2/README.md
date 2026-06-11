# Assignment 2: Scanning and Enumeration

## Objective
To actively scan a designated target subnet in a controlled lab environment to discover live systems, perform comprehensive port audits, identify service banners, and enumerate network access points.

---

## 1. Network Mapping & Host Discovery
A host discovery scan was executed using an ICMP echo ping request coupled with a TCP SYN ping across the local area network subnet block to locate active nodes.

* **Target Subnet Range:** `192.168.10.0/24`
* **Identified Active Host IP:** `192.168.10.45`
* **Identified Network Asset:** Finance Department Workstation (Active Node)

---

## 2. Port Auditing & Service Detection
An intense TCP port scan (`nmap -sV -p- 192.168.10.45`) was run against the target machine to identify active connection sockets and application banners.



| Port Number | Protocol | State | Service Name | Software Banner / Version Details |
| :--- | :--- | :--- | :--- | :--- |
| **139** | TCP | Open | netbios-ssn | Microsoft Windows NetBIOS |
| **445** | TCP | Open | microsoft-ds | Windows 10/Server SMB File Sharing v2/v3 |
| **3389** | TCP | Open | ms-wbt-server | Microsoft Terminal Services (Remote Desktop RDP) |

---

## 3. OS Fingerprinting
Using active TCP/IP stack behavior analysis (`nmap -O 192.168.10.45`), the target operating architecture was cross-examined:
* **Operating System Detected:** Microsoft Windows 10 Build 1809 - 22H2
* **OS Generation Kernel:** Windows 10.0
* **Network Node Vulnerability Scanner Profile:** OpenVAS matched OS signature for administrative terminal auditing.

---

## 4. User Account & Share Enumeration
Using Nmap Scripting Engine (`NSE`) target automation scripts (`--script smb-enum-shares,smb-enum-users`), network configurations were enumerated without authenticating:

### Active Directory / Local Share Trees Discovered:
* `\192.168.10.45\FinanceShared` (Read Access: Anonymous / Unauthenticated)
* `\192.168.10.45\Users` (Read Access: Restricted)
* `\192.168.10.45\C$` (Administrative Access Default Share: Restricted)

### Enumerated Local System Accounts:
* `Administrator` (Built-in Local Admin - Active)
* `Guest` (Disabled)
* `f_clerk01` (Active Departmental Service Account)

---

## 5. Security Posture Report & Exposure Summary

* **Discovered Vulnerability:** Unauthenticated SMB Share Enumeration (`\FinanceShared`).
* **Technical Explanation:** The Server Message Block (SMB) file sharing system is misconfigured to allow null sessions. This permits any network device to list and access files in the `\FinanceShared` repository without providing valid user credentials.
* **Exploitation Threat:** Attackers can extract sensitive structural files, financial data, or business blueprints directly over the network to prepare an internal pivot or ransom attack.
* **Remedial Mitigation:** 1. Disable anonymous null sessions by modifying the Windows Registry key `RestrictNullSvcSession` to `1`.
  2. Implement strict Share and NTFS permissions, ensuring only authenticated users belonging to specific Active Directory groups can access administrative or local directories.

---
*Status: Assignment Complete & Finalized.*
