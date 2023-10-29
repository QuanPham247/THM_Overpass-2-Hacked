# THM_Overpass-2-Hacked
**Part 1: Forensics, Analyse the pcap**
Since Overpass is a web application, http is what we want to filter.

What was the URL of the page they used to upload a reverse shell?

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/d8d947cf-b058-41f3-bcaf-78d7193e9639)

What payload did the attacker use to gain access?
We can see there is a POST request. Let's follow its HTTP stream

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/f1950d2b-09af-4b2d-8923-0e5dd9d78982)

What password did the attacker use to privesc?
Based on the payload, we can see that it wants to open a reverse shell back to the attacker machine. 

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/51609cab-7973-404a-b7e2-1742b2161d0e)

Filter out the destination IP, then follow the TCP stream of the first packet with the destination port of 4242. 

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/ba1d0cb4-71da-4059-a104-727387b891ee)

How did the attacker establish persistence?
Take a look at the rest of TCP stream, after gaining privileges, he was having a look at /etc/shadow, and clone a github repo. 

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/3d9897ad-4377-43c1-828a-5c0abad470ac)

Using the fasttrack wordlist, how many of the system passwords were crackable?

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/27e8b2d8-21a2-4230-a084-502bb5c6edd0)

We're using John to crack passwords from the shadow file.

Copy the user's hashes to a text file and use John to crack it. 

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/47ec7994-ffc2-4a87-b400-e4151c418401)



**Part 2: Research - Analyse the code**:

What's the default hash for the backdoor?
https://github.com/NinjaJc01/ssh-backdoor/blob/master/main.go

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/9d73b260-ae84-42c3-9924-153e12ce6329)


What's the hardcoded salt for the backdoor?

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/b950b677-60c2-4955-b5b2-acac2dece41e)



What was the hash that the attacker used? - go back to the PCAP for this!
![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/ca7fae01-a512-44d3-b460-3b0203348293)

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/935b32e0-35b4-4017-854f-d9fbf2b44f05)



Crack the hash using rockyou and a cracking tool of your choice. What's the password?
![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/74429f7d-e31b-4f9f-8f46-93b2285cf3b1)

Through this function, we know that the hash password uses SHA512, with password + salt. 
Copy the hash from question 3 and the salt from question 2, separated by a colon. 

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/17d52dd0-dd8a-41c9-a267-69b6121d8e8f)

For this task, I'm using hashcat since John, for some reason, doesn't recognize the format.
hashcat -m 1710 -a 0 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

**Part 3:** 
The attacker defaced the website. What message did they leave as a heading?
![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/b199e690-bfbd-4a91-b4f2-57bbbb6a1371)

Using the information you've found previously, hack your way back in!
In the pcap file, notice that the hacker has left a backdoor using SSH port 2222. We can log back in using the cracked password in the previous part. 
![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/93711244-2e1f-41a0-9b21-cfe0ddc3a99e)

What's the user flag?
Check james's home directory.

What's the root flag? 
ls -la. 
![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/cd713518-5619-4ac6-bc94-b9261ead4146)

It's a file that is owned by root but has suid. 
![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/c541817a-0bf3-456d-9b65-878ec3bb8cd8)

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/6555466d-7478-472e-a24d-8c0fad8953a8)
If you only ran ./.suid_bash, the command is still executed under James's privilege. 

![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/8f1780dc-acd5-4653-be9c-bdf1035d60f7)
Specifies the -p option, will not reset the user id back to James. 
![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/e9447146-98ab-47af-b68b-87a1e3070767)

**P/s**: I really enjoyed this room, what an eye-opening and educational experience. 

References: 
+ https://www.youtube.com/watch?v=XtySdRYCbiY
+ https://superuser.com/questions/532121/what-does-p-do-on-shell-script
+ https://hashcat.net/wiki/doku.php?id=example_hashes





