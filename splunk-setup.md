# 🖥️ Splunk Setup for SOC Lab

This document provides a **step-by-step guide** to install, configure, and use **Splunk** as the centralized SIEM for your SOC-focused homelab.  

Splunk will collect logs from **Suricata, Wazuh**, and other sources, allowing you to build dashboards, alerts, and visualize security events.

---

## 🔹 1. Prerequisites

- Ubuntu/Debian 20.04+ or similar Linux OS
- Minimum 4 GB RAM, 2 CPU cores
- 20+ GB disk space for logs
- Internet connection for downloading Splunk packages
- sudo privileges

---

## 🔹 2. Download and Install Splunk

1. Download the latest **Splunk Debian package**:

```bash
wget -O splunk.deb 'https://download.splunk.com/products/splunk/releases/latest/linux/splunk.deb'
```

2. Install the package:

```bash
sudo dpkg -i splunk.deb
```

3. Start Splunk for the first time and accept the license:

```bash
/opt/splunk/bin/splunk start --accept-license
```

4. Enable Splunk to start at boot:

```bash
/opt/splunk/bin/splunk enable boot-start
```

---

## 🔹 3. Access Splunk Web Interface

- Open your browser and go to:

```
http://<SOC_VM_IP>:8000
```

- Default login:  
  - **Username:** admin  
  - **Password:** The one you set during first start

- Change the password immediately after the first login.

---

## 🔹 4. Configure Data Inputs

Splunk can ingest logs from **Wazuh, Suricata**, and other sources.

### 4.1 Add a File/Directory Input

1. Go to **Settings → Data Inputs → Files & Directories**.
2. Click **Add Data**.
3. Select the path where logs are stored, e.g., `/var/log/suricata/` or `/var/ossec/logs/`.
4. Choose **Source type** or let Splunk auto-detect.
5. Click **Next → Review → Submit**.

### 4.2 Forward Logs from Wazuh

1. Install the [Wazuh Splunk App](https://documentation.wazuh.com/current/integration/splunk/index.html).
2. Configure the **Wazuh manager** to forward logs to Splunk.
3. Verify logs appear in **Search & Reporting**.

---

## 🔹 5. Indexes

- Create dedicated **indexes** for different sources for better organization:

```conf
[suricata]
homePath   = $SPLUNK_DB/suricata/db
coldPath   = $SPLUNK_DB/suricata/colddb
thawedPath = $SPLUNK_DB/suricata/thaweddb

[wazuh]
homePath   = $SPLUNK_DB/wazuh/db
coldPath   = $SPLUNK_DB/wazuh/colddb
thawedPath = $SPLUNK_DB/wazuh/thaweddb
```

- Add this file in `$SPLUNK_HOME/etc/system/local/indexes.conf` and restart Splunk:

```bash
/opt/splunk/bin/splunk restart
```

---

## 🔹 6. Build Dashboards

1. Go to **Search & Reporting**.
2. Search logs:

```spl
index=suricata action="alert"
index=wazuh severity>=7
```

3. Save searches as **Panels** and combine them into **Dashboards**.
4. Example panels:
   - **Top 10 alert types**
   - **Suspicious IP addresses**
   - **Brute-force login attempts**

---

## 🔹 7. Alerts

- Go to **Alerts → Create Alert**.
- Example: Trigger alert if a **critical Suricata alert** occurs:

```spl
index=suricata action="alert" severity="critical"
```

- Configure actions: email, script, webhook, etc.

---

## 🔹 8. Tips for SOC Lab

- Use **Forwarders** to collect logs from multiple endpoints.
- Enable **SSL** for secure log transport.
- Regularly clean up old indexes to save space.
- Combine with **Azure Sentinel** or other SIEMs for hybrid monitoring.
