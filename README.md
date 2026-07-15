# 🛡️ Wazuh SIEM Installation Guide

![OS](https://img.shields.io/badge/OS-Ubuntu%2022.04-orange)
![SIEM](https://img.shields.io/badge/SIEM-Wazuh-blue)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen)
![License](https://img.shields.io/badge/License-MIT-green)
![Maintained](https://img.shields.io/badge/Maintained-Yes-success)

A complete step-by-step guide to install **Wazuh SIEM** on Ubuntu, including Manager, Indexer, and Dashboard setup.

---

## 📌 Overview

Wazuh is an open-source Security Information and Event Management (SIEM) solution used for:

* 🔍 Threat Detection
* 📊 Log Analysis
* 🛡️ Intrusion Detection
* ⚙️ Compliance Monitoring

This guide demonstrates a **single-node deployment** using the official installation script.

---

## 🖥️ System Requirements

| Component | Requirement                   |
| --------- | ----------------------------- |
| OS        | Ubuntu 20.04 / 22.04          |
| RAM       | Minimum 4GB (8GB Recommended) |
| CPU       | 2 Cores+                      |
| Storage   | 20GB+                         |

---

## 🚀 Installation Steps

### 🔹 Step 1: Update System

```bash
sudo apt update && sudo apt upgrade -y
```

---

### 🔹 Step 2: Install Required Packages

```bash
sudo apt install curl apt-transport-https unzip wget -y
```

---

### 🔹 Step 3: Download Wazuh Installer

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
chmod +x wazuh-install.sh
```

---

### 🔹 Step 4: Install Wazuh (All-in-One)

```bash
sudo bash wazuh-install.sh -a
```

> ⏳ Installation may take 10–20 minutes.

---

### 🔐 Step 5: Get Login Credentials

If you miss credentials:

```bash
sudo tar -xvf wazuh-install-files.tar
```

---

### 🌐 Step 6: Access Dashboard

Open your browser:

```
https://<YOUR_SERVER_IP>
```

Example:

```
https://192.168.1.10
```

⚠️ Accept SSL warning if shown.

---

## 🖥️ Service Status Check

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

---

## ➕ Add Wazuh Agent (Optional)

### Install Agent

```bash
curl -sO https://packages.wazuh.com/4.x/apt/wazuh-agent.deb
sudo WAZUH_MANAGER='YOUR_SERVER_IP' dpkg -i wazuh-agent.deb
```

### Start Agent

```bash
sudo systemctl daemon-reexec
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

---

## 🔄 Restart Services

```bash
sudo systemctl restart wazuh-manager
sudo systemctl restart wazuh-indexer
sudo systemctl restart wazuh-dashboard
```

---

## ❌ Troubleshooting

### 🔴 Dashboard Not Opening

```bash
sudo systemctl restart wazuh-dashboard
```

### 🔴 API Connection Failed

```bash
sudo systemctl restart wazuh-manager
```

### 🔴 Open Required Ports

```bash
sudo ufw allow 443/tcp
sudo ufw allow 1514/tcp
sudo ufw allow 1515/tcp
```

---

## 📁 Project Structure

```
wazuh-installation/
├── README.md
├── screenshots/
└── commands.txt
```

---

## 📸 Screenshots (Add Here)

* Dashboard Login Page
* Agent Status
* Alerts View
* System Overview

---

## 🎯 Learning Outcome

After completing this project, you will be able to:

* Install and configure Wazuh SIEM
* Understand SIEM architecture
* Monitor system logs and security events
* Detect potential threats

---

## 💡 Author

👤 Your Name
🎓 Cyber Security Engineering Student

---

## ⭐ Final Note

> This project demonstrates practical implementation of SIEM using Wazuh, showcasing real-world cybersecurity monitoring and threat detection capabilities.
