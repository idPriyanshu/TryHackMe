# 🛡️ SOC Alert Case Report – Alert ID: 1009

## Alert Summary
- **Alert ID:** 1009  
- **Alert Title:** Reply to suspicious email  
- **Severity:** Low  
- **Category:** Phishing  
- **Date Detected:** July 4th, 2025 at 07:16  
- **Detection Source:** Email logs  
- **Direction:** Outbound  
- **Sender:** yani.zubair@tryhatme.com  
- **Recipient:** conor@modernmillinerygroup.online  
- **Subject:** Unlock Ancient Hat Secrets with This Ancient Pyramid Scheme  

---

## Time of Activity
- **Timestamp:** 07/04/2025 07:14:21.300

---

## List of Affected Entities
- Internal User: `yani.zubair@tryhatme.com`  
- External Recipient: `conor@modernmillinerygroup.online`  
- Infrastructure: Email system (MTA logs, content filtering, outbound policies)

---

## Reason for Classifying as True Positive
- The email sent from Yani Zubair's account matched a **subject line used in known phishing campaigns**, observed in multiple prior alerts.
- The subject contained **social engineering indicators** ("pyramid scheme") and **matched other confirmed phishing attempts**.
- Although classified as a "reply," no inbound message from the recipient exists, indicating Yani **initiated** the communication.
- The domain `.online` is unusual and not part of regular business activity.

---

## Reason for Escalating the Alert
- **No escalation required.**
  - No confirmed account compromise (e.g., suspicious logins or mass spam activity).
  - No evidence of malicious payload due to redacted content.
  - Single outbound incident, not indicative of worm-like propagation.
  - Covered under phishing awareness and detection improvement scope.

---

## Recommended Remediation Actions
1. Notify Yani Zubair and provide phishing-awareness guidance.
2. Flag or block the external domain `modernmillinerygroup.online` if not business-related.
3. Fine-tune the detection rule to better identify real replies vs. new outbound messages.
4. Monitor Yani’s account for further anomalous activity.
5. Correlate with future alerts using the same subject pattern to identify phishing campaigns.

---

## List of Attack Indicators
- **Subject:** Unlock Ancient Hat Secrets with This Ancient Pyramid Scheme
- **External Domain:** modernmillinerygroup.online
- **Outbound Message Without Prior Correspondence**
- **TLD Pattern:** `.online` associated with suspicious messages across multiple alerts

---

## Alert Status: Closed – True Positive (No Escalation)

---