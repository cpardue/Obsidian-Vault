## Credential Stuffing
Credential Stuffing: Injecting breached account creds in hopes of account takeover
The art of credential stuffing is taking breach data and throwing it at a website
Google foxyproxy
Add it to your Firefox you built-in-proxy using naive
Add a Burpsuite proxy
Open Burp
Close, Next, Defaults
Open target webpage
Find the sign-in page
In Burp, turn on Intercept
Sign in with username AAAAA password BBBBB
In Burp, check intercepted request
Highlight and send AAAAA and BBBBB to intruder
Add both as parameters
Now you can use Sniper or Pitchfork
Sniper is single-field
Pitchfork is wordlist mode
Set userlist payload 1
Set passlist payload 2
Start Attack
Ok
While it's running, you're looking for status code results, Length differences, and results
One of these results is not like the others...
