---
title: Broken Access Control
date: 2024-01-10 08:01:35 +0300
subtitle: Product Design
image: '/images/810.png'
---
# Broken Access Control

## **Objective**
Learn to identify and exploit Broken Access Control vulnerabilities in a web application to gain unauthorized access to restricted resources, and understand how to mitigate such vulnerabilities.

---

## **Purpose**
Broken Access Control occurs when an application fails to properly enforce restrictions on what authenticated users can access or perform. This lab demonstrates how attackers exploit these flaws to access unauthorized resources and how developers can secure their applications.

---

## **Tools Required**
- **Kali Linux**: For testing and tools.
- A vulnerable web application (e.g., Damn Vulnerable Web Application - DVWA or OWASP Juice Shop).
- **Burp Suite Community Edition**: For intercepting and manipulating requests.

---

## **Lab Topology**
- **Kali Linux**: Running tools like Burp Suite.
- **DVWA or Juice Shop**: Hosting the vulnerable web application.

---

## **Walkthrough**

### **Task 1: Setting Up the Environment**
1. **Install a Vulnerable Application**:
   - Use **DVWA** or **OWASP Juice Shop**. For DVWA, follow [installation instructions](https://github.com/digininja/DVWA).
   - Run the application on a local or virtual server (e.g., XAMPP, WAMP, Docker).

2. **Access the Application**:
   - Navigate to the application in a browser:
     ```
     http://<application_ip>/dvwa
     ```
     Replace `<application_ip>` with the hosting machine’s IP.

3. **Login to DVWA**:
   - Default credentials:
     ```
     Username: admin
     Password: password
     ```

4. **Set Security Level**:
   - Navigate to **DVWA Security**.
   - Set the security level to **Low** for testing.

---

### **Task 2: Understanding Broken Access Control**
1. **What is Broken Access Control?**
   - It occurs when an application does not properly enforce restrictions, allowing users to access resources they should not have permissions for.

2. **Common Indicators**:
   - Accessible URLs for unauthorized users.
   - Missing role-based access checks.
   - Privilege escalation vulnerabilities.

---

### **Task 3: Identifying Vulnerable Endpoints**
1. **Inspect the Application**:
   - Look for pages or resources intended for administrators or other restricted users (e.g., `/admin`, `/settings`).

2. **Test URL Manipulation**:
   - Access restricted pages directly by modifying the URL in the browser:
     ```
     http://<application_ip>/admin
     ```
     - Observe whether the page loads without authentication.

3. **Enumerate Directories**:
   - Use tools like **Gobuster** to discover hidden directories:
     ```bash
     gobuster dir -u http://<application_ip> -w /usr/share/wordlists/dirb/common.txt
     ```
   - Identify directories or files such as `/admin`, `/backup`, or `/config`.

---

### **Task 4: Exploiting Broken Access Control**
1. **Horizontal Privilege Escalation**:
   - Test if a user can access another user’s data by modifying parameters.
   - Example:
     ```http
     GET /profile?id=2 HTTP/1.1
     Host: <application_ip>
     ```
     - Change `id=2` to another value (e.g., `id=3`) to access another user’s profile.

2. **Vertical Privilege Escalation**:
   - Attempt to access administrator functionalities.
   - Modify parameters such as roles in intercepted requests using **Burp Suite**:
     - Intercept a request and change:
       ```
       role=user
       ```
       to:
       ```
       role=admin
       ```

3. **Forceful Browsing**:
   - Manually navigate to restricted resources by guessing their paths (e.g., `/admin/dashboard`).

4. **Testing Authentication Bypass**:
   - Remove cookies or tokens and observe if the restricted resource is still accessible.
   - Use **Burp Suite** to intercept and remove `Authorization` headers in requests.

---

### **Task 5: Mitigating Broken Access Control**
1. **Implement Role-Based Access Control (RBAC)**:
   - Ensure resources are accessible only to authorized roles.

2. **Use Secure Session Management**:
   - Tie user sessions to roles and permissions.

3. **Validate Requests on the Server**:
   - Verify permissions on the server side for every request.

4. **Audit Application Logs**:
   - Monitor access logs for unusual activity, such as repeated attempts to access restricted resources.

5. **Test Regularly**:
   - Conduct periodic security tests to identify and fix access control vulnerabilities.

---

## **Best Practices**
1. **Minimize Sensitive Endpoints**:
   - Avoid exposing unnecessary administrative or sensitive endpoints.

2. **Encrypt Sensitive Data**:
   - Protect sensitive information using encryption in transit and at rest.

3. **Educate Developers**:
   - Train developers to implement proper access control checks in code.

4. **Use a Web Application Firewall (WAF)**:
   - Deploy a WAF to detect and block unauthorized access attempts.

---

## **Key Takeaways**
1. Broken Access Control can lead to unauthorized access and privilege escalation.
2. Proper implementation of server-side access checks is critical for security.
3. Regular testing and monitoring can prevent exploitation of access control flaws.

---

## **Troubleshooting Tips**
1. **Restricted Page Loads Normally**:
   - Confirm the application does not have hardcoded authentication bypasses.
   - Check for client-side restrictions (e.g., JavaScript-based role checks).

2. **No Results from Gobuster**:
   - Use a different wordlist or include file extensions in the scan.

3. **Application Becomes Unresponsive**:
   - Restart the server hosting the vulnerable application.
   - Reduce the intensity of automated tools to avoid overwhelming the server.

By completing this lab, you now understand how to identify and exploit Broken Access Control vulnerabilities and the importance of implementing robust access control mechanisms.