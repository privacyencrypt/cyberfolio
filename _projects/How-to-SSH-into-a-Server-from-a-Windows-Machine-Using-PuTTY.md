---
title: How to SSH into a Server from a Windows Machine Using PuTTY
date: 2023-12-13 08:01:35 +0300
subtitle: Product Design
image: '/images/413.png'
---
# How to SSH into a Server from a Windows Machine Using PuTTY

## **Objective**
Learn how to use **PuTTY**, a free SSH client for Windows, to securely connect to a remote server. This lab covers setting up PuTTY, establishing an SSH connection, and troubleshooting common issues.

---

## **Prerequisites**
1. **PuTTY Installed on Your Windows Machine**:
   - Download PuTTY from the official website: [https://www.putty.org](https://www.putty.org).
   - Install PuTTY by running the downloaded `.msi` file and following the on-screen instructions.

2. **Access to a Remote Server**:
   - Ensure you have the following information:
     - **Server IP/Hostname**: e.g., `192.168.1.100` or `example.com`.
     - **Username**: e.g., `admin`.
     - **Password** or **Private Key** for authentication.
   - SSH must be enabled on the remote server.

3. **Basic Understanding of SSH**:
   - SSH (Secure Shell) is a protocol for securely accessing and managing remote systems over a network.

---

## **Step 1: Launching PuTTY**
1. Open PuTTY from the Start Menu or search for `PuTTY` in the Windows search bar.
2. You’ll see the PuTTY Configuration window, which is the central hub for setting up SSH connections.

---

## **Step 2: Configuring the SSH Connection**
1. In the **Session** category:
   - **Host Name (or IP address)**: Enter the server’s IP or hostname (e.g., `192.168.1.100`).
   - **Port**: Use `22` (default SSH port).
   - **Connection Type**: Ensure **SSH** is selected.

2. Save the configuration for future use:
   - Enter a name in the **Saved Sessions** field (e.g., `MyServer`).
   - Click **Save**.

   **Tip**: Saved sessions allow you to quickly reconnect without re-entering details.

---

## **Step 3: Establishing the Connection**
1. Click **Open** to start the SSH session.
2. A terminal window will open, and you may see a security alert:
   - If it’s your first time connecting to the server, PuTTY will display the server’s SSH fingerprint.
   - Click **Yes** to accept and save the server’s key.

3. Enter your credentials:
   - **Username**: Type your username and press **Enter**.
   - **Password**: Enter your password (you won’t see it displayed) and press **Enter**.

   **Insight**: Once authenticated, you’ll see a command-line interface for the remote server.

---

## **Step 4: Using SSH with Key-Based Authentication (Optional)**
1. Generate an SSH key pair (if not already done):
   - Use a tool like PuTTYgen (bundled with PuTTY) to create a key pair:
     - Open PuTTYgen and click **Generate**.
     - Move your mouse randomly in the blank area to generate entropy.
     - Save the private key (e.g., `mykey.ppk`) and copy the public key.

2. Add the public key to the server:
   - Append the public key to the `~/.ssh/authorized_keys` file on the remote server.

3. Configure PuTTY to use the private key:
   - In the **Connection > SSH > Auth** category, click **Browse** under **Private key file for authentication**.
   - Select your `.ppk` file.

4. Save the session and connect.

---

## **Step 5: Troubleshooting Common Issues**
1. **Connection Timeout**:
   - Ensure the server is online and accessible.
   - Verify the server’s firewall allows SSH traffic on port `22`.

2. **Authentication Failed**:
   - Double-check your username and password.
   - If using a key, ensure it matches the public key on the server.

3. **Server Host Key Changed**:
   - If you see a warning about a changed host key, it may indicate a security risk.
   - Confirm with the server administrator before proceeding.

4. **Unable to Resolve Hostname**:
   - Ensure the server hostname is correct and DNS is functioning.
   - Use the IP address if the hostname fails.

---

## **Step 6: Closing the Session**
1. To end the SSH session, type:
   ```bash
   exit
   ```
2. Close the PuTTY terminal window.

---

## **Additional Tips and Insights**
1. **Command-Line Basics**:
   - Learn basic Linux commands to navigate and interact with the server.

2. **Port Forwarding**:
   - Use PuTTY for SSH tunneling or port forwarding by configuring the **Connection > SSH > Tunnels** settings.

3. **Secure the Server**:
   - Disable password authentication and use key-based authentication for enhanced security.
   - Restrict SSH access to specific IPs or use a non-default port.

4. **Logging Sessions**:
   - Save session logs for auditing:
     - Go to **Session > Logging** and configure logging options before connecting.

---

## **Key Takeaways**
1. PuTTY is a versatile tool for SSH access to remote servers from Windows machines.
2. Understanding basic PuTTY configurations and SSH concepts ensures secure and efficient connections.
3. Troubleshooting skills are essential for resolving common connectivity issues.
