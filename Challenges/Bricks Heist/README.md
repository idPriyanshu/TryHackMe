# Bricks Heist Challenge - TryHackMe

## Overview
This writeup documents the steps taken to exploit a WordPress-based machine on TryHackMe. The attack involved reconnaissance, WordPress vulnerability scanning, remote code execution (RCE), privilege escalation, and analyzing encoded data for final answers.

## Steps to Exploitation

### 1. Modifying the Hosts File
- Added the target IP and site to `/etc/hosts` for proper domain resolution.

### 2. Scanning for Open Ports
- Used **Nmap** to scan for open ports:
  ```sh
  nmap -sC -sV -A <TARGET_IP>
  ```
- Found open ports: **22 (SSH), 80 (HTTP), 443 (HTTPS), and 3306 (MySQL)**.
- Discovered `robots.txt` indicating a **wp-admin** page.

### 3. WordPress Enumeration
- Accessed the website and found the **WordPress admin login page**.
- Scanned the site using **WPScan**:
  ```sh
  wpscan --url http://example.com --api-token <YOUR_API_KEY>
  ```
- Encountered an SSL error, so added `--disable-tls-checks` to bypass it.
- Found **multiple vulnerabilities**, including an **RCE exploit**.

### 4. Exploiting RCE in WordPress
- Searched for an exploit for the identified WordPress version.
- Encountered multiple module errors.
- Created a **Python virtual environment** and installed required modules:
  ```sh
  python3 -m venv env
  source env/bin/activate
  pip install <required_modules>
  ```
- Successfully exploited the vulnerability and gained a **shell**.

### 5. Privilege Escalation
- Started by checking running services:
  ```sh
  ps aux
  ```
- Found two suspicious processes:
  - **nm-inet-dialog** (a network manager service)
  - **ubuntu.service** (appeared unusual)
- Discovered **inet.conf** in the network manager.
- Extracted **two encoded strings** from `inet.conf`.

### 6. Decoding the Encoded Strings
- Used **CyberChef** to decode the strings.
- Found that the output contained a **Bech32 Bitcoin address**.
- Recognized it as a **Native Segwit** address (bc1 prefix used for deposits).
- Checked the **Blockchain Explorer**:
  ```sh
  https://www.blockchain.com/explorer/search?search=<BITCOIN_ADDRESS>
  ```
- Retrieved a **transaction code**.
- Googled the transaction code and found a match on **Open Sanctions**:
  ```sh
  https://www.opensanctions.org/entities/ofac-4d99af7cc172df2b9fbb833cb5af39dbbc011a2c/
  ```
- The final answer was **Lockbit**, a known ransomware group.

## Conclusion
This room provided hands-on experience with:
- **Nmap Scanning**
- **WordPress Enumeration (WPScan)**
- **Exploiting WordPress RCE**
- **Privilege Escalation Techniques**
- **Blockchain Analysis & Open Sanctions Research**

## Final Notes
Thanks for reading this write-up! I hope you found it insightful and helpful in understanding real-world exploitation scenarios.

---

### Disclaimer
This write-up is for **educational purposes only**. Unauthorized testing or attacks on websites without permission is illegal and punishable under cybersecurity laws.

