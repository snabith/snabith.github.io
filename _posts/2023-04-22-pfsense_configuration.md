---
title: Pfsense_config
categories: [Homelab]
tags: [homelab, pfsense] # TAG names should always be lowercase
img_path: /assets/img/homelab
---

[main](/posts/homelab_intro)
### Pfsense installation steps:
- Use the ISO, install, go with the defaults
  - Official: [Installation Walkthrough - pfSense Documentation (netgate.com)](https://docs.netgate.com/pfsense/en/latest/install/install-walkthrough.html)
  - File system selection - UFS vs ZFS (ZFS) => [Should I use ZFS or UFS for my file system?](https://www.reddit.com/r/PFSENSE/comments/gyq5x3/should_i_use_zfs_or_ufs_for_my_file_system/)
  - Reboot and Halt (option 6)
  - [pfsense basic setup](https://nguvu.org/pfsense/pfsense-baseline-setup/#dns%20configuration)

## Pfsense network configuration:
- The three networks shall be configured as mentioned in the main page.
	- You can identify the networks based on the MAC address of the interface (vmware > settings)
	  ![VMWare MAC address information](pfsense_vmware_settings_mac.png){: width = "40%"}
	  - WAN's default gateway shall be `192.168.189.2` and I chose not to use IPv6.
	  - Enable DHCP on LAN and OPT1(DMZ) networks. I chose a range of (`x.x.x.128` to `x.x.x.192`) for this. 
- Set the interface addresses: (`/24` networks)
	- WAN: `192.168.189.254`
	- LAN: `10.10.20.254`
	- OPT1(DMZ): `10.10.10.254`

![DNS setup](dns_general_setup.png)

## Firewall rules
![wan](fw_rules_wan.png)
![lan](fw_rules_lan.png)
![dmz](fw_rules_dmz.png)

## Port-Forwarding:
![port-translation](fw_portforward_metasploitable2.png)