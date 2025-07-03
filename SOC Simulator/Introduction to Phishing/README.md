# 🛡️ SOC Simulator Challenge: Email-Based Threat Investigations

This repository contains the documented investigations of **four email security alerts** from a SOC (Security Operations Center) simulator environment. The investigations involve analyzing potentially malicious emails, verifying user interaction, inspecting network logs, and classifying the alerts appropriately based on available evidence.

---

## 📆 Alerts Overview

| Alert ID | Title                                                  | Category | Severity | Verdict                         |
| -------- | ------------------------------------------------------ | -------- | -------- | ------------------------------- |
| 8814     | Inbound Email Containing Suspicious External Link      | Phishing | Medium   | False Positive                  |
| 8815     | Inbound Email Containing Suspicious External Link      | Phishing | Medium   | Confirmed Phishing (Blocked)    |
| 8816     | Access to Blacklisted External URL Blocked by Firewall | Firewall | High     | Confirmed Malicious URL Blocked |
| 8817     | Inbound Email Containing Suspicious External Link      | Phishing | Medium   | Confirmed Phishing (Allowed)    |

## ✅ Solved Alert: 8814 – Suspicious External Link in Inbound Email

### 📨 Alert Details

* **Timestamp**: 2025-07-03 06:46:11
* **Recipient**: [j.garcia@thetrydaily.thm](mailto:j.garcia@thetrydaily.thm)
* **Sender**: [onboarding@hrconnex.thm](mailto:onboarding@hrconnex.thm)
* **Subject**: Action Required: Finalize Your Onboarding Profile
* **Attachment**: None
* **URL**: [https://hrconnex.thm/onboarding/15400654060/j.garcia](https://hrconnex.thm/onboarding/15400654060/j.garcia)
* **Direction**: Inbound

### 🗭 Investigation Workflow

#### 1. **Email Review**

* Inspected email body and metadata using Splunk search queries.
* Identified the use of a suspicious external link with impersonation of HR systems.

#### 2. **Domain and Link Analysis**

* Domain `hrconnex.thm` mimicked HR function but was not recognized as a trusted source.
* Link used urgency and social engineering.

#### 3. **Proxy/Firewall Log Search**

* No evidence found in logs indicating user `j.garcia` clicked or accessed the URL.

#### 4. **URL Scan**

* Used internal URL scanning tool available in analyst VM.
* Result: No malware, phishing, or active threats detected at the time of analysis.

#### 5. **User Review**

* User identified as a standard employee (non-VIP, active).
* No suspicious follow-up activity recorded.

### 💾 Verdict

> **False Positive**: Although the email used phishing techniques, no user interaction or threat indicators were found. URL was clean and not active malicious content.

### 🛡️ Actions Taken

* Classified as phishing and blocked.
* User notified; no compromise detected.
* Domain and shortened link added to email and network blocklists.

### ⏰ Time of Activity

* 2025-07-03 06:47:20

### 📍 List of Affected Entities

* User: [h.harris@thetrydaily.thm](mailto:h.harris@thetrydaily.thm)
* Sender: [urgents@amazon.biz](mailto:urgents@amazon.biz)
* URL: [http://bit.ly/3sHkX3da12340](http://bit.ly/3sHkX3da12340)

### ✅ Reason for Classifying as True Positive

* Email content leveraged urgency and impersonation of a trusted brand.
* URL was confirmed as **malicious** by internal scanner.
* User clicked the link, confirming interaction.
* Firewall blocked the outbound request, preventing compromise.

### 📈 Reason for Escalating the Alert

* Malicious link present and interacted with.
* Could have led to user credential theft or malware delivery if not blocked.
* Demonstrates a targeted phishing attempt with click engagement.

### 🛠️ Recommended Remediation Actions

* Ensure bit.ly domains are flagged and sandboxed automatically.
* Conduct phishing awareness training for user `h.harris`.
* Monitor user's endpoint for any delayed payloads.
* Add URL and domain to threat intelligence and SIEM alert rules.

### 🧾 List of Attack Indicators

* Sender domain: `amazon.biz`
* Suspicious shortened link: `http://bit.ly/3sHkX3da12340`
* Redirection to malicious site
* Email theme: urgent package delivery failure
* Case documented in SOC portal.
* User not impacted; no containment necessary.
* Email classified as phishing-style social engineering, but not an active threat.

---

## ✅ Solved Alert: 8815 – Amazon Package Phishing Attempt

### 📨 Alert Details

* **Timestamp**: 2025-07-03 06:47:20
* **Recipient**: [h.harris@thetrydaily.thm](mailto:h.harris@thetrydaily.thm)
* **Sender**: [urgents@amazon.biz](mailto:urgents@amazon.biz)
* **Subject**: Your Amazon Package Couldn’t Be Delivered – Action Required
* **Attachment**: None
* **URL**: [http://bit.ly/3sHkX3da12340](http://bit.ly/3sHkX3da12340)
* **Direction**: Inbound

### 🗭 Investigation Workflow

#### 1. **Email Review**

* Analyzed the email content and identified a shortened URL leading to an external site.
* Email text leveraged a fake delivery failure and urgency to entice a click.

#### 2. **Link and Domain Analysis**

* The link used a `bit.ly` shortener and redirected to a **malicious site** (confirmed via internal URL scanner).

#### 3. **Firewall Log Search**

* Searched firewall/proxy logs for outbound attempts to the shortened URL.
* Found an access attempt from the user `h.harris`.
* **Connection was blocked** by the firewall.

#### 4. **User Review**

* `h.harris` is a standard user.
* No suspicious activity observed beyond the blocked attempt.

### 💾 Verdict

> **Confirmed Phishing Attempt** – The email contained a malicious external link. The user attempted to click, but the firewall successfully blocked the connection.

### 🛡️ Actions Taken

* Classified as phishing and blocked.
* User notified; no compromise detected.
* Domain and shortened link added to email and network blocklists.

### ⏰ Time of Activity

* 2025-07-03 06:47:20

### 📍 List of Affected Entities

* User: [h.harris@thetrydaily.thm](mailto:h.harris@thetrydaily.thm)
* Sender: [urgents@amazon.biz](mailto:urgents@amazon.biz)
* URL: [http://bit.ly/3sHkX3da12340](http://bit.ly/3sHkX3da12340)

### ✅ Reason for Classifying as True Positive

* Email content leveraged urgency and impersonation of a trusted brand.
* URL was confirmed as **malicious** by internal scanner.
* User clicked the link, confirming interaction.
* Firewall blocked the outbound request, preventing compromise.

### 📈 Reason for Escalating the Alert

* Malicious link present and interacted with.
* Could have led to user credential theft or malware delivery if not blocked.
* Demonstrates a targeted phishing attempt with click engagement.

### 🛠️ Recommended Remediation Actions

* Ensure bit.ly domains are flagged and sandboxed automatically.
* Conduct phishing awareness training for user `h.harris`.
* Monitor user's endpoint for any delayed payloads.
* Add URL and domain to threat intelligence and SIEM alert rules.

### 🧾 List of Attack Indicators

* Sender domain: `amazon.biz`
* Suspicious shortened link: `http://bit.ly/3sHkX3da12340`
* Redirection to malicious site
* Email theme: urgent package delivery failure

---

## ✅ Solved Alert: 8816 – Blacklisted URL Blocked by Firewall

### 🔥 Alert Details

* **Timestamp**: 2025-07-03 06:48:34
* **Datasource**: Firewall
* **Action**: Blocked
* **Source IP**: 10.20.2.17
* **Destination IP**: 67.199.248.11
* **URL**: [http://bit.ly/3sHkX3da12340](http://bit.ly/3sHkX3da12340)
* **Application**: web-browsing
* **Protocol**: TCP

### 🗭 Investigation Workflow

#### 1. **URL Analysis**

* The URL is a shortened `bit.ly` link confirmed to redirect to a **malicious website** using internal scanning tools.

#### 2. **Firewall Logs**

* Verified in firewall logs that an outbound attempt was made to the malicious URL.
* The firewall successfully **blocked** the request based on threat intelligence feeds.

#### 3. **IP Attribution**

* Identified that source IP `10.20.2.17` is assigned to a single user at the time of activity.
* No further malicious outbound activity from this IP after the alert.

### 💾 Verdict

> **True Positive** – Access attempt to a confirmed malicious URL was detected and blocked.

### 🛡️ Actions Taken

* Alert confirmed and escalated.
* Source IP monitored for further behavior.
* URL added to blocklists and monitored.

### ⏰ Time of Activity

* 2025-07-03 06:48:34

### 📍 List of Affected Entities

* Source IP: 10.20.2.17
* Destination IP: 67.199.248.11
* Malicious URL: [http://bit.ly/3sHkX3da12340](http://bit.ly/3sHkX3da12340)

### ✅ Reason for Classifying as True Positive

* The URL was confirmed to be malicious.
* Outbound request to blacklisted domain was verified in logs.
* Blocked by firewall based on threat intel feeds.

### 📈 Reason for Escalating the Alert

* Prevented delivery of possible malware/phishing payload.
* Indicates user attempted access to known malicious site.
* Useful for internal awareness and training.

### 🛠️ Recommended Remediation Actions

* Flag and monitor source IP for future incidents.
* Add domain to all internal threat blocklists.
* Notify user and reinforce phishing awareness.
* Update SIEM detection rules for similar shortened URL patterns.

### 🧾 List of Attack Indicators

* Malicious URL: [http://bit.ly/3sHkX3da12340](http://bit.ly/3sHkX3da12340)
* Destination IP: 67.199.248.11
* Application: web-browsing
* Protocol: TCP

---

## ✅ Solved Alert: 8817 – Microsoft Phishing Link (Allowed Connection)

### 📨 Alert Details

* **Timestamp**: 2025-07-03 06:49:38
* **Recipient**: [c.allen@thetrydaily.thm](mailto:c.allen@thetrydaily.thm)
* **Sender**: [no-reply@m1crosoftsupport.co](mailto:no-reply@m1crosoftsupport.co)
* **Subject**: Unusual Sign-In Activity on Your Microsoft Account
* **Attachment**: None
* **URL**: [https://m1crosoftsupport.co/login](https://m1crosoftsupport.co/login)
* **Direction**: Inbound

### 🗭 Investigation Workflow

#### 1. **Email Review**

* Inspected the suspicious email claiming a Microsoft sign-in alert.
* The domain `m1crosoftsupport.co` impersonates Microsoft with subtle typosquatting.

#### 2. **URL Analysis**

* Used internal scanner and confirmed the link to be **malicious**.
* Designed to steal user credentials through a fake login page.

#### 3. **Firewall Logs Review**

* Located the firewall log showing the outbound request to the malicious domain.
* Action: **Allowed** – connection to `https://m1crosoftsupport.co/login` was not blocked.

#### 4. **Source IP Mapping**

* IP `10.20.2.25` was used to initiate the connection.
* No further malicious behavior detected from the host after the alert timeframe.

### 💾 Verdict

> **Confirmed Phishing Attempt** – The user accessed a known phishing page impersonating Microsoft. Firewall did not block the connection, but no signs of post-compromise activity were found.

### 🛡️ Actions Taken

* Escalated to ensure user awareness and endpoint monitoring.
* Source IP reviewed for anomalies (none found).
* Domain added to threat blocklist and detection alerts.

### ⏰ Time of Activity

* 2025-07-03 06:50:47

### 📍 List of Affected Entities

* User: [c.allen@thetrydaily.thm](mailto:c.allen@thetrydaily.thm)
* Source IP: 10.20.2.25
* Malicious Domain: m1crosoftsupport.co

### ✅ Reason for Classifying as True Positive

* Email used impersonation and urgency.
* URL was confirmed malicious.
* Connection attempt was made and allowed.

### 📈 Reason for Escalating the Alert

* Phishing domain was contacted.
* Could lead to credential theft if user interacted with the page.
* Firewall allowed the connection — exposure occurred.

### 🛠️ Recommended Remediation Actions

* Notify and train user regarding phishing impersonation attempts.
* Conduct a scan of the user’s machine for any malicious artifacts.
* Add domain to global blocklist and SIEM detection rules.
* Implement stricter URL filtering policies.

### 🧾 List of Attack Indicators

* Phishing Domain: m1crosoftsupport.co
* Malicious URL: [https://m1crosoftsupport.co/login](https://m1crosoftsupport.co/login)
* Source IP: 10.20.2.25
* Application: web-browsing
* Protocol: TCP

## 📚 Skills Demonstrated

* Splunk log analysis (email, proxy)
* Threat identification and IOC correlation
* URL reputation analysis and redirect tracing
* Case documentation and response classification
---