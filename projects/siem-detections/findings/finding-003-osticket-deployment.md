# Finding 003 — osTicket Help Desk Deployment

## Environment
| Component | Details |
|-----------|---------|
| VM | osTicket |
| IP Address | 192.168.56.30 |
| OS | Ubuntu 24.04 LTS |
| Software | osTicket v1.18.1 |
| Web Stack | Apache 2.4 / MySQL 8.0.45 / PHP 8.3.6 |

## Objective
Deploy a help desk ticketing system to simulate a real SOC/IT support workflow where security alerts and IT issues generate trackable, assignable tickets — mirroring enterprise environments that use tools like ServiceNow or Jira Service Management.

## Prerequisites Installed
| Package | Purpose |
|--------|---------|
| apache2 | Web server to serve osTicket |
| mysql-server | Database backend for ticket storage |
| php8.3 + libapache2-mod-php8.3 | PHP runtime and Apache integration |
| php8.3-mysql | PHP extension for MySQL connectivity |
| php8.3-gd | Image processing support |
| php8.3-mbstring | Multi-byte string handling |
| php8.3-xml | XML parsing for email processing |
| php8.3-imap | Email fetching support |
| php8.3-intl | Internationalization support |
| php8.3-apcu | In-memory caching for performance |
| unzip / curl / wget | Download and extraction utilities |

## Deployment Steps

### 1. LAMP Stack Installation
```bash
sudo apt update
sudo apt install -y apache2 mysql-server php php8.3-mysql \
  php8.3-gd php8.3-mbstring php8.3-xml php8.3-imap \
  php8.3-intl php8.3-apcu php8.3-cli unzip curl
sudo apt install -y libapache2-mod-php8.3
sudo a2enmod php8.3
sudo systemctl restart apache2
```

### 2. Download osTicket
```bash
cd /tmp
wget https://github.com/osTicket/osTicket/releases/download/v1.18.1/osTicket-v1.18.1.zip
unzip osTicket-v1.18.1.zip -d /tmp/osticket
```

### 3. Deploy to Web Root
```bash
sudo mv /tmp/osticket/upload /var/www/html/osticket
sudo chown -R www-data:www-data /var/www/html/osticket
sudo chmod -R 755 /var/www/html/osticket
sudo cp /var/www/html/osticket/include/ost-sampleconfig.php \
        /var/www/html/osticket/include/ost-config.php
sudo chown www-data:www-data /var/www/html/osticket/include/ost-config.php
sudo chmod 0666 /var/www/html/osticket/include/ost-config.php
```

### 4. MySQL Database Setup
```sql
CREATE DATABASE osticket;
CREATE USER 'osticket'@'localhost' IDENTIFIED BY '<password>';
GRANT ALL PRIVILEGES ON osticket.* TO 'osticket'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
> Note: Dedicated least-privilege DB user created — osTicket does not connect as root.

**Screenshot — Database creation confirmed:**

![Database Creation](screenshots/osTicketDatabaseCreation.png)

### 5. Web Installer
Navigated to `http://192.168.56.30/osticket/setup/` and completed installation with:
- Helpdesk Name: LAB Help Desk
- Non-default admin username
- Dedicated system and admin email addresses under lab.local domain

**Screenshot — Successful installation:**

![osTicket Installation Complete](screenshots/osTicketInstall.png)

### 6. Post-Install Hardening
```bash
# Lock config file to read-only
sudo chmod 0644 /var/www/html/osticket/include/ost-config.php

# Remove installer directory to prevent reinstallation attacks
sudo rm -rf /var/www/html/osticket/setup
```

**Screenshot — Config locked and setup directory removed:**

![Post-Install Hardening](screenshots/SecureConfFile.png)

## Verification

**Screenshot — Admin Control Panel accessible:**

![osTicket Admin Panel](screenshots/osTicketFirstLogon.png)

- All PHP prerequisites passed (green) in osTicket web installer
- Admin Control Panel accessible at `http://192.168.56.30/osticket/scp/`
- MySQL database confirmed operational with dedicated user
- Setup directory confirmed removed post-install

## Security Notes
- **Setup directory removed** immediately after installation — leaving `/setup/` exposed allows unauthenticated reinstallation of osTicket, wiping the database
- **Config file locked to 0644** — prevents web processes from modifying database credentials after install
- **Least-privilege database user** — osTicket DB user has access only to the `osticket` database, not all MySQL databases
- **Non-default admin username** — avoids common credential stuffing targets like `admin` or `administrator`
- **Separate system and admin email addresses** — follows principle of separation of duties

## Next Steps
- [ ] Install Wazuh agent on osTicket VM (192.168.56.30) for SIEM visibility
- [ ] Integrate Wazuh alerts with osTicket to auto-generate tickets on detections
- [ ] Simulate help desk workflows: password resets, account lockouts, malware alerts
- [ ] Document ticket lifecycle as Finding 004

## Status
✅ Complete — osTicket v1.18.1 deployed and secured
