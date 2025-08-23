> OMG RESIPEAK EVIL MACHINE, BEST MACHINE SO FAR (MUCH BETTER THAN MR ROBOT CTF)
![[Pasted image 20250823162137.png]]
# [Challenge](https://tryhackme.com/room/biohazard)

- # ENUM
	- ### nmap (`nmap -p- -A -T4 --min-rate=5000 biohazard`)
		- 21 -> ftp
		- 22 -> ssh
		- 80 -> http (Apache 2.4.29)

- # EXPLOITATION & FLAGS
	- ## THE MANSION
		- `/mansionmain` -> `/diningRoom` -> `emblem{fec832623ea498e20bf4fe1821d58727}`
		- `/teaRoom` -> `lock_pick{037b35e2ff90916a9abf99129c8e1837}`
		- `/artRoom` -> Found a map!
			- Location:
				/diningRoom/  
				/teaRoom/  
				/artRoom/  
				/barRoom/  
				/diningRoom2F/  
				/tigerStatusRoom/  
				/galleryRoom/  
				/studyRoom/  
				/armorRoom/  
				/attic/
		- `/barRoom` -> lockpick -> `NV2XG2LDL5ZWQZLFOR5TGNRSMQ3TEZDFMFTDMNLGGVRGIYZWGNSGCZLDMU3GCMLGGY3TMZL5` -- (`base32`) --> `music_sheet{362d72deaf65f5bdc63daece6a1f676e}`
		- `/barRoom357162e3db904857963e6e0b64b96ba7` -> play the piano with the music sheet -> `gold_emblem{58a8c41a9d08b8a4e38d02a4d7ff4843}`
		- In the `/diningRoom` put the golden emblem -> `klfvg ks r wimgnd biz mpuiui ulg fiemok tqod. Xii jvmc tbkg ks tempgf tyi_hvgct_jljinf_kvc` -- (vigenere) --> `there is a shield key inside the dining room. The html page is called the_great_shield_key` -> `shield_key{48a7a9227cd7eb89f0a062590798cbac}`
		- `/diningRoom2F`: `Lbh trg gur oyhr trz ol chfuvat gur fgnghf gb gur ybjre sybbe. Gur trz vf ba gur qvavatEbbz svefg sybbe. Ivfvg fnccuver.ugzy` -- (vigenere) --> `You get the blue gem by pushing the status to the lower floor. The gem is on the diningRoom first floor. Visit sapphire.html` -> `blue_jewel{e1d457e96cac640f863ec7bc475d48aa}`
		- `/tigerStatusRoom` -> blue jewel -> ```crest 1:  
			S0pXRkVVS0pKQkxIVVdTWUpFM0VTUlk9  
			Hint 1: Crest 1 has been encoded twice  
			Hint 2: Crest 1 contanis 14 letters  
			Note: You need to collect all 4 crests, combine and decode to reavel another path  
			The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
			- Crest 1 -- (base64) --- (base32) --> `RlRQIHVzZXI6IG`
		- `/gallery` -> ```crest 2:
			GVFWK5KHK5WTGTCILE4DKY3DNN4GQQRTM5AVCTKE
			Hint 1: Crest 2 has been encoded twice
			Hint 2: Crest 2 contanis 18 letters
			Note: You need to collect all 4 crests, combine and decode to reavel another path
			The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
			- Crest 2 -- (base32) --- (base58) --> `h1bnRlciwgRlRQIHBh
		- `/armorRoom` -> shield key -> ```crest 3:
				MDAxMTAxMTAgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAxMDAgMDExMDAxMDAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAxMDAgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMDAgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMTEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMDEgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTAxMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMDEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMDAgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTEwMDA=
				Hint 1: Crest 3 has been encoded three times
				Hint 2: Crest 3 contanis 19 letters
				Note: You need to collect all 4 crests, combine and decode to reavel another path
				The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
			- Crest 3 -- (base64) --- (binary) --- (hex) --> `c3M6IHlvdV9jYW50X2h`
		- `/attic` -> shield emblem -> ```crest 4:
			gSUERauVpvKzRpyPpuYz66JDmRTbJubaoArM6CAQsnVwte6zF9J4GGYyun3k5qM9ma4s
			Hint 1: Crest 2 has been encoded twice
			Hint 2: Crest 2 contanis 17 characters
			Note: You need to collect all 4 crests, combine and decode to reavel another path
			The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
			- Crest 4 -- (base58) -- (hex) --> `pZGVfZm9yZXZlcg==`
		 - Full crest: `RlRQIHVzZXI6IGh1bnRlciwgRlRQIHBhc3M6IHlvdV9jYW50X2hpZGVfZm9yZXZlcg==` -- (base64) --> `FTP user: hunter, FTP pass: you_cant_hide_forever`
	- ## THE GUARD HOUSE
		- Let's connect with FTP (`hunter:you_cant_hide_forever`) and get all the files
		- `important.txt`: ```Jill,
			I think the helmet key is inside the text file, but I have no clue on decrypting stuff. Also, I come across a /hidden_closet/ door but it was locked.
			
			From,
			Barry
		- Let's use `steghide` on `001-key.jpg`:
			- `cGxhbnQ0Ml9jYW`
		- `002-key.jpg` has a comment in it!
			- `5fYmVfZGVzdHJveV9`
		- `003-key-jpg` has a `.txt` in it!
			- `key.txt` -> `3aXRoX3Zqb2x0`
		- `cGxhbnQ0Ml9jYW5fYmVfZGVzdHJveV93aXRoX3Zqb2x0` -- (base64) --> `plant42_can_be_destroy_with_vjolt`
		- Decrypt `helmet_key.txt.gpg` (`gpg -d helmet_key.txt.gpg`)
			- `helmet_key{458493193501d2b94bbab2e727f8db4b}`
	- ## THE REVISIT
		- `/studyRoom` -> helmet key -> `doom.tar.gz`
			- Decompress it (`tar -xzvf doom`)
				- `eagle_medal.txt`
					- `SSH user: umbrella_guest`
		- `/hidden_closet` -> helmet key ->  `wpbwbxr wpkzg pltwnhro, txrks_xfqsxrd_bvv_fy_rvmexa_ajk` -- (vigenere) --> `weasker login password, stars_members_are_my_guinea_pig`
			- `SSH password: T_virus_rules`
	- ## UNDERGROUND LABORATORY 
		- `/home/weasker/weasker_note.txt`: ```Weaker: Finally, you are here, Jill.
			Jill: Weasker! stop it, You are destroying the  mankind.
			Weasker: Destroying the mankind? How about creating a 'new' mankind. A world, only the strong can survive.
			Jill: This is insane.
			Weasker: Let me show you the ultimate lifeform, the Tyrant.
			
			(Tyrant jump out and kill Weasker instantly)
			(Jill able to stun the tyrant will a few powerful magnum round)
			
			Alarm: Warning! warning! Self-detruct sequence has been activated. All personal, please evacuate immediately. (Repeat)
			Jill: Poor bastard
		- Let's find Chris: `find / -type f | grep "chris" 2>/dev/null`
			- `/home/umbrella_guest/.jailcell/chris.txt`: ``` Jill: Chris, is that you?
				Chris: Jill, you finally come. I was locked in the Jail cell for a while. It seem that weasker is behind all this.
				Jil, What? Weasker? He is the traitor?
				Chris: Yes, Jill. Unfortunately, he play us like a damn fiddle.
				Jill: Let's get out of here first, I have contact brad for helicopter support.
				Chris: Thanks Jill, here, take this MO Disk 2 with you. It look like the key to decipher something.
				Jill: Alright, I will deal with him later.
				Chris: see ya.
				
				MO disk 2: albert

- # PRIVILEGE ESCALATION
	- `su weasker`
	- `sudo -l` -> (ALL : ALL) ALL
		- `sudo su`
			- We're ROOT!
```
In the state of emergency, Jill, Barry and Chris are reaching the helipad and awaiting for the helicopter support.

Suddenly, the Tyrant jump out from nowhere. After a tough fight, brad, throw a rocket launcher on the helipad. Without thinking twice, Jill pick up the launcher and fire at the Tyrant.

The Tyrant shredded into pieces and the Mansion was blowed. The survivor able to escape with the helicopter and prepare for their next fight.

The End

flag: 3c5794a00dc56c35f2bf096571edf3bf
```