# 🛡️ SOC-Focused Home Lab

A hands-on **Security Operations Center (SOC)** lab designed to simulate real-world threat detection, monitoring, and incident response workflows.  
This project showcases the integration of multiple security tools like **Suricata, Splunk, Azure Sentinel, Wireshark, and Wazuh** to build a detection and response pipeline.

---

## 🎯 Objectives
- Build a SOC-focused homelab environment for practicing detection, response, and analysis.
- Simulate attacks and monitor them using enterprise-grade SIEM and IDS solutions.
- Develop and fine-tune detection rules, dashboards, and alerts.
- Gain hands-on experience in log collection, correlation, and threat hunting.

---

## 🛠️ Tools & Technologies
| Tool / Platform          | Purpose |
|--------------------------|---------|
| **Suricata**             | Network intrusion detection & traffic monitoring |
| **Splunk**               | SIEM platform for log aggregation & alerting |
| **Azure Sentinel**       | Cloud-native SIEM for threat detection & investigation |
| **Wireshark**            | Deep packet inspection and traffic analysis |
| **Wazuh**                | Endpoint security, log analysis, and SIEM integration |
| **Kali Linux / Attack VM**| Attack simulation and testing detection workflows |

---

## 🧩 Lab Architecture
```mermaid
graph TD
A[Attack VM - Kali Linux] -->|Simulated Attacks| B[Suricata IDS]
B -->|Alert Logs| C[Wazuh Manager]
C -->|Correlated Alerts| D[Splunk SIEM]
D -->|Dashboards & Alerts| E[Azure Sentinel]
E -->|Cloud Monitoring| F[Security Analyst]

