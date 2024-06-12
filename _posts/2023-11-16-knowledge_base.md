---
title: Knowledge Base - [Start Here]
categories: [knowledge Base]
tags: [netsec]     # TAG names should always be lowercase
pin: true
---

> `Note to self`: Productivity and creativity improve when focus is on self-improvement, rather than publication. Do more and get better. 
{:.prompt-tip}
A periodic review should help.

- This page serves as a guide to what one can learn from this site.

### Recent updates:
- Back on the Portswigger series. I plan to wrap this up in the next two months. 
- Wasn't selected. Disappointed(I hadn't expected that outcome) but, I'm grateful for the opportunity to connect with experienced security professionals. Gotta try harder!
- I finished my final round of interviews with Meta for the Product Security Engineer role!
- Jenkins integration with GitHub for DevSecOps.
- [Fixed] AWS Lightsail CPU usage issue => Add swap space to the Ubuntu server.

## Series list:
### [Homelab series](/posts/homelab_intro):
- Use VMware to set up a virtual lab environment.
    - Learn how to set up the networks, install a firewall, set the firewall rules, install multiple operating systems, host a web server, set up a virtual router, and enable port forwarding to simulate an attack.
- Perform your first attack: recon > scan > initial access > recon > escalate privileges > set up persistence.
- Learn about data exfiltration using DNS and maintaining persistence using a DNS-based command center.
    - Set up an authoritative DNS server using digitaloceans.
- Packet capture and traffic analysis. 
- Active directory pen-testing resources.
- Recommendations, and takeaways from the series. 

### [Network security series](/posts/network-security-intro):
- Network security concepts.
- Common attacks at each layer.
- DNS, attacks against DNS, DNSSEC, and short notes on BGP security properties.
- How a TLS session is established.
    - Implement [tls13.xargs.org](https://tls13.xargs.org/) using Python.
    - My project: [TLS 1.3 handshake](https://github.com/snabith/tls13_handshake)
- Practical implementation of attacks, and mitigations using socket programming.
- Web security concepts: security headers, HSTS, CSP, CORS, and other security standards.

### [Portswigger Web academy seires](/posts/burp-suite-intro):
- One-page summaries of what I learnt from each module of port-swigger web academy.
- Lab solutions and scripts.

### Pentest series:
- Summaries on how I approach a pentest. 
- Insights and resources that I found useful through my journey towards my PNPT certification. 

### Misc:
- Self-hosting with AWS Lightsail, Nginx Reverse Proxy, and Nginx server (docker containers). [Upcoming]
    - Setting up your Jenkins pipeline and integrating it with GitHub. [Upcoming]
- Single SignOn setup using Authentik. [Upcoming]
- Problems I faced with IPv6 (NAT64 and DNS64). [Upcoming]

## Resources:
- [TCM Security Academy](https://academy.tcm-sec.com/), their YouTube channel: [The Cyber Mentor - YouTube](https://www.youtube.com/@TCMSecurityAcademy)
- [Web Security Academy](https://portswigger.net/web-security)
- [TryHackMe](https://tryhackme.com/)
- Practice: [HTB](https://www.hackthebox.com/)
- Concepts: 
    - Chapters 1-6 from `Security in Computing` 5ed by Pfleeger, Pfleeger, and Margulies
    - Cryptographic algorithms to understand TLS - Chapters 20,21 from `Computer Security - Principles and Practice` by Stallings, and Brown. 
    - The internet is vast, but books can give you a structure to learn more from. You can stack new technologies and standards on top of these. 