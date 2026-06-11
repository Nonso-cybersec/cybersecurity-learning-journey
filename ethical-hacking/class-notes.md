# Ethical Hacking — Class Notes

## Class 1 — Foundation & Methodology

### What Is Ethical Hacking

Before this class, hacking felt like a grey area. 
The foundation cleared that up immediately.

Ethical hacking is the authorised testing of systems — 
web applications, networks, APIs, and software — to find 
vulnerabilities before malicious actors do. The key word 
is **authorised**. A signed document defining the scope 
is what separates an ethical hacker from a criminal. 
Same skills, same tools, completely different legal standing.

The goal is always to help the business — find the 
weaknesses so they can be fixed, not exploited.

---

### PTES — The 7 Phases

The Penetration Testing Execution Standard gives structure 
to what could otherwise be chaotic work. Every professional 
pentest follows this framework.

**1. Pre-Engagement**
The most important phase. Define the scope, rules of 
engagement, and get written authorisation. No scope = 
no protection. This is where the legal foundation is laid.

**2. Intelligence Gathering**
OSINT and reconnaissance. Collect everything publicly 
available about the target without touching their systems. 
Job postings, LinkedIn profiles, WHOIS records, Shodan — 
all fair game. The attacker is invisible at this stage.

**3. Threat Modelling**
Identify the organisation's assets and map how an attacker 
would prioritise and approach them. What is most valuable? 
What is most exposed?

**4. Vulnerability Analysis**
Scan the identified assets for weaknesses. This is where 
tools like Nessus come in — methodically checking every 
service against known CVEs.

**5. Exploitation**
Attempt to exploit the discovered vulnerabilities to gain 
access. Controlled, documented, and within scope.

**6. Post Exploitation**
Once inside — maintain access, move laterally, exfiltrate 
data (simulated), and cover tracks. This phase mirrors 
exactly what a real attacker would do, which is why 
understanding it makes SOC detection better.

**7. Reporting**
The deliverable. Every finding documented with evidence, 
severity rating, business impact, and remediation steps. 
A pentest without a report helps nobody.

---

### OWASP & WSTG

**OWASP** — Open Web Application Security Project. 
Focuses specifically on web application security. 
The OWASP Top 10 is the industry standard list of 
the most critical web vulnerabilities.

**WSTG** — Web Security Testing Guide. The definitive 
manual for web application penetration testing. 
Every web pentest methodology traces back to this guide.

---

### Key Insight — Offensive Skills Make Better Defenders

The most valuable thing about learning ethical hacking 
as a SOC analyst is context. When Suricata fires an 
ET SCAN alert, I now know exactly what the attacker 
was doing — running an Nmap scan, probing for open 
ports, looking for a way in.

Understanding the attack makes the defence instinctive.

---

### SOC Detection Mapping

| PTES Phase | SOC Detection Opportunity |
|---|---|
| Intelligence Gathering | Honeypot triggers, unusual DNS queries |
| Vulnerability Analysis | Nmap scan → Suricata ET SCAN alert |
| Exploitation | IDS signature match, anomalous traffic |
| Post Exploitation | Hidden process → Wazuh rootcheck alert |
| Lateral Movement | Failed logins → successful login correlation |

