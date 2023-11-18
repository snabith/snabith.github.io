---
title: VMware configuration
categories: [homelab]
tags: [homelab, vmware] # TAG names should always be lowercase
img_path: /assets/img/homelab
---

[main](/posts/homelab_intro)

Vmware configuration settings for the [homelab setup](/posts/homelab_intro)

## vmware settings:

![VMware Network Settings](vmware_nw.png){: width="60%"}

- Uncheck DHCP on WAN, LAN, and DMZ networks to avoid clash between vmware and pfsense

### VMWare NAT (WAN) settings:

![VMware NAT settings](vmware_NAT_info.png){: width="60%" .normal}

![VMWare DNS settings](vmware_DNS_info.png){: width="80%" .normal}
