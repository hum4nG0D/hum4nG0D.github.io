---
title: "Guacamole: Browser Rendering SSH, VNC and RDP"
description: "Browser rendering SSH, VNC and RDP sessions with Apache Guacamole."
date: "2023-03-25"
categories: [Lab]
tags: [Apache Guacamole, Cloudron, Digitalocean, Cloudflare, Zero Trust, SSH, RDP, VNC]
pin: true
math: true
mermaid: true
image:
  path: "/assets/img/guacamole/guacamole.png"
  alt: "Apache Guacamole"
---

I recently tried out [Apache Guacamole](https://guacamole.apache.org/) to enable browser-based rendering of SSH, RDP, and VNC sessions, and here’s how I made it all work seamlessly with Cloudron, Cloudflare, and a DigitalOcean VM.

While [Cloudflare Access](https://developers.cloudflare.com/cloudflare-one/) offers Zero Trust connectivity and works great for HTTP-based apps, it doesn’t currently support browser rendering for RDP. Since connecting back to home servers requires port forwarding—something I want to avoid exposing—we’ll host Guacamole on a **DigitalOcean VM** instead. Although using **Cloudflare Tunnel** would make things even simpler, especially for securing remote services without opening ports, we’re intentionally **not using Cloudflare Tunnel** in this setup to demonstrate a more traditional approach. To enhance security, we’ll configure the **DigitalOcean firewall** to only allow traffic from **Cloudflare IPs**, preventing direct IP access. We’ll also manage DNS using Cloudflare and a general API token to automate everything through Cloudron.

## Project Stack

- **Public Domain** – A public accessible domain 
- **Guacamole** – Web-based remote desktop gateway
- **Cloudron** – App platform that handles SSL, DNS, and app deployment
- **DigitalOcean** – Cloud VM host
- **Cloudflare** – DNS management and security

## Step-by-Step Setup

### **1.** **Create a DigitalOcean VM**

- Spin up a fresh Ubuntu VM (minimum 1GB RAM recommended).
- Since we're using Cloudron, make sure to choose Ubuntu 24.04. 
- Add SSH keys for secure login.

![Digitalocean](/assets/img/guacamole/do-stats.png)

### **2.** Set Up Cloudron

Cloudron makes deploying web apps ridiculously easy—and it comes with built-in Let’s Encrypt SSL, DNS integration, and app management.

- Install Cloudron:

[Cloudron.io](https://www.cloudron.io/get.html)

```
wget https://cloudron.io/cloudron-setup
chmod +x ./cloudron-setup
./cloudron-setup
```

- Follow the Cloudron web setup wizard once it’s installed.
- Use your domain and subdomain (e.g., `my.yourdomain.com`) in the setup.

### **3.** Cloudflare DNS Configuration

- In Cloudflare, create a DNS A record pointing guac.yourdomain.com to your DigitalOcean VM IP.
- **Important:** Temporarily **disable** the orange proxy (DNS only mode) so Cloudron can verify and install Let’s Encrypt SSL.

![Cloudflare](/assets/img/guacamole/dns.png)

- Create a **Cloudflare API Token** (scoped for DNS zone edits) and add it during Cloudron DNS setup so it can manage records automatically.

### **4.** Install Guacamole on Cloudron

- Head to the Cloudron App Store, should be `https://my.<your-domain>.com`

![Cloudron](/assets/img/guacamole/cloudron-login.png)

- Search for “Guacamole” and install it to your chosen subdomain.

![Cloudron](/assets/img/guacamole/cloudron-apps.png)

- Cloudron takes care of everything—SSL, database, and user management.

![Cloudron](/assets/img/guacamole/cloudron-guac.png)

### **5.** Configure Guacamole SSH Connection

- Log into Guacamole via the browser.
- Go to **Settings → Connections → New Connection**.
- Choose protocol: SSH.
- Enter your target server’s IP, port, and SSH credentials (or key).
- Save the settings.

- You can now access Guacamole directly by your designated hostname. 

![Apache Guacamole](/assets/img/guacamole/guac-login.png)

- You can configure SSH, RDP and VNC in settings

![Apache Guacamole](/assets/img/guacamole/guac-session.png)

-  SSH session in browser

![Apache Guacamole](/assets/img/guacamole/guac-ssh.png)

### **6.** Tighten Firewall Rules

To prevent direct IP access:

- In DigitalOcean firewall settings:
  - Allow only incoming traffic from [Cloudflare IP ranges](https://www.cloudflare.com/ips/) on port 80 and 443 since Guacamole proxies SSH over HTTPS.

![Digitalocean](/assets/img/guacamole/do-firewall.png)



Once everything is working, go back to Cloudflare and **enable the orange proxy** to benefit from CDN and DDoS protection. You now have a fully secure, browser-accessible remote SSH interface. While this Guacamole setup works well and provides browser-based access to SSH, RDP, and VNC, it’s not without its drawbacks. Guacamole still feels dated in terms of UI and overall user experience—it gets the job done, but lacks the polish and modern security features of newer solutions. That said, adding **Cloudflare Zero Trust** on top of this setup can significantly enhance security by introducing identity-based access controls, MFA, and logging. It’s a great way to lock down access without touching your firewall rules. Personally, I still prefer **Cloudflare’s simplicity and flexibility**. Although it doesn’t support browser-based RDP rendering just yet, using cloudflared or WARP, I can still securely access RDP sessions from anywhere without exposing ports directly to the internet. In the end, this project proves that while Guacamole offers a functional browser rendering experience, Cloudflare continues to lead with a more modern, secure, and easy-to-manage approach.
