---
title: Using netstat to View Networking Information
date: 2023-12-21 08:01:35 +0300
subtitle: Product Design
image: '/images/521.png'
---
# Using netstat to View Networking Information

## **Objective**
Learn how to use the **netstat** command to monitor and troubleshoot network connections, routing tables, and interface statistics on your system.

---

## **Prerequisites**
1. **Operating System**:
   - Available on Linux, macOS, and Windows.
   - Ensure you have administrative privileges for certain commands.

2. **Basic Networking Knowledge**:
   - Familiarity with IP addresses, ports, protocols (TCP/UDP), and routing.

3. **Access to a Command-Line Interface**:
   - Open a terminal (Linux/macOS) or Command Prompt/PowerShell (Windows).

---

## **Step 1: Viewing Active Network Connections**
1. Open a terminal or Command Prompt.
2. Run the basic `netstat` command:
   ```bash
   netstat
   ```
   - Displays active TCP connections on the system.
3. Analyze the output:
   - **Proto**: Protocol (e.g., TCP, UDP).
   - **Local Address**: IP address and port of the local system.
   - **Foreign Address**: IP address and port of the remote system.
   - **State**: Status of the connection (e.g., ESTABLISHED, LISTENING).

---

## **Step 2: Displaying Listening Ports**
1. Show listening ports:
   ```bash
   netstat -l
   ```
   - Lists all listening TCP and UDP ports.
2. For more detailed output, include numeric addresses:
   ```bash
   netstat -ln
   ```

3. On Windows, use:
   ```cmd
   netstat -an
   ```

   **Insight**: Listening ports indicate services awaiting incoming connections.

---

## **Step 3: Including Process Information**
1. View processes associated with network connections:
   ```bash
   netstat -p
   ```
   - Requires administrative privileges.

2. On Linux:
   ```bash
   sudo netstat -tulnp
   ```
   - **`-t`**: Show TCP connections.
   - **`-u`**: Show UDP connections.
   - **`-n`**: Display numeric addresses.
   - **`-p`**: Include process IDs and names.

3. On Windows:
   ```cmd
   netstat -o
   ```
   - Shows the PID (Process Identifier) of each connection.

---

## **Step 4: Monitoring Network Statistics**
1. Display protocol-specific statistics:
   ```bash
   netstat -s
   ```
   - Lists packet counts, errors, and protocol-specific information (e.g., TCP retransmissions).

2. Filter statistics for specific protocols:
   - Linux:
     ```bash
     netstat -st
     ```
   - Windows:
     ```cmd
     netstat -sp tcp
     ```

   **Tip**: Use this to analyze network health and troubleshoot issues.

---

## **Step 5: Viewing Routing Tables**
1. Show the routing table:
   ```bash
   netstat -r
   ```
   - Displays the system's routing table, including destination networks, gateways, and interfaces.

2. Analyze the output:
   - **Destination**: Target network or host.
   - **Gateway**: Router used to reach the destination.
   - **Iface**: Network interface handling the traffic.

3. On Windows, include:
   ```cmd
   route print
   ```
   - Similar output but includes additional details.

---

## **Step 6: Real-Time Monitoring**
1. Continuously monitor network connections:
   - Linux:
     ```bash
     watch -n 2 netstat -tulnp
     ```
     - Refreshes every 2 seconds.
   - macOS:
     ```bash
     netstat -w 2
     ```
   - Windows:
     ```cmd
     netstat -e 5
     ```
     - Refreshes every 5 seconds.

2. Analyze changing states, new connections, or high traffic rates.

---

## **Step 7: Troubleshooting with netstat**
1. **Identify Open Ports**:
   - Use `netstat -l` to find services listening for incoming connections.
   - Cross-check with application logs to verify legitimacy.

2. **Diagnose High Traffic**:
   - Use `netstat -tulnp` to identify processes generating excessive traffic.

3. **Detect Unauthorized Access**:
   - Review foreign addresses for unusual connections.

4. **Verify Service Availability**:
   - Confirm critical services are listening on expected ports.

---

## **Additional Tips and Insights**
1. **Filter Output**:
   - Combine `netstat` with `grep` (Linux/macOS) or `findstr` (Windows) for targeted queries.
     - Example:
       ```bash
       netstat -tulnp | grep 80
       ```

2. **Use Modern Alternatives**:
   - On Linux, use `ss` for enhanced features and faster output:
     ```bash
     ss -tulnp
     ```

3. **Security Considerations**:
   - Regularly monitor open ports and active connections to detect potential vulnerabilities.

4. **Combine with Other Tools**:
   - Use `tcpdump` or Wireshark alongside `netstat` for deeper traffic analysis.

---

## **Key Takeaways**
1. `netstat` is a versatile tool for monitoring network connections, routing, and traffic statistics.
2. Understanding its options helps troubleshoot and secure network configurations.
3. Regular use ensures better awareness of system network activity and potential security threats.
