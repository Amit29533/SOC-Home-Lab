# 🛡️ Suricata IDS Setup for SOC Lab

This document provides a **step-by-step guide** to install, configure, and use **Suricata** as the network intrusion detection system (IDS) in your SOC-focused homelab.

Suricata will monitor network traffic, detect threats, and forward alerts/logs to **Splunk** and **Wazuh** for centralized analysis.

---

## 🔹 1. Prerequisites

- Ubuntu/Debian 20.04+ or similar Linux OS
- Minimum 2 GB RAM, 2 CPU cores
- 10+ GB disk space
- Internet connection
- sudo privileges

---

## 🔹 2. Install Suricata

1. Update packages:

```bash
sudo apt update && sudo apt upgrade -y
```

2. Install Suricata:

```bash
sudo apt install suricata -y
```

3. Verify the installation:

```bash
suricata --build-info
suricata --version
```

---

## 🔹 3. Configure Suricata

1. Backup default config:

```bash
sudo cp /etc/suricata/suricata.yaml /etc/suricata/suricata.yaml.bak
```

2. Edit Suricata configuration:

```bash
sudo nano /etc/suricata/suricata.yaml
```

- Set **HOME_NET** to your network, e.g., `HOME_NET: "[192.168.1.0/24]"`
- Configure logging outputs (eve-log) for JSON format for Splunk/Wazuh:

```yaml
outputs:
  - eve-log:
      enabled: yes
      filetype: regular
      filename: /var/log/suricata/eve.json
      types:
        - alert:
            payload: yes
            payload-printable: yes
            packet: yes
        - dns
        - http
        - tls
```

3. Test the configuration:

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml
```

---

## 🔹 4. Start Suricata

- Enable and start the service:

```bash
sudo systemctl enable suricata
sudo systemctl start suricata
```

- Check status:

```bash
sudo systemctl status suricata
```

---

## 🔹 5. Monitor Logs

- Default log location: `/var/log/suricata/`
- EVE JSON log for Splunk/Wazuh: `/var/log/suricata/eve.json`
- Test alerts with `tail -f`:

```bash
tail -f /var/log/suricata/eve.json
```

---

## 🔹 6. Integrate with Splunk

1. Use Splunk **File/Directory Input** to monitor `/var/log/suricata/eve.json`.
2. Configure **Source Type** as `json`.
3. Verify that Suricata alerts appear in Splunk **Search & Reporting**.

---

## 🔹 7. Key Tips

- Update rules frequently using **Suricata-Update**:

```bash
sudo suricata-update
sudo systemctl restart suricata
```

- For testing, use **Kali Linux VM** to generate attacks (e.g., nmap, metasploit).
- Combine with **Wazuh** for endpoint correlation and centralized alerting.

---

## 🔹 8. Recommended Directory Structure for SOC Lab

```
SOC-Homelab/
├── SURICATA_SETUP.md
├── SPLUNK_SETUP.md
├── WAZUH_SETUP.md
├── diagrams/
├── images/
└── configs/
```
