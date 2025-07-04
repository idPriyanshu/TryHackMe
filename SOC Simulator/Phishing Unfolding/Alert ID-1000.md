# 🛡️ SOC Case Report – Alert ID: 1000

## 📌 Alert Overview

**Alert Title:** Suspicious Email from External Domain  
**Alert ID:** 1000  
**Category:** Phishing  
**Severity:** Low  
**Date Detected:** July 4, 2025  
**Detection Source:** Email Gateway  
**Direction:** Inbound  
**Detection Note:** Detection rule flagged due to external sender with unusual top-level domain (`.online`)

---

## 🧠 What Was Detected?

A suspicious email was received by an internal employee (`miguel.odonnell@tryhatme.com`) from an external sender with the email address `boone@hatventuresworldwide.online`. The subject line contained classic phishing lure language:

> "You've Won a Free Trip to Hat Wonderland – Click Here to Claim"

No attachments were present, and the message body was redacted due to internal data handling policies.

---

## 🔍 How the Case Was Investigated

### Step 1: Initial Alert Analysis
- Queried logs for the alert-triggering email
- Validated that the sender domain was external and uncommon
- Checked for known phishing language in the subject line

### Step 2: Outbound Behavior Correlation
- Queried email logs (`sourcetype=email`) for any outbound messages by `miguel.odonnell@tryhatme.com` after the alert timestamp
- Detected multiple outbound messages with phishing-style subjects sent within 20 minutes

### Step 3: IOC and Campaign Linkage
- Identified reuse of subject lines similar to other SOC alerts
- Compared timestamps with other users to determine campaign spread
- Started tracking other potentially compromised users (Ashwin Johnston, Cain O’Moore)

---

## 🔬 Key Findings

### Time of Activity:
- Start: 07/04/2025 06:58:57 IST (inbound phishing email received)
- End: 07/04/2025 07:18:51 IST (last suspicious outbound message sent)

### Affected Internal User:
- `miguel.odonnell@tryhatme.com`

### External Recipients Targeted by Compromised Account:
- `silas@styleinfluencerhub.info`
- `mark@hatcouturecompany.net`
- `oskar@chicchapeauconclave.com`
- `caroline@fashionhatchronicle.com`

### Outbound Phishing Subject Lines:
- "Exclusive Discount: Buy One Low-Quality Product, Get 100 More!"
- "FWD: Upcoming Trade Show Attendance: Meet our Hat Experts"
- "RE: RE: Interview Request for Company Spotlight Feature Article"

### Attack Indicators (IOCs):
- **Sender Domain:** `hatventuresworldwide.online`
- **Phishing Subject (Inbound):** "You've Won a Free Trip to Hat Wonderland – Click Here to Claim"
- **Direction:** Inbound → Outbound (spam propagation)

---

## ✅ Classification: True Positive

**Reasoning:**
- Clear phishing pattern observed
- Compromised user sent multiple external phishing-style messages
- Behavior aligned with other confirmed phishing accounts
- Matches known phishing subject reuse across cases

---

## 🚨 Escalation: Required

**Escalation Justification:**
- Account compromise confirmed
- Third-party reputational and security risk
- Tied to a broader phishing campaign across multiple employees

---

## 🛠️ Remediation Actions Taken

- Account disabled and isolated
- Password reset and session invalidation
- Sender domain blacklisted at the gateway
- External recipients flagged for notification
- Additional cases (Ashwin, Cain) under investigation
- IOC report generated for SOC team

---

**Status:** Closed (Initial Case Completed)  

---
