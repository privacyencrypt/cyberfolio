---
title: Conducting a Cross-Site Scripting (XSS) Attack
date: 2024-12-05 08:01:35 +0300
subtitle: Product Design
image: '/images/405.png'
---
# Lab 5: Conducting a Cross-Site Scripting (XSS) Attack

## **Objective**
Understand the mechanics of a Cross-Site Scripting (XSS) attack, how attackers exploit vulnerable web applications, and the steps to mitigate these vulnerabilities. This lab focuses on executing a stored or reflected XSS attack in a controlled environment.

---

## **Prerequisites**
1. **Kali Linux or Any Linux Distro**:
   - Ensure you have a web browser and testing tools like Burp Suite or OWASP ZAP installed.

2. **Vulnerable Web Application**:
   - Use an intentionally vulnerable application like DVWA (Damn Vulnerable Web Application) or OWASP Juice Shop.
   - **Tip**: Install DVWA using XAMPP, Docker, or directly on your system.

3. **Basic Web Knowledge**:
   - Familiarity with HTML, JavaScript, and how websites process user input.
   - **Insight**: Understanding web forms, cookies, and scripts is crucial for this lab.

---

## **Step 1: Setting Up the Environment**
1. Launch your vulnerable web application.
   - For DVWA, start the web server and navigate to `http://localhost/dvwa` in your browser.

2. Set the DVWA security level to **low**:
   - Log in (default credentials: `admin` / `password`).
   - Navigate to `DVWA Security` and set the security level to `Low`.

3. Verify the XSS module is available:
   - Click on the **XSS (Stored)** or **XSS (Reflected)** section in DVWA.

---

## **Step 2: Understanding XSS Types**
1. **Reflected XSS**:
   - The malicious script is embedded in a URL and reflected back to the user.
   - Example: A search bar displays the search query directly in the response.

2. **Stored XSS**:
   - The malicious script is stored on the server (e.g., in a comment or form) and executed when users view the affected page.

---

## **Step 3: Conducting a Reflected XSS Attack**
1. Open the **XSS (Reflected)** module in DVWA.
2. Enter a test payload into the input field:
   ```html
   <script>alert('XSS');</script>
   ```
3. Submit the form.
4. Observe if an alert box appears:
   - **Insight**: The alert box indicates the input was reflected and executed by the browser.

5. Modify the payload to capture sensitive data:
   ```html
   <script>document.write(document.cookie);</script>
   ```
   - **Tip**: Cookies can store session tokens or authentication details.

---

## **Step 4: Conducting a Stored XSS Attack**
1. Open the **XSS (Stored)** module in DVWA.
2. Enter a malicious payload into a form field (e.g., a comment box):
   ```html
   <script>alert('Stored XSS');</script>
   ```
3. Submit the form.
4. Reload the page and observe if the script executes when the comment is displayed.
   - **Insight**: Stored XSS is more dangerous as it affects all users who view the page.

5. Use an advanced payload to redirect users to a malicious site:
   ```html
   <script>window.location='http://malicious-site.com';</script>
   ```

---

## **Step 5: Mitigation Techniques**
1. **Input Validation**:
   - Validate user input on both client and server sides.
   - Reject input containing suspicious characters (e.g., `<`, `>`).

2. **Output Encoding**:
   - Encode output to ensure special characters are rendered harmless (e.g., `&lt;` instead of `<`).

3. **Content Security Policy (CSP)**:
   - Implement a CSP to restrict the execution of unauthorized scripts.

4. **Use HTTPOnly Cookies**:
   - Prevent JavaScript from accessing sensitive cookies.

---

## **Step 6: Cleaning Up**
1. Remove any malicious payloads or reset the DVWA database to its default state.
2. Close any vulnerable services or applications to secure your environment.

---

## **Additional Tips and Insights**
1. **Test Safely**:
   - Only test XSS vulnerabilities in a controlled environment where you have explicit permission.

2. **Educate Users**:
   - Encourage awareness about the dangers of clicking on untrusted links or entering sensitive data into unknown sites.

3. **Use XSS Testing Tools**:
   - Tools like Burp Suite, OWASP ZAP, or XSSer can automate the detection of XSS vulnerabilities.

4. **Practice Regularly**:
   - Experiment with different payloads to understand how various sanitization techniques work.

---

## **Key Takeaways**
1. XSS attacks exploit weaknesses in how web applications handle user input and output.
2. Reflected XSS typically targets individual users, while Stored XSS affects all users interacting with the compromised data.
3. Preventing XSS requires a combination of input validation, output encoding, and modern security practices like CSP.
