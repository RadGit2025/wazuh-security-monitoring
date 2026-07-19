# Fix ssh connection

- ✅ Wazuh Dashboard is running.
- ✅ Wazuh Indexer is running.
- ❌ Wazuh Manager failed.
- ❌ SSH is not working, so you cannot connect using MobaXterm, PuTTY, or WinSCP.

Let's fix the SSH server first.

---

# Step 1: Check if SSH is installed

On the Ubuntu server console, run:

```bash
dpkg-l |grep openssh-server
```

If nothing appears, install it:

```bash
sudo apt update
sudo apt install openssh-server-y
```

or

```bash
sudo systemctl status sshd
```

Output:

If it says **inactive** or **failed**, run:

```bash
sudo systemctlstartssh
```

Then

```bash
sudo systemctl enablessh
```

if it is active

 `sudo systemctl status ssh` shows **active (running)**, that's good news—it means the SSH service itself is running.

The problem is likely one of these:

- SSH isn't listening on the expected interface or port.
- The VM's network configuration is preventing access.
- A firewall is blocking port 22.
- You're using the wrong IP address in MobaXterm/PuTTY.

Please run these commands **one by one** 

```bash
hostname-I
```

```bash
sudo ss-tlnp |grep :22
```

```bash
ip a
```

```bash
sudo ufw status
```

<img width="887" height="610" alt="image" src="https://github.com/user-attachments/assets/504fd4d6-71c0-4324-b43b-7683b6384434" />


✅ **SSH service is running**

```
Started OpenBSD Secure Shell server.
```

✅ **SSH is listening on port 22**

```
0.0.0.0:22
[::]:22
```

✅ **Firewall is disabled**

```
Status: inactive
```

✅ **Your Ubuntu IP address is**

```
10.0.2.15
```

So the SSH server is working correctly

lets check network configuration

### Use a Bridged Adapter

1. Power off your Ubuntu VM.
2. Open VirtualBox → Settings → Network → Adapter 1
3. Set:
    - **Attached to:** `Bridged Adapter`
    - **Name:** Select your Wi-Fi or Ethernet adapter.
4. Start the VM.
5. Run:
    
    ```bash
    hostname-I
    ```
    
6. You'll get an IP like `192.168.1.xxx`.
7. Use that IP in MobaXterm or PuTTY.

Connect ubuntu server with Mobaxterm (ssh connection)

 IP like `192.168.x.xxx`

Go to session and click it
<img width="1137" height="220" alt="image" src="https://github.com/user-attachments/assets/9ab916d5-a0e3-4890-88ce-1b63ff5f5b88" />

<img width="1011" height="672" alt="image" src="https://github.com/user-attachments/assets/89e2944c-784c-4d9e-a8a9-bb12b09fab02" />

<img width="991" height="566" alt="image" src="https://github.com/user-attachments/assets/4b33dad6-ab2f-4c9a-81ad-fc125c1e79f5" />


Input IP address and username.

<img width="1157" height="1035" alt="image" src="https://github.com/user-attachments/assets/2c232bc3-d543-4280-82b4-129715b87b47" />


Give the password and the interface will look like this


