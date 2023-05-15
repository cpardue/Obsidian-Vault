## Burp Suite
A web proxy, you know

### Initial setup
Start burp
Open firefox
Goto Preferences, Settings, enter 127.0.0.1:8080 for the Proxy
Uh use FoxyProxy extension instead
Go to https://burp

10/44

Allow cert permanently
Click on CA Certificate, save
Go back into Firefox Preferences, Privacy and Security, View Certificates, Import, select the CA Cert, Open, check both
boxes, OK

### Info gathering
It intercepts the web requests that are sent to and from webservers
Target tab shows intercepted traffic, including linked API traffic and web plugins
Use this info to enumerate the website by clicking through responses
BurpSuite Pro is $399 per year, btw

### Burpsuite intro
start foxyproxy
start burp
intercept one request
send to Repeater
edit the sent requests directly through Repeater and see response in real time (GET/POST etc)
go to target
set scope to each target and port
check server headers for information disclosure of webserver