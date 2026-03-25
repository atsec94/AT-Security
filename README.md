# AT-Security

# Hello, I'm Austin Tucker
<a href="https://linkedin.com/in/austin-tucker-0964aaa8">
  <img src="https://img.shields.io/badge/-LinkedIn-0072b1?&style=for-the-badge&logo=linkedin&logoColor=white" />
</a>

I'm a cybersecurity professional (M.S. Cybersecurity Technology, UMGC) focusing on **SOC analysis, DFIR, and vulnerability management**. I build homelabs that mirror real-world environments and document everything — diagrams, methodology, detections, and measurable outcomes.

- **Current focus:** SIEM deployment & detection engineering, Windows event log analysis, CIS hardening
- **Target roles:** Help Desk, SOC Analyst (Tier 1/2)
- **Methodologies:** MITRE ATT&CK mapping, CIS Benchmarks, CVE/CVSS analysis, NIST 800-61 IR lifecycle

---

## Active Project

### 🔵 SIEM & Detection Engineering — Wazuh Homelab
**Status: In Progress**

Deployed a Wazuh 4.14.4 SIEM in a segmented VirtualBox environment with a Windows Server 2022 Domain Controller and Windows 11 workstation as monitored endpoints. Established a documented baseline before any hardening or attack simulation.

**What's been built so far:**
- Wazuh OVA deployed on Amazon Linux 2023 with static IP configuration via `systemd-networkd`
- Agents deployed on DC01 (Windows Server 2022) and CL1 (Windows 11 Pro)
- Baseline captured: 22 Critical CVEs, CIS score 27% (99/359 controls passing), 622 medium alerts in first 24 hours
- MITRE ATT&CK tactics mapped to normal AD operations to establish behavioral baseline

**Upcoming:**
- Brute force simulation from Kali → detection via Event ID 4625 / Wazuh rule 18152
- Sysmon deployment with SwiftOnSecurity config
- CIS hardening via GPO with before/after score comparison

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
  <img src="https://img.shields.io/badge/-Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/-VirtualBox-183A61?style=for-the-badge&logo=virtualbox&logoColor=white" />
  <img src="https://img.shields.io/badge/-Windows_Server_2022-0078D4?style=for-the-badge&logo=windows&logoColor=white" />
</div>

---

## Contact
- 💬 LinkedIn: <a href="https://linkedin.com/in/austin-tucker-0964aaa8">austin-tucker-0964aaa8</a>
- 🧰 Projects: see `/projects` folder above
