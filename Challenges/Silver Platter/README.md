# Silver Platter Challenge - TryHackMe

## Challenge Overview
**Silver Platter** is a TryHackMe challenge by **TeneBrae93** where the objective is to breach the server. The challenge involves reconnaissance, brute force attacks, privilege escalation, and identifying vulnerabilities in a Silverpeas web application.

---

## Reconnaissance

### Nmap Scan
We begin with an **Nmap** scan and find three open ports:
- **22** - SSH
- **80** - nginx web server
- **8080** - Another web server

### Directory Enumeration
Using **Gobuster**, we enumerate directories on both web servers.

**Port 80:**
```bash
gobuster -u 'http://silverplatter.thm/' -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt
```
No useful results were found.

**Port 8080:**
```bash
gobuster -u 'http://silverplatter.thm:8080/' -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt
```
We discover two directories:
- **/website** (Redirects to /website/, which is forbidden)
- **/console** (Redirects to a static page: nodirect.html)

We also identify a potential username **scr1ptkiddy** from the **Contact** page.

---

## Web Access and Exploitation

### Silverpeas Login Page
The **Silverpeas** login page is found at:
```bash
http://silverplatter.thm:8080/silverpeas
```
We attempt various bypass methods but ultimately need credentials.

### Brute-Forcing Login
Since we have the username, we generate a password list from the main webpage using **Cewl**:
```bash
cewl http://silverplatter.thm > passwords.txt
```
Then, we use **Hydra** to brute-force the login:
```bash
hydra -l scr1ptkiddy -P passwords.txt silverplatter.thm -s 8080 http-post-form "/silverpeas/AuthenticationServlet:Login=^USER^&Password=^PASS^&DomainId=0:F=Login or password incorrect"
```
We successfully retrieve credentials and log in as **scr1ptkiddy**.

---

## Privilege Escalation

### Gaining Shell as Tim
Inside **Silverpeas**, we find a **notification system**.
- Inspecting message **ID 5** reveals an **IDOR vulnerability**.
- Message **ID 6** contains **SSH credentials for user tim**.

We use these credentials to log in:
```bash
ssh tim@silverplatter.thm
```
We retrieve the **user flag** from Tim’s home directory.

### Gaining Shell as Tyler
Checking **Tim’s groups**, we find that he belongs to the **adm** group, allowing access to logs:
```bash
cat /var/log/auth.log | grep tyler
```
We uncover **database credentials** and attempt to reuse them to switch users:
```bash
su tyler
```
We successfully log in as **tyler**.

### Root Privilege Escalation
Checking **sudo permissions**:
```bash
sudo -l
```
Tyler can execute commands **as root without a password**. We escalate privileges with:
```bash
sudo su
```
Now, as **root**, we retrieve the final flag:
```bash
cat /root/root.txt
```

---

## Unintended Paths

1. **Silverpeas Login Bypass**: Instead of brute-forcing, we can **omit the password field** in the request and log in successfully.
2. **Logging in as Manager**: Using the same login bypass method, we gain access to **Manager**, retrieve Tim’s SSH credentials, and proceed with the escalation.

---

## Conclusion
The **Silver Platter** challenge involves:
- **Reconnaissance**: Identifying services and users.
- **Brute-force attacks**: Cracking the web login.
- **IDOR vulnerability exploitation**: Retrieving credentials.
- **Privilege escalation**: Moving from user to root.

By following these steps, we successfully breach the server and obtain the final flag. 🚀

