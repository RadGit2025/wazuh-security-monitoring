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

!image.png

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

!image.png

!image.png

!image.png

Input IP address and username.

Give the password and the interface will look like this

!image.png
