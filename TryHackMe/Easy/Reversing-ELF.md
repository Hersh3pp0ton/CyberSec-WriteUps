# [Challenges](https://tryhackme.com/room/reverselfiles)

- # Crack #1
	- `./crackme1`

- # Crack #2
	- The password is in the decompiled code: ![[Pasted image 20250822175011.png]]
	- `./crackme2 super_secret_password`

- # Crack #3
	- `strings crackme3` -> `ZjByX3kwdXJfNWVjMG5kX2xlNTVvbl91bmJhc2U2NF80bGxfN2gzXzdoMW5nNQ==` --- (base64) --> `f0r_y0ur_5ec0nd_le55on_unbase64_4ll_7h3_7h1ng5`

- # Crack #4
	- Let's use `pwndbg` and put a breakpoint at the address `0x004006c7` (before the call at the `strcmp()`) and let's check the registers:![[Pasted image 20250823120738.png]]
	- Here it is!

- # Crack #5
	- Let's use `pwndbg` and put a breakpoint at the address `0x00400837`: ![[Pasted image 20250823125904.png]]
	  ![[Pasted image 20250823125935.png]]
	  (RDI is the first parameter (test), RSI is the second one, the correct input)

- # Crack #6
	- ![[Pasted image 20250823130402.png]]
	  Very easy this time, just check `my_secure_test()`

- # Crack #7
	- ![[Pasted image 20250823130805.png]]
	  All you need is converting `0x7a69` in decimal (31337) and give it as the choice.

- # Crack #8
	- ![[Pasted image 20250823131124.png]]
	  Like the last one, just give that number as input.
