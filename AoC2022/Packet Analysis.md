The Story  

![AoC Day 13](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/1893fdf9cd7a4530786f64fa5dad0825.png)  

Check out SecurityNinja's video walkthrough for Day 13 [here](https://www.youtube.com/watch?v=rSyR8YFbOlI)!  

After receiving the phishing email on Day 6 and investigating malware on Day 12, it seemed everything was ready to go back to normal. However, monitoring systems started to show suspicious traffic patterns just before closing the case. Now Santa's SOC team needs help in analysing these suspicious network patterns.  

Learning Objectives

-   Learn what traffic analysis is and why it still matters.
-   Learn the fundamentals of traffic analysis.
-   Learn the essential Wireshark features used in case investigation.
-   Learn how to assess the patterns and identify anomalies on the network.
-   Learn to use additional tools to identify malicious addresses and conduct further analysis.
-   Help the Elf team investigate suspicious traffic patterns.

Packets and Packet Analysis?

Packets are the most basic unit of the network data transferred over the network. When a message is sent from one host to another, it is transmitted in small chunks; each called a packet. Packet analysis is the process of extracting, assessing and identifying network patterns such as connections, shares, commands and other network activities, like logins, and system failures, from the prerecorded traffic files. 

Why Does Packet Analysis Still Matter?

Network traffic is a pure and rich data source. A Packet Capture (PCAP) of network events provides a rich data source for analysis. Capturing live data can be focused on traffic flow, which only provides statistics on the network traffic. On the other hand, identifying and investigating network patterns in-depth is done at the packet level. Consequently, threat detection and real-time performance troubleshooting cannot be done without packet analysis.

Today, most network-based detection mechanisms and notification systems ingest and parse packet-level information to create alerts and statistical data. Also, most red/blue/purple teaming exercises are optimised with packet-level analysis. Lastly, even encoded/encrypted network data still provides value by pointing to an odd, weird, or unexpected pattern or situation, highlighting that packet analysis still matters.

Points to consider when working with PCAPs

There are various points to consider before conducting packet analysis. The essential points are listed below.

**Point**

**Details**

Network and standard protocols knowledge.  

Knowledge of the network and protocol operations is a must. An analyst must know how the protocols work and which protocol provides particular information that needs to be used for analysis. Also, knowing the "normal" and "abnormal" behaviours and patterns is a big plus!   

Familiarity with attack and defence concepts.  

You can't detect what you don't know. An analyst must know "how the attacks are conducted" to identify "what is happening" and decide "where to look".  

**Practical experience in analysis tools.**

You can't burn down the haystack to find a needle! An analyst must know how to use the tools to extract particular information from packet bytes.

When the time comes to do "packet level analysis", it might sound hard to implement the theory in practice. But creating "checklists" and "mini playbooks" will make the analysis process considerably easier. A simple process checklist for practical packet analysis is shown below.   

**Required Check**

**Details**

Hypothesis  

Having a hypothesis is important before starting packets.

The analyst should know what to look for before starting an analysis. 

Packet Statistics  

Viewing the packet statistics can show the analyst the weight of the traffic in the capture file.  
It helps analysts see the big picture in terms of protocols, endpoints and conversations.   

Known Services  

The services used in everyday operations like web browsing, file sharing and mailing are called known services.  
The analyst should know which protocol is associated with which service.  
Sometimes adversaries use the known services for their benefit, so it is important to know what "the normal" looks like. **Note:** Service is a capability/application that facilitates network operations between users and applications. The protocol is a set of rules that identify the data processing and transmission over the network. 

Unknown Services  

Unknown services are potential red flags.  
The analyst should know how to research unknown protocols and services and quickly use them for the sake of the analysis.  

Known patterns   

Known patterns represent the analyst's knowledge and experience.  
The analyst should know the most common and recent case patterns to successfully detect the anomalies at first glance.  

**Environment**

The analyst has to know the nature and dynamics of the working environment. This includes IP address blocks, hostname and username structure, used services, external resources, maintenance schedules, and average traffic load.

You will need a tool to record, view and investigate the packets. There are a couple of tools that help users investigate traffic and packet captures. In this task, we will use Wireshark.

What is Wireshark and How to Use It?

Wireshark is an industry-standard tool for network protocol analysis and is essential in any traffic and packet investigation. You can view, save and break down the network traffic with it. You can learn more about Wireshark by completing the [**Wireshark module**](https://tryhackme.com/module/wireshark).

A quick tool demonstration for fundamental analysis is shown below. Now click on the **Start Machine** button at the top of the task to launch the **Virtual Machine**. The machine will start in a split-screen view. In case the VM is not visible, use the blue Show Split View button at the top-right of the page.

After starting the given VM, open the Wireshark and go through the walkthrough below. Once you double-click the PCAP file, it will load up in the tool. Alternatively, you can open the tool, drag and drop the file, or use the **"File"** menu. 

![Wireshark file open](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/305ec782315c682aefeabf1de52d5c71.png)  

After opening a pcap file for the first time, it might look daunting to decide where to focus. Breaking down all the packets in a tree-based view will make the analysis easier. Statistics will show the overall usage of the ports and services, so it will be slightly easier to see the big picture in the capture file.

-   Use the "Statistics --> Protocol Hierarchy" menu to view the overall usage of the ports and services.

Now, look at the output. The majority of the traffic is on TCP and HTTP: 

![Wireshark protocol hierarchy](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/691c28c05b1602462c5350e0f9a61cc4.png)

You can also view the connections by IP and TCP/UDP protocols to view the overall usage of the ports and services (including the total packet numbers transferred over a particular port and between two hosts). The next step is viewing the IP conversations to spot if there is a weird/suspicious/not usual IP address in use.  

-   Close the protocol hierarchy window, use the "Statistics --> Conversations" section and navigate to the IPv4 section to view the list of IP conversations. 

**Note:** Navigate to the TCP/UDP sections to view the TCP/UDP conversation details.

![Wireshark conversations](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/889fa7ef773feb93dc4f6925cc46eb1b.png)

Now we have a detailed list of the IP addresses, port numbers, and the number of packets transferred from one endpoint to another. This information will help us identify suspicious IP addresses, connections and ports. Analyse the details carefully; we may discover the IP addresses and services used by the Bandit Yeti APT!  

Let's analyse the findings in this section; navigate to the TCP part and look at the results, the port 80 is used as a communication medium in TCP. Port 80 represents the HTTP service. Next, you can view that DNS service is also used by navigating to the UDP section. Now we have two target protocols to analyse. Before continuing on specific protocol analysis, you should have completed the following checks and answered some analysis questions.

![Detective Elf](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/aae52a7dddfb2baf5c042bab7ef48422.png)

-   **Checks to do**

-   Packet statistics
-   Service identification
-   IP reputation check

-   **Questions to answer**

-   Which IP addresses are in use?
-   Has a suspicious IP address been detected?
-   Has suspicious port usage been detected?
-   Which port numbers and services are in use?
-   Is there an abnormal level of traffic on any port or service?  
    

Note: You can use the OSINT tools mentioned on Day 6 to conduct a reputation check on suspicious IP/domain addresses. Note that you can't rely on the reputation check if nothing suspicious is detected at this stage. You still need to go further to discover potential anomalies. Also, if you can't recall or identify the port numbers and service names, you can use the "Google Dorks" search techniques shown on Day 3 to search and learn port numbers and service names.  

After viewing the conversations, we collected the following information.

-   Source and destination IP addresses
-   Protocols
-   Port numbers
-   Services

Now let's focus on the HTTP and DNS. As a nature of these protocols, everything transferred over these protocols is cleartext. At this stage, filtering the DNS packets to view the interacted domains is a good start before deep diving into cleartext data.

-   Close the statistics window, and type `DNS` in the search bar to apply a filter and view the DNS packets.

DNS packets will help us to identify connected domain addresses to decide if they are affiliated with the suspicious threat and Bandit Yeti APT! Click on the first packet and use the lower left section of the tool (Packet Details Pane) to view the packet details. There are multiple collapsed sections; click on the `Domain Name System` section to expand and view the DNS details of the packets. There are additional collapsed sections under the corresponding section; expand them to view all available details. You will find the interacted domain under the `queries` section. See the below example and continue the analysis by analysing all available DNS packets.

![Wireshark dns filter](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/fcd28b2747d0c620201d923443e553f4.png)

Before continuing on HTTP analysis, ensure you have completed the following checks and answered the analysis questions.  

-   Checks to do

-   DNS queries
-   DNS answers

-   Questions to answer

-   Which domain addresses are communicated?
-   Do the communicated domain addresses contain unusual or suspicious destinations? 
-   Do the DNS queries look unusual, suspicious or malformed?

We discovered the connected domain addresses, and now we are one step closer to identifying if these patterns are part of the adversarial actions of the Bandit Yeti APT. You should notice the obvious anomalous sign in the domain address at this stage! Let's filter the HTTP packets to view the traffic details and understand what happened!  

-   Use the `HTTP` filter to filter and view the HTTP packets.

Click on the first packet and view the details. In HTTP, the **"GET Request"** is used by the client to send a request to a server. Usually, this request ends up with receiving data/resources. Therefore, we will look at these requests to see if any endpoint is asked for a resource from other servers over the HTTP protocol. 

Apply the filter and expand the `Hypertext Transfer Protocol` section. Expand the subsections as well and focus on the GET requests. You will find the requested resource paths under the `Full Request URI` section. Also, you can evaluate the `user-agent` section to check if there is anomalous or non-standard user-agent info. See the below example and continue on analysis by analysing all available HTTP packets.

![Wireshark http filter](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/cc58cfbbd1ef429365c806c3746ae94c.png)  

Before continuing to the next steps, ensure you have completed the following checks and answered the analysis questions.

-   Checks to do

-   HTTP GET requests
-   Requested URIs
-   HTTP requests host addresses
-   Used user-agents

-   Questions to answer

-   Which addresses are communicated?
-   Is there any resource share event between addresses?
-   If there is a file share event, which addresses hosts which files?
-   Do the user-agent fields look unusual, suspicious or malformed?  
    

Here, you should identify the stealthy connections of the Bandit Yeti APT. It looks like the adversarial group chose to use the daily used services and create less noise over the common protocols to avoid being detected.  
  
The investigation case doesn't contain any obvious anomaly patterns like scanning, brute force and exploitation. However, it contains suspicious connections and file shares. These two are red flags and require in-depth analysis. Still, there are a few steps more before concluding the case and elevating it to upper-level analysts. In sum, we detected two unidentified domain addresses, one highly associated with the Bandit Yeti APT. Also, we have already identified the IP addresses, port numbers and domain addresses. The next step is focusing on file shares. Let's extract the shared files and conduct fundamental checks on the files before finishing the analysis.

-   Use the "File --> Export Object --> HTTP" menu to extract files shared over the HTTP.

Look at the results. There are two files shared over the HTTP. Use the `Save All` option and save them on the desktop. Now close/minimise the Wireshark GUI and open a terminal window on the desktop. Use the `sha256sum` and `VirusTotal` tools shown on Day 6 to calculate the file hash value and to conduct hash-based file reputation analysis. 

![Wireshark export objects](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/9cfd2f36bcc8a9b5883a2853d027614f.png)

Before concluding the analysis, ensure you have completed the following checks and answered the analysis questions.  

-   Checks to do

-   Shared files
-   File hashes (SHA256)
-   Hash reputation check

-   Questions to answer

-   What are shared files?
-   Does the hash reputation marked as suspicious or malicious?
-   Which domain hosts the suspicious/malicious file?

After completing the demonstrated steps, we verified that one shared file was malicious. Before concluding the analysis, you need to correlate the findings and recall which address was hosting the malicious file.

After finishing all the shown steps and completing the required checks, you are finished with the fundamental packet analysis process of the given case. The following steps are creating a report of your findings and escalating the sample to the upper-level analysts, who will conduct a more in-depth analysis.![Bandit Yeti APT](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/1a2c4c3e1b40bc65cec5b68d5c8d3176.png)  

Your report should include the following information you collected in this task.

-   Suspicious IP addresses associated with Bandit Yeti APT
-   Suspicious domain addresses associated with Bandit Yeti APT
-   Connection with suspicious addresses
-   Requested URIs
-   Used user-agents
-   Shared file names, extensions and hashes
-   Server names hosted the shared files

This was the initial analysis process of a PCAP file. An in-depth analysis will create detection rules to strengthen the implemented defences and block these activities in the future.