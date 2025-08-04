---
title: "Credential Disclosure via Attacker-Controlled Endpoint in IBM Cloud Pak for Data (CP4D)"
description: "Credential Disclosure via Attacker-Controlled Endpoint in IBM Cloud Pak for Data (CP4D)."
date: "2025-04-01"
categories: [PoC]
tags: [Vulnerability, IBM, CP4D, Information Disclosure]
pin: false
math: true
mermaid: true
image:
  path: "/assets/img/cp4d/ibm-cp4d.png"
  alt: "CP4D Credential Disclosure"
---

### **Disclosure Context:**

This vulnerability was **responsibly reported to IBM during a security assessment in April 2023**. Despite multiple attempts to coordinate, **IBM has not issued a CVE ID after more than two years**.

Given the extended delay and the lack of response from IBM, this disclosure is being published **publicly and de-anonymized** in accordance with standard responsible disclosure practices to raise awareness and enable remediation.

------

### **Disclosure Timeline:**

- Vulnerability discovered: April 2023
- Reported to IBM: April 2023
- Follow-ups: Multiple (no CVE issued)
- Public disclosure: April 2025

---

### **Affected Product:**

| Application               | Tested Version |
|---------------------------|----------------|
| IBM Cloud Pak for Data    | 4.6.1          |

Other versions may also be affected.

------

### **Summary:**

A **credential disclosure vulnerability** exists in IBM Cloud Pak for Data, specifically in the **FTP connection configuration** within projects. Users with elevated permissions (e.g., “Editor” role) can modify the destination IP address and port of a pre-configured FTP connection, allowing them to redirect connection attempts to a **malicious server** under their control. When the “Test Connection” function is triggered, the system sends the **decrypted username and password** to the attacker’s server — effectively exposing the stored credentials, which were originally encrypted at rest. This allows an attacker to **harvest valid credentials** without needing direct access to the encrypted storage or higher privileges.

------

### **Steps to Reproduce:**

1. Log in as a user with **“Editor” role** access in a project
2. Navigate to: Project > Data access > Connections
3. Select an existing **FTP connection**
4. Modify the **server IP address and port** to point to an attacker-controlled machine listening on FTP (e.g., with responder)
5. Click **“Test Connection”**
6. Observe on the attacker-controlled server that:
   - The client connects
   - The **username and password are received in plaintext**

![CP4D Credential Disclosure via Attacker Controled Endpoint](/assets/img/cp4d/cred-leak.png)

------

### **Vulnerability Type:**

- **CWE-522**: Insufficiently Protected Credentials
- **CWE-441**: Unintended Proxy or Credential Forwarding
- **CWE-285**: Improper Authorization
