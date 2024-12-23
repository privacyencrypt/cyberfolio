---
title: Using ipconfig to View and Modify Network Information on Windows
date: 2025-01-08 08:01:35 +0300
subtitle: Product Design
image: '/images/418.png'
---
# Using ipconfig to View and Modify Network Information on Windows

## **Objective**
Learn how to use the **ipconfig** command on a Windows system to view and modify network information. This lab covers basic usage, advanced options, and practical troubleshooting scenarios.

---

## **Prerequisites**
1. **Windows Operating System**:
   - A Windows machine with administrative privileges.

2. **Basic Understanding of Networking**:
   - Familiarity with concepts like IP addresses, subnet masks, and default gateways.

3. **Access to Command Prompt**:
   - Open Command Prompt by pressing `Win + R`, typing `cmd`, and pressing **Enter**.

---

## **Step 1: Viewing Basic Network Information**
1. Open Command Prompt.
2. Run the `ipconfig` command:
   ```cmd
   ipconfig
   ```
3. Analyze the output:
   - **IPv4 Address**: The primary address of the system (e.g., `192.168.1.100`).
   - **Subnet Mask**: Defines the network portion of the IP address (e.g., `255.255.255.0`).
   - **Default Gateway**: The router IP address used to access external networks (e.g., `192.168.1.1`).

---

## **Step 2: Viewing Detailed Network Information**
1. Use the `/all` flag to display detailed information about all network adapters:
   ```cmd
   ipconfig /all
   ```
2. Review additional details:
   - **Physical Address**: The MAC address of the adapter.
   - **DHCP Enabled**: Indicates whether the system is using DHCP to obtain an IP address.
   - **DNS Servers**: Lists the configured DNS server addresses.

   Example Output:
   ```
   Ethernet adapter Ethernet:
      IPv4 Address. . . . . . . . . . . : 192.168.1.100
      Subnet Mask . . . . . . . . . . . : 255.255.255.0
      Default Gateway . . . . . . . . . : 192.168.1.1
      DNS Servers . . . . . . . . . . . : 8.8.8.8
   ```

---

## **Step 3: Releasing and Renewing IP Addresses**
### **Releasing the Current IP Address**
1. Run the following command to release the current IP:
   ```cmd
   ipconfig /release
   ```
2. Verify that the adapter no longer has an assigned IP.

### **Renewing the IP Address**
1. Run the following command to obtain a new IP:
   ```cmd
   ipconfig /renew
   ```
2. Verify that the adapter has received a new IP address.

   **Tip**: Use this when troubleshooting DHCP-related issues.

---

## **Step 4: Flushing the DNS Cache**
1. Clear the DNS resolver cache to remove outdated entries:
   ```cmd
   ipconfig /flushdns
   ```
2. Verify that the cache was cleared:
   ```cmd
   ipconfig /displaydns
   ```
   **Insight**: Flushing the DNS cache resolves issues with accessing recently changed domains.

---

## **Step 5: Displaying Specific Adapter Information**
1. View information for a specific adapter by filtering the output:
   ```cmd
   ipconfig | findstr /C:"IPv4"
   ```
2. Combine `ipconfig` with `findstr` to customize your queries.

   **Tip**: Use this to quickly locate details without sifting through long outputs.

---

## **Step 6: Setting a Static IP Address**
1. Open the **Network and Sharing Center**:
   - Right-click the network icon in the system tray and select **Open Network & Internet settings**.
2. Navigate to the adapter settings:
   - Click **Change adapter options** > Right-click your network adapter > Select **Properties**.
3. Set a static IP:
   - Double-click **Internet Protocol Version 4 (TCP/IPv4)**.
   - Choose **Use the following IP address** and enter:
     - IP Address: e.g., `192.168.1.150`
     - Subnet Mask: `255.255.255.0`
     - Default Gateway: e.g., `192.168.1.1`
     - DNS Servers: e.g., `8.8.8.8` and `8.8.4.4`

   **Tip**: Use static IPs for servers or devices that require consistent addresses.

---

## **Step 7: Troubleshooting Network Issues**
1. **No Internet Access**:
   - Use `ipconfig /release` and `ipconfig /renew` to reset your IP.
   - Verify the default gateway and DNS server settings.

2. **Incorrect DNS Resolution**:
   - Run `ipconfig /flushdns` to clear cached entries.
   - Test domain resolution using `ping <domain>`.

3. **No IP Address Assigned**:
   - Ensure the network adapter is enabled:
     ```cmd
     netsh interface show interface
     ```
   - Enable the adapter if necessary:
     ```cmd
     netsh interface set interface "<Adapter Name>" enable
     ```

---

## **Additional Tips and Insights**
1. **Combine ipconfig with Other Tools**:
   - Use `ping`, `tracert`, or `nslookup` alongside `ipconfig` for comprehensive troubleshooting.

2. **Automate Tasks**:
   - Create batch scripts for repetitive tasks like flushing DNS or releasing/renewing IPs.

3. **IPv6 Support**:
   - View IPv6 details using `ipconfig /all`. Disable IPv6 if not needed for simpler configurations.

4. **DNS Over HTTPS (DoH)**:
   - Check if your network supports DoH by reviewing your DNS resolver settings.

---

## **Key Takeaways**
1. The `ipconfig` command is a fundamental tool for viewing and modifying network configurations on Windows.
2. Understanding its various options enhances your ability to troubleshoot common networking issues.
3. Combining `ipconfig` with other utilities provides a comprehensive approach to diagnosing connectivity problems.
