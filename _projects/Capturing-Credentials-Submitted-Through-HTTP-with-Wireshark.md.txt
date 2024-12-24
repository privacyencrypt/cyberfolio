---
title: Capturing Credentials Submitted Through HTTP with Wireshark
date: 2024-01-20 08:01:35 +0300
subtitle: Product Design
image: '/images/820.png'
---
# Capturing Credentials Submitted Through HTTP with Wireshark

## **Objective**
Learn how to use **Wireshark**, a network protocol analyzer, to capture and analyze HTTP traffic to identify credentials sent in plaintext. Understand how to secure networks and applications against such vulnerabilities.

---

## **Purpose**
HTTP traffic is often transmitted in plaintext, making it susceptible to interception. Wireshark enables you to analyze network traffic to demonstrate how attackers can capture sensitive information, such as login credentials, and how to mitigate such risks.

---

## **Tools Required**
- **Kali Linux** (or any system with Wireshark installed).
- A test network or environment where HTTP traffic can be captured.
- A web application that transmits credentials via HTTP (test environment only).

---

## **Lab Topology**
- **Kali Linux**: Running Wireshark to capture network traffic.
- **Target Web Application**: A test website transmitting credentials over HTTP.

---

## **Walkthrough**

### **Task 1: Setting Up Wireshark**
1. **Verify Installation**:
   - Wireshark is pre-installed on Kali Linux. To check:
     ```bash
     wireshark --version
     ```
   - If not installed, install it:
     ```bash
     sudo apt update && sudo apt install wireshark -y
     ```

2. **Run Wireshark as Root**:
   - Start Wireshark:
     ```bash
     sudo wireshark
     ```
   - Select the network interface connected to the target environment (e.g., `eth0` or `wlan0`).

---

### **Task 2: Capturing HTTP Traffic**
1. **Set a Capture Filter**:
   - Use the following capture filter to focus on HTTP traffic:
     ```
     tcp port 80
     ```
   - Enter the filter in the **Capture Filter** field and start the capture.

2. **Generate HTTP Traffic**:
   - On the target machine, navigate to a test website over HTTP (e.g., `http://testsite.local`).
   - Submit a login form with test credentials (e.g., `username: admin`, `password: password123`).

3. **Stop the Capture**:
   - In Wireshark, click the **Stop** button once the traffic has been generated.

---

### **Task 3: Analyzing Captured Traffic**
1. **Filter HTTP Requests**:
   - Use the display filter to show HTTP requests:
     ```
     http
     ```

2. **Locate the Login Request**:
   - Look for POST requests to the login endpoint (e.g., `/login`).
   - Select the relevant packet and view its details in the **Packet Details** pane.

3. **Extract Credentials**:
   - Expand the **Hypertext Transfer Protocol** section to find the form data.
   - Example output:
     ```
     username=admin&password=password123
     ```

---

### **Task 4: Securing HTTP Traffic**
1. **Use HTTPS**:
   - Configure web servers to enforce HTTPS using SSL/TLS certificates.

2. **Implement Secure Authentication**:
   - Use secure mechanisms such as hashed passwords and token-based authentication.

3. **Disable Plaintext Protocols**:
   - Prevent the use of HTTP by redirecting all traffic to HTTPS.

4. **Monitor Networks**:
   - Use intrusion detection systems (IDS) to flag unencrypted traffic containing sensitive data.

---

## **Best Practices**
1. **Use Authorized Environments Only**:
   - Perform traffic analysis only in environments where you have explicit permission.

2. **Encrypt All Traffic**:
   - Ensure all sensitive data is transmitted over secure channels (e.g., HTTPS, VPNs).

3. **Educate Users**:
   - Train users to recognize insecure connections and avoid submitting credentials over HTTP.

4. **Regularly Audit Systems**:
   - Conduct routine checks to ensure encryption is enforced across all endpoints.

---

## **Key Takeaways**
1. HTTP traffic is transmitted in plaintext, making it vulnerable to interception.
2. Wireshark is a powerful tool for demonstrating the risks of unencrypted communications.
3. Enforcing HTTPS and secure practices mitigates the risk of credential interception.

---

## **Troubleshooting Tips**
1. **No Traffic Captured**:
   - Ensure the correct network interface is selected.
   - Verify that HTTP traffic is being generated in the test environment.

2. **Cannot Find Credentials**:
   - Confirm the form data is transmitted over HTTP and not HTTPS.
   - Check the application’s network behavior to ensure it does not encrypt data.

3. **Wireshark Permission Issues**:
   - Run Wireshark as root or ensure your user has permissions to capture packets.

By completing this lab, you now understand how to capture and analyze HTTP traffic for credentials and how to secure networks against such vulnerabilities.