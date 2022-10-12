### Agent Sudo Walkhthrough on Try Hack Me Course

welcome to my pages , this is the first my contekan at walkthrough to share my activity on learning hack in THM
 okay . maybe in this section i try to re try from another tutorial at other website such as medium.com tks to medium and authors

Visit https://tryhackme.com/room/agentsudoctf and join the room

lets start and deploy the machine.
<img width="1243" alt="image" src="https://user-images.githubusercontent.com/25836726/194892342-bad52878-b1ac-4b10-a6f5-bb6d0e419911.png">
 okay we got the target machine.
 (in this case maybe ip target machine is change because in other step because iam work at the different time)
 
 Enumerate the ip target machine and get all the important information, 
 we can start with nmap , lets try this command
 nmap -help to know whats the default command at NMAP.
 
 nmap -v -A IP Target Machine
 
 1. How many open ports?
<img width="597" alt="image" src="https://user-images.githubusercontent.com/25836726/195363793-3f5f381a-38cb-4421-ba9f-5191f2a2a3be.png">
port 21 22 and 80 is open , so 3 port is open

2. How you redirect yourself to a secret page?
user-agent

before it lets try explore another opportunity in this target machine.
first , lets check visit the target machine in port 80 with burpsuite

in this section we try to change user agent with intruder in burpsuite, and we have the result show Response user-agent : C is different .. and when we check te output location direct to a page
<img width="665" alt="image" src="https://user-images.githubusercontent.com/25836726/195391567-dd5889b8-4584-4891-8739-fd821e180125.png">

lets go to check the page
<img width="628" alt="image" src="https://user-images.githubusercontent.com/25836726/195391925-8c667a74-b07b-4159-9370-b6f93b8f7367.png">
frow that , we know another information such as username "chris"

What is the agent name?chris

from the nmap we know
port 21 and 22 is FTP , let we try to login using that username . but after it we dont know the pass.
just only 1 solution is bruceforce or try hydra tools

<img width="697" alt="image" src="https://user-images.githubusercontent.com/25836726/195393501-69088282-1053-416f-9dfe-867b1e3a9d06.png">

FTP password:crystal
from it we know the acsess ..

so lets to login using ftp accsess

<img width="698" alt="image" src="https://user-images.githubusercontent.com/25836726/195394190-133333f7-4f2d-4964-9016-a381d2eedcc8.png">
we got 3 files

lets check one by one the file

By viewing the “To_agentJ.txt” file, the message was a login

The password for the chris is stored in the fake picture.

So we use “Steghide” to retrieve some hidden info and also checked by “ExifTool” for the “cutie.png” file, but nothing came up. Trying to unzip it with unzip cutie.png, we get an error message: skipping: To_agentR.txt need PK compat. v5.1 (can do v4.6)

I used 7zip: 7z x cutie.png to try and extract it, but we get a password prompt.

After we tried with “binwalk,” and we got a zip file inside the “cutie.png” file and extracted it from “cutie.png,” but it was encrypted.

<img width="747" alt="image" src="https://user-images.githubusercontent.com/25836726/195395675-6f6bc7c9-a1ec-4dd1-b0f2-1cb76b5ef888.png">

With this, we get an extracted folder inside which we got zip file 8702.zip.

We can try to brute-force this with John the Ripper. First, we need to process the zip file into a format suitable for use with JtR. This can be done with zip2john.

<img width="656" alt="image" src="https://user-images.githubusercontent.com/25836726/195395831-388c6ebc-a395-43fb-84e0-5dccdf034162.png">

We know that our zip is encrypted. That’s a bummer. But we can get the password by using zip2john and then use john to crack the hash

zip2john 8702.zip > Output.txt

john Output.txt
<img width="733" alt="image" src="https://user-images.githubusercontent.com/25836726/195395904-b33b12ea-f0bd-43d8-aa45-9fab42072078.png">

We got alien a password, so we tried to extract the zip file but unzip command didn’t work, so we used this command to use

7z e 8702.zip

and password as alien

<img width="641" alt="image" src="https://user-images.githubusercontent.com/25836726/195396134-c1545e42-c72b-4278-894b-4c05c7bd36fb.png">
<img width="701" alt="image" src="https://user-images.githubusercontent.com/25836726/195396204-4029281f-8e1b-40df-86f0-3d5169c97f8b.png">
The text in quotes looks like what we want, but it looks like it is encoded. This message was from Agent R. We decoded this QXJlYTUx message using Cyberchef. Cyberchef suggests auto decoding. CyberChef works like magic and suggests auto decoding using Base64.

<img width="725" alt="image" src="https://user-images.githubusercontent.com/25836726/195396297-9eef4a23-16f7-4fb1-a377-9fc570c63653.png">

<img width="742" alt="image" src="https://user-images.githubusercontent.com/25836726/195396351-f9be5197-206f-43c8-bcbd-4af24fd18368.png">

<img width="670" alt="image" src="https://user-images.githubusercontent.com/25836726/195396565-45771254-a8b6-4073-9abc-07febefd3712.png">

<img width="742" alt="image" src="https://user-images.githubusercontent.com/25836726/195396613-b5f472e0-8548-4d41-9d53-0318147ff1ef.png">

User Flag & Privilege Escalation

Now we logged in through SSH into the machine using the username and password we found. Now we can read the user flag

ssh james@10.10.25.88

with password hackerrules!

<img width="741" alt="image" src="https://user-images.githubusercontent.com/25836726/195396871-a2ad5e96-63bd-4e92-9921-c7df96472963.png">

And the other is an image. We need to find out where the image is from. You can use the command below to download the image from the machine and do a reverse image search on Google

<img width="659" alt="image" src="https://user-images.githubusercontent.com/25836726/195397021-695c2159-162b-4082-b460-4436bc9aa99d.png">

Case CLose report

*soucre https://vivekanandagn.medium.com/agent-sudo-walkthrough-tryhackme-571f97c355e5






 
