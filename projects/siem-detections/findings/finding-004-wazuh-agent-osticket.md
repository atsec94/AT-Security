# Finding 004 — Wazuh Agent Deployment on osTicket VM

## Environment
| Component | Details |
|-----------|---------|
| VM | osTicket |
| IP Address | 192.168.56.30 |
| OS | Ubuntu 24.04.4 LTS |
| Wazuh Agent Version | v4.14.4 |
| Wazuh Server | 192.168.56.20 |

## Objective
Deploy a Wazuh agent on the osTicket help desk VM to bring it under SIEM monitoring, ensuring full visibility across all lab endpoints. With this deployment, every VM in the lab is now monitored — DC01, CL1, and osTicket all reporting to the Wazuh SIEM.

## Steps Completed

### 1. Import Wazuh GPG Key
```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --no-default-keyring \
  --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && \
  sudo chmod 644 /usr/share/keyrings/wazuh.gpg
```
Imports the Wazuh package signing key so Ubuntu trusts packages from the Wazuh repository. Without this step, apt will reject the packages as unverified.

### 2. Add Wazuh Repository
```bash
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" \
  | sudo tee -a /etc/apt/sources.list.d/wazuh.list
sudo apt update
```
Adds the official Wazuh apt repository and refreshes the package list so the agent package is available to install.

### 3. Install Wazuh Agent
```bash
sudo WAZUH_MANAGER='192.168.56.20' WAZUH_AGENT_NAME='osTicket' apt install -y wazuh-agent
```
Installs the agent and pre-registers it with the Wazuh server at 192.168.56.20 with the agent name `osTicket`. Setting these environment variables at install time avoids manual config file editing.

### 4. Enable and Start the Agent
```bash
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
sudo systemctl status wazuh-agent
```
Reloads systemd to recognize the new service, enables it to start on boot, starts it immediately, and confirms it is running.

## Verification

**Screenshot — Wazuh agent active on osTicket VM:**

![Wazuh Agent Running](../screenshots/WazuhOsTicketIntegration.png)

**Screenshot — osTicket confirmed in Wazuh Endpoints dashboard:**

![osTicket in Wazuh Dashboard](../screenshots/osTicketAddedToDash.png)

- Agent ID: 003
- Agent Name: osTicket
- IP Address: 192.168.56.30
- OS: Ubuntu 24.04.4 LTS
- Version: v4.14.4
- Status: ● active

## Lab Coverage — Post Deployment
| Agent | IP | OS | Status |
|---|---|---|---|
| DC01 | 192.168.56.10 | Windows Server 2022 | Monitored |
| CL1 | 192.168.56.11 | Windows 11 Pro | Monitored |
| osTicket | 192.168.56.30 | Ubuntu 24.04.4 LTS | Monitored ✅ |

All lab endpoints are now reporting to the Wazuh SIEM. The entire environment has SIEM visibility.

## Security Notes
- **Full lab coverage achieved** — no blind spots remaining across the environment
- **Agent auto-starts on boot** — `systemctl enable` ensures monitoring resumes automatically if the VM is restarted
- **Named agent registration** — using `WAZUH_AGENT_NAME='osTicket'` makes the agent immediately identifiable in the dashboard without manual renaming

## Next Steps
- [ ] Integrate Wazuh alerts with osTicket to auto-generate tickets from SIEM detections
- [ ] Simulate help desk workflows — password resets, account lockouts, malware alerts
- [ ] Verify Apache and MySQL logs from osTicket VM are flowing into Wazuh

## Status
✅ Complete — osTicket VM fully monitored by Wazuh SIEM
