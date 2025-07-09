---
title: "Building a Covert AI Chat App"
description: "Building a Covert AI Chat App: End-to-End Encrypted Gemini Chat hosted on Cloudflare Pages."
date: "2025-01-09"
categories: [Lab]
tags: [AI, Gemini]
pin: false
math: true
mermaid: true
image:
  path: "/assets/img/gc/gemini.png"
  alt: "End-to-end Encrypted AI Chat App"
---

In an age where AI is becoming indispensable, it's increasingly frustrating to find access to powerful AI chat applications blocked in many corporate environments. These restrictions, often implemented through network categorization and certificate inspection, aim to control internet usage but can inadvertently stifle productivity and innovation. What if you could build your *own* AI chat application, powered by Gemini, that bypasses these limitations while ensuring your conversations remain private?

This is precisely the premise of a recent fun side project: a Gemini chat application with end-to-end encryption, hosted entirely on Cloudflare Pages. The goal? To create a personal AI assistant that's accessible anywhere, without raising red flags, and with the added security of knowing your data is truly yours.

**The "Why" Behind the Project: Bypassing the Blocks**

Corporate networks often employ robust filtering systems that categorize websites and block access to those deemed "unproductive" or "non-business related." AI chat applications, despite their potential for boosting efficiency, often fall into these forbidden categories. Furthermore, many organizations use "certificate inspection" (also known as SSL/TLS inspection) where they decrypt, inspect, and then re-encrypt your internet traffic. While this is done for security purposes, it means that even "encrypted" communication is visible to IT.

This project directly addresses these challenges. By building a custom chat application and layering on end-to-end encryption, the aim is to:

1.  **Bypass Categorization:** Since it's a custom-built application hosted on a generic platform like Cloudflare Pages, it's less likely to be immediately flagged by content filters designed for well-known AI services.
2.  **Defeat Certificate Inspection:** With true end-to-end encryption, the data exchanged between your client and your server (which then talks to Gemini) remains encrypted even as it passes through the corporate network's inspection points. To any monitoring entity, the traffic will appear as random, indecipherable gibberish.

**A Look Under the Hood: Development Details**

The development of this secure Gemini chat application revolves around a few key components and a simple yet effective encryption strategy:

* **Simple Chat Interface:** The front-end is designed to be straightforward and user-friendly. A clean chat interface allows for seamless interaction, focusing purely on the conversation with Gemini.

![Gemini Chat App](/assets/img/gc/gc.png)

* **Gemini Free API:** The core intelligence of the application comes from Google's Gemini API. Leveraging the free tier makes this project accessible and cost-effective for personal use.
* **Cloudflare Pages for Hosting & Environment Variables:** Cloudflare Pages provides a fantastic platform for deploying static sites and front-end applications, complete with serverless functions (Cloudflare Workers) that can handle API calls. Critically, Cloudflare Pages allows for the secure storage of sensitive information like API keys and encryption keys as environment variables. This prevents these secrets from being exposed in the client-side code.
* **AES Encryption (with a 32-character key):** Advanced Encryption Standard (AES) is a symmetric encryption algorithm widely regarded for its security and efficiency. For this project, a 32-character (256-bit) AES key is generated and used for all encryption and decryption operations.
* **The Encryption/Decryption Flow:** This is where the magic happens, ensuring end-to-end privacy:
    1.  **User Sends Message:** When a user types a message and hits send, the client-side JavaScript code takes the plaintext message and **encrypts it using the AES key** *before* it leaves the user's browser.
    2.  **Server Receives Encrypted Message:** The encrypted message is then sent to the Cloudflare Pages serverless function.

![Encrypted Message](/assets/img/gc/payload.png)

    3.  **Server Decrypts Message:** Upon receipt, the serverless function **decrypts the message** using the *same AES key* (which is securely stored as an environment variable).
    4.  **API Call to Gemini:** The now plaintext message is used to make the API call to the Gemini service.
    5.  **Gemini Responds, Server Encrypts:** Gemini processes the request and sends back a response. Before this response is sent back to the client, the serverless function **encrypts the response message** using the AES key.
    6.  **Client Receives and Decrypts:** The encrypted response is sent back to the user's browser. The client-side JavaScript then **decrypts the message** and displays the Gemini AI's response in readable form.

![Encrypted Response](/assets/img/gc/response.png)

* **Cloudflare Access for Zero Trust:** Like always, I like to add the app into Cloudflare Access so we can protect the app with another layer of authentication. 

![Cloudflare Access Login](/assets/img/gc/cf-login.png)

**Security Considerations (and a Disclaimer)**

It's important to reiterate that this is a "fun project" with "no harm intended." While the end-to-end encryption significantly enhances privacy against network monitoring, it's crucial to understand the limitations:

* **Key Management:** The AES key is shared between the client and the server. While stored securely as an environment variable on Cloudflare Pages, if this key were compromised, the encryption would be broken. For true multi-user E2E, a more sophisticated key exchange mechanism (like Diffie-Hellman) would be necessary, where each user has their own public/private key pair. This project simplifies for a single-user, self-hosted scenario.
* **Trust in Cloudflare:** While Cloudflare is a reputable platform, you are inherently trusting them with the secure storage and execution of your serverless function and environment variables.
* **Compliance:** This approach is not a substitute for adhering to corporate IT policies. Bypassing network restrictions, even for a "fun project," could violate acceptable use policies. Always be aware of your organization's rules.

**The Power of Personalization and Privacy**

This project demonstrates how readily available technologies can be combined to create powerful, personalized tools that prioritize user privacy. By taking control of the entire stack – from the client UI to the serverless backend and hosting – individuals can craft bespoke solutions that fit their unique needs, even within restrictive environments. It's a testament to the versatility of platforms like Cloudflare Pages and the accessibility of AI APIs like Gemini, enabling developers to build innovative applications with a focus on security and autonomy.