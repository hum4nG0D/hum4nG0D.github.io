---
title: RCE via JDBC Driver Connection in IBM CP4D
description: Authenticated Remote Code Execution via JDBC Driver Upload in IBM Cloud Pak for Data (CP4D).
author: hum4ng0d
date: 2025-04-01
categories: [Exploit, PoC]
tags: [Vulnerability, RCE, Remote Code Execution, IBM, CP4D, Cloud Pak For Data]
pin: true
math: true
mermaid: true
image:
  path: /assets/img/cp4d/ibm-cp4d.png
  alt: CP4D RCE
---

### **Disclosure Context:**

This vulnerability was **responsibly reported to IBM during a security assessment in April 2023**. Despite multiple attempts to coordinate, **IBM has not acknowledged or issued a CVE ID after more than two years**.

Given the extended delay and the lack of response from IBM, this disclosure is being published **publicly and de-anonymized** in accordance with standard responsible disclosure practices to raise awareness and enable remediation.

------

### **Disclosure Timeline:**

- Vulnerability discovered: April 2023
- Reported to IBM: April 2023
- Follow-ups: Multiple (no CVE issued)
- Public disclosure: April 2025

---

### **Affected Product:**

Application: **IBM Cloud Pak for Data (CP4D)**

Tested Version: **4.6.1**

Other versions may also be affected.

------

### **Summary:**

A remote code execution (RCE) vulnerability was identified in IBM CP4D’s **JDBC Connection** feature. The platform accepts user-supplied JDBC JAR files for custom connections, but **fails to validate or sandbox the uploaded code**. As a result, arbitrary Java code embedded within the JAR is **executed on the server** when the “Test connection” action is triggered. This allows **authenticated users** with access to the connection configuration interface to execute arbitrary commands on the backend server — leading to **full remote code execution**.

------

### **Steps to Reproduce:**

1. **Navigate to**: Platform connections > JDBC drivers
2. **Upload** a malicious JAR file containing a reverse shell payload in a class named Client
3. Go to New connection > Generic JDBC
4. Under **Jar URIs**, select the uploaded JAR
5. In **JDBC driver class**, choose: `Client`
6. Click **“Test Connection”**
7. The server loads and executes the `Client` class from the JAR file, resulting in execution of arbitrary code on the server.

![CP4D RCE](/assets/img/cp4d/rce.png)

------

### **Vulnerability Type:**

- CWE-94: **Improper Control of Generation of Code (‘Code Injection’)**
- CWE-502: **Deserialization of Untrusted Data**
- CWE-434: **Unrestricted Upload of File with Dangerous Type**
