---
title: How to Crack Passwords with Hashcat
date: 2023-12-30 08:01:35 +0300
subtitle: Product Design
image: '/images/530.png'
---
# How to Crack Passwords with Hashcat

## **Objective**
Learn how to use **Hashcat**, a powerful password recovery tool, to crack password hashes in a controlled environment. This lab demonstrates how to identify hash types, perform dictionary and brute-force attacks, and interpret results.

---

## **Prerequisites**
1. **Hashcat Installed**:
   - Install Hashcat on your system:
     - **Linux**:
       ```bash
       sudo apt update && sudo apt install hashcat
       ```
     - **Windows**:
       - Download from [https://hashcat.net/hashcat/](https://hashcat.net/hashcat/).
     - **macOS**:
       ```bash
       brew install hashcat
       ```

2. **Hash File**:
   - A file containing password hashes to test.
   - Example hash:
     ```
     $6$rounds=656000$6DxTvhECQZs...$UEQoPV...
     ```

3. **Wordlist File** (for dictionary attacks):
   - Download a wordlist like **rockyou.txt** from [SecLists](https://github.com/danielmiessler/SecLists).

4. **GPU Support (Optional)**:
   - Hashcat can utilize GPUs for faster cracking. Ensure proper GPU drivers are installed.

5. **Testing Environment**:
   - Ensure you have explicit permission to test any hashes.

---

## **Step 1: Identifying the Hash Type**
1. **Use Hashcat’s Built-in Documentation**:
   ```bash
   hashcat --help
   ```

2. **Identify the Hash Type**:
   - Use tools like [hash-identifier](https://github.com/blackploit/hash-identifier):
     ```bash
     sudo apt install hash-identifier
     hash-identifier
     ```
   - Enter your hash to determine its type.

3. **Example Hash Types**:
   - MD5: `$1$...`
   - SHA-256: `$5$...`
   - bcrypt: `$2b$...`

---

## **Step 2: Performing a Dictionary Attack**
1. Use the following command:
   ```bash
   hashcat -m <hash_type> -a 0 <hash_file> <wordlist_file>
   ```
   - Replace `<hash_type>` with the hash mode (e.g., `0` for MD5, `100` for SHA1).
   - Replace `<hash_file>` with the file containing the hashes.
   - Replace `<wordlist_file>` with your wordlist.

2. Example:
   ```bash
   hashcat -m 0 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt
   ```

3. Monitor Progress:
   - Hashcat will display the cracking progress and results in real-time.

4. View Cracked Passwords:
   ```bash
   cat hashcat.potfile
   ```

---

## **Step 3: Performing a Brute-Force Attack**
1. Use the following command:
   ```bash
   hashcat -m <hash_type> -a 3 <hash_file> <mask>
   ```
   - Replace `<mask>` with a pattern (e.g., `?a?a?a?a` for any 4-character password).

2. Example:
   ```bash
   hashcat -m 0 -a 3 hashes.txt ?d?d?d?d
   ```
   - `?d`: Digit (0–9).
   - `?l`: Lowercase letter.
   - `?u`: Uppercase letter.
   - `?a`: Any character.

3. Increase Password Length:
   - Use the `--increment` flag to try progressively longer passwords:
     ```bash
     hashcat -m 0 -a 3 --increment hashes.txt ?d?d?d
     ```

---

## **Step 4: Cracking Salts and Complex Hashes**
1. Some hashes include salts (additional random data).
2. Add the salt to the hash file if needed:
   ```
   $6$salt$hash
   ```
3. Use the appropriate hash type and wordlist for cracking.

---

## **Step 5: Using Rules for Smarter Cracking**
1. Apply a rule file for transformations:
   ```bash
   hashcat -m <hash_type> -a 0 -r <rule_file> <hash_file> <wordlist_file>
   ```
   - Replace `<rule_file>` with a rules file (e.g., `rules/best64.rule`).

2. Example:
   ```bash
   hashcat -m 0 -a 0 -r rules/best64.rule hashes.txt /usr/share/wordlists/rockyou.txt
   ```

3. Hashcat will modify entries in the wordlist based on the rules (e.g., adding numbers, reversing).

---

## **Step 6: Interpreting Results**
1. Cracked passwords are stored in the `hashcat.potfile`.
2. View the results:
   ```bash
   cat hashcat.potfile
   ```
3. Example Output:
   ```
   $6$rounds=656000$6DxTvhECQZs...:password123
   ```
   - The cracked password follows the hash.

---

## **Step 7: Ethical Considerations**
1. **Permission Required**:
   - Only test hashes you own or have explicit authorization to test.

2. **Secure the Hash File**:
   - Store hashes and cracked passwords securely to prevent misuse.

3. **Minimize Brute-Force Attacks**:
   - Focus on dictionary and rule-based attacks to save resources and time.

---

## **Additional Tips and Insights**
1. **Optimize Performance**:
   - Use `--force` to bypass warnings but only in safe environments.
   - Ensure GPU drivers are updated for better performance.

2. **Combine with Other Tools**:
   - Use `john` or `hashid` for hash identification and cracking alongside Hashcat.

3. **Automate Testing**:
   - Create scripts to automate hash cracking tasks.

4. **Test Custom Wordlists**:
   - Generate wordlists with tools like `crunch` or `CeWL` tailored to the target.

---

## **Key Takeaways**
1. Hashcat is a powerful tool for cracking password hashes in a controlled, ethical environment.
2. Understanding hash types and selecting the appropriate attack method is critical for success.
3. Always follow ethical guidelines and ensure permission for any testing activity.
