---
title: Cryptography
tags: [Computer Science]

---

Cryptography is intrinsically linked to the **CIA** Triad (**Confidentiality**, **Integrity**, and **Availability**) because it serves as a primary method for achieving these fundamental information security goals. Cryptography, through encryption and decryption, directly supports confidentiality by making data unreadable to unauthorized parties. It also enhances integrity by using techniques like hash functions and digital signatures to detect unauthorized modifications and ensure data authenticity. While cryptography doesn't directly address availability like network redundancy, it indirectly contributes by securing data from breaches that could render it inaccessible.

**How Cryptography relates to each part of the CIA Triad:**
* **Confidentiality (What it is):** Protecting sensitive information from unauthorized disclosure.
**Cryptography's Role:** Cryptography, specifically encryption, transforms plain text into unreadable ciphertext. Only those with the correct decryption key can revert it back to its original form, ensuring that even if unauthorized users gain access to the data, they cannot understand it. 

* **Integrity (What it is):** Ensuring data is accurate, complete, and trustworthy by preventing unauthorized modification.
**Cryptography's Role:** Cryptographic hash functions create a unique "fingerprint" of the data. Any alteration to the data, even a minor one, will result in a different hash value, making tampering evident. Digital signatures, also a cryptographic technique, use private keys to sign data, allowing the recipient to verify the sender's identity and that the data hasn't been changed since it was signed. 

* **Availability (What it is):** Ensuring that systems and data are accessible and usable by authorized individuals when needed.
**Cryptography's Role:** While not its primary function, cryptography indirectly supports availability. By securing systems and data from breaches that could lead to denial-of-service attacks or data loss, cryptography helps maintain the necessary access and uptime for users to access information when required. 

---


| Function | Purpose |
| -------- | -------- |
| Encryption     | Message Confidentiality     |
| Authentication     | User Identification     |
| Hashing     | Message Integrity     |



# Encryption
## Wi-Fi Encryption
* **Purpose:** To scramble data so it is unreadable to anyone who is not a legitimate participant on the network. 
* **How it works:** Uses algorithms to transform data into a secret code. Only devices with the correct encryption keys can decrypt and read the information. 
* **Examples:**
**AES (Advanced Encryption System):** A strong encryption standard used in WPA2 and WPA3 to protect data confidentiality. 
**WEP (Wired Equivalent Privacy):** An older, now-vulnerable standard that uses static keys and is no longer recommended due to its weak security. 

# Authentication
## Wi-Fi Authentication
* **Purpose:** To ensure that only authorized users and devices are permitted to join the Wi-Fi network. 
* **How it works:** Typically involves a password or network key that users must provide to connect to the network. More advanced methods, like those in WPA2-Enterprise, use unique user identifiers for enhanced security. 
* **Examples:**
**WPA/WPA2/WPA3-Personal:** Users enter a pre-shared password to access the network. 
**WPA2/WPA3-Enterprise:** Uses RADIUS servers and digital certificates to provide more robust, unique authentication for each user, common in business environments. 

# Hashing
Distinct cryptographic hash functions, not encryption algorithms, that produce a unique fixed-size "fingerprint" of data, called a hash or digest.
* MD5
* SHACryptography is intrinsically linked to the **CIA** Triad (**Confidentiality**, **Integrity**, and **Availability**) because it serves as a primary method for achieving these fundamental information security goals. Cryptography, through encryption and decryption, directly supports confidentiality by making data unreadable to unauthorized parties. It also enhances integrity by using techniques like hash functions and digital signatures to detect unauthorized modifications and ensure data authenticity. While cryptography doesn't directly address availability like network redundancy, it indirectly contributes by securing data from breaches that could render it inaccessible.

**How Cryptography relates to each part of the CIA Triad:**
* **Confidentiality (What it is):** Protecting sensitive information from unauthorized disclosure.
**Cryptography's Role:** Cryptography, specifically encryption, transforms plain text into unreadable ciphertext. Only those with the correct decryption key can revert it back to its original form, ensuring that even if unauthorized users gain access to the data, they cannot understand it. 

* **Integrity (What it is):** Ensuring data is accurate, complete, and trustworthy by preventing unauthorized modification.
**Cryptography's Role:** Cryptographic hash functions create a unique "fingerprint" of the data. Any alteration to the data, even a minor one, will result in a different hash value, making tampering evident. Digital signatures, also a cryptographic technique, use private keys to sign data, allowing the recipient to verify the sender's identity and that the data hasn't been changed since it was signed. 

* **Availability (What it is):** Ensuring that systems and data are accessible and usable by authorized individuals when needed.
**Cryptography's Role:** While not its primary function, cryptography indirectly supports availability. By securing systems and data from breaches that could lead to denial-of-service attacks or data loss, cryptography helps maintain the necessary access and uptime for users to access information when required. 

---


| Function | Purpose |
| -------- | -------- |
| Encryption     | Message Confidentiality     |
| Authentication     | User Identification     |
| Hashing     | Message Integrity     |



# Encryption
## Wi-Fi Encryption
* **Purpose:** To scramble data so it is unreadable to anyone who is not a legitimate participant on the network. 
* **How it works:** Uses algorithms to transform data into a secret code. Only devices with the correct encryption keys can decrypt and read the information. 
* **Examples:**
**AES (Advanced Encryption System):** A strong encryption standard used in WPA2 and WPA3 to protect data confidentiality. 
**WEP (Wired Equivalent Privacy):** An older, now-vulnerable standard that uses static keys and is no longer recommended due to its weak security. 

# Authentication
## Wi-Fi Authentication
* **Purpose:** To ensure that only authorized users and devices are permitted to join the Wi-Fi network. 
* **How it works:** Typically involves a password or network key that users must provide to connect to the network. More advanced methods, like those in WPA2-Enterprise, use unique user identifiers for enhanced security. 
* **Examples:**
**WPA/WPA2/WPA3-Personal:** Users enter a pre-shared password to access the network. 
**WPA2/WPA3-Enterprise:** Uses RADIUS servers and digital certificates to provide more robust, unique authentication for each user, common in business environments. 

# Hashing
Distinct cryptographic hash functions, not encryption algorithms, that produce a unique fixed-size "fingerprint" of data, called a hash or digest.
* MD5
* SHA