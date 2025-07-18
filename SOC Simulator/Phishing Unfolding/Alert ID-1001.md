# 🛡️ SOC Case Report – Alert ID: 1001

## 📌 Alert Overview

**Alert Title:** Suspicious Email from External Domain  
**Alert ID:** 1001  
**Category:** Phishing  
**Severity:** Low  
**Date Detected:** July 4, 2025  
**Detection Source:** Email Gateway  
**Direction:** Inbound  
**Detection Note:** Alert triggered due to inbound email from an external domain with unusual TLD (`.de`)

---

## 🧠 What Was Detected?

A suspicious email was received by internal user `michelle.smith@tryhatme.com` from the external address `maximillian@chicmillinerydesigns.de`. The subject was:

> "VIP Hat Resort Stay: Your Dream Vacation Awaits, Just Pay Shipping"

There were no attachments, and the email body was withheld due to privacy and security policy.

---

## 🔍 How the Case Was Investigated

### Step 1: Email Flow Review
- Queried outbound messages from `michelle.smith@tryhatme.com` after the phishing email
- No outbound phishing messages observed

### Step 2: Log Correlation
- Reviewed internal and external communication logs before and after the alert
- All communication aligned with normal internal workflows or legitimate business correspondence

### Step 3: IOC Cross-Match
- Compared sender and subject to known IOCs from related alerts (e.g., Alert 1000)
- No reuse of known phishing subjects or behavioral indicators found

### Step 4: Additional Context Checks
- No abnormal login activity observed
- No sandbox verdicts or click events linked to this email
- No additional alerts involving the user

---

## 🔬 Key Findings

### Time of Activity:
- 07/04/2025 06:59:57 IST (email received)

### Related Entities:
- **Internal User:** `michelle.smith@tryhatme.com`
- **External Sender:** `maximillian@chicmillinerydesigns.de`

### Email Subject:
- "VIP Hat Resort Stay: Your Dream Vacation Awaits, Just Pay Shipping"

### Outcome:
- No outbound phishing activity
- No signs of compromise or malicious behavior

---

## ✅ Classification: False Positive

**Reasoning:**
- The user did not interact with the phishing email in a suspicious way
- No propagation, no forwarding, and no reuse of attacker language
- Account behavior aligned with standard patterns
- Alert rule is known to need fine-tuning, as acknowledged in the alert note

---

**Status:** Closed (False Positive)  
