# 🔐 TryHackMe – W1seGuy CTF Walkthrough

> **Category:** Cryptography / Reverse Engineering
> **Difficulty:** Easy-Medium
> **Platform:** TryHackMe
> **Author of Writeup:** Priyanshu Singh
> **Date Solved:** July 2025

---

## 🧐 Overview

The **W1seGuy** CTF is a classic reverse engineering challenge that involves understanding and breaking a simple **XOR-based encryption** mechanism to retrieve two flags. The room provides a Python source file that simulates a TCP server generating encrypted messages using a randomly chosen key.

The objective is to:

1. Analyze the encryption logic.
2. Derive the 5-character XOR key.
3. Decrypt the XOR-encoded message to reveal Flag 1.
4. Submit the recovered key to the server to receive Flag 2.

---

## 📁 Files Provided

* `source.py`: Contains the Python server logic that encrypts the flag using XOR and serves it over TCP.

---

## 🧪 Code Analysis Summary

The key logic from the provided source is:

```python
for i in range(0,len(flag)):
    xored += chr(ord(flag[i]) ^ ord(key[i%len(key)]))
```

This tells us:

* A random 5-character key is generated (`res = ''.join(random.choices(..., k=5))`)
* The key repeats every 5 characters.
* The fake flag `THM{thisisafakeflag}` is XORed with the key.
* The result is hex-encoded and sent to the client.
* If the client correctly sends the key back, they receive the real flag.

---

## ⚙️ Exploit Strategy

### 🔍 Observations:

* All flags start with `THM{` and end with `}` — a strong known-plaintext pattern.
* Since XOR is reversible, we can **extract the encryption key** using the encrypted hex output and known plaintext.

### 🧠 XOR Key Recovery Logic:

We XOR the first 4 bytes of the encrypted hex with `'T'`, `'H'`, `'M'`, `'{'` respectively, and the last byte with `'}'`, because the key repeats every 5 characters.

---

## 🗞️ Decryption Script (Python)

```python
def recover_key_from_partial_plaintext(hex_cipher):
    cipher_bytes = bytes.fromhex(hex_cipher)

    known_positions = {
        0: 'T',
        1: 'H',
        2: 'M',
        3: '{',
        -1: '}'
    }

    key = [0] * 5
    for i, plain_char in known_positions.items():
        cipher_index = i if i >= 0 else len(cipher_bytes) + i
        key_index = cipher_index % 5
        key[key_index] = cipher_bytes[cipher_index] ^ ord(plain_char)

    key_str = ''.join(chr(b) for b in key)
    return key_str

def decrypt_with_key(hex_cipher, key):
    cipher_bytes = bytes.fromhex(hex_cipher)
    return ''.join(chr(cipher_bytes[i] ^ ord(key[i % len(key)])) for i in range(len(cipher_bytes)))

if __name__ == "__main__":
    hex_cipher = input("Enter the hex:\n").strip()
    key = recover_key_from_partial_plaintext(hex_cipher)
    print(f"[+] Recovered key: {key}")
    print(f"[+] Decrypted Flag 1: {decrypt_with_key(hex_cipher, key)}")
```

---

## 🚀 Example Execution

```bash
$ nc 10.10.38.145 1337
This XOR encoded text has flag 1:
241e013619413720231d352e380c1d04622f260a31383e7e081c1a35253c0222357d1c022e033f14

Enter the hex: 241e013619413720231d352e380c1d04622f260a31383e7e081c1a35253c0222357d1c022e033f14
[+] Recovered key: *****
[+] Decrypted Flag 1: THM{***********************}

# Input the key when prompted:
What is the encryption key? *****
Congrats! That is the correct key! Here is flag 2: THM{*******************************}
```

---

## 🏁 Flags

* **Flag 1:** `THM{***********************}`
* **Flag 2:** `THM{*******************************}`

---

## 📚 Key Concepts Used

| Concept                | Explanation                                       |
| ---------------------- | ------------------------------------------------- |
| XOR Encryption         | Reversible logic where A ⊕ B = C ⇒ A = C ⊕ B      |
| Known-Plaintext Attack | Used known flag structure to reverse engineer key |
| Python scripting       | Automating XOR decryption                         |
| Netcat                 | Manual interaction with TCP server                |
| CyberChef (optional)   | Visual reverse XOR and encoding/decoding          |

---

## 💡 Lessons Learned

* XOR encryption with a repeating key is **weak** against known-plaintext attacks.
* Always assume attackers can guess patterns like flag formats.
* Static code analysis + scripting = 🔓 powerful combination.

---

## 🏷️ Tags

`#cryptography` `#ctf` `#tryhackme` `#python` `#xorencryption` `#xordecryption` `#reverseengineering` `#w1seguy`

---

## ✍️ Author

**Priyanshu Singh**
Cybersecurity | Reverse Engineering | CTF Enthusiast
📍 Indian Institute of Information Technology, Kottayam
🔗 [LinkedIn](https://linkedin.com/in/priyanshu-singh-16462026b) | [GitHub](https://github.com/idPriyanshu)
