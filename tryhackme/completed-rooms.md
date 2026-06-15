# TryHackMe — Completed Rooms

**Profile:** tryhackme.com/p/paulnonso925
**Completed:** 19 rooms

---

## Windows Fundamentals Module

### Key Takeaways

**Windows Fundamentals 1**
Developed deeper understanding of the Windows desktop, 
NTFS file system, User Account Control (UAC), and core 
system components — foundation for understanding Windows 
security events in a SOC environment.

**Windows Fundamentals 2**
Learned system configuration and resource monitoring — 
Task Manager, Resource Monitor, MSConfig. Directly 
applicable to identifying suspicious processes during 
incident investigation.

**Windows Fundamentals 3**
Focused on securing Windows systems — Windows Defender, 
BitLocker, Windows Update, and hardening techniques. 
Connects directly to understanding what a hardened 
endpoint looks like vs a compromised one.

### Why Windows Matters For SOC Work
Most enterprise environments run Windows. A SOC analyst 
investigating alerts on Windows endpoints must understand 
normal system behaviour to identify abnormal activity. 
These fundamentals are the baseline for that knowledge.

---

## Networking Rooms

**Introductory Networking** — OSI model, TCP/IP, 
networking tools foundation

**What is Networking?** — How devices communicate, 
IP addresses, MAC addresses

**DNS in Detail** — How DNS resolves names to IPs, 
DNS attack vectors, DNS enumeration

---

## Security Rooms

**Junior Security Analyst Intro** — Day in the life 
of a SOC analyst, alert triage workflow

**Defensive Security Intro** — Blue team fundamentals, 
SIEM, threat intelligence

**Offensive Security Intro** — Ethical hacking 
introduction, legal framework

**Cyber Kill Chain** — 7 stages of a cyberattack — 
Reconnaissance through Actions on Objectives

**SOC Fundamentals** — SOC team structure, processes, 
and tools

---

## Other Rooms
- Linux Fundamentals Part 1
- Search Skills
- Careers in Cyber
- Virtualisation Basics
- Operating Systems: Introduction
- Windows Basics

---

## Passive Reconnaissance Room — June 12, 2026

### What The Room Covered

**Core Concept**
Passive recon is the backbone of both penetration 
testing and threat hunting. Even with privacy laws 
like GDPR and CCPA, organisations leave enormous 
amounts of data publicly available. Defenders and 
ethical hackers work side by side — finding what 
adversaries can discover before they do.

---

### WHOIS
- Runs on TCP port 43
- Returns domain registration details — registrar, 
  name servers, registration dates, admin contacts
- Attackers focus on registration data for phishing 
  patterns — mimicking legitimate domains
- On January 28, 2025, ICANN replaced WHOIS with 
  RDAP (Registration Data Access Protocol)
- RDAP returns data in JSON format and aligns with 
  modern data privacy laws (GDPR/CCPA)
- RDAP can be configured to limit what attackers 
  discover through passive recon

---

### DNS Lookups — nslookup and dig

**nslookup**
Queries DNS records — translates domains to IPs, 
finds mail servers (MX), SPF records, DMARC records.

**dig**
Preferred over nslookup for:
- Cleaner output
- Better for scripting and automation
- More detailed DNS information

**SOC relevance:**
DNS records should always be audited:
- Check for malicious mail servers
- Remove services no longer in use
- Prevent subdomain takeover attacks

---

### DNS Dumpster
Uses certificate transparency logs, historical DNS 
data, and search engine cache to discover subdomains 
that organisations have forgotten about.

Forgotten subdomains often contain:
- Debugging logs
- Test applications
- Weak passwords
- Exposed code

Defenders must audit all subdomains regularly,
forgotten assets are easy attack targets.

---

### Certificate Transparency (CT) Logs
- All SSL/TLS certificates issued by Certificate 
  Authorities are logged publicly
- Each certificate contains Subject Alternative Names 
  (SANs) — showing all subdomains covered
- CT logs update in real time — more comprehensive 
  than DNS Dumpster
- No rate limits on querying CT logs
- Defenders should monitor CT logs to catch new 
  certificates issued for their domains — early 
  warning of subdomain takeover attempts

---

### Shodan
- Search engine for internet-facing devices
- Indexes open ports, services, and banners
- Finds exposed servers, cameras, routers, and 
  industrial systems
- Attackers use it to find vulnerable targets without 
  touching them
- Defenders use it to see what their organisation 
  looks like from an attacker's perspective

---

### Key Takeaway

Every tool in this room represents something an 
attacker can use for free, legally, without touching 
your systems. The defender's job is to know what's 
exposed and minimise it before the attacker finds it.

This is why regular passive recon audits of one's own 
organisation are essential security hygiene.

# TryHackMe - OSINT Challenge Walkthrough

## Objective

The objective of this challenge was to build a profile of a target using only passive reconnaissance techniques. The investigation began with a single image provided by the room and required gathering information exclusively from publicly available sources.

> No active exploitation, unauthorized access, or intrusive techniques were used during this exercise.

---

## Methodology

### 1. Metadata Analysis

The image metadata was examined to identify information embedded within the file.

**Information Discovered**

* City: London

**Tools Used**

* ExifTool
* Online metadata viewers

**Purpose**

Metadata can reveal location information, device details, timestamps, and other contextual information that may assist further investigation.

---

### 2. Username Enumeration

Information extracted from the image was used to identify potential usernames associated with the target.

**Information Discovered**

* GitHub profile linked to the target

**Tools Used**

* Search engines
* GitHub

**Purpose**

Usernames often act as digital identifiers and can be reused across multiple platforms.

---

### 3. Email Discovery

The GitHub profile contained publicly accessible contact information.

**Information Discovered**

* Public email address

**Tools Used**

* GitHub profile review

**Purpose**

Email addresses can be valuable intelligence for social engineering and account correlation activities.

---

### 4. Wireless Network Identification

A broadcast SSID associated with the target was identified and searched within a public wireless database.

**Information Discovered**

* Wireless Access Point (WAP) SSID

**Tools Used**

* Wigle.net

**Purpose**

Public Wi-Fi databases can provide information regarding network names and approximate locations where devices have connected.

---

### 5. Social Media and Public Content Review

Publicly available posts were reviewed to identify additional contextual information.

**Information Discovered**

* Holiday destination: New York

**Purpose**

Travel patterns, interests, and behavioural information can be useful during intelligence gathering.

---

### 6. Source Code Review

Public web content associated with the target was examined.

**Information Discovered**

* Password exposed within WordPress page source code

**Purpose**

Misconfigured websites and exposed credentials remain a common source of information leakage.

---

## Findings Summary

| Information Type      | Result                             |
| --------------------- | ---------------------------------- |
| City                  | London                             |
| Email Address         | Discovered via GitHub              |
| GitHub Account        | Identified                         |
| Wireless Network SSID | Identified through Wigle           |
| Travel Information    | New York holiday location          |
| Password Exposure     | Found within WordPress source code |

---

## Security Implications

This exercise demonstrates how seemingly harmless public information can be combined to build a comprehensive profile of an individual.

Potential risks include:

* Social engineering attacks
* Credential-based attacks
* Targeted phishing campaigns
* Identity profiling
* Account enumeration

The investigation required no exploitation of systems and relied entirely on publicly accessible information.

---

## Key Lessons Learned

* Metadata should be removed from files before public sharing.
* Public repositories should be regularly audited for exposed credentials.
* Website source code should be reviewed for sensitive information exposure.
* Individuals and organizations should periodically assess their digital footprint.
* OSINT techniques are valuable for both offensive and defensive security teams.

---

## Conclusion

This challenge demonstrated how a single image can serve as the starting point for a comprehensive OSINT investigation. Through passive reconnaissance alone, it was possible to uncover location data, online identities, contact information, network details, travel history, and exposed credentials.

The exercise highlights the importance of digital footprint management and reinforces the principle that security begins long before an attacker attempts exploitation.

## Sakura Room — OSINT Investigation Writeup
**Platform:** TryHackMe
**Date:** June 12-14, 2026
**Difficulty:** Medium

---

### Overview

This room simulates a real OSINT investigation — 
building a complete profile of an attacker using 
only publicly available information. Starting from 
a single image file, I traced the attacker across 
multiple platforms, the blockchain, the dark web, 
and physical geolocation.

The lesson: a little information at a time builds 
a complete picture of a person.

---

### Task 1 — Identity Discovery (ExifTool)

**Objective:** Identify the attacker from a file.

Loaded the provided file URL into ExifTool to 
extract metadata. ExifTool revealed the attacker's 
username embedded in the file metadata.

**Tool:** ExifTool
**Finding:** Attacker username extracted from metadata

---

### Task 2 — Social Media & Email (GitHub + Twitter/X)

**Objective:** Find the attacker's email and full name.

Searched the username across platforms via Google.
Found a GitHub account with matching characteristics.

### Obtaining the Email Address — PGP Key Analysis

The GitHub profile contained a PGP public key. 
Most people share PGP keys for encrypted communication 
without realising the key itself contains identifying 
metadata in the User ID (UID) field.

**Steps:**

1. Saved the PGP key block to a file:
```bash
nano key.asc
```

2. Inspected the key with GPG:
```bash
gpg --with-fingerprint key.asc
```

**Output:**

pub   rsa3072 2021-01-23 [SC] [expired: 2023-01-22]

uid   SakuraSnowAngel83@protonmail.com

sub   rsa3072 2021-01-23 [E] [expired: 2023-01-22]

**Finding:** Email address `SakuraSnowAngel83@protonmail.com`
revealed in the UID field.

**Key observations:**
- RSA 3072-bit key — strong encryption standard
- Key expired January 2023 — account possibly abandoned
- ProtonMail address — attacker was privacy-conscious
  yet leaked their email through a public key

**OSINT lesson:** PGP keys are shared publicly by 
design — but the metadata they contain makes them 
valuable intelligence sources. A privacy tool 
became an exposure point.
- **Full name** — found on Twitter/X profile (@SakuraLoverAiko)

**Key lesson:** People reuse usernames across platforms. 
One username becomes a pivot point to find everything else.

---

### Task 3 — Cryptocurrency (Ethereum Blockchain)

**Objective:** Find the attacker's cryptocurrency wallet.

Deep dive into the GitHub account revealed an 
Ethereum repository. The commit history contained 
a file named "creating mining script" — inside was 
the attacker's Ethereum wallet address.

Used **Etherscan.io** to trace blockchain transactions:
- Received payments from **Ethermine** on January 23, 2021
- Also received **Tether (USDT)** as payment
- Transaction history fully public on the blockchain

**Key lesson:** Blockchain transactions are permanent 
and public. A wallet address reveals complete 
financial history — who paid you, when, and how much.

---

### Task 4 — Dark Web & WiFi Geolocation (BSSID)

**Objective:** Find the attacker's home location via WiFi.

**The investigation chain:**

1. Found Twitter handle @SakuraLoverAiko
2. Tweet contained MD5 hash linking to a DeepPaste 
   (.onion) paste — "Regular WiFi and Passwords"
3. Paste contained SSID: **DK1F-G**
4. Searched DK1F-G on **Wigle.net Advanced Search**
5. Result: **BSSID: 84:AF:EC:34:FC:F8**
6. Coordinates: Lat 40.60, Long 140.46 → **Hirosaki, Japan**

**Challenge faced:** DeepPaste was offline. The paste 
site had changed domains. Had to find the SSID from 
a screenshot of the paste rather than accessing it 
directly. This is real OSINT — sources go offline, 
investigators must adapt.

**Key lesson:** SSID names are not unique — always 
verify with BSSID (MAC address) which is globally 
unique to one physical router. Wigle.net maps WiFi 
networks globally using crowdsourced wardriving data.

---

### Task 5 — Homebound (Physical Geolocation)

**Objective:** Find the attacker's last location.

Aiko posted a photo of cherry blossoms on Twitter 
with the caption "Checking out some last minute 
cherry blossoms before heading home!"

**Google Image Search** on the cherry blossom photo 
identified the location as the **DCA Airport line** 
(Washington DC area).

Aiko also mentioned visiting the **Sakura Lounge** 
as her last location — research revealed the Sakura 
Lounge is operated by **Japan Airlines at Haneda Airport**.

**Lake Inawashiro** in Fukushima Prefecture identified 
from another image using Google Images — corroborating 
the Japan home location established by the WiFi BSSID.

---

### Complete Attacker Profile Built

| Data Point | Value | Source |
|---|---|---|
| Username | SakuraLoverAiko | ExifTool metadata |
| Full name | Aiko | Twitter/X profile |
| Email | Found on GitHub | GitHub profile |
| Crypto wallet | 0xa102397d...B53abB6ef | GitHub commit history |
| Crypto payments | Ethermine + Tether | Etherscan.io |
| WiFi SSID | DK1F-G | DeepPaste (.onion) |
| BSSID | 84:AF:EC:34:FC:F8 | Wigle.net |
| Home city | Hirosaki, Japan | Wigle.net coordinates |
| Last location | Haneda Airport (JAL Sakura Lounge) | Google + research |
| Holiday destination | Washington DC area | Google Image Search |

---

### Key Takeaways

**1. Usernames are pivot points**
One username across multiple platforms connects 
identity, email, social media, and professional accounts.

**2. Blockchain is permanently public**
A wallet address exposes complete financial history. 
Cryptocurrency is not anonymous — it's pseudonymous 
at best.

**3. WiFi geolocation is powerful**
BSSID + Wigle.net can pinpoint a person's home 
location from a single social media post mentioning 
their WiFi network name.

**4. Images carry invisible data**
Metadata, visual landmarks, and reverse image search 
can geolocate any photo posted online.

**5. Dead ends are part of real OSINT**
The DeepPaste site was offline. Real investigations 
hit dead ends — the skill is knowing how to find 
the same information through alternative routes.

**6. Small data points build big pictures**
No single piece of information was conclusive alone. 
Combined across platforms — identity, location, 
finances, movement patterns — a complete profile emerged.
