# 🖥️ Wazuh Setup for SOC Lab

This document provides a **step-by-step guide** to install, configure, and use **Wazuh** for endpoint security, log analysis, and SIEM integration in your SOC-focused homelab.

Wazuh collects endpoint logs, monitors system events, detects threats, and can forward alerts to **Splunk** or **Azure Sentinel**.

---

## 🔹 1. Prerequisites

- Ubuntu/Debian 20.04+ or similar Linux OS
- Minimum 2 GB RAM, 2 CPU cores per VM
- 10+ GB disk space
- sudo privileges

---

## 🔹 2. Install Wazuh Manager

1. Add the Wazuh repository and install dependencies:

```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo apt-key add -
echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list
sudo apt update
sudo apt install wazuh-manager -y
```

2. Start and enable the Wazuh manager:

```bash
sudo systemctl enable wazuh-manager
sudo systemctl start wazuh-manager
sudo systemctl status wazuh-manager
```

---

## 🔹 3. Install Wazuh Agent

1. On each endpoint (Linux/Windows):

```bash
sudo apt install wazuh-agent -y
```

2. Configure the agent to connect to the manager:

```bash
sudo nano /var/ossec/etc/ossec.conf
# Set <server-ip> to your manager IP
```

3. Start and enable the agent:

```bash
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
sudo systemctl status wazuh-agent
```

---

## 🔹 4. Configure Log Forwarding to Splunk

1. Install the Wazuh Splunk App:

- [Wazuh Splunk App Documentation](https://documentation.wazuh.com/current/integration/splunk/index.html)

2. Forward alerts from Wazuh to Splunk by monitoring `/var/ossec/logs/alerts/alerts.json`.

3. Verify logs in Splunk **Search & Reporting**.

---

## 🔹 5. Configure Log Forwarding to Azure Sentinel

1. Use **Wazuh-to-Syslog** or **Logstash** to forward alerts to Azure Sentinel.

2. Configure Wazuh to send JSON alerts over Syslog to Sentinel's workspace.

3. Test that alerts are ingested correctly.

---

## 🔹 6. Key Tips

- Update Wazuh rules frequently:

```bash
sudo wazuh-control restart
```

- Monitor agents via Wazuh dashboard or CLI:

```bash
sudo wazuh-agent-control -l
```

- Test alert detection using **attack simulations** from Kali VM.

- Combine with **Suricata** and **Splunk** for full SOC lab visibility.

---

## 🔹 7. Recommended Directory Structure for SOC Lab

```
SOC-Homelab/
├── SPLUNK_SETUP.md
├── SURICATA_SETUP.md
├── WAZUH_SETUP.md
├── diagrams/
├── images/
└── configs/
```
