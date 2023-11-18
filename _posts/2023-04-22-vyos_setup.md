---
title: VyOS setup
categories: [homelab]
tags: [homelab, vyos] # TAG names should always be lowercase
img_path: /assets/img/homelab
---

[main](/posts/homelab_intro)

Download: [VyOS Community](https://vyos.net/get/) (select legacy > ova for vmware)

```bash
# for vyos
conf
set interfaces ethernet eth0 address 192.168.189.253/24
set interfaces ethernet eth1 address 18.0.0.253/24
set protocols static route 0.0.0.0/0 netx-hop 192.168.189.254
# set interfaces ethernet eth0 address dhcp
set service dhcp-server shared-network-name LAN subnet 18.0.0.0/24 default-router '18.0.0.253'
set service dhcp-server shared-network-name LAN subnet 18.0.0.0/24 name-server '18.0.0.253'
set service dhcp-server shared-network-name LAN subnet 18.0.0.0/24 lease '86400'
set service dhcp-server shared-network-name LAN subnet 18.0.0.0/24 range 0 start 18.0.0.128
set service dhcp-server shared-network-name LAN subnet 18.0.0.0/24 range 0 stop '18.0.0.199'

set service dns forwarding cache-size '0'
set service dns forwarding listen-address '18.0.0.253'
set service dns forwarding allow-from '18.0.0.0/24'
set system name-server 8.8.8.8
set system name-server 1.1.1.1
set service dns forwarding system

# The following settings will configure [SNAT](https://docs.vyos.io/en/equuleus/configuration/nat/index.html#source-nat) rules for our internal/LAN network, allowing hosts to communicate through the outside/WAN network via IP masquerade.

set nat source rule 100 outbound-interface 'eth0'
set nat source rule 100 source address '18.0.0.0/24'
set nat source rule 100 translation address masquerade
commit
save
exit

# troubleshooting:
show interfaces ethernet eth1
delete interfaces ethernet eth1 address <address_to_delete>
```

port forward on vyos: [NAT — VyOS 1.3.x (equuleus) documentation](https://docs.vyos.io/en/equuleus/configuration/nat/index.html)

```bash
# static ip assignments
set protocols static arp 18.0.0.222 hwaddr 00:0C:29:98:D1:FA
show protocols static arp
```

## VyOS router Port Translation

```bash
# port forwarding using NAT
set nat destination rule 10 description 'Port Forward: HTTP to 18.0.0.222'
set nat destination rule 10 destination port '80'
set nat destination rule 10 inbound-interface 'eth0'
set nat destination rule 10 protocol 'tcp'
set nat destination rule 10 translation address '18.0.0.222'

set firewall name OUTSIDE-IN rule 20 action 'accept'
set firewall name OUTSIDE-IN rule 20 destination address '18.0.0.222'
set firewall name OUTSIDE-IN rule 20 destination port '80'
set firewall name OUTSIDE-IN rule 20 protocol 'tcp'
set firewall name OUTSIDE-IN rule 20 state new 'enable'
commit
save # simillaryl port 443 for rules 11 and 21
exit
```
