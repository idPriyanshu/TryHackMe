
## TryHackMe: TakeOver CTF Walkthrough

## 📘 Summary

This room is based on identifying a **subdomain takeover** vulnerability. The scenario simulates a company infrastructure migration and challenges you to exploit misconfigured DNS or cloud storage services.

---

## 📂 Table of Contents

- [🔎 Nmap Scan](#-nmap-scan)
- [🌐 Subdomain Enumeration](#-subdomain-enumeration)
- [🔐 Inspecting SSL Certificate](#-inspecting-ssl-certificate)
- [☁️ AWS Takeover Flag](#️-aws-takeover-flag)
- [📚 Conclusion](#-conclusion)

---

## 🔎 Nmap Scan

We begin with a simple port scan using `nmap` to identify which services are running on the target domain:

```bash
nmap futurevera.thm -oN nmapResults.txt
````

**Results:**

```
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
443/tcp open  https
```

Only basic web and SSH services are open. Our focus will be on `HTTP` and `HTTPS`.

---

## 🌐 Subdomain Enumeration

I started enumerating possible subdomains using `gobuster` with the `SecLists` DNS wordlists:

```bash
gobuster vhost -u http://futurevera.thm -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -t 50 -o subdomains.txt
```

This led me to discover the subdomain:

```bash
support.futurevera.thm
```

To access it properly, I added it to `/etc/hosts`:

```bash
10.10.125.247 futurevera.thm support.futurevera.thm
```

Visiting `https://support.futurevera.thm`, we’re met with a web interface that appears under construction.

---

## 🔐 Inspecting SSL Certificate

While inspecting the SSL certificate of `support.futurevera.thm`, I noticed it included an **additional subdomain** in the `Subject Alternative Name (SAN)` section.

> 💡 Always check certificates for hidden subdomains!

This was the critical discovery. I added the new subdomain into `/etc/hosts`:

```bash
10.10.125.247 futurevera.thm support.futurevera.thm secretXYZ.support.futurevera.thm
```

> (The actual secret subdomain is masked here for demonstration)

---

## ☁️ AWS Takeover Flag

Accessing the new subdomain via `http://secretXYZ.support.futurevera.thm`, I got redirected to an **unclaimed AWS S3 bucket URL**, like:

```
http://flag{example_flag_here}.s3-website-us-west-3.amazonaws.com
```

Even though the bucket was unclaimed (404 Not Found), the **flag** was embedded in the URL — indicating the vulnerable subdomain pointing to an unconfigured S3 resource. That’s a textbook subdomain takeover!

---

## 🧠 What is a Subdomain Takeover?

A **Subdomain Takeover** occurs when a subdomain is pointing to a third-party service (e.g., AWS, GitHub Pages), but that service is not properly claimed or configured. Attackers can **claim ownership** of the dangling resource and host malicious content.

### 🧠 Learn More:

* [HackTricks - Subdomain Takeover](https://book.hacktricks.xyz/pentesting-web/domain-subdomain-takeover)
* [HackerOne Guide](https://www.hackerone.com/application-security/guide-subdomain-takeovers)
* [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/Security/Subdomain_takeovers)

---

## 📚 Conclusion

This room taught me how subdomain takeovers occur and how misconfigured cloud services or forgotten DNS entries can be exploited. Using tools like:

* `gobuster` + `SecLists` for vhost enumeration
* SSL certificate inspection
* Manual redirection tracking

...I successfully discovered and exploited the takeover point.

---

## 🏁 Flag

* ✅ Final Flag: `flag{***********************}` (retrieved from redirected S3 URL)

---

## 👨‍💻 Author

**Priyanshu Singh**
Cybersecurity & CTF Enthusiast
🎓 IIIT Kottayam
🔗 [GitHub](https://github.com/idPriyanshu) | [LinkedIn](https://linkedin.com/in/priyanshu-singh-16462026b)

---

