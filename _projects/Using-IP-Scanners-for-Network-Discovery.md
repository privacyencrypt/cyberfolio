---
title: Using IP Scanners for Network Discovery
date: 2023-12-06 08:01:35 +0300
subtitle: Product Design
image: '/images/523.png'
---
# Using IP Scanners for Network Discovery

## **Objective**
Learn how to use IP scanning tools to discover devices on a network, identify open ports, and gather basic network information. This lab covers tools like **Angry IP Scanner** and **Nmap**, highlighting their usage for network discovery and reconnaissance.

---

## **Prerequisites**
1. **IP Scanner Installed**:
   - **Angry IP Scanner**:
     - Download from [https://angryip.org](https://angryip.org).
     - Install and configure according to your operating system.
   - **Nmap**:
     - Install via your package manager:
       ```bash
       sudo apt update && sudo apt install nmap
       ```

2. **Basic Networking Knowledge**:
   - Familiarity with IP ranges, subnets, and ports.

3. **Testing Environment**:
   - Use a local or controlled network for scanning.
   - **Ensure explicit permission** to scan any network.

---

## **Step 1: Understanding IP Scanning**
1. **What is IP Scanning?**
   - IP scanners identify devices on a network by probing IP addresses within a specified range.

2. **Key Uses**:
   - Discover active devices.
   - Identify open ports and services.
   - Detect unauthorized devices on the network.

---

## **Step 2: Scanning with Angry IP Scanner**
1. **Launch Angry IP Scanner**:
   - Open the application after installation.

2. **Set the IP Range**:
   - Enter the range to scan (e.g., `192.168.1.1-192.168.1.254`).
   - **Tip**: Use `CIDR` notation for larger ranges (e.g., `192.168.1.0/24`).

3. **Configure Scan Options**:
   - Open the **Tools > Preferences** menu.
   - Set options for scanning ping, ports, and additional details.

4. **Start the Scan**:
   - Click the **Start** button.
   - Observe the list of devices as they are discovered.

5. **Analyze the Results**:
   - IP Address: Lists all discovered devices.
   - Hostname: Resolves the name of the device (if available).
   - Open Ports: Displays ports with active services.

6. **Export the Results** (Optional):
   - Go to **File > Save As** to export the scan data in CSV or text format.

---

## **Step 3: Scanning with Nmap**
1. **Basic Network Discovery**:
   - Run the following command to scan an IP range:
     ```bash
     nmap -sn 192.168.1.0/24
     ```
   - **`-sn`**: Performs a ping scan to discover live hosts.

2. **Port Scanning**:
   - Scan for open ports on live hosts:
     ```bash
     nmap 192.168.1.0/24
     ```
   - **Insight**: This reveals which ports are open and potentially exploitable.

3. **Service and Version Detection**:
   - Use the `-sV` flag to detect services and versions:
     ```bash
     nmap -sV 192.168.1.0/24
     ```

4. **OS Detection**:
   - Use the `-O` flag to detect operating systems:
     ```bash
     nmap -O 192.168.1.0/24
     ```

5. **Combining Options**:
   - Perform a detailed scan with multiple options:
     ```bash
     nmap -sC -sV -O 192.168.1.0/24
     ```
   - **`-sC`**: Runs default scripts for additional information.

---

## **Step 4: Analyzing Scan Results**
1. **Active Hosts**:
   - Identify devices that responded to the scan.

2. **Open Ports**:
   - Review ports for services like HTTP (80), SSH (22), or RDP (3389).

3. **Hostname and MAC Address**:
   - Use this information to identify devices and their manufacturers.

4. **Potential Vulnerabilities**:
   - Cross-reference open ports and services with known vulnerabilities.

---

## **Step 5: Ethical and Legal Considerations**
1. **Permission Required**:
   - Only scan networks you own or have explicit authorization to scan.

2. **Impact on Network Performance**:
   - Large scans can generate significant traffic; use cautiously on production networks.

3. **Data Security**:
   - Ensure scan results are stored securely and shared only with authorized personnel.

---

## **Step 6: Troubleshooting Common Issues**
1. **No Devices Found**:
   - Verify the IP range and ensure devices are powered on.
   - Check for firewalls blocking ICMP or scan traffic.

2. **Scan Too Slow**:
   - Reduce the scan intensity or narrow the IP range.
   - Use Nmap’s `-T4` flag for faster scanning:
     ```bash
     nmap -T4 192.168.1.0/24
     ```

3. **Incomplete Results**:
   - Run scans as a superuser (use `sudo` on Linux).
   - Ensure the scanning tool has sufficient permissions.

---

## **Additional Tips and Insights**
1. **Combine Tools**:
   - Use Angry IP Scanner for quick scans and Nmap for detailed analysis.

2. **Automate Scans**:
   - Schedule periodic scans with scripts to monitor for new devices.

3. **Export and Visualize Data**:
   - Use tools like Excel or Splunk to analyze exported scan results.

4. **Enhance Accuracy**:
   - For Nmap, include the `-Pn` flag to skip ping checks on firewalled hosts.

---

## **Key Takeaways**
1. IP scanners are essential for discovering and auditing network devices.
2. Understanding scan results helps identify potential security risks and misconfigurations.
3. Always follow ethical guidelines and use scanning tools responsibly.
