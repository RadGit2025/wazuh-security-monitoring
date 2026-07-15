Wazuh Installation Guide (Ubuntu 22.04)

A Full guideline of Wazuh installation 

 Environment Setup
 
System Requirements
OS: Ubuntu 22.04 / 20.04
RAM: Minimum 4GB (recommended 8GB)
CPU: 2 cores+
Storage: 16GB+

Step 1: Update System
sudo apt update && sudo apt upgrade -y

Step 2: Required Packages

sudo apt install curl

sudo apt install curl unzip apt-transport-https gnupg -y

Step 3: Wazuh Install
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh

chmod +x wazuh-install.sh

sudo bash wazuh-install.sh -a

Install:

Wazuh Manager
Wazuh Indexer
Wazuh Dashboard

Step 4: Login info save
User: admin

Password: XXXXX
(Must save the password)

Get Dashboard Credentials

Installation শেষে automatically username & password show করবে।

👉 if miss 

sudo tar -xvf wazuh-install-files.tar

Step 5: Get Dashboard Credentials

Installation শেষে automatically username & password show করবে।

Step 6: Access Wazuh Dashboard

Go to Browser

https://<YOUR_SERVER_IP>

👉 Example:

https://127.0.0.1

Step 7: Check Services Status
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard

 Step 8: Add Agent (Optional)

Agent install (Ubuntu):

curl -sO https://packages.wazuh.com/4.x/apt/wazuh-agent.deb
sudo WAZUH_MANAGER='YOUR_SERVER_IP' dpkg -i wazuh-agent.deb

 Start agent:

sudo systemctl daemon-reexec

sudo systemctl enable wazuh-agent

sudo systemctl start wazuh-agent

Step 9: Restart Services (If Needed)

sudo systemctl restart wazuh-manager
sudo systemctl restart wazuh-indexer
sudo systemctl restart wazuh-dashboard
