# 🛡️ Wazuh + VirusTotal Threat Intelligence Integration Lab

###  Building a File Integrity Monitoring → Threat Intelligence Pipeline (Home SOC Lab)

![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-1BA0D7?style=for-the-badge\&logo=wazuh\&logoColor=white)
![VirusTotal](https://img.shields.io/badge/Threat%20Intel-VirusTotal-394EFF?style=for-the-badge\&logo=virustotal\&logoColor=white)
![VirtualBox](https://img.shields.io/badge/Virtualization-VirtualBox-183A61?style=for-the-badge\&logo=virtualbox\&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)


---

##  Detection Pipeline

```text
File Created / Modified
        ↓
Wazuh FIM / Syscheck
        ↓
Hash Extraction
        ↓
VirusTotal Integration
        ↓
API Lookup
        ↓
Enriched Alert
        ↓
Wazuh Dashboard
```

>  **Critical Insight:** VirusTotal integration only works when FIM/Syscheck generates file-hash alerts. Without FIM → no VirusTotal lookup.

---

## Lab Environment

```text
Ubuntu Server
├── Wazuh Manager
├── Wazuh Dashboard
├── Wazuh Indexer
├── FIM / Syscheck
└── VirusTotal Integration

Windows Endpoint
├── Wazuh Agent
├── Event Logs
└── Vulnerability Detection
```

---

## 🛠️ Tools & Technologies

| Tool             | Role                |
| ---------------- | ------------------- |
| Wazuh            | SIEM platform       |
| Ubuntu Server    | Manager host        |
| Windows Endpoint | Monitored system    |
| VirusTotal API   | Threat intelligence |
| VirtualBox       | Virtualization      |
| SSH / PowerShell | Remote access       |

---

##  Pre-Lab Status

* ✔ Wazuh Manager installed & running
* ✔ Dashboard accessible
* ✔ Windows agent connected
* ✔ Logs flowing correctly
* ✔ VirusTotal account + API key ready

---

## Implementation Steps

### 1️ Verify Wazuh Manager

```bash
sudo systemctl status wazuh-manager --no-pager
```

---

### 2️ Confirm VirusTotal Integration Files

```bash
ls -la /var/ossec/integrations/
```

Expected:

```text
virustotal
virustotal.py
```

---

### 3️ Configure VirusTotal Integration

```bash
sudo nano /var/ossec/etc/ossec.conf
```

Add:

```xml
<integration>
  <name>virustotal</name>
  <api_key>YOUR_API_KEY</api_key>
  <group>syscheck</group>
  <alert_format>json</alert_format>
</integration>
```

---

### 4️ Validate Configuration

```bash
sudo /var/ossec/bin/wazuh-analysisd -t
```

---

### 5️ Restart Wazuh

```bash
sudo systemctl restart wazuh-manager
```

---

### 6️ Confirm Integration Loaded

```bash
sudo grep -i virustotal /var/ossec/logs/ossec.log | tail -20
```

---

##  Issue #1 — No Alerts Generated

Despite correct configuration:

* ❌ No VirusTotal alerts
* ❌ No FIM events from Windows

###  Root Cause

```text
Windows agent was not generating FIM/Syscheck alerts
→ No file hash
→ No VirusTotal trigger
```

---

##  Solution Strategy

Instead of debugging Windows FIM, a **local monitored directory** was used:

```text
/opt/vt-test
```

---

### Create Directory

```bash
sudo mkdir -p /opt/vt-test
sudo chown root:wazuh /opt/vt-test
sudo chmod 775 /opt/vt-test
```

---

### Enable FIM Monitoring

Edit config:

```xml
<directories realtime="yes" check_all="yes">/opt/vt-test</directories>
```

---

##  Issue #2 — XML Configuration Error

Incorrect:

```xml
&gt; /opt/vt-test &lt;
```

Correct:

```xml
<directories realtime="yes" check_all="yes">/opt/vt-test</directories>
```

###  Recovery

```bash
sudo cp ossec.conf.backup /var/ossec/etc/ossec.conf
sudo systemctl restart wazuh-manager
```

---

## Testing

### FIM Test

```bash
echo "test" | sudo tee /opt/vt-test/test.txt
```

---

### VirusTotal Trigger Check

```bash
sudo grep -ia "virustotal" /var/ossec/logs/alerts/alerts.json
```

---

###  EICAR Test

```bash
printf '%s\n' 'X5O!...H+H*' | sudo tee /opt/vt-test/eicar.com
```

Expected:

```text
VirusTotal API triggered
```

---

##  Dashboard Verification

Go to:

**Wazuh Dashboard → Threat Hunting**

Search:

```text
rule.groups:virustotal
```

---

##  Problems & Fixes

| Problem          | Cause          | Solution            |
| ---------------- | -------------- | ------------------- |
| No alerts        | No FIM events  | Use local directory |
| Copy-paste issue | TTY limitation | Use SSH             |
| Wazuh crash      | Broken XML     | Restore backup      |
| API limit        | Free tier      | Normal behavior     |

---

##  Key Commands

```bash
# Status
sudo systemctl status wazuh-manager

# Logs
sudo tail -f /var/ossec/logs/ossec.log

# Alerts
sudo grep virustotal alerts.json

# Validate config
sudo /var/ossec/bin/wazuh-analysisd -t
```

---

##  Final Status

| Component     | Status |
| ------------- | ------ |
| Wazuh Manager | ✅      |
| Dashboard     | ✅      |
| Agent         | ✅      |
| FIM           | ✅      |
| VirusTotal    | ✅      |
| Alerts        | ✅      |

---

