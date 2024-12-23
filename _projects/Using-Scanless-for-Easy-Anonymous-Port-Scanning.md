---
title: Using Scanless for Easy Anonymous Port Scanning
date: 2023-12-26 08:01:35 +0300
subtitle: Product Design
image: '/images/526.png'
---
# Using Scanless for Easy Anonymous Port Scanning

## **Objective**
Learn how to use **Scanless**, a tool for performing anonymous port scans by leveraging third-party online scanning services. This lab covers installation, usage, and ethical considerations for utilizing Scanless.

---

## **Prerequisites**
1. **Linux or macOS Environment**:
   - Scanless can also run on Windows via WSL (Windows Subsystem for Linux).

2. **Python Installed**:
   - Verify Python is installed:
     ```bash
     python3 --version
     ```
   - If not installed, install Python via your package manager:
     ```bash
     sudo apt update && sudo apt install python3 python3-pip
     ```

3. **Scanless Installed**:
   - Install Scanless using pip:
     ```bash
     pip3 install scanless
     ```
   - Verify the installation:
     ```bash
     scanless --help
     ```

4. **Ethical Considerations**:
   - Only scan systems you own or have explicit permission to test.

---

## **Step 1: Understanding Scanless**
1. **What is Scanless?**
   - Scanless acts as a wrapper for online port scanning services.
   - It allows you to perform port scans anonymously by using external tools.

2. **Why Use Scanless?**
   - Avoids exposing your IP address when performing port scans.
   - Convenient for quick, remote scans.

3. **Common Use Cases**:
   - Testing firewalls and security configurations.
   - Verifying open ports on external systems.

---

## **Step 2: Performing a Basic Scan**
1. Use the following command to scan a target:
   ```bash
   scanless -t <target>
   ```
   - Replace `<target>` with the target’s domain or IP address (e.g., `example.com`).

2. Example:
   ```bash
   scanless -t 192.168.1.1
   ```

3. Review the output:
   - The tool will query online scanning services and display open ports for the target.

---

## **Step 3: Listing Available Scanners**
1. Display all supported online scanning services:
   ```bash
   scanless --list
   ```
2. Example Output:
   ```
   Available scanners:
   - hackertarget
   - yougetsignal
   - viewdns
   ```

3. Use the service name for targeted scans.

---

## **Step 4: Using a Specific Scanner**
1. Specify a scanner with the `-s` option:
   ```bash
   scanless -t <target> -s <scanner>
   ```
   - Replace `<scanner>` with the desired service (e.g., `hackertarget`).

2. Example:
   ```bash
   scanless -t example.com -s hackertarget
   ```

3. Review the results specific to that service.

---

## **Step 5: Outputting Results to a File**
1. Save the results to a file using the `-o` option:
   ```bash
   scanless -t <target> -o <filename>
   ```
   - Replace `<filename>` with the desired file name (e.g., `scan_results.txt`).

2. Example:
   ```bash
   scanless -t example.com -o results.txt
   ```

3. View the saved results:
   ```bash
   cat results.txt
   ```

---

## **Step 6: Troubleshooting**
1. **Scanless Not Found**:
   - Ensure Python and pip are installed and properly configured.
   - Reinstall Scanless:
     ```bash
     pip3 install --upgrade scanless
     ```

2. **No Results**:
   - Verify the target is reachable.
   - Use a different scanner if the current one fails.

3. **Network Issues**:
   - Ensure your system has internet access, as Scanless relies on online services.

---

## **Step 7: Ethical and Legal Considerations**
1. **Permission Required**:
   - Only scan systems you own or have explicit authorization to test.

2. **Respect Privacy**:
   - Do not use Scanless to scan systems without consent.

3. **Minimize Exposure**:
   - Avoid scanning high-profile targets to prevent unwanted attention.

---

## **Additional Tips and Insights**
1. **Combine with Other Tools**:
   - Use Scanless alongside Nmap or Metasploit for comprehensive testing.

2. **Monitor Service Status**:
   - Online scanning services may experience downtime. Try multiple services if one fails.

3. **Automate Scans**:
   - Write scripts to automate periodic scans and save results for analysis.

4. **Security Awareness**:
   - Understand that online scanning services may log your queries.

---

## **Key Takeaways**
1. Scanless simplifies anonymous port scanning by leveraging external services.
2. Understanding its usage and limitations ensures effective and ethical scanning.
3. Always follow ethical guidelines and obtain permission before scanning any system.
