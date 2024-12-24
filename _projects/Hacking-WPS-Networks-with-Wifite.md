---
title: Hacking WPS Networks with Wifite
date: 2024-01-19 08:01:35 +0300
subtitle: Product Design
image: '/images/819.png'
---
# Hacking WPS Networks with Wifite

## **Objective**
Learn how to use **Wifite**, an automated wireless attack tool, to target WPS-enabled networks and understand how to secure such networks against attacks.

---

## **Purpose**
WPS (Wi-Fi Protected Setup) is a feature that simplifies connecting devices to wireless networks but often contains vulnerabilities that can be exploited. Wifite automates the process of attacking WPS-enabled networks to demonstrate the risks of leaving WPS enabled.

---

## **Tools Required**
- **Kali Linux** (or any Linux distribution with Wifite installed).
- A Wi-Fi adapter capable of monitor mode and packet injection (e.g., Alfa AWUS036NHA).
- A test wireless network with WPS enabled.

---

## **Lab Topology**
- **Kali Linux**: Running Wifite.
- **Target Wi-Fi Network**: A WPS-enabled test network.

---

## **Walkthrough**

### **Task 1: Setting Up Wifite**
1. **Verify Wifite Installation**:
   - Wifite is pre-installed on Kali Linux. To check:
     ```bash
     wifite --help
     ```
   - If not installed, install it using:
     ```bash
     sudo apt update && sudo apt install wifite -y
     ```

2. **Enable Monitor Mode**:
   - Identify your Wi-Fi adapter:
     ```bash
     iwconfig
     ```
   - Enable monitor mode using airmon-ng:
     ```bash
     sudo airmon-ng start <interface>
     ```
     Replace `<interface>` with your adapter name (e.g., `wlan0`).

---

### **Task 2: Scanning for WPS Networks**
1. **Launch Wifite**:
   - Start Wifite:
     ```bash
     sudo wifite
     ```

2. **Scan for Networks**:
   - Wifite will scan for nearby wireless networks.
   - Look for networks with **WPS** enabled.

3. **Select Target**:
   - Choose a WPS-enabled network from the list by entering its number.

---

### **Task 3: Attacking the Target Network**
1. **Initiate WPS Attack**:
   - Wifite will automatically attempt to brute-force the WPS PIN of the target network.
   - Monitor the progress in the terminal.

2. **Capture WPA Key**:
   - If successful, Wifite will display the WPA passphrase for the target network.
   - Example output:
     ```
     [!] WPA PSK: "password123"
     ```

3. **Handle WPS Lock**:
   - If the WPS PIN attack locks the router:
     - Wait for the lock to reset.
     - Restart Wifite with throttled requests:
       ```bash
       sudo wifite --wps-time 300
       ```

---

### **Task 4: Securing WPS Networks**
1. **Disable WPS**:
   - Access the router’s administrative interface and disable WPS entirely.

2. **Use Strong WPA2/3 Passwords**:
   - Ensure the network password is strong and unique.

3. **Update Router Firmware**:
   - Regularly update firmware to patch known vulnerabilities.

4. **Monitor for Suspicious Activity**:
   - Use router logs or network monitoring tools to detect unusual activity.

---

## **Best Practices**
1. **Use Authorized Targets Only**:
   - Ensure you have explicit permission to test the network.

2. **Combine Tools for Comprehensive Testing**:
   - Use Wifite alongside tools like Aircrack-ng and Reaver for detailed assessments.

3. **Educate Users**:
   - Train users on securing home and enterprise networks.

4. **Regularly Audit Networks**:
   - Conduct periodic assessments to ensure networks remain secure.

---

## **Key Takeaways**
1. Wifite automates the exploitation of WPS vulnerabilities.
2. Disabling WPS is the most effective way to secure networks against these attacks.
3. Strong passwords and updated firmware enhance network security.

---

## **Troubleshooting Tips**
1. **No Networks Detected**:
   - Ensure the Wi-Fi adapter is in monitor mode.
   - Verify the adapter supports packet injection.

2. **WPS Lock Issues**:
   - Wait for the lock to reset and use throttled requests.

3. **Wifite Crashes**:
   - Update Wifite to the latest version:
     ```bash
     sudo apt update && sudo apt upgrade wifite
     ```

By completing this lab, you now understand how to use Wifite to exploit WPS vulnerabilities and how to secure networks against such attacks.