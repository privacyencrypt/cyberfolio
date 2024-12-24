---
title: Automate WordPress Scanning with WPScan
date: 2024-01-04 08:01:35 +0300
subtitle: Product Design
image: '/images/804.png'
---
# Automate WordPress Scanning with WPScan

## **Objective**
Learn how to use **WPScan**, a WordPress vulnerability scanner, to identify security issues in WordPress installations. This lab guides you through installing WPScan, configuring it, and running scans to detect potential vulnerabilities.

---

## **Purpose**
WordPress powers a significant percentage of websites globally, making it a frequent target for attackers. WPScan is a specialized tool for identifying vulnerabilities in WordPress sites, including:
- Outdated plugins and themes.
- Weak usernames and passwords.
- WordPress core vulnerabilities.

---

## **Tools Required**
- **Kali Linux** (or any Linux distribution with WPScan installed).
- A WordPress site to test (ensure you have permission to scan the site).

---

## **Lab Topology**
- **Kali Linux**: Running WPScan for vulnerability scanning.
- **Target WordPress Site**: A test WordPress site hosted locally or remotely.

---

## **Walkthrough**

### **Task 1: Installing WPScan**
1. **Update Your System**:
   - Ensure your system is up to date:
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

2. **Install WPScan**:
   - Install WPScan using the package manager:
     ```bash
     sudo apt install wpscan -y
     ```

3. **Verify Installation**:
   - Check that WPScan is installed correctly:
     ```bash
     wpscan --version
     ```
     Example output:
     ```
     WPScan 3.8.16
     ```

---

### **Task 2: Configuring WPScan**
1. **API Token Setup**:
   - WPScan requires an API token for full functionality, including plugin and theme vulnerability detection.
   - Sign up for an account at [https://wpscan.com](https://wpscan.com).
   - Copy your API token from your account.

2. **Set the API Token**:
   - Use the following command to configure WPScan with your API token:
     ```bash
     export WPSCAN_API_TOKEN=<your_api_token>
     ```
   - Replace `<your_api_token>` with the token you copied from the WPScan website.

---

### **Task 3: Running a Basic Scan**
1. **Identify the Target**:
   - Determine the URL of the WordPress site to scan (e.g., `http://testsite.local`).

2. **Run the Scan**:
   ```bash
   wpscan --url <target_url>
   ```
   Example:
   ```bash
   wpscan --url http://testsite.local
   ```

3. **Analyze Output**:
   - WPScan will enumerate basic information about the WordPress installation, including version details and vulnerabilities.

---

### **Task 4: Enumerating WordPress Users**
1. **Enumerate Users**:
   - To list usernames for the WordPress site:
     ```bash
     wpscan --url <target_url> --enumerate u
     ```
     Example:
     ```bash
     wpscan --url http://testsite.local --enumerate u
     ```

2. **Analyze Output**:
   - Review the list of discovered usernames.
   - Use this information to assess potential weak credentials.

---

### **Task 5: Enumerating Plugins and Themes**
1. **Enumerate Plugins**:
   ```bash
   wpscan --url <target_url> --enumerate p
   ```
   Example:
   ```bash
   wpscan --url http://testsite.local --enumerate p
   ```

2. **Enumerate Themes**:
   ```bash
   wpscan --url <target_url> --enumerate t
   ```
   Example:
   ```bash
   wpscan --url http://testsite.local --enumerate t
   ```

3. **Analyze Vulnerabilities**:
   - WPScan will display outdated or vulnerable plugins/themes with associated CVEs (Common Vulnerabilities and Exposures).

---

### **Task 6: Testing for Weak Passwords**
1. **Prepare a Wordlist**:
   - Use a pre-installed wordlist (e.g., `/usr/share/wordlists/rockyou.txt`) or create your own.

2. **Run the Password Attack**:
   ```bash
   wpscan --url <target_url> --enumerate u --passwords <wordlist_path>
   ```
   Example:
   ```bash
   wpscan --url http://testsite.local --enumerate u --passwords /usr/share/wordlists/rockyou.txt
   ```

3. **Analyze Results**:
   - WPScan will attempt to log in with the discovered usernames and passwords from the wordlist.
   - Review the output for any successful attempts.

---

## **Best Practices**
1. **Use Authorized Targets**:
   - Only scan WordPress sites you own or have explicit permission to test.

2. **Update WPScan Regularly**:
   - Ensure WPScan and its vulnerability database are up to date:
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

3. **Prioritize Remediation**:
   - Address vulnerabilities in plugins, themes, and weak passwords first.

4. **Schedule Regular Scans**:
   - Automate scans to ensure ongoing security monitoring.

---

## **Key Takeaways**
1. WPScan is a specialized tool for identifying WordPress vulnerabilities.
2. Enumerating plugins, themes, and users helps pinpoint specific security risks.
3. Always follow ethical guidelines and perform scans in a controlled environment.

---

## **Troubleshooting Tips**
1. **Connection Errors**:
   - Ensure the target URL is accessible from your machine.
   - Verify network connectivity and resolve DNS issues if needed.

2. **Missing API Token**:
   - Ensure your API token is set correctly using:
     ```bash
     echo $WPSCAN_API_TOKEN
     ```

3. **Incomplete Results**:
   - Check that the target WordPress site has public access to its `/wp-json` and other directories.

4. **Permission Denied**:
   - Use `sudo` if encountering permission issues during installation or execution.