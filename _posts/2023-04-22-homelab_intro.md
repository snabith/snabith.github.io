---
title: Virtual enterprise network intro
categories: [Homelab]
tags: [homelab] # TAG names should always be lowercase
img_path: /assets/img/homelab
pin: false
---
## Project overview:
### Intro:
This post can help you set up a home lab on vmware. You can simulate attacks, check how a firewall/IDS works, understand the logs and network traffic, and learn more from it. These instructions are provided based on what I could take away from Jeff McJunkin's [kickass home lab](http://bit.ly/kickasslab) [SANS youtube](https://youtu.be/uzqwoufhwyk). It's a great resource for creating your own lab.

If you set this up previously and just want a quick review of the commands while replicating the attacks: [Refresher mode](/posts/quick_version)

What can one takeaway from the homelab series:

### How?
Using an example. It contains:
- Details on how to set up a network using vmware.
- An example attack scenario that involves privilege escalation, and persistence (`dnscat2`)
- Instructions on how to:
	- Collect the network traffic (`pfsense` packet capture)
	- Setup Active Directory

### My favorite parts:
1. Setting up your own Authoritative DNS server for DNS-based c2 using dnscat2
2. Active Directory setup
3. Port-forwarding setup on `PfSense` and `VyOS` router

### How did this help me:
1. I wanted to see what the network traffic looks like during a DNS-based c2. Good thing I had a lab setup, I was able to understand that in an instant. 
2. Now, because it is on a level2 hypervisor - VMware workstation pro, I can copy the `pcap` files to my host OS, share, teach, train, and get feedback faster.

### What I found challenging:
1. Setting up the `PfSense` - firewall rules, DNS, port-forwarding clashes with VMware.
2. Authoritative DNS server - DNS propagation and TTL.

### Eye-openers:
1. Snapshots ~ Time travel. (Dormamu, I've co...)
2. For DNS, set low TTL values till things work out. 
3. After you know how to set up `pfsense`, you wonder how you struggled with such a simple and elegant software

### Possible use cases:
- Perform an attack -> export the network dump (`pcap` files) -> train employees to find the exploited vulnerabilities
- A quick[^1] and safe environment for trying firewall rules, exploitation techniques, and tools.
- You can create vulnerable systems, share them, grow as a group, perform pen-test or red team scenarios, and make reports.
- Perform forensics on exploited machines

### What one can get from this manual:
- I made a bunch of mistakes, and you can avoid them
- Links and references that helped me understand the topics

## Home lab summary:

### Example network:
![Home Lab Network Diagram](home_lab_nw.png)

### Phase1 - Setup and initial access:
#### Network - VMWare configuration:
- Make a NAT network (`192.168.189.0/24`). This shall be the WAN network for the PfSense firewall. [HowTo - VMware configuration](/posts/vmware_configuration)
- Make a host-only network (`10.10.20.0/24`). This acts as the LAN network on Pfsense. (DHCP is enabled, but we assign static addresses)
- Make another host-only network (`10.10.10.0/24`) -DMZ network (configured as OPT1 on Pfsense) with the metasploitable2 machine. The metaspoloitable2 machine has a connection to the LAN network but is currently offline.
	- Port forward the default metasploitable2's website to the WAN port 8080.
- Create one more host-only network (`18.0.0.0/24`) for the attacker. Install the VyOS router with the same gateway (`192.168.189.2/24`) as the PfSense WAN network.
	- This is an extra step that can help you simulate your scenario and make it a bit more realistic/challenging.
	- Enable port-forwarding on the attacker's router (VyOS router - port 80, 443) for the reverse shells. 

#### Pfsense configuration:
- Install and configure the pfsense networks. [HowTo- Pfsense configuration](/posts/pfsense_configuration)
- Assign static addresses to the machines
- Set up the firewall rules to allow inbound traffic to the DMZ network's webserver.
- Other rules: [firewall](/posts/pfsense_configuration/#firewall-rules)

#### Setup port-forwarding on pfsense:
- Uncheck vmware dhcp to avoid the clash between vmware NAT, DHCP, and port-forwarding rules with pfsense.[HowTo- Disable vmware DHCP](/posts/vmware_configuration/#vmware-settings)
- [HowTo - Port forwarding on pfsense](/posts/pfsense_configuration/#port-forwarding)

#### packet capture:
**Pfsense webconfigurator:** Diagnostics > packet capture

#### VyOS setup:
- [NAT — VyOS 1.3.x (equuleus) documentation](https://docs.vyos.io/en/equuleus/configuration/nat/index.html)
- [VyOS Community](https://vyos.net/get/) (select legacy > ova for vmware)
- [HowTo - VyOS router setup](/posts/vyos_setup)
- [HowTo - VyOS port translation](/posts/vyos_setup/#vyos-router-port-translation)

#### Attack scenario:
- Recon the WAN network `192.168.189.0/24` using `nmap` (Disable host discover `-Pn`). You'll find the firewall address
- Scan the firewall IP address, and you may find the open port for the website. Learn more by using `nmap` or other tools like Nesus.
- `searchsploit` or `exploit.db` for payloads. Exploit the system, gain user access, and point the reverse shell to the VyOS router address port 80
- Escalate privileges, and gain root access through another reverse shell. Use port 443 of the same router.
- Check the current network interfaces. You will find an offline interface.
- Bring up the interface. It connects to the LAN network.
- Scan the internal (LAN) network using `nmap`, find vulnerable systems, move laterally, and learn more about the enterprise. [HowTo - metasploitable2 exploitation](/posts/quick_version)

#### Forensics:
- Use the `pcap` file from the packet capture. Learn what vulnerabilities the attacker used for exploitation.
- You can perform memory analysis using `vmem` files (you can see them when you take a snapshot of the machine). These files can help in a scenario where the SOC team finds abnormal activity and takes a memory dump of that system.

### Phase2 - DNS attacks summary:
> Setup an authoritative DNS server and get a command an control server using dnscat2. [HowTo - dnscat2](/posts/c2_over_dns)

### Active Directory Setup:
[Active Directory on Azure](https://kamran-bilgrami.medium.com/ethical-hacking-lessons-building-free-active-directory-lab-in-azure-6c67a7eddd7f)

- I learnt AD through PEH course by TCM security. I highly recommend going through the course if you are serious about active directory pentesting. The above link is a recommendation by Heath Adams, and it was a great source for setting up your AD network on Azure (similar setup locally). 

Update: The link is now for medium members only. So, go with: https://refabr1k.gitbook.io/oscp/windows/attacking-ad/ad-hacking-lab-setup if you can't use the medium link.

## Note
1. Use LAN segments if you are dealing with malware analysis. You may need to follow additional steps to isolate the machine.

My takeaway:
- I believe that something like this could've been useful for my past self to understand basic concepts of networking and penetration testing. So, I'm sure it can help someone who wants to learn or teach.
- Looks too simple or basic? Good, that was the point. Can you do better? That is the expected outcome. If you can understand and follow along, you can make and test better scenarios.

## Personal recommendations:
- TCM security's PEH course is a great source to learn about Active Directory setup and pentesting. I highly recommend their PEH course, EPP, and MPP courses. You can find more about the courses offered by TCM security here: [academy.tcm-sec.com](https://academy.tcm-sec.com/courses)
- Although tryhackme.com is a great place for beginners to get a hang of practical tools that are used in cybersecurity, I highly recommend anyone entering the cybersecurity domain to learn and understand network security concepts. Tools can help you do something, understanding the concepts can help you make better decisions in securing an organization. The latter matters more.
	- I will be making blogs explaining a few network security concepts using practical examples through socket programming.


[^1]: This is faster to set up, and easier to work with when compared to working with a level 1 hypervisor.