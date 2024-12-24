---
title: Using Gobuster to Discover Directories
date: 2024-01-07 08:01:35 +0300
subtitle: Product Design
image: '/images/807.png'
---
# Using Gobuster to Discover Directories

## **Objective**
Learn how to use **Gobuster**, a directory and file brute-forcing tool, to discover hidden directories, files, and resources on a web server. This lab demonstrates the process of running scans and analyzing results effectively.

---

## **Purpose**
Directory and file enumeration is an essential part of reconnaissance during penetration testing. Gobuster allows testers to identify hidden or unlinked resources that could expose sensitive information or vulnerabilities.

---

## **Tools Required**
- **Kali Linux** (or any system with Gobuster installed).
- A target web application or server (ensure you have permission to test).

---

## **Lab Topology**
- **Kali Linux**: Running Gobuster for directory discovery.
- **Target Server**: A test web server (e.g., a DVWA instance or an intentionally vulnerable web application).

---

## **Walkthrough**

### **Task 1: Installing Gobuster**
1. **Verify Installation**:
   - Gobuster comes pre-installed on Kali Linux. Verify by running:
     ```bash
     gobuster --help
     ```
   - If not installed, install it with:
     ```bash
     sudo apt update && sudo apt install gobuster -y
     ```

2. **Check Version**:
   - Confirm Gobuster is installed and up-to-date:
     ```bash
     gobuster --version
     ```

---

### **Task 2: Setting Up the Target**
1. **Identify the Target URL**:
   - Choose a target web server (e.g., `http://testsite.local`).

2. **Prepare a Wordlist**:
   - Use the built-in wordlists located in `/usr/share/wordlists`.
   - Example: `/usr/share/wordlists/dirb/common.txt`.

3. **Confirm Target Accessibility**:
   - Ensure the target is reachable by running:
     ```bash
     ping <target_url>
     ```
   - Replace `<target_url>` with the target's domain or IP address.

---

### **Task 3: Running a Basic Directory Scan**
1. **Run Gobuster**:
   - Use the following command to start a basic directory scan:
     ```bash
     gobuster dir -u <target_url> -w <wordlist>
     ```
     - Replace `<target_url>` with the target’s URL.
     - Replace `<wordlist>` with the path to your wordlist (e.g., `/usr/share/wordlists/dirb/common.txt`).

   Example:
   ```bash
   gobuster dir -u http://testsite.local -w /usr/share/wordlists/dirb/common.txt
   ```

2. **Analyze Output**:
   - Gobuster will display discovered directories and their response codes:
     ```
     /admin               (Status: 200)
     /uploads             (Status: 403)
     /backup              (Status: 301)
     ```
   - **Status Codes**:
     - `200`: OK (accessible resource).
     - `301/302`: Redirect (valid resource, redirects to another location).
     - `403`: Forbidden (exists but access is restricted).

---

### **Task 4: Advanced Options**
1. **Use a Custom Extension**:
   - Search for specific file extensions (e.g., `.php`, `.txt`):
     ```bash
     gobuster dir -u <target_url> -w <wordlist> -x <extensions>
     ```
     Example:
     ```bash
     gobuster dir -u http://testsite.local -w /usr/share/wordlists/dirb/common.txt -x php,txt
     ```

2. **Adjust the Number of Threads**:
   - Increase threads for faster scanning:
     ```bash
     gobuster dir -u <target_url> -w <wordlist> -t <threads>
     ```
     Example:
     ```bash
     gobuster dir -u http://testsite.local -w /usr/share/wordlists/dirb/common.txt -t 50
     ```

3. **Save Output to a File**:
   - Save results for later analysis:
     ```bash
     gobuster dir -u <target_url> -w <wordlist> -o <output_file>
     ```
     Example:
     ```bash
     gobuster dir -u http://testsite.local -w /usr/share/wordlists/dirb/common.txt -o results.txt
     ```

4. **Use HTTPS with Insecure Certificates**:
   - If the target uses HTTPS with an invalid certificate:
     ```bash
     gobuster dir -u https://<target_url> -w <wordlist> -k
     ```

---

### **Task 5: Interpreting Results**
1. **Locate Sensitive Directories**:
   - Identify admin pages, backup directories, or other potentially sensitive resources.

2. **Test Discovered Paths**:
   - Use a browser or tool like `curl` to verify and analyze the content of discovered directories.

3. **Correlate Findings**:
   - Combine results with other reconnaissance tools (e.g., Nmap) to build a detailed picture of the target.

---

## **Best Practices**
1. **Respect Authorization**:
   - Only scan targets you own or have explicit permission to test.

2. **Use Appropriate Wordlists**:
   - Tailor wordlists to the target’s context (e.g., web application type).

3. **Combine with Other Tools**:
   - Use Gobuster alongside tools like Nmap or Nikto for comprehensive scanning.

4. **Log Findings**:
   - Always save scan results for documentation and analysis.

---

## **Key Takeaways**
1. Gobuster is a fast and efficient tool for discovering hidden directories and files.
2. Proper wordlist selection significantly impacts the success of directory enumeration.
3. Always follow ethical guidelines when conducting scans.

---

## **Troubleshooting Tips**
1. **No Results**:
   - Ensure the target URL is correct and reachable.
   - Try different wordlists or extensions.

2. **Slow Scans**:
   - Increase the number of threads with `-t`.
   - Use a smaller wordlist for quicker results.

3. **Certificate Errors**:
   - Use the `-k` option to bypass HTTPS certificate validation.

By completing this lab, you now understand how to use Gobuster for directory discovery and how to interpret and act on the results effectively.