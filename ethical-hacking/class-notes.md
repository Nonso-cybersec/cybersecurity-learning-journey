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

---

## Class 2 — Passive Reconnaissance & Footprinting

### What Is Footprinting

Footprinting is the digital trail a target leaves on 
the internet — and the art of finding it without being 
detected.

Every organisation leaves traces:
- Domain registration records
- Employee LinkedIn profiles
- Old job postings revealing tech stacks
- Cached pages of deleted content
- Subdomains indexed by search engines

Even when a target tries to remove their presence — 
cached pages, archive sites, and data brokers preserve 
those traces. A skilled OSINT analyst knows where to look.

---

### Passive vs Active Reconnaissance

**Passive — Stealth**
No packets sent to the target. Uses third-party sources 
— search engines, social media, WHOIS databases. 
The target has zero visibility that they are being 
researched. Completely legal.

**Active — Noisy**
Directly probes the target. Tools like Nmap send packets 
to the target's systems. A well-configured IDS/Suricata 
will detect this — as proven in my home lab when Nmap 
scans triggered immediate ET SCAN alerts in Wazuh.

Key distinction: passive uses public data, active 
generates network traffic that leaves evidence.

---

### Google Dorking

Advanced search operators that turn Google into an 
OSINT weapon:

| Dork | What It Finds |
|---|---|
| `site:target.com` | All indexed pages on a domain |
| `filetype:pdf site:target.com` | Leaked internal documents |
| `intitle:"index of"` | Exposed directory listings |
| `inurl:admin` | Admin login pages |

These find information the target didn't intend to expose 
but failed to restrict. Entirely passive — no interaction 
with the target's servers.

---

### WHOIS — The Domain ID Card

WHOIS reveals:
- Who registered the domain and when
- When it expires
- Name servers (reveals hosting provider)
- Administrative contact emails and phones

This data alone can reveal the target's tech stack, 
hosting provider, and sometimes direct contact details 
for social engineering.

---

### AMASS — Passive Subdomain Enumeration

```bash
amass enum -passive -d target.com
```

Scrapes 50+ sources without touching the target's DNS:
- Finds subdomains stealthily
- Maps IP ranges and hosting infrastructure
- Integrates Shodan, Censys, VirusTotal, SecurityTrails

---

### The OSINT Workflow



The OSINT framework follows 4 structured stages — 
turning scattered public data into actionable intelligence.

**Stage 1 — Scoping**
Define exactly who and what you are investigating.
- Target name and domain
- Core assets (websites, subdomains, employees)
- Boundaries — what is in scope and what is not
Without clear scope, you collect noise instead of intelligence.

**Stage 2 — Harvesting**
Actively collect data from public sources:
- WHOIS — domain registration details
- Google Dorks — exposed files and pages
- theHarvester — emails, subdomains, IPs
- Amass — passive subdomain enumeration
- Social media — employee names, tech stacks, org structure
This stage answers: *"What can I find?"*

**Stage 3 — Aggregation**
Merge all findings from different sources into one 
unified profile. Data from WHOIS, Google, LinkedIn, 
and theHarvester are combined — painting a complete 
picture of the target's digital presence.
This stage answers: *"What does it all mean together?"*

**Stage 4 — Analysis**
Review the aggregated profile and identify:
- Weak points and misconfigurations
- Unsecured ports and exposed services
- Leaked documents and credentials
- Attack vectors worth pursuing
This stage answers: *"Where are the opportunities?"*

---

### Why This Workflow Matters For SOC Work

Understanding how attackers gather intelligence helps 
defenders reduce their attack surface. Running this 
workflow against one's own organisation reveals what 
attackers see — before they use it against you.

---

### Practical — theHarvester on Tesla.com

**Command run:**
```bash
theHarvester -d tesla.com -b google
```

**What theHarvester does:**
Queries public sources to harvest:
- Email addresses associated with the domain
- Subdomains discovered through search engines
- IP addresses and hosting information

**Why Tesla.com:**
Public company — all information gathered is publicly 
available. Using a real target makes the exercise 
meaningful and realistic.


