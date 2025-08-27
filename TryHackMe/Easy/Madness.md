# [Challenge]()

- # ENUM
	- ### nmap(`nmap -A -p- -T4 --min-rate=5000 madness`)
		- 22 -> ssh
		- 80 -> http (Apache 2.4.18)

- # EXPLOITATION
	- There's an image in the website: let's change the magic bytes into `FF D8 FF E0 00`:
		- hidden directory: `/th1s_1s_h1dd3n`
	- "Secret entered: " maybe it's an URL parameter, let's check: `http://madness/th1s_1s_h1dd3n/index.php?secret=1`
		- "Secret Entered: 1" -> "`It's between 0-99 but I don't think anyone will look here`"
	- Let's make a script:
```python
import requests

URL = "http://madness/th1s_1s_h1dd3n/index.php?secret="

for i in range(1, 99):
	response = requests.get(URL + f"{i}")
	if not "wrong" in response.text:
		print(response.text)
		break
	else:
		print(f"{i} is wrong...")
```

`<p>Secret Entered: 73</p>`
`<p>Urgh, you got it right! But I won't tell you who I am! y2RPJ4QaPF!B</p>`

- Let's try using `steghide` in the previous image:
	- We have `hidden.txt`!
		- username:  `wbxre` -- (`ROT13`) --> `joker`
- I had no clue where the password was, so I searched for a writeup and... it's in the room's image 🤦
	- password: `*axA&GF8dP`


- # PRIVILEGE ESCALATION
	- We can't `sudo -l` :\
	- Let's check for SUID bit: `/bin/screen-4.5.0`, `/bin/screen-4.5.0.old`
		- Let's check for a [vuln](https://www.exploit-db.com/exploits/41154)
			- Let's execute it on sh
				- It doesn't work... let's execute command by command and...
					- We're ROOT!