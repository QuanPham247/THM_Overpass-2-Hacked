# THM_Overpass-2-Hacked
Since Overpass is a web application, http is what we want to filter.

What was the URL of the page they used to upload a reverse shell?
![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/d8d947cf-b058-41f3-bcaf-78d7193e9639)

What payload did the attacker use to gain access?
We can see there is a POST request. Let's follow its HTTP stream
![image](https://github.com/QuanPham247/THM_Overpass-2-Hacked/assets/97132705/f1950d2b-09af-4b2d-8923-0e5dd9d78982)

What password did the attacker use to privesc?
Based on the payload, we can see that it wants to open a reverse shell back to the attacker machine. 

