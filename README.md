# AT-Security

# Hello, I'm Austin Tucker
<a href="https://linkedin.com/in/austin-tucker-0964aaa8">
  <img src="https://img.shields.io/badge/-LinkedIn-0072b1?&style=for-the-badge&logo=linkedin&logoColor=white" />
</a>

I'm a cybersecurity professional (M.S. Cybersecurity Technology, UMGC) focusing on **SOC analysis, DFIR, and vulnerability management**. I build homelabs that mirror real-world environments and document everything — diagrams, methodology, detections, and measurable outcomes.

- **Current focus:** SIEM deployment & detection engineering, help desk operations, Windows event log analysis
- **Target roles:** Help Desk, SOC Analyst (Tier 1/2)
- **Methodologies:** MITRE ATT&CK mapping, CIS Benchmarks, CVE/CVSS analysis, NIST 800-61 IR lifecycle

---

## Active Project

### SIEM, Detection Engineering & Help Desk Operations — Homelab
**Status: In Progress**

A fully segmented VirtualBox homelab simulating a small enterprise environment with a Windows domain, SIEM, attacker VM, and help desk ticketing system. All findings are documented with evidence, methodology, and security rationale.

**Lab Environment:**
| VM | IP | OS | Role |
|----|----|----|------|
| DC01 | 192.168.56.10 | Windows Server 2022 | Domain Controller (LAB.local) |
| CL1 | 192.168.56.11 | Windows 11 Pro | Domain Workstation |
| Wazuh | 192.168.56.20 | Amazon Linux 2023 | SIEM |
| Kali | 192.168.56.50 | Kali Linux | Attacker |
| osTicket | 192.168.56.30 | Ubuntu 24.04 | Help Desk Ticketing + Wazuh Agent |

**Completed:**
- ✅ Wazuh 4.14.4 SIEM deployed with agents on DC01, CL1, and osTicket VM
- ✅ Sysmon deployed on both Windows endpoints with SwiftOnSecurity config
- ✅ Baseline captured: 22 Critical CVEs, CIS score 27%, 622 medium alerts in first 24 hours
- ✅ MITRE ATT&CK tactics mapped to normal AD operations
- ✅ Finding 001 — SMB brute force attack simulated and detected (Event ID 4625 / Wazuh rule 18152)
- ✅ Finding 002 — Account lockout policy implemented and verified via GPO
- ✅ Finding 003 — osTicket v1.18.1 deployed on Ubuntu 24.04 (LAMP stack, post-install hardening applied)
- ✅ Finding 004 — Wazuh agent deployed on osTicket VM — full lab SIEM coverage achieved
- ✅ Finding 005 — Wazuh → osTicket REST API integration — SIEM alerts auto-generate help desk tickets

**Up Next:**
- [ ] Trigger real Wazuh alert with DC01 and CL1 running — confirm end-to-end auto-ticket generation
- [ ] Simulate help desk workflows — account lockout response, malware alert triage
- [ ] CIS hardening via GPO with before/after score comparison
- [ ] Patch critical CVEs, rescan to show vulnerability reduction

→ [Full project documentation](./projects/siem-detections/README.md)

---

## Certifications
<div>
  <a href="https://www.credly.com/badges/61f8acf6-6ddb-4f9d-b909-6ad3f04cef12/linked_in">
    <img src="https://img.shields.io/badge/-A%2B-4D4D4D?&style=for-the-badge&logo=CompTIA&logoColor=white" />
  </a>
  <a href="https://www.credly.com/badges/02ec147c-df2d-49a7-b3b4-4374de0dd038/linked_in?t=s9mbmm">
    <img src="https://img.shields.io/badge/-Network%2B-007ACC?&style=for-the-badge&logo=CompTIA&logoColor=white" />
  </a>
  <img src="https://img.shields.io/badge/-Security%2B-FF0000?&style=for-the-badge&logo=CompTIA&logoColor=white" />
</div>

---

## Tools In Use (This Lab)
<div>
  <img src="https://img.shields.io/badge/-Wazuh-494949?&style=for-the-badge&logoColor=white" />
  <img src="https://img.shields.io/badge/-osTicket-F57C00?&style=for-the-badge&logoColor=white" />
  <img src="https://img.shields.io/badge/-Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/-VirtualBox-183A61?style=for-the-badge&logo=virtualbox&logoColor=white" />
  <img src="https://img.shields.io/badge/-Windows_Server_2022-0078D4?style=for-the-badge&logo=windows&logoColor=white" />
  <img src="https://img.shields.io/badge/-Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" />
  <img src="https://img.shields.io/badge/-Sysmon-4D4D4D?style=for-the-badge&logoColor=white" />
  <img src="https://img.shields.io/badge/-Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
</div>

---

## Contact
- 💬 LinkedIn: <a href="https://linkedin.com/in/austin-tucker-0964aaa8">austin-tucker-0964aaa8</a>
- 🧰 Projects: see `/projects` folder above
