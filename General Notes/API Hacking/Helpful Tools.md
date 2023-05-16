## Install a Hypervisor (VMware)
Download and install VMware to host your Kali Linux VM.
[https://www.vmware.com/go/getplayer-win](https://www.vmware.com/go/getplayer-win) 

### **Install Kali Linux**
Download and Install the Kali VMWare 64-bit VM.
[https://www.kali.org/get-kali/#kali-virtual-machines](https://www.kali.org/get-kali/#kali-virtual-machines) 
For additional help with installation, please read the Kali.org docs ([https://www.kali.org/docs/installation/](https://www.kali.org/docs/installation/)).
User: kali
Pass: kali

### **Update Kali**
Once you have your Kali VM up and running, open the Kali Linux Terminal and use the following commands to update your system:
$ sudo apt update -y
$ sudo apt upgrade -y
$ sudo apt dist-upgrade -y

### **Update User Accounts**
When starting a new operating system it is always a great idea to update default credentials:
$ sudo passwd kali    (enter in a new more complex password)
$ sudo useradd -m hapihacker
$ sudo usermod -a -G sudo hapihacker
$ sudo chsh -s /bin/zsh hapihacker

### **Burp Suite Community Edition**
Burp Suite should come stock with the latest version of Kali, but if it does not then use the following command:
$ sudo apt-get install burpsuite -y
 Download Jython ([https://www.jython.org/download.html](https://www.jython.org/download.html)) and add the .jar file to the Extender Options:
![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/uShnJZrYSwObdQdeTKgn_Setup1.PNG)
Under the Extender BApp Store search for Autorize and install the extension.

### **Foxy Proxy Standard**
While Firefox is open use the shortcut **CTRL+Shift+A** or navigate to [https://addons.mozilla.org/en-US/firefox/addon](https://addons.mozilla.org/en-US/firefox/addon).
1.  Search for FoxyProxy Standard.
2.  Add FoxyProxy to Firefox.
3.  Install FoxyProxy Standard and add it to your browser.
4.  Click the fox icon at the top-right corner of your browser (next to the URL) and select Options.
5.  Select Proxies >Add New Proxy >Manual Proxy Configuration.
6.  Add 127.0.0.1 as the host IP address.
7.  Update the port to 8080 (Burp Suite’s default proxy settings).
8.  Under the General tab, rename the proxy to **BurpSuite**.
9.  Add a second new proxy:
    1.  Add 127.0.0.1 as the host IP address.
    2.  Update the port to 5555
    3.  Under the General tab, rename the proxy to **Postman**

### **Burp Suite Certificate**
1.  Start Burp Suite.
2.  Open your browser of choice.
3.  Using FoxyProxy, select the BurpSuite proxy. Navigate to [http://burpsuite](http://burpsuite/) and click the CA Certificate. This should initiate the download of the Burp Suite CA certificate.
4.  Save the certificate somewhere you can find it.
5.  Open your browser and import the certificate. In Firefox, open Preferences and use the search bar to look up certificates. Import the certificate.  
    ![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/pNXd3clsQOySA6FP6NLQ_Capturecert.PNG)
6.  In Chrome, open Settings, use the search bar to look up certificates,  
    select More>Manage Certificates>Authorities, and import the certificate. If you do not see the BurpSuite cacert.der certificate. (You may need to expand the file type options to “DER” or “All files").  
    ![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/94VQmADbSqCAZbaz2zKT_Capturecert2.PNG)
![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/eu50S2KuTmuIdfHu47Q3_Capturecert3.PNG)
Now that you have the PortSwigger CA certificate added to your browser, you should be able to intercept traffic without experiencing issues.

### **MITMweb Certificate Setup**
Now we will also import the cert for MITMweb through a very similar process.
1.  Stop burpsuite (it's listening on 8080 and mitmweb needs that to work)
2.  Start mitmweb from the terminal:  
    $mitmweb
3.  Use FoxyProxy in Firefox to send traffic to the BurpSuite proxy (8080).
4.  Using Firefox Visit mitm.it.  
    ![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/2tR5PEZbQLU0Oh8rNK7E_cert101.PNG)
5.  Download the mitmproxy-ca-cert.pem for Firefox. 
6.  Return to the Firefox certificates (see Burp Suite Certificate instructions).  
    ![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/94VQmADbSqCAZbaz2zKT_Capturecert2.PNG)
7.  Import the MITMweb (mitmproxy-ca-cert.pem) certificate.  
    ![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/mEXrG0xJSQeDsjpAWqR5_Capturecert4.PNG)

### **Install Postman**
$ sudo wget https://dl.pstmn.io/download/latest/linux64 -O postman-linux-x64.tar.gz && sudo tar -xvzf postman-linux-x64.tar.gz -C /opt && sudo ln -s /opt/Postman/Postman /usr/bin/postman

### Install mitmproxy2swagger
$ sudo pip3 install mitmproxy2swagger  
  
### **Install Git**
$ sudo apt-get install git

 **Install Docker**
$ sudo apt-get install docker.io docker-compose

### **Install Go**
$ sudo apt install golang-go

### **The JSON Web Token Toolkit v2**
$ cd /opt
$ sudo git clone [https://github.com/ticarpi/jwt_tool](https://github.com/ticarpi/jwt_tool)
$ cd jwt_tool
$ python3 -m pip install termcolor cprint pycryptodomex requests
**(Optional) Make an alias for jwt_tool.py**
$ sudo chmod +x jwt_tool.py
$ sudo ln -s /opt/jwt_tool/jwt_tool.py /usr/bin/jwt_tool

### **Install Kiterunner**
$ sudo git clone  [https://github.com/assetnote/kiterunner.git](https://github.com/assetnote/kiterunner.git)
$ cd kiterunner
$ sudo make build
$ sudo ln -s /opt/kiterunner/dist/kr /usr/bin/kr

### **Install Arjun**
$ sudo git clone [https://github.com/s0md3v/Arjun.git](https://github.com/s0md3v/Arjun.git)

### **Install OWASP ZAP**
$ sudo apt install zaproxy
Once ZAP is installed, make sure to navigate to the Manage Add-Ons (CTRL+U). Make sure to apply updates for the Fuzzer and OpenAPI Support.

### **Useful Wordlists**
**SecLists ([https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists))**
$ sudo wget -c https://github.com/danielmiessler/SecLists/archive/master.zip -O SecList.zip \  
&& sudo unzip SecList.zip \  
&& sudo rm -f SecList.zip
**Hacking-APIs ([https://github.com/hAPI-hacker/Hacking-APIs](https://github.com/hAPI-hacker/Hacking-APIs))**
$ sudo wget -c [https://github.com/hAPI-hacker/Hacking-APIs/archive/refs/heads/main.zip](https://github.com/hAPI-hacker/Hacking-APIs/archive/refs/heads/main.zip) -O HackingAPIs.zip \  
&& sudo unzip HackingAPIs.zip \  
&& sudo rm -f HackingAPIs.zip

