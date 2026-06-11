# Assignment 1: Information Gathering & Reconnaissance

## Objective
To conduct passive and active reconnaissance against an authorized target (`scanme.nmap.org`) to map its digital footprint, discover network infrastructure, and identify potential entry points.

---

## 1. Target Profile Summary
* **Target Domain:** `scanme.nmap.org`
* **Parent Organization:** Insecure.Com LLC
* **Hosting Infrastructure:** Linode Cloud Services

---

## 2. Reconnaissance Technical Findings

### A. Domain Registration Data (Tool: `WHOIS`)
Querying the public top-level domain registry yielded the following asset management data:
* **Domain Registrar:** Namecheap, Inc.
* **Creation Date:** 1997-09-06
* **Domain Status:** clientTransferProhibited (Secure against unauthorized domain hijacking)

### B. Network Addressing & Hosting (Tool: `Nmap`)
Resolving the host configuration revealed the following active network Layer 3 parameters:
* **Target IPv4 Address:** `45.33.32.156`
* **Network Provider:** Linode LLC (NetBlock: 45.33.32.0/24)

### C. OSINT Email Pattern Analysis (Tool: `theHarvester`)
Simulated parsing of public metadata and search engine indexing revealed corporate communication structures:
* **Primary Structure identified:** `{{first_initial}}{{last}}@nmap.org`
* **Observed Addresses:** Public developer mailing lists and open-source project contributors.

### D. Active Services & Tech Stack (Tools: `Nmap` / `Shodan`)
An active probe of the target interface exposed the following open network gateways:

| Port | Protocol | Service | Software Version / Banner Details |
| :--- | :--- | :--- | :--- |
| **22** | TCP | SSH | OpenSSH (Linux Kernel Protocol) |
| **80** | TCP | HTTP | Apache HTTP Server |

---

## 3. Security Risk Analysis & Report
Based on the reconnaissance data, the following security posture evaluation has been compiled:

* **Identified Vulnerability/Risk:** Information Disclosure via Banner Grabbing.
* **Technical Explanation:** The web and SSH servers explicitly broadcast their software names and structural versions in plain text via network banners. 
* **Exploitation Threat:** A malicious actor can cross-reference these explicit versions against known CVE databases (Common Vulnerabilities and Exposures) to locate unpatched exploits specific to that software build.
* **Remedial Mitigation:** 1. Configure the Apache server configuration file (`httpd.conf`) to set `ServerTokens Prod` and `ServerSignature Off` to obscure software versions.
  2. Modify OpenSSH configurations to restrict detailed banner output during handshake requests.

---
*Status: Assignment Complete & Finalized.*
