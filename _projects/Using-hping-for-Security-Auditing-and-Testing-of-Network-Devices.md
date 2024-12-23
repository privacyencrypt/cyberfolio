---
title: Using hping for Security Auditing and Testing of Network Devices
date: 2023-12-20 08:01:35 +0300
subtitle: Product Design
image: '/images/420.png'
---
# Using hping for Security Auditing and Testing of Network Devices

## **Objective**
Learn how to use **hping**, a command-line packet crafting tool, to test and audit network devices for vulnerabilities. This lab covers sending custom packets, performing network discovery, and testing firewall rules.

---

## **Prerequisites**
1. **Linux Environment with hping Installed**:
   - Verify if `hping` is installed:
     ```bash
     hping3 --version
     ```
   - Install it if necessary:
     ```bash
     sudo apt update && sudo apt install hping3
     ```

2. **Basic Networking Knowledge**:
   - Familiarity with IP addresses, TCP/UDP, and ICMP protocols.

3. **Testing Environment**:
   - A target system or network device for testing.
   - **Ensure explicit permission** to test any device or network.

---

## **Step 1: Understanding hping**
1. hping is a packet crafting tool that supports:
   - TCP, UDP, and ICMP packets.
   - Customizable headers for testing firewalls and routers.
   - Network discovery and testing techniques like SYN flooding.

2. View the help menu to explore options:
   ```bash
   hping3 --help
   ```

---

## **Step 2: Sending Basic Ping Requests**
1. Send an ICMP echo request (like `ping`):
   ```bash
   hping3 -1 <target_ip>
   ```
   - Replace `<target_ip>` with the target’s IP address.
   - Example:
     ```bash
     hping3 -1 192.168.1.1
     ```

2. Observe the output:
   - **ICMP Reply**: Indicates the target is reachable.
   - **No Response**: May indicate the host is down or ICMP traffic is blocked.

---

## **Step 3: TCP SYN Scanning**
1. Perform a TCP SYN scan on a specific port:
   ```bash
   hping3 -S -p <port> <target_ip>
   ```
   - Replace `<port>` with the port number (e.g., `80` for HTTP).
   - Example:
     ```bash
     hping3 -S -p 80 192.168.1.1
     ```

2. Interpret the response:
   - **SA**: Indicates the port is open.
   - **RA**: Indicates the port is closed.
   - **No Response**: May indicate the port is filtered by a firewall.

3. Scan a range of ports:
   ```bash
   hping3 -S -p ++<start_port> <target_ip>
   ```
   - Example:
     ```bash
     hping3 -S -p ++22 192.168.1.1
     ```

---

## **Step 4: UDP Scanning**
1. Send a UDP packet to a specific port:
   ```bash
   hping3 --udp -p <port> <target_ip>
   ```
   - Example:
     ```bash
     hping3 --udp -p 53 192.168.1.1
     ```

2. Interpret the response:
   - **No Response**: Indicates the port is open/filtered.
   - **ICMP Port Unreachable**: Indicates the port is closed.

---

## **Step 5: Firewall Testing**
1. Test if a firewall blocks ICMP traffic:
   ```bash
   hping3 -1 <target_ip>
   ```

2. Check for blocked TCP packets:
   ```bash
   hping3 -S -p <port> <target_ip>
   ```

3. Bypass firewalls using fragmentation:
   ```bash
   hping3 -S -p <port> --frag <target_ip>
   ```
   - Fragmented packets may bypass poorly configured firewalls.

---

## **Step 6: SYN Flooding (Stress Testing)**
1. Launch a SYN flood attack:
   ```bash
   sudo hping3 -S -p <port> --flood <target_ip>
   ```
   - **`--flood`**: Sends packets as fast as possible without waiting for replies.
   - Example:
     ```bash
     sudo hping3 -S -p 80 --flood 192.168.1.1
     ```

   **Caution**: Perform stress testing only in controlled environments.

---

## **Step 7: Custom Packet Headers**
1. Set a custom source port:
   ```bash
   hping3 -S -p <port> --baseport <source_port> <target_ip>
   ```

2. Set a specific TCP window size:
   ```bash
   hping3 -S -p <port> --win <size> <target_ip>
   ```
   - Replace `<size>` with the desired TCP window size.

3. Spoof the source IP:
   ```bash
   hping3 -S -p <port> --spoof <fake_ip> <target_ip>
   ```

   **Insight**: Spoofing is used to test security measures like IP filtering.

---

## **Step 8: Monitoring and Logging**
1. Use verbose mode to see detailed packet information:
   ```bash
   hping3 -V -1 <target_ip>
   ```

2. Log the output to a file for analysis:
   ```bash
   hping3 -S -p <port> <target_ip> > hping_log.txt
   ```

---

## **Step 9: Mitigation Techniques**
1. **Rate Limiting**:
   - Configure firewalls to limit ICMP, TCP, and UDP traffic rates.

2. **Use Stateful Firewalls**:
   - Filter traffic based on connection states to prevent SYN flooding.

3. **Monitor Network Traffic**:
   - Use tools like Wireshark to detect anomalous packet patterns.

4. **Implement Intrusion Detection Systems (IDS)**:
   - IDS tools can identify and block malicious traffic generated by tools like hping.

---

## **Additional Tips and Insights**
1. **Ethical Use**:
   - Use hping only in environments where you have explicit permission to test.

2. **Combine Tools**:
   - Use hping alongside `tcpdump` or Wireshark to analyze packet flows in real-time.

3. **Regular Testing**:
   - Periodically audit firewalls and network devices to ensure they are properly configured.

4. **Documentation**:
   - Document your findings and remediation steps to improve security practices.

---

## **Key Takeaways**
1. hping is a powerful tool for crafting and sending custom packets to test network security.
2. Understanding its options enables you to perform effective network discovery and vulnerability assessments.
3. Always follow ethical guidelines and use hping responsibly in authorized environments.
