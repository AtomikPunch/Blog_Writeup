---
id: 1
slug: "setting-up-temporary-web-server-kali-linux"
title: "Setting Up a Temporary Web Server on Kali Linux for Project Demonstration"
description: "A guide on configuring a Kali Linux virtual machine and using ngrok to host a temporary web server for project demonstrations accessible from outside the local network."
date: "2025-07-30"
readTime: "6 min" 
tags:
  - Web Server
  - Kali Linux
  - Virtual Machine
  - Ngrok
  - Development Environment
  - Networking
featured: true
---

# Setting Up a Temporary Web Server for Project Demonstration

This write-up details the process of setting up a temporary web server on a Kali Linux virtual machine to host a website for demonstration purposes. The objective was to create a development environment accessible to individuals outside the local network, leveraging `ngrok` for external access without requiring a full VPS and DNS setup.

## 1. Virtual Machine Configuration

[cite_start]Initially, a Kali Linux virtual machine was chosen for hosting the project[cite: 2]. The virtual machine's specifications, as shown in the image, were deemed sufficient for this temporary purpose:

* [cite_start]**Memory:** 2 GB [cite: 14]
* [cite_start]**Processors:** 4 [cite: 15]
* [cite_start]**Hard Disk (SCSI):** 80.1 GB [cite: 16]
* [cite_start]**Network Adapter:** NAT (initially) [cite: 17]
* [cite_start]**USB Controller:** Present [cite: 18]
* [cite_start]**Sound Card:** Auto detect [cite: 19]
* [cite_start]**Display:** Auto detect [cite: 20]

[cite_start]To allow the virtual machine to have a unique IP address on the network, distinct from the host machine, the network connection type was changed from NAT to "Bridged: Connected directly to the physical network"[cite: 22, 44]. This configuration ensures the virtual machine behaves as an independent device on the network.

## 2. Project Setup on Kali Linux

After configuring the network, the following steps were taken to set up the web project:

1.  [cite_start]**Identify IP Address:** The IP address of the virtual machine, which would be used to access the website, was identified using the `ip a` command[cite: 57, 59]. [cite_start]In this instance, the IP address was `192.168.1.94`[cite: 67].

    ```bash
    ip a
    ```

2.  **System Update:** The system was updated to ensure all packages were current.

    ```bash
    sudo apt update
    ```

3.  **Install Dependencies:** `git` and `curl` were installed as prerequisites.

    ```bash
    sudo apt install -y curl git
    ```

4.  **Install Node.js:** Node.js version 18.x was installed to run the web application.

    ```bash
    curl -fsSL [https://deb.nodesource.com/setup_18.x](https://deb.nodesource.com/setup_18.x) | sudo -E bash -
    sudo apt install -y nodejs
    ```

5.  **Clone and Build Project:** The project repository was cloned, dependencies were installed, and the project was built.

    ```bash
    npm install
    npm run build
    ```

6.  **Start Web Server:** The web application was started, listening on all network interfaces.

    ```bash
    npm start ---H 0.0.0.0
    ```

[cite_start]At this point, the website was accessible locally within the same network as the virtual machine via `http://<ipadress>:3000`[cite: 84, 85].

## 3. Exposing the Website with Ngrok

To make the website accessible to a wider audience outside the local network, `ngrok` was utilized. [cite_start]Ngrok is a service that provides a publicly accessible URL which tunnels traffic to a local server[cite: 86, 87]. This proved to be a free, fast, and ideal solution for temporary demonstrations.

1.  **Install Ngrok:** Ngrok was installed on the virtual machine using the provided commands.

    ```bash
    curl -sSL [https://ngrok-agent.s3.amazonaws.com/ngrok.asc](https://ngrok-agent.s3.amazonaws.com/ngrok.asc) \
     sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null \
     && echo "deb [https://ngrok-agent.s3.amazonaws.com](https://ngrok-agent.s3.amazonaws.com) buster main" \
     | sudo tee /etc/apt/sources.list.d/ngrok.list \
     && sudo apt update \
     && sudo apt install ngrok
    ```

2.  **Authenticate Ngrok:** An ngrok account was created, and the authentication token was added to the virtual machine.

    ```bash
    ngrok config add-authtoken <auth_token>
    ```

3.  **Start Ngrok Tunnel:** With the web server running (`npm start -- -H 0.0.0.0`), an ngrok tunnel was initiated, forwarding traffic from a generated public URL to the virtual machine's IP address and port 3000.

    ```bash
    ngrok http http://ip:3000
    ```

[cite_start]As a result, a forwarding URL such as `https://1a982fa8566e.ngrok-free.app` was generated, redirecting to `http://192.168.1.94:3000`[cite: 116, 125].

## 4. Conclusion

[cite_start]This setup successfully enabled external access to the website hosted on a virtual machine, allowing for demonstrations to colleagues and friends regardless of their network location[cite: 126, 127]. This temporary solution is valuable for showcasing projects and gathering feedback without the immediate need for a dedicated Virtual Private Server (VPS) or domain name system (DNS) configuration. [cite_start]This exercise served as a practical learning experience in network tunneling and temporary web hosting solutions[cite: 128].

[cite_start]For continuous, 24/7 accessibility and a permanent URL, the next step would involve setting up a dedicated VPS[cite: 129].