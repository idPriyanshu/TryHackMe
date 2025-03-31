# TryHackMe - Neighbour Challenge Write-up

## Overview
The "Neighbour" challenge on TryHackMe focuses on exploiting an **Insecure Direct Object Reference (IDOR)** vulnerability to gain unauthorized access to another user's profile.

---

## Steps to Solve

### 1. Access the Login Page
- Navigate to the machine's IP address provided by TryHackMe.
- You will be greeted with a login form.

### 2. Inspect the Page Source for Credentials
- On the login page, press `Ctrl + U` or right-click and select **View Page Source**.
- Search for any hints or credentials.
- You will find guest credentials hidden in the source code:
  ```
  Username: guest
  Password: guest
  ```

### 3. Log in with Guest Credentials
- Enter `guest` as both username and password to log in.
- You will be redirected to your profile page.

### 4. Identify the IDOR Vulnerability
- Observe the URL after logging in, which looks like:
  ```
  http://<Machine_IP>/profile.php?user=guest
  ```
- The `user` parameter is being passed in the URL, which could indicate an IDOR vulnerability.

### 5. Exploit the IDOR Vulnerability
- Modify the `user` parameter in the URL from `guest` to `admin`:
  ```
  http://<Machine_IP>/profile.php?user=admin
  ```
- Press **Enter** and check if you can access the admin's profile.
- If successful, you have exploited an **IDOR vulnerability**.

### 6. Retrieve the Flag
- On the admin's profile page, look for the flag.
- Copy the flag and submit it on TryHackMe to complete the challenge.

---

## Conclusion
The **Neighbour Challenge** demonstrates how **Insecure Direct Object Reference (IDOR)** can allow unauthorized access to sensitive information. Always ensure proper access controls and user authentication to mitigate such vulnerabilities.

---

## References
- [TryHackMe Neighbour Challenge](https://tryhackme.com/)
- [Insecure Direct Object References (IDOR) Explained](https://owasp.org/www-community/attacks/Insecure_Direct_Object_Reference)

Happy Hacking! 🚀
