# DNS Server Attack - TryHackMe

## Overview
This writeup documents the process of attacking a DNS server to retrieve the flag from the domain `givemetheflag.com`. The challenge requires us to be on the same network as the target machine and use DNS reconnaissance techniques to extract information.

## Prerequisites
- Access to the target network via:
  - **OpenVPN** (Configuration file required)
  - **Attack Box** (Available by pressing the "Start Attack Box" button)
- Target machine must be running (Press the "Start Machine" button to get the IP)
- Basic knowledge of DNS and reconnaissance tools

## Steps to Attack the DNS Server

### 1. Establishing Network Access
- Connect to the network using **OpenVPN** or the **Attack Box**.
- Start the target machine and note down its **IP address**.

### 2. Understanding the Challenge
- The challenge requires making a special request for `givemetheflag.com` to extract information from the DNS server.
- The target DNS server is hosted at the **IP address** obtained from the running machine.

### 3. Performing DNS Reconnaissance
- We can use **nslookup** or **dig** to query the target DNS server and retrieve the required information.

#### Using `nslookup`
```sh
nslookup givemetheflag.com <TARGET_IP>
```
Example:
```sh
nslookup givemetheflag.com 10.10.43.202
```
- This command queries the DNS server for information about `givemetheflag.com`.
- The output contains the **flag**.

#### Using `dig`
```sh
dig @<TARGET_IP> givemetheflag.com
```
Example:
```sh
dig @10.10.43.202 givemetheflag.com
```
- The `dig` command provides a more detailed output with additional DNS record information.

### 4. Extracting the Flag
- After executing either `nslookup` or `dig`, locate the flag in the response.
- Congratulations! You have successfully completed the challenge.

## Conclusion
This challenge demonstrated how DNS reconnaissance can be used to retrieve domain-related information from a target server. By leveraging tools like `nslookup` and `dig`, we extracted critical information in a real-world penetration testing scenario.

---

### Disclaimer
This writeup is for **educational purposes only**. Unauthorized attacks on networks and servers without permission are illegal and punishable under cybersecurity laws.

