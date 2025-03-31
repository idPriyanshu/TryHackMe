# Pickle Rick Challenge - TryHackMe

## Overview
The **Pickle Rick Challenge** on **TryHackMe** is a Rick and Morty-themed beginner-friendly CTF that involves exploiting a web server to find three ingredients. These ingredients will help Rick transform back into a human.

## Steps to Solve

### 1. Deploy the Machine and Start Enumeration
Start by scanning the target machine using **Nmap** to identify open ports and running services:
```bash
sudo nmap -A -T4 -v -oN scan_results <TARGET_IP>
```
This typically reveals an HTTP server running on port 80.

### 2. Web Enumeration
- Open the IP in a browser: `http://<TARGET_IP>:80`.
- Inspect the webpage source code and check **robots.txt** for hidden files:
  ```bash
  curl http://<TARGET_IP>/robots.txt
  ```
- Use **GoBuster** to brute-force directories and files:
  ```bash
  gobuster dir -u http://<TARGET_IP> -x php,html,css,js,txt,pdf -w <WORDLIST>
  ```
- This reveals three files: **login.php, portal.php, and robots.txt**.

### 3. Finding Login Credentials
- The **robots.txt** file contains `Wubbalubbadubdub`, which appears to be a password.
- The source code reveals a username: `R1ckRul3s`.
- Use these credentials to log in to **portal.php**:
  - **Username:** R1ckRul3s
  - **Password:** Wubbalubbadubdub

### 4. Exploiting the Web Shell
- After logging in, the **portal.php** page provides a **command execution panel**.
- Test Linux commands like `ls` to explore the system.

### 5. Retrieving the First Ingredient
- List files in the current directory:
  ```bash
  ls -a
  ```
- This reveals `Sup3rS3cretPickl3Ingred.txt`.
- Since `cat` is disabled, use alternative commands to read the file:
  ```bash
  tac Sup3rS3cretPickl3Ingred.txt
  grep . Sup3rS3cretPickl3Ingred.txt
  ```
- **First Ingredient:** `mr. meeseek hair`

### 6. Finding the Second Ingredient
- Check the `clue.txt` file, which hints at looking in `/home/`.
- List user directories:
  ```bash
  ls /home
  ```
- The user `rick` has a file named `second ingredients`.
- Read its contents:
  ```bash
  tac /home/rick/second\ ingredients
  ```
- **Second Ingredient:** `1 jerry tear`

### 7. Escalating Privileges for the Third Ingredient
- Check user privileges:
  ```bash
  sudo -l
  ```
- The `www-data` user can run commands as root without a password.
- List files in the `/root/` directory:
  ```bash
  sudo ls /root
  ```
- This reveals `3rd.txt`. Read its contents:
  ```bash
  sudo cat /root/3rd.txt
  ```
- **Third Ingredient:** `fleeb juice`

## Conclusion
We successfully exploited the webserver, gained access to a command shell, and escalated privileges to retrieve all three ingredients. This challenge provided hands-on experience in web enumeration, command execution, and privilege escalation, making it a great introductory CTF for cybersecurity learners.
