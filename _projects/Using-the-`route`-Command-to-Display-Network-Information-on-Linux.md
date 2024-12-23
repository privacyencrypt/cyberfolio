---
title: Using the `route` Command to Display Network Information on Linux
date: 2023-12-25 08:01:35 +0300
subtitle: Product Design
image: '/images/525.png'
---
# Using the `route` Command to Display Network Information on Linux

## **Objective**
Learn how to use the `route` command on Linux to view and manipulate the system’s routing table. This lab focuses on displaying network information, adding and deleting routes, and troubleshooting connectivity issues.

---

## **Prerequisites**
1. **Linux Environment**:
   - A Linux system with administrative privileges.

2. **Basic Networking Knowledge**:
   - Familiarity with IP addresses, gateways, and subnet masks.

3. **Installed Tools**:
   - Ensure the `route` command is available:
     ```bash
     route --version
     ```
   - If not available, install the `net-tools` package:
     ```bash
     sudo apt update && sudo apt install net-tools
     ```

---

## **Step 1: Viewing the Current Routing Table**
1. Open a terminal.
2. Run the `route` command:
   ```bash
   route
   ```
3. Analyze the output:
   - **Destination**: The target network or host.
   - **Gateway**: The next-hop address used to reach the destination.
   - **Genmask**: The subnet mask associated with the destination.
   - **Iface**: The network interface handling the traffic.

   Example Output:
   ```
   Kernel IP routing table
   Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
   default         192.168.1.1     0.0.0.0         UG    0      0        0 eth0
   192.168.1.0     *               255.255.255.0   U     0      0        0 eth0
   ```

   **Key Flags**:
   - **U**: Route is up.
   - **G**: Route is a gateway.

4. Use `-n` to display numeric addresses:
   ```bash
   route -n
   ```
   - This avoids resolving hostnames for faster output.

---

## **Step 2: Adding a Static Route**
1. Add a route to a specific network:
   ```bash
   sudo route add -net <network> netmask <subnet_mask> gw <gateway> dev <interface>
   ```
   - Replace `<network>` with the target network (e.g., `192.168.2.0`).
   - Replace `<subnet_mask>` with the subnet mask (e.g., `255.255.255.0`).
   - Replace `<gateway>` with the gateway IP (e.g., `192.168.1.1`).
   - Replace `<interface>` with the network interface (e.g., `eth0`).

   Example:
   ```bash
   sudo route add -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.1.1 dev eth0
   ```

2. Verify the new route:
   ```bash
   route -n
   ```

---

## **Step 3: Deleting a Static Route**
1. Remove a route:
   ```bash
   sudo route del -net <network> netmask <subnet_mask>
   ```
   - Example:
     ```bash
     sudo route del -net 192.168.2.0 netmask 255.255.255.0
     ```

2. Confirm the route has been removed:
   ```bash
   route -n
   ```

---

## **Step 4: Setting the Default Gateway**
1. Add or modify the default gateway:
   ```bash
   sudo route add default gw <gateway_ip> dev <interface>
   ```
   - Replace `<gateway_ip>` with the IP of the default gateway (e.g., `192.168.1.1`).
   - Replace `<interface>` with the network interface (e.g., `eth0`).

   Example:
   ```bash
   sudo route add default gw 192.168.1.1 dev eth0
   ```

2. Verify the default route:
   ```bash
   route -n
   ```

3. Remove the default gateway if needed:
   ```bash
   sudo route del default gw <gateway_ip>
   ```

---

## **Step 5: Troubleshooting with the Routing Table**
1. **Check for Missing Routes**:
   - Ensure there is a route for the target network in the routing table.

2. **Diagnose Gateway Issues**:
   - Use `ping` to verify the gateway is reachable:
     ```bash
     ping <gateway_ip>
     ```

3. **Incorrect Routes**:
   - Remove and re-add routes if the traffic is being misrouted.

4. **Monitor Route Usage**:
   - Use the `ip route show` command for real-time route information:
     ```bash
     ip route show
     ```

---

## **Additional Tips and Insights**
1. **Use Persistent Routes**:
   - Add routes to the `/etc/network/interfaces` file or systemd network configuration for persistence across reboots.

2. **Combine Tools**:
   - Use `traceroute` or `ping` alongside `route` to troubleshoot end-to-end connectivity.

3. **Transition to Modern Tools**:
   - The `route` command is deprecated on some distributions. Use `ip route` for enhanced functionality:
     ```bash
     ip route add <network>/<prefix> via <gateway> dev <interface>
     ```

4. **Security Awareness**:
   - Regularly audit routes to ensure no unauthorized changes have been made.

---

## **Key Takeaways**
1. The `route` command is essential for viewing and managing routing tables in Linux.
2. Understanding how to add, delete, and troubleshoot routes helps ensure proper network connectivity.
3. Transitioning to modern tools like `ip route` ensures compatibility with newer Linux distributions.
