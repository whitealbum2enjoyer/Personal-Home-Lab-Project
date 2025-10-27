# My Home Lab Project

<!-- Optional: Add a brief 1-2 sentence overview of your lab here. -->

This repository documents the setup, configuration, and evolution of my personal home lab, built primarily for learning networking, system administration, and self-hosting services.

<!-- Optional: Add a picture of your lab setup -->

<!--  -->

<!-- Caption: Figure 1: The main server running on repurposed hardware. -->

## Table of Contents

* Goal
* Hardware
* Software & Operating Systems
* Key Services Deployed
* Network Configuration
* Network Diagram
* Key Configuration Files
* Challenges & Solutions
* Future Plans
* Resources Used

## Goal

<!-- Explain why you built this lab. What did you want to learn or achieve? -->

The primary goal of this project is to gain hands-on experience with:

* Linux system administration (specifically Arch Linux).
* Containerization using Docker.
* Network services like DNS (Pi-hole) and reverse proxies (Nginx Proxy Manager).
* Self-hosting applications (Nextcloud).
* Basic network security principles (VPNs, Firewall Rules).
* Utilizing older hardware effectively.

## Hardware

<!-- List the physical components of your lab. Be specific! -->

* Server:

  * Model: Dell Optiplex [Your Model] / [Your Old Laptop Model]

  * CPU: [e.g., Intel Core i5-xxxx]

  * RAM: [e.g., 16GB DDR3]

  * Storage: [e.g., 256GB SSD (OS) + 1TB HDD (Data)]

* Networking Gear:

  * Router: [e.g., ISP Provided Router / pfSense Box]

  * Switch: [e.g., TP-Link 5-port Unmanaged Switch (if applicable)]

* Other:

  * [e.g., Raspberry Pi for Pi-hole (if separate)]

  * [e.g., External USB Drive for Backups]

<!-- Add a picture of your hardware if you like -->

<!--  -->

<!-- Caption: Figure 2: The repurposed laptop serving as the core of the lab. -->

## Software & Operating Systems

<!-- List the main OS and virtualization/container software. -->

* Operating System: [e.g., Arch Linux / Ubuntu Server 22.04 LTS / Proxmox VE]

* Containerization: Docker & Docker Compose

Virtualization: [e.g., Proxmox / VMware ESXi (if used)]

## Key Services Deployed

<!-- List the main applications/services running. Explain briefly what each does. -->

* Pi-hole:

  * Purpose: Network-wide ad-blocking and local DNS server.

  * Access: http://[pihole-ip]/admin (or via reverse proxy)

* Nginx Proxy Manager (NPM):

  * Purpose: Manages reverse proxy entries, handles SSL certificates (Let's Encrypt), and simplifies secure access to services.

  * Access: http://[npm-ip]:81

* Nextcloud:

  * Purpose: Private cloud storage, file sharing, and collaboration suite.

  * Access: https://nextcloud.yourdomain.com (via NPM)

* WireGuard VPN:

  * Purpose: Secure remote access to the home network.

  * Configuration: Managed via [e.g., wg-easy Docker container / manual config files].

* [Add More Services As You Deploy Them]:

  * Purpose: [Explain what it does]

  * Access: [How you access it]

## Network Configuration

<!-- Describe your basic network setup. Use bullet points. -->

* IP Addressing Scheme: [e.g., 192.168.1.0/24]

* Server IP: [e.g., Static IP 192.168.1.50 assigned via DHCP reservation]

* DNS: All network clients configured (via DHCP) to use Pi-hole ([Pi-hole IP]) as the primary DNS server.

* Firewall: [e.g., Using pfSense firewall / basic router firewall]. Key rules include:

  * Port forwarding rules for ports 80 and 443 to Nginx Proxy Manager server.

  * Port forwarding rule for WireGuard VPN port [e.g., 51820/udp] to the VPN server.

  * [Add any other significant rules, like VLAN configurations if you set them up]

* Domain Name: Using [yourdomain.com] managed via [e.g., Cloudflare DNS / Google Domains].

## Network Diagram

<!-- Embed your network diagram image here. Upload it to the repository first. -->

<!-- Caption: Figure 3: Visual representation of the home lab network topology. -->

## Key Configuration Files

<!-- Paste relevant snippets (not huge files) of your configs. Use code blocks! -->

Docker Compose (docker-compose.yml)

# Example snippet for Pi-hole
```services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      # Add other ports as needed
    environment:
      TZ: 'America/Chicago'
      WEBPASSWORD: '[YourAdminPassword]'
    volumes:
      - './etc-pihole:/etc/pihole'
      - './etc-dnsmasq.d:/etc/dnsmasq.d'
    restart: unless-stopped
```

Nginx Proxy Manager (Example Proxy Host Setup)

<!-- Describe a typical setup or paste a relevant config part if possible. Often this is GUI-based, so describe the steps. -->

Domain: nextcloud.yourdomain.com

Scheme: http

Forward Hostname / IP: [IP address of Nextcloud container/server]

Forward Port: [Port Nextcloud runs on, e.g., 80]

SSL: Enabled using Let's Encrypt certificate. Force SSL and HTTP/2 support enabled.

WireGuard Config (wg0.conf - Client Example)

# Example client config
[Interface]
PrivateKey = [CLIENT_PRIVATE_KEY]
Address = 10.0.0.2/32
DNS = [Pi-hole IP]

[Peer]
PublicKey = [SERVER_PUBLIC_KEY]
PresharedKey = [OPTIONAL_PRESHARED_KEY]
Endpoint = [yourdomain.com or public IP]:[WireGuard Port]
AllowedIPs = 192.168.1.0/24, 10.0.0.1/32  # Allows access to home LAN and VPN server itself


## Challenges & Solutions

<!-- This is CRITICAL. Describe problems you faced and how you fixed them. -->

Challenge: Initially had issues accessing services externally via Nginx Proxy Manager.

Solution: Realized the router's firewall was blocking ports 80/443. Created port forwarding rules to allow traffic to the NPM server's IP address.

Challenge: Pi-hole DNS wasn't being used by all devices on the network.

Solution: Configured the router's DHCP server settings to explicitly assign the Pi-hole's IP address as the primary DNS server for all clients.

Challenge: [Describe another problem, e.g., Nextcloud performance issues, Docker container conflicts, SSL certificate renewal problems]

Solution: [Explain how you diagnosed and fixed it, e.g., researched logs, adjusted resource limits, found a specific command]

## Future Plans

<!-- What do you want to add or improve next? -->

Implement automated backups for Docker volumes and configurations.

Set up monitoring using Grafana and Prometheus.

Explore setting up VLANs to segment network traffic.

Deploy [Another Service, e.g., Plex Media Server, Home Assistant].

Upgrade server hardware or add more storage.

## Resources Used

<!-- List tutorials, guides, or documentation you found helpful. -->

YouTube - Christian Lempa's Home Server Series

Docker Documentation

Nginx Proxy Manager Guide

Arch Wiki

Specific Stack Overflow post or blog article that helped solve a problem
