# Ethical Hacking Lab Setup

## Why I Built This

Cybersecurity is not just about defence. To truly protect 
a system, you need to understand how attackers think and 
operate. This lab gives me a safe, legal environment to 
practise offensive techniques — the same ones real 
attackers use — so I can better detect and defend against 
them in my SOC work.

---

## Attack Machine
**Kali Linux** — the industry standard for penetration 
testing. Pre-loaded with hundreds of offensive security 
tools. This is where all attacks originate in my lab.

---

## Target Machines

**DevGuru (VulnHub)**
A deliberately vulnerable web application server. 
Real services, real misconfigurations, real attack surface.

**Metasploitable 2**
A vulnerable Ubuntu server designed specifically for 
exploitation practice. Packed with outdated services 
and weak credentials — a goldmine for beginners learning 
how real attacks work.

**Kioptrix Level 1**
An old Red Hat Linux server from the early 2000s. 
Old OS means old vulnerabilities with well-documented 
exploits. Teaches the complete attack chain from 
reconnaissance to shell access.

---

## Tools Configured

**FoxyProxy**
Browser extension that routes Firefox traffic through 
Burp Suite with one click. Essential for web application 
testing.

**Burp Suite**
Intercepting proxy for web application testing. Sits 
between the browser and the target — captures, reads, 
and modifies HTTP/HTTPS requests before they reach 
the server. The most important tool for web app pentesting.

**Nessus Essentials**
Professional vulnerability scanner. Goes beyond Nmap 
port scanning — checks every discovered service against 
a database of known CVEs and gives severity ratings 
(Critical, High, Medium, Low) with remediation guidance.

---

## Network Configuration

All VMs connected to **LabNetwork** (VirtualBox NAT Network 
— 10.0.2.0/24). Isolated from my home network but allowing 
VM-to-VM communication and controlled internet access.

| VM | IP | Role |
|---|---|---|
| Kali Linux | 10.0.2.15 | Attacker |
| DevGuru | 10.0.2.3 | Target |
| Metasploitable 2 | 10.0.2.x | Target |
| Kioptrix | 10.0.2.x | Target |

---

## SOC Analyst Perspective

Every tool in this lab has a detection counterpart:
- Nmap scan → Suricata ET SCAN alert
- Burp Suite traffic → unusual HTTP patterns in Wazuh
- Metasploit exploit → process injection alert
- Credential brute force → failed login alerts

Building offensive skills makes me a better defender — 
I know what to look for because I know how it's done.
