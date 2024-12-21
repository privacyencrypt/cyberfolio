---
title: Evil Twin Attack Using Airgeddon
date: 2024-05-12 08:01:35 +0300
subtitle: Product Design
image: '/images/409.jpg'
---
# Evil Twin Attack Using Airgeddon

## **Objective**
Learn how to use **Airgeddon**, a multi-use tool for wireless attacks, to simulate an Evil Twin attack. This lab demonstrates how attackers clone legitimate Wi-Fi networks to trick users into connecting and stealing their credentials.

---

## **Prerequisites**
1. **Kali Linux or Any Linux Distro with Airgeddon Installed**:
   - Verify Airgeddon is installed:
     ```bash
     airgeddon --help
     ```
   - Install it if necessary:
     ```bash
     git clone https://github.com/v1s1t0r1sh3r3/airgeddon.git
     cd airgeddon
     sudo bash airgeddon.sh
     ```

2. **Wireless Network Adapter**:
   - Ensure you have a wireless adapter that supports monitor mode.
   - **Tip**: Use the command `airmon-ng` to check compatibility.

3. **Controlled Environment**:
   - Perform this attack in a controlled lab setup with permission to test.

4. **Basic Wireless Knowledge**:
   - Familiarity with SSIDs, BSSIDs, channels, and encryption types.

---

## **Step 1: Launching Airgeddon**
1. Start Airgeddon in a terminal:
   ```bash
   sudo bash airgeddon.sh
   ```
2. Select your language and press **Enter**.
3. Airgeddon will detect your wireless interfaces. Choose the one you want to use for the attack.

---

## **Step 2: Preparing the Wireless Adapter**
1. Put your wireless adapter into monitor mode:
   - Airgeddon provides an option to set your adapter to monitor mode automatically. Select it from the menu.
2. Verify that the adapter is in monitor mode:
   ```bash
   iwconfig
   ```
   - Look for `Mode:Monitor` under your wireless interface.

---

## **Step 3: Scanning for Networks**
1. Select the **Handshake Tools** option in Airgeddon.
2. Choose the **Scan for Targets** option to identify nearby Wi-Fi networks.
3. Review the list of available networks:
   - Note the target SSID, BSSID, and channel.

   **Tip**: Choose a network with active clients for better results.

---

## **Step 4: Capturing the Handshake**
1. Select the target network from the list.
2. Deauthenticate connected clients to force them to reconnect and capture the handshake:
   - Airgeddon provides automated deauth attack tools.
3. Confirm that the handshake was captured:
   - Look for a confirmation message in the terminal or a handshake file saved in the Airgeddon directory.

---

## **Step 5: Setting Up the Evil Twin Network**
1. Navigate to the **Evil Twin Attack** menu in Airgeddon.
2. Select the **Fake Access Point** option.
   - Airgeddon will create a cloned version of the target network.
3. Configure the Evil Twin:
   - Set the SSID to match the target network.
   - Choose the same channel as the target.

   **Insight**: Matching the SSID and channel increases the likelihood of victims connecting to the fake network.

---

## **Step 6: Capturing Credentials**
1. Launch the fake access point:
   - Airgeddon will set up a captive portal to mimic a legitimate login page.
2. Monitor connected clients:
   - Airgeddon will display a list of devices that connect to the Evil Twin.
3. When users enter their credentials, Airgeddon will log them.
   - **Tip**: Review logs in the designated directory for captured data.

---

## **Step 7: Cleaning Up**
1. Stop all Airgeddon processes:
   - Exit the Airgeddon interface to disable the fake access point and monitoring tools.
2. Reset your wireless adapter to managed mode:
   ```bash
   sudo airmon-ng stop <interface_name>
   ```
   - Replace `<interface_name>` with your wireless adapter (e.g., `wlan0mon`).

3. Verify normal operation:
   ```bash
   iwconfig
   ```
   - Ensure the adapter is back in managed mode.

---

## **Step 8: Mitigation Techniques**
1. **Educate Users**:
   - Train users to recognize fake Wi-Fi networks and avoid entering credentials on suspicious portals.

2. **Use Strong Authentication**:
   - Implement WPA3 or WPA2-Enterprise for better security.

3. **Monitor Network Activity**:
   - Regularly scan for rogue access points on your network.

4. **Enable Certificate Validation**:
   - Ensure that legitimate networks use SSL/TLS certificates for login pages.

5. **Multi-Factor Authentication (MFA)**:
   - Require MFA to prevent credential theft from leading to account compromise.

---

## **Additional Tips and Insights**
1. **Ethical Considerations**:
   - Only perform Evil Twin attacks in environments where you have explicit permission.
   - Unauthorized attacks are illegal and unethical.

2. **Practice in a Lab**:
   - Set up a controlled lab environment to safely learn and refine your skills.

3. **Explore Other Airgeddon Features**:
   - Airgeddon includes tools for WPS attacks, password cracking, and more. Experiment to broaden your understanding.

4. **Combine Tools**:
   - Use tools like Wireshark alongside Airgeddon to analyze network traffic and understand how data flows during the attack.

---

## **Key Takeaways**
1. Evil Twin attacks exploit user trust by mimicking legitimate Wi-Fi networks.
2. Airgeddon simplifies the process of creating a fake access point and capturing credentials.
3. Awareness, education, and strong network security practices are critical to mitigating these attacks.
