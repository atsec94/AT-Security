# SIEM & Detection Engineering — Wazuh Homelab

## Objective

Deploy a functional SIEM (Wazuh) in a segmented homelab environment, onboard Windows endpoints as monitored agents, establish a verified baseline, and document real alert findings. This project demonstrates the ability to deploy, configure, and operate a SIEM — a core skill for SOC Analyst and Help Desk roles.

---

## Lab Architecture

```
+---------------------------+       +---------------------------+
|   DC01 (192.168.56.10)    |       |   CL1 (192.168.56.11)     |
|   Windows Server 2022     |       |   Windows 11 Pro          |
|   Domain Controller       |       |   Domain: LAB.local       |
|   Wazuh Agent v4.14.4     |       |   Wazuh Agent v4.14.4     |
+------------+--------------+       +------------+--------------+
             |                                   |
             +---------------+-------------------+
                             |
                  +----------+----------+
                  |  Wazuh Server       |
                  |  192.168.56.20      |
                  |  Amazon Linux 2023  |
                  |  Wazuh v4.14.4 OVA  |
                  +----------+----------+
                             |
                  +----------+----------+
                  |  KALI (192.168.56.50)|
                  |  Attacker Machine   |
                  |  Debian/Kali Linux  |
                  +---------------------+

Network: VirtualBox Host-Only (192.168.56.0/24)
Hypervisor: Oracle VirtualBox
```

---

## Environment Details

| VM | IP Address | OS | Role |
|---|---|---|---|
| DC01 | 192.168.56.10 | Windows Server 2022 Standard Eval | Domain Controller (LAB.local) |
| CL1 | 192.168.56.11 | Windows 11 Pro | Domain-joined workstation |
| Wazuh | 192.168.56.20 | Amazon Linux 2023 | SIEM / Log aggregation |
| KALI | 192.168.56.50 | Debian/Kali Linux | Attacker (isolated) |

---

## Build Steps

### Phase 1 — Wazuh Server Deployment
- Downloaded Wazuh 4.14.4 OVA from official Wazuh documentation
- Imported into VirtualBox, assigned Host-Only network adapter (`192.168.56.0/24`)
- Resolved static IP configuration issue on Amazon Linux 2023 using `systemd-networkd` (`/etc/systemd/network/10-eth0.network`) — AL2023 does not use netplan or traditional network-scripts at boot
- Configured static IP `192.168.56.20/24`, DNS pointing to DC01 (`192.168.56.10`)
- Verified connectivity: Wazuh → DC01 (0% packet loss), Wazuh → CL1 (0% packet loss)
- Accessed Wazuh dashboard via `https://192.168.56.20`

![Wazuh Static IP Confirmed](screenshots/StaticIPConfiguredPreAgentDeployment.png)
![Wazuh Dashboard Confirmed](screenshots/WazuhStaticIPDashboardConfirmedPreAgentDeployment.png)

### Phase 2 — Agent Deployment
- DC01: Temporarily added NAT adapter for internet access, downloaded and installed `wazuh-agent-4.14.4-1.msi`, removed NAT adapter post-install
- CL1: Same process as DC01
- Both agents confirmed Active in Wazuh Endpoints dashboard
- NAT adapters removed from both machines post-installation — domain controllers and workstations should not have direct internet access in production

```powershell
# Agent install command used on both endpoints
msiexec /i wazuh-agent.msi /q WAZUH_MANAGER='192.168.56.20' WAZUH_AGENT_NAME='DC01'
NET START WazuhSvc
```

![DC01 Agent Running](screenshots/WazuhDC01Running.png)
![CL1 Agent Running](screenshots/WazuhCL1Running.png)
![Both Agents Active](screenshots/WazuhCL1Agent.png)

### Phase 3 — Baseline Documentation
Immediately after agent deployment, documented the following baseline state across both endpoints before any hardening or attack simulation.

---

## Baseline Findings — March 25, 2026 (DC01 + CL1)

### Vulnerability Detection (Combined — Both Agents)
| Severity | Count |
|---|---|
| Critical | 24 |
| High | 1,259 |
| Medium | 517 |
| Low | 15 |

**Per-agent breakdown:**
| Agent | Total Findings | Primary Vulnerable Package |
|---|---|---|
| DC01 | 1,450 | Windows Server 2022 Standard Eval 10.0.20348.587 |
| CL1 | 365 | Windows 11 Pro 10.0.26100.4946 |

**Top CVEs identified at baseline:**
| CVE | Count |
|---|---|
| CVE-2026-0102 | 3 |
| CVE-2026-21223 | 3 |
| CVE-2025-14174 | 2 |
| CVE-2025-24052 | 2 |
| CVE-2025-24990 | 2 |

These findings reflect unpatched evaluation builds on both endpoints. Baseline intentionally captured before patching to document before/after delta.

![Vulnerability Overview](screenshots/WazuhVulnOverview.png)
![DC01 Agent Detail](screenshots/DC01vulns.png)

### CIS Benchmark — Windows Server 2022 (DC01)
| Policy | Passed | Failed | Score |
|---|---|---|---|
| CIS Microsoft Windows Server 2022 Benchmark v2.0.0 | 99 | 260 | **27%** |

A 27% CIS compliance score reflects a default, unconfigured domain controller. This is the expected baseline for a freshly promoted DC with no hardening applied. Target: improve score through GPO hardening (documented in AD Attack & Defense project).

### MITRE ATT&CK — Top Tactics (First 24 Hours, DC01)
| Tactic | Alert Count |
|---|---|
| Defense Evasion | 71 |
| Persistence | 71 |
| Privilege Escalation | 71 |
| Initial Access | 70 |

**Analyst note:** These alerts reflect normal AD operations mapped to ATT&CK techniques — not active attacks. Event sources include logon/logoff events, scheduled tasks, and service operations. Establishing this baseline is critical for distinguishing normal from anomalous behavior in subsequent detection exercises.

### Alert Summary (First 24 Hours — Both Agents)
| Severity | Count |
|---|---|
| Critical | 0 |
| High | 0 |
| Medium | 622 |
| Low | 477 |

![DC01 Dashboard](screenshots/WazuhDC01.png)

---

## Connectivity Verification

![Wazuh to DC01](screenshots/WazuhToDC.png)
![Wazuh to CL1](screenshots/WazuhToCL1.png)

---

## Lessons Learned

- **Amazon Linux 2023 networking:** AL2023 does not use netplan (Ubuntu) or persist traditional `ifcfg` scripts via the `network` service at boot. Static IP configuration requires `systemd-networkd` with a `.network` file in `/etc/systemd/network/`. This is a common gotcha with the Wazuh OVA.
- **Temporary internet access for agent install:** When endpoints have no internet access (host-only network), the cleanest approach is temporarily attaching a NAT adapter, installing the agent, then removing the adapter. Adapter removal must be verified post-install — leaving internet access on a DC is a security risk.
- **Baseline before hardening:** Capturing CIS score, vulnerability count, and MITRE alert baseline before making any changes creates measurable proof of improvement — a critical element of any professional security engagement.
- **Document scope clearly:** Initial baseline captured DC01 only. Full baseline updated once CL1 agent was confirmed active. Always note which endpoints are included in aggregate findings.

---

## Next Steps

- [ ] Simulate brute force attack from Kali, detect via Event ID 4625 / Wazuh rule 18152
- [ ] Install Sysmon on DC01 and CL1 with SwiftOnSecurity config
- [ ] Document first alert investigation with full analyst workflow
- [ ] Begin CIS hardening via GPO, rescan to show score improvement
- [ ] Patch critical CVEs, rescan to show vulnerability reduction

---

## Screenshots Index

| File | Description |
|---|---|
| `StaticIPConfiguredPreAgentDeployment.png` | Wazuh VM static IP confirmed post-reboot |
| `WazuhStaticIPDashboardConfirmedPreAgentDeployment.png` | Dashboard accessible at static IP pre-agent deployment |
| `WazuhDC01Running.png` | WazuhSvc running on DC01 |
| `WazuhCL1Running.png` | WazuhSvc running on CL1 |
| `WazuhCL1Agent.png` | Both agents active in Wazuh Endpoints view |
| `WazuhDC01.png` | DC01 agent detail view in dashboard |
| `WazuhDC01Agent.png` | DC01 agent registration confirmation |
| `DC01vulns.png` | DC01 vulnerability and MITRE baseline |
| `WazuhVulnOverview.png` | Combined vulnerability overview — DC01 + CL1 |
| `WazuhToDC.png` | Connectivity verification Wazuh → DC01 |
| `WazuhToCL1.png` | Connectivity verification Wazuh → CL1 |
