# THM_Overpass-2-Hacked
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
