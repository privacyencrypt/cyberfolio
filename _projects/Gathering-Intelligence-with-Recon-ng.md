---
title: Gathering Intelligence with Recon-ng
date: 2024-05-12 08:01:35 +0300
subtitle: Product Design
image: '/images/project-2.jpg'
---
# Lab 3: Gathering Intelligence with Recon-ng

## **Objective**
Learn how to use **Recon-ng**, a reconnaissance framework, to gather intelligence about a target domain. This exercise helps you understand how attackers collect data during the information-gathering phase and how to use this knowledge for defensive purposes.

---

## **Prerequisites**
1. **Kali Linux or Any Linux Distro with Recon-ng Installed**:
   - To check if Recon-ng is installed, type:
     ```bash
     recon-ng --version
     ```
   - Install it if necessary:
     ```bash
     sudo apt update && sudo apt install recon-ng
     ```

2. **Basic Understanding of Domains and DNS**:
   - Familiarity with domain names, subdomains, and DNS records.
   - **Tip**: If you’re unsure, review the basics of how DNS works.

3. **Target Domain**:
   - Identify a domain you own or have explicit permission to test.

---

## **Step 1: Launching Recon-ng**
1. Start your Kali Linux environment and open a terminal.
2. Launch Recon-ng by typing:
   ```bash
   recon-ng
   ```
3. You will see the Recon-ng framework interface, which looks similar to a Metasploit console.

---

## **Step 2: Setting Up a Workspace**
1. Create a workspace to organize your reconnaissance data:
   ```bash
   workspace create <workspace_name>
   ```
   - Replace `<workspace_name>` with a descriptive name (e.g., `testdomain`).
   - **Tip**: Workspaces help you keep data for different projects separate.

2. Verify your active workspace:
   ```bash
   workspace list
   ```
   - The active workspace will be marked with an asterisk (`*`).

---

## **Step 3: Adding a Target Domain**
1. Set the target domain for reconnaissance:
   ```bash
   add domains <target_domain>
   ```
   - Replace `<target_domain>` with the domain you’re analyzing (e.g., `example.com`).
2. Verify the domain was added:
   ```bash
   show domains
   ```

---

## **Step 4: Installing and Using Modules**
Recon-ng uses modules to perform specific tasks, such as gathering subdomains, identifying DNS records, or retrieving WHOIS data.

1. View available modules:
   ```bash
   modules search
   ```
   - **Tip**: Use keywords like `whois`, `dns`, or `subdomain` to narrow the search.

2. Load a module:
   ```bash
   modules load <module_name>
   ```
   - Replace `<module_name>` with the name of the module (e.g., `recon/domains-hosts/whois_pocs`).

3. Set module options:
   ```bash
   options set <option_name> <value>
   ```
   - Replace `<option_name>` with the required parameter (e.g., `source`) and `<value>` with the corresponding value (e.g., your target domain).

4. Run the module:
   ```bash
   run
   ```
   - **Tip**: Use `options show` to check which parameters need to be set before running.

5. Review the output:
   - The results will display directly in the terminal or be saved in the database for later use.

---

## **Step 5: Gathering Subdomains**
1. Use the `recon/domains-hosts/brute_hosts` module to find subdomains:
   ```bash
   modules load recon/domains-hosts/brute_hosts
   ```
2. Set the `source` option to your target domain:
   ```bash
   options set source <target_domain>
   ```
3. Run the module:
   ```bash
   run
   ```
4. View the discovered subdomains:
   ```bash
   show hosts
   ```
   - **Insight**: Subdomains can reveal internal services or overlooked attack surfaces.

---

## **Step 6: Retrieving WHOIS Information**
1. Use the `recon/domains-hosts/whois_pocs` module to gather WHOIS data:
   ```bash
   modules load recon/domains-hosts/whois_pocs
   ```
2. Set the `source` to your target domain and run the module:
   ```bash
   options set source <target_domain>
   run
   ```
3. Review the collected WHOIS information, including registrant details and administrative contacts.
   - **Tip**: WHOIS data often reveals useful details about the organization and network.

---

## **Step 7: Exporting Results**
1. Export collected data for reporting or further analysis:
   ```bash
   output csv <file_name>
   ```
   - Replace `<file_name>` with a descriptive name (e.g., `recon_results.csv`).
2. Verify the exported file is saved in the current working directory:
   ```bash
   ls
   ```

---

## **Step 8: Cleaning Up**
1. Remove sensitive data from the workspace if needed:
   ```bash
   delete domains
   delete hosts
   ```
2. Exit Recon-ng:
   ```bash
   exit
   ```

---

## **Additional Tips and Insights**
1. **Ethical Guidelines**:
   - Only gather intelligence on systems or domains you own or have explicit permission to test.
   - **Insight**: Unauthorized reconnaissance is illegal and can lead to severe consequences.

2. **Leveraging Recon-ng’s Database**:
   - Recon-ng stores collected data in an internal database for easy access.
   - **Tip**: Use commands like `show hosts`, `show domains`, and `show contacts` to view stored data.

3. **API Keys for Enhanced Functionality**:
   - Some modules require API keys for external services (e.g., Shodan, VirusTotal).
   - Configure API keys using:
     ```bash
     keys add <service> <api_key>
     ```

4. **Continuous Practice**:
   - Experiment with different modules and targets to deepen your understanding.
   - **Insight**: Regular practice with Recon-ng builds proficiency in gathering and analyzing intelligence.

---

## **Key Takeaways**
1. Recon-ng is a powerful framework for collecting and managing reconnaissance data.
2. Its modular design allows for flexible and targeted information gathering.
3. Ethical use and adherence to legal guidelines are critical when performing reconnaissance.
