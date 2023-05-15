hashcat --help for more info

-m 5600 is NTLMv2, according to --help
You can do "hashcat --help | grep NTLM" if you want. 

What you'll need: 
1. Hashcat installed (which hashcat)
2. A file consisting of one hash. 
3. A wordlist. 
4. To know which module to use (hashcat -- help | grep NTLMv2 or whatever)

>hashcat -m 5600 file.txt wordlist.txt

If in a VM, try adding --force. Might still fail. 
If so, then just go to hashcat.com and install the Windows binary, run directly from your OS. 
Don't forget to copy over your hash and wordlist. 

From Windows: 
>cd (hashcat binary location)
>hashcat64.exe -m 5600 hashfile.txt wordlist.txt -O

-O is for Optimize

### MISC and tricks 

`https://www.notsosecure.com/one-rule-to-rule-them-all/`

```
# MAX POWER 
# force the CUDA GPU interface, optimize for <32 char passwords and set the workload to insane (-w 4). 
# It is supposed to make the computer unusable during the cracking process 
# Finnally, use both the GPU and CPU to handle the cracking --force -O -w 4 --opencl-device-types 1,2

  

### Wrapcat - Automating hashcat commands 

https://twitter.com/Haax9_/status/1340354639464722434?s=20 
https://github.com/Haax9/Wrapcat  
$ python wrapcat.py -m 1000 -f HASH_FILE.txt -p POT_FILE.txt --full --save

  

### Attack modes 

-a 0 
# Straight : hash dict 
-a 1 
# Combination : hash dict dict 
-a 3 
# Bruteforce : hash mask 
-a 6 
# Hybrid wordlist + mask : hash dict mask 
-a 7 
# Hybrid mask + wordlist : hash mask dict

  

### Charsets 

?l 
# Lowercase a-z 
?u 
# Uppercase A-Z 
?d 
# Decimals 
?h 
# Hex using lowercase chars 
?H 
# Hex using uppercase chars 
?s 
# Special chars 
?a 
# All (l,u,d,s) 
?b 
# Binary

  

### Options 

-m 
# Hash type 
-a 
# Attack mode 
-r 
# Rules file 
-V # Version --status 
# Keep screen updated 
-b 
# Benchmark 
--runtime 
# Abort after X seconds 
--session [text] 
# Set session name 
--restore 
# Restore/Resume session 
-o filename 
# Output to filename 
--username 
# Ignore username field in a hash 
--potfile-disable 
# Ignore potfile and do not write 
--potfile-path 
# Set a potfile path 
-d 
# Specify an OpenCL Device 
-D 
# Specify an OpenCL Device Type 
-l 
# List OpenCL Devices & Types 
-O 
# Optimized Kernel, Passwords <32 chars 
-i 
# Increment (bruteforce) 
--increment-min 
# Start increment at X chars 
--increment-max 
# Stop increment at X chars

  

### Examples 

# Benchmark MD4 hashes 
hashcat -b -m 900  
# Create a hashcat session to hash Kerberos 5 tickets using wordlist 
hashcat -m 13100 -a 0 --session crackin1 hashes.txt wordlist.txt -o output.pot  
# Crack MD5 hashes using all char in 7 char passwords 
hashcat -m 0 -a 3 -i hashes.txt ?a?a?a?a?a?a?a -o output.pot  
# Crack SHA1 by using wordlist with 2 char at the end  
hashcat -m 100 -a 6 hashes.txt wordlist.txt ?a?a -o output.pot  
# Crack WinZip hash using mask (Summer2018!) 
hashcat -m 13600 -a 3 hashes.txt ?u?l?l?l?l?l?l?d?d?d?d! -o output.pot  
# Crack MD5 hashes using dictionnary and rules 
hashcat -a 0 -m 0 example0.hash example.dict -r rules/best64.rules  
# Crack MD5 using combinator function with 2 dictionnaries 
hashcat -a 1 -m 0 example0.hash example.dict example.dict  
# Cracking NTLM hashes 
hashcat64 -m 1000 -a 0 -w 4 --force --opencl-device-types 1,2 -O d:\hashsample.hash "d:\WORDLISTS\realuniq.lst" -r OneRuleToRuleThemAll.rule  
# Cracking hashes from kerberoasting 
hashcat64 -m 13100 -a 0 -w 4 --force --opencl-device-types 1,2 -O d:\krb5tgs.hash d:\WORDLISTS\realhuman_phill.txt -r OneRuleToRuleThemAll.rule

# You can use hashcat to perform combined attacks # For example by using wordlist + mask + rules hashcat -a 6 -m 0 prenoms.txt ?d?d?d?d -r rules/yourule.rule  
# Single rule used to uppercase first letter --> Marie2018 
hashcat -a 6 -m 0 prenoms.txt ?d?d?d?d -j 'c'

  

### Scenario - Cracking large files (eg NTDS.dit) 

# Start by making a specific potfile and cracked files (clean environment) 
# - domain_ntds.dit 
# - domain_ntds_potfile.pot  
# Goal is to run many different instances with different settings, so each one have # to be quite quick  
# You can generate wordlist using CeWL 
# It usually works pretty well 
cewl -d 5 -m 4 -w OUTFILE -v URL cewl -d 5 -m 4 -w OUTFILE -o -v URL  
# With some basic dictionnary cracking (use known wordlists) # rockyou, hibp, crackstation, richelieu, kaonashi, french and english  
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 0 rockyou.txt --force -O  
# Then start to use wordlists + masks + simple rule 
# For special chars, you can use a custom charset : "?!%$&#-_@+=* " 
# Multiple tests, multiples masks and multiples wordlists (including generated ones) 
.\hashcat64.exe -m 1000 hashs.txt -a 6 .\french\* '?d?d?d?d' -j c --increment --force -O .\hashcat64.exe -m 1000 hashs.txt -a 6 .\french\* -1 .\charsets\custom.chr '?1' -j c --force -O 
.\hashcat64.exe -m 1000 hashs.txt -a 6 .\french\* -1 .\charsets\custom.chr '?d?1' -j c --force -O 
.\hashcat64.exe -m 1000 hashs.txt -a 6 .\french\* -1 .\charsets\custom.chr '?d?d?1' -j c --force -O 
.\hashcat64.exe -m 1000 hashs.txt -a 6 .\french\* -1 .\charsets\custom.chr '?d?d?d?1' -j c --force -O 
.\hashcat64.exe -m 1000 hashs.txt -a 6 .\french\* -1 .\charsets\custom.chr '?d?d?d?d?1' -j c --force -O 
.\hashcat64.exe -m 1000 hashs.txt -a 6 CEWL_WORDLIST.txt -1 .\charsets\custom.chr '?d?d?d?d?1' -j c --force -O 
.\hashcat64.exe ...  
# Same commands and behavior but using mask after the tested word (mode 7) 
.\hashcat64.exe -m 1000 hashs.txt -a 7 '?d?d?d?d' .\french\* -j c --increment --force -O  
# Then, wordlists + complex rules 
# Once again run against multiple wordlists (including generated ones) 
# Kaonashi and OneRuleToRuleThemAll can produce maaaaaassive cracking time 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 0 french.txt -r .rules\best64.rule --force -O 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 0 french.txt -r .rules\OneRuleToRuleThemAll.rule --force -O 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 0 french.txt -r .rules\best64.rule --force -O 
.\hashcat64.exe ...  
# Then smart bruteforce using masks (custom charset can be usefull too) 
# Can be quite long, depending on the mask. Many little tests with different masks 
# Knowing for example that password is min 8 char long, only 8+ masks 
# Play by incrementing or decrementing char vs decimal (you can also use specific charset to reduce time) 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 3 '?u?l?l?l?d?d?d?d' --force -O 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 3 '?u?l?l?l?l?d?d?d' --force -O 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 3 '?u?l?l?l?l?l?d?d' --force -O 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 3 -1 
.\charset\custom '?u?l?l?l?l?l?d?1' --force -O 
.\hashcat64.exe ...  
# Then increment mask size and play again 
# Can be longer for 9 char and above.. Up to you to decide which masks and how long you wanna wait 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 3 '?u?l?l?l?d?d?d?d?d' --force -O 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 3 '?u?l?l?l?l?d?d?d?d' --force -O 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 3 '?u?l?l?l?l?l?d?d?d' --force -O 
.\hashcat64.exe ...  
# If you have few hashes and small/medium wordlist, you can use random rules 
# And make several loops 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 0 wl.txt -g 1000000  --force -O -w 3  
# You can use combination attacks 
# For example, combine different names, or combine names with dates.. Then apply masks 
# Directly using hashcat 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 1 wordlist1.txt wordlist2.txt --force -O 
# Or in memory feeding, it allows you to use rules but not masks 
.\combinator.exe wordlist1.txt wordlist2.txt | 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 0 -rules .\rules\best64.rule --force -O # Or create the wordlist before and use it 
.\combinator.exe wordlist1.txt wordlist2.txt .\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot -a 6 combinedwordlist.txt '?d?d?d?d' -j c --increment --force -O  
# Finally use your already cracked passwords to build a new wordlist 
.\hashcat64.exe -m 1000 hashs.txt --potfile-path potfile.pot --show | %{$_.split(':')[1]} > cracked.txt .\hashcat64.exe -m 1000 hashs.txt -a 6 cracked.txt '?d?d?d?d' -j c --increment --force -O 
.\hashcat64.exe -m 1000 hashs.txt -a 0 cracked.txt -r .rules\OneRuleToRuleThemAll.rule --force -O  
# You can also checks the target in popular leaks to find some password 
# Then try reuse or rules on them
```
