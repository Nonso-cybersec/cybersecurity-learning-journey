# Active Reconnaissance — Nmap Against Metasploitable 2

## Context

Active reconnaissance is the second phase of PTES — 
directly probing the target to gather technical 
intelligence. Unlike passive recon, this generates 
network traffic the target can detect.

Metasploitable 2 is a deliberately vulnerable Ubuntu 
VM built for security training. Every vulnerability 
is intentional — it is the perfect target to learn 
active recon techniques safely and legally.

---

## The TCP 3-Way Handshake

Before understanding Nmap scans, you need to 
understand what they manipulate:
Client → SYN      → Server   "Can we talk?"

Client ← SYN-ACK  ← Server   "Yes, I'm ready"

Client → ACK      → Server   "Great, let's start"

Nmap exploits this handshake to map networks — 
sending partial or malformed packets and reading 
how the target responds.

---

## Port States

| State | Meaning |
|---|---|
| Open | Service actively listening — door is open |
| Closed | Port reachable but nothing listening |
| Filtered | Firewall blocking — can't even see the door |

---

## Scans Performed

### Phase 1 — SYN Scan (-sS)
```bash
nmap -sS 10.0.2.15
```

The "half-open" or "stealth" scan. Sends a SYN 
packet — if the port responds with SYN-ACK it's 
open. Nmap immediately sends RST to close the 
connection without completing the handshake.

**Why use it:** Faster than full connect scan. 
Often bypasses basic logging since no full 
connection is ever established.

**Result:** 20+ open ports discovered including 
FTP, SSH, HTTP, MySQL, PostgreSQL, VNC, IRC.

---

### Phase 2 — Version Detection (-sV)
```bash
nmap -sV 10.0.2.15
```

Interrogates each open port to identify exact 
software and version running.

**Key findings:**
| Port | Service | Version |
|---|---|---|
| 21/tcp | FTP | vsftpd 2.3.4 |
| 22/tcp | SSH | OpenSSH 4.7p1 |
| 80/tcp | HTTP | Apache httpd 2.2.8 |
| 3306/tcp | MySQL | MySQL 5.0 |
| 5432/tcp | PostgreSQL | PostgreSQL 8.3.0 |
| 1524/tcp | Shell | Metasploitable root shell |

**Why versions matter:** vsftpd 2.3.4 contains 
a famous backdoor — CVE-2011-2523. Anyone who 
sends a smiley face `:)` in the username triggers 
a root shell on port 6200. A version number alone 
reveals critical vulnerabilities.

---

### Phase 3 — OS Detection (-O)
```bash
nmap -O 10.0.2.15
```

Sends malformed packets and analyses responses. 
Every OS handles anomalies differently — Nmap 
compares responses against its database.

**Result:** Linux 2.6.9 – 2.6.33

**Why it matters:** Knowing the OS helps select 
targeted exploits. A Windows exploit won't work 
on Linux and vice versa.

---

### Phase 4 — Aggressive Full Scan (-A -p- -T4)
```bash
nmap -A -p- -T4 10.0.2.15
```

The complete picture:
- `-A` — version detection + OS detection + 
  script scanning + traceroute
- `-p-` — scan all 65,535 ports not just top 1000
- `-T4` — aggressive timing for faster results

**Additional findings:**
- Anonymous FTP login allowed on port 21
- SSH host keys exposed
- SMB message signing disabled (dangerous)
- IRC running UnrealIRCd (known backdoor version)
- Apache Tomcat 5.5 on port 8180

---

## Most Critical Finding — Port 1524
1524/tcp open  shell  Metasploitable root shell

A bind shell — a backdoor giving direct root 
access to anyone who connects. No credentials 
required.

```bash
nc 10.0.2.15 1524
```

One command. Instant root shell. Complete compromise.

Every other port requires finding and exploiting 
a vulnerability. Port 1524 skips all of that — 
it is an open door directly to the highest 
privilege level on the system.

**In a real environment this would mean:**
- Complete control of the machine
- Access to all data and credentials
- Ability to move laterally to every connected system

---

## SOC Analyst Perspective

Every open port and service version revealed by 
Nmap is something Suricata and Wazuh should be 
monitoring. In my SOC home lab:

- Nmap -sS scan → triggers ET SCAN alerts in Suricata
- Unusual connections to port 1524 → should trigger 
  immediate high severity alert
- vsftpd 2.3.4 smiley face exploit → detectable 
  via anomalous FTP traffic patterns

The attacker's reconnaissance becomes the defender's 
alert. Understanding active recon from both sides 
is what makes a complete security practitioner.

---

![Nmap SYN Scan](/images/Nmap%201.png)
![Nmap Version Detection](/images/Nmap%202.png)
![Nmap OS Detection](/images/Nmap%203.png)
![Nmap Aggressive Scan Part 1](/images/Nmap%204.png)
![Nmap Aggressive Scan Part 2](/images/Nmap%205.png)
