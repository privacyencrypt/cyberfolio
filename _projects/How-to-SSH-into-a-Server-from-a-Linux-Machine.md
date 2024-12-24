---
title: How to SSH into a Server from a Linux Machine
date: 2023-12-14 08:01:35 +0300
subtitle: Product Design
image: '/images/414.png'
---
# How to SSH into a Server from a Linux Machine

## **Objective**
Learn how to use the **ssh** command on a Linux machine to securely connect to a remote server. This lab covers the steps for establishing an SSH connection, setting up key-based authentication, and troubleshooting common issues.

---

## **Prerequisites**
1. **Linux Environment**:
   - Ensure the `ssh` command is available by running:
     ```bash
     ssh -V
     ```
   - If not installed, install OpenSSH client:
     ```bash
     sudo apt update && sudo apt install openssh-client
     ```

2. **Access to a Remote Server**:
   - You will need the following:
     - **Server IP/Hostname**: e.g., `192.168.1.100` or `example.com`.
     - **Username**: e.g., `admin`.
     - **Password** or **Private Key** for authentication.
   - Ensure SSH is enabled on the server.

3. **Basic Understanding of SSH**:
   - SSH (Secure Shell) is used for secure communication between systems over a network.

---

## **Step 1: Establishing a Basic SSH Connection**
1. Open a terminal on your Linux machine.
2. Use the following command to connect:
   ```bash
   ssh <username>@<server_ip>
   ```
   - Replace `<username>` with your server username (e.g., `admin`).
   - Replace `<server_ip>` with the server’s IP or hostname (e.g., `192.168.1.100`).

3. If prompted, accept the server’s SSH key:
   ```
   The authenticity of host '192.168.1.100 (192.168.1.100)' can't be established.
   RSA key fingerprint is ...
   Are you sure you want to continue connecting (yes/no)?
   ```
   - Type `yes` and press **Enter**.

4. Enter your password when prompted:
   - After successful authentication, you will gain access to the server’s command line.

---

## **Step 2: Using Key-Based Authentication (Optional)**
### **Generating an SSH Key Pair**
1. On your Linux machine, generate an SSH key pair:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```
   - **`-t rsa`**: Specifies the RSA algorithm.
   - **`-b 4096`**: Sets the key size to 4096 bits for stronger security.

2. Save the key when prompted:
   - Press **Enter** to use the default location (`~/.ssh/id_rsa`).
   - Optionally, set a passphrase for added security.

### **Copying the Public Key to the Server**
1. Use the following command to copy your public key to the server:
   ```bash
   ssh-copy-id <username>@<server_ip>
   ```
   - Replace `<username>` and `<server_ip>` with your credentials.

2. Enter your password when prompted.
3. Verify the key was added:
   - Check the server’s `~/.ssh/authorized_keys` file to ensure your key is listed.

### **Connecting Using the Private Key**
1. SSH into the server using your private key:
   ```bash
   ssh <username>@<server_ip>
   ```
2. If a passphrase was set for your key, enter it when prompted.

---

## **Step 3: Advanced SSH Options**
1. **Specifying a Port**:
   - If the server uses a non-default SSH port, specify it with the `-p` option:
     ```bash
     ssh -p <port> <username>@<server_ip>
     ```
   - Replace `<port>` with the custom port number.

2. **Verbose Mode**:
   - Use the `-v` flag to see detailed output for debugging:
     ```bash
     ssh -v <username>@<server_ip>
     ```

3. **Running a Single Command**:
   - Execute a command on the server without starting an interactive session:
     ```bash
     ssh <username>@<server_ip> "<command>"
     ```
     - Example:
       ```bash
       ssh admin@192.168.1.100 "ls -l /var/www"
       ```

4. **Using Config Files**:
   - Simplify frequent connections by editing the SSH configuration file:
     ```bash
     nano ~/.ssh/config
     ```
     - Add the following:
       ```
       Host myserver
           HostName 192.168.1.100
           User admin
           Port 22
       ```
     - Connect using:
       ```bash
       ssh myserver
       ```

---

## **Step 4: Troubleshooting Common Issues**
1. **Connection Timeout**:
   - Ensure the server is online and reachable.
   - Verify the server firewall allows SSH traffic (default port `22`).

2. **Permission Denied**:
   - Double-check your username and password.
   - If using a key, ensure it matches the server’s authorized keys.

3. **Host Key Changed**:
   - If the server’s SSH key changes, you’ll see a warning:
     ```
     WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
     ```
   - Resolve this by editing the `~/.ssh/known_hosts` file and removing the old entry:
     ```bash
     nano ~/.ssh/known_hosts
     ```

4. **Unable to Resolve Hostname**:
   - Verify the hostname is correct and DNS is functioning.
   - Use the server’s IP address if necessary.

---

## **Step 5: Securing Your SSH Server**
1. **Disable Password Authentication**:
   - On the server, edit the SSH configuration file:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
   - Set the following:
     ```
     PasswordAuthentication no
     ```
   - Restart the SSH service:
     ```bash
     sudo systemctl restart ssh
     ```

2. **Change the Default Port**:
   - Edit the SSH configuration file and change the `Port` value:
     ```bash
     Port 2222
     ```
   - Restart the SSH service.

3. **Use Fail2Ban**:
   - Install and configure Fail2Ban to block repeated login attempts.

---

## **Additional Tips and Insights**
1. **Command-Line Proficiency**:
   - Learn basic Linux commands to effectively use the server once connected.

2. **Combine Tools**:
   - Use tools like SCP or Rsync alongside SSH for secure file transfers.

3. **Logging**:
   - Check the server’s SSH logs for troubleshooting:
     ```bash
     sudo tail -f /var/log/auth.log
     ```

4. **Multi-Factor Authentication**:
   - Enable MFA for an additional layer of security.

---

## **Key Takeaways**
1. The `ssh` command is a powerful tool for securely accessing remote servers from Linux.
2. Understanding key-based authentication enhances both security and convenience.
3. Regularly review and secure your SSH configurations to prevent unauthorized access.
