---
title: "Stored XSS via SVG Upload in IBM CP4D and Event Stream"
description: "Stored Cross-Site Scripting (XSS) via SVG File Upload in IBM Cloud Pak for Data and IBM Event Streams."
date: "2025-04-01"
categories: [PoC]
tags: [Vulnerability, XSS, IBM, CP4D]
pin: false
math: true
mermaid: true
image:
  path: "/assets/img/cp4d/ibm-cp4d.png"
  alt: "CP4D Stored XSS"
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
| IBM Event Stream          | 11.1.3         |

Other versions may also be affected.

------

### **Summary:**

A **stored cross-site scripting (XSS)** vulnerability exists in the **Customization page** of IBM CP4D and IBM Event Streams. The application allows the upload of **SVG images** without sanitization, enabling embedded `<script>` tags to be executed when the image is rendered in the UI or accessed directly via URL. 

------

### **Steps to Reproduce:**

1. **Log in** to IBM CP4D/IBM Event Stream as an authenticated user

2. Navigate to: Administration > Customization or relevant branding section

3. Upload the SVG file which includes XSS alert:

   ```
   <svg xmlns="http://www.w3.org/2000/svg">
     <script>onload=alert(`1`);</script>
   </svg>
   ```

4. After upload, navigate to the **Home** page where the image is displayed

5. **Right-click** the uploaded image and select **“Copy Image Address”**

6. **Paste** the image URL in a new browser tab

7. The `<script>` inside the SVG executes immediately in the browser, demonstrating a persistent **stored XSS** vector.

![CP4D Stored XSS](/assets/img/cp4d/stored-xss-1.png)

![CP4D Stored XSS](/assets/img/cp4d/stored-xss-2.png)

![CP4D Stored XSS](/assets/img/cp4d/stored-xss-3.png)

------

### **Vulnerability Type:**

- **CWE-79**: Improper Neutralization of Input During Web Page Generation (‘Cross-site Scripting’)
- **CWE-434**: Unrestricted Upload of File with Dangerous Type
- **CWE-200**: Exposure of Sensitive Information to an Unauthorized Actor (via URL path disclosure)
