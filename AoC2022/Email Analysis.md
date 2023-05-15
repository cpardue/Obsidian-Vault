The Story  

![AoC Day 6](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/05efac424e76531bc2a88d0327d1df4a.png)  

Check out CyberSecMeg's video walkthrough for Day 6 [here](https://www.youtube.com/watch?v=dcGvnZ4JDHI)!

Elf McBlue found an email activity while analysing the log files. It looks like everything started with an email...

Learning Objectives

-   Learn what email analysis is and why it still matters.
-   Learn the email header sections.
-   Learn the essential questions to ask in email analysis.
-   Learn how to use email header sections to evaluate an email.
-   Learn to use additional tools to discover email attachments and conduct further analysis.
-   Help the Elf team investigate the suspicious email received.

What is Email Analysis?

Email analysis is the process of extracting the email header information to expose the email file details. The email header contains the technical details of the email like sender, recipient, path, return address and attachments. Usually, these details are enough to determine if there is something suspicious/abnormal in the email and decide on further actions on the email, like filtering/quarantining or delivering. This process can be done manually and with the help of tools.

There are two main concerns in email analysis.

-   **Security issues:** Identifying suspicious/abnormal/malicious patterns in emails.
-   **Performance issues:** Identifying delivery and delay issues in emails.

In this task, we will focus on security concerns on emails, a.k.a. phishing. Before focusing on the hands-on email analysis, you will need to be familiar with the terms "social engineering" and "phishing".

-   **Social engineering:** Social engineering is the psychological manipulation of people into performing or divulging information by exploiting weaknesses in human nature. These "weaknesses" can be curiosity, jealousy, greed, kindness, and willingness to help someone.
-   **Phishing:** Phishing is a sub-section of social engineering delivered through email to trick someone into either revealing personal information and credentials or executing malicious code on their computer.

Phishing emails will usually appear to come from a trusted source, whether that's a person or a business. They include content that tries to tempt or trick people into downloading software, opening attachments, or following links to a bogus website. You can find more information on phishing by completing the [**phishing module**](https://tryhackme.com/module/phishing).

Does the Email Analysis Still Matter?

Yes! Various academic research and technical reports highlight that phishing attacks are still extremely common, effective and difficult to detect. It is also part of penetration testing and red teaming implementations (paid security assessments that examine organisational cybersecurity). Therefore, email analysis competency is still an important skill to have. Today, various tools and technologies ease and speed up email analysis. Still, a skilled analyst should know how to conduct a manual analysis when there is no budget for automated solutions. It is also a good skill for individuals and non-security/IT people!

Important Note: In-depth analysis requires an isolated environment to work. It is only suggested to download and upload the received emails and attachments if you are in the authorised team and have an isolated environment. Suppose you are outside the corresponding team or a regular user. In that case, you can evaluate the email header using the raw format and conduct the essential checks like the sender, recipient, spam score and server information. Remember that you have to inform the corresponding team afterwards.

How to Analyse Emails?

Before learning how to conduct an email analysis, you need to know the structure of an email header. Let's quickly review the email header structure.

**Field**

**Details**

**From**

The sender's address.

**To**

The receiver's address, including CC and BCC.

**Date**

Timestamp, when the email was **sent.**

**Subject**

The subject of the email.

**Return Path**

The return address of the reply, a.k.a. "Reply-To".

If you reply to an email, the reply will go to the address mentioned in this field.

**Domain Key and DKIM Signatures**

Email signatures are provided by email services to identify and authenticate emails.

**SPF**

Shows the server that was used to send the email.

It will help to understand if the actual server is used to send the email from a specific domain.

**Message-ID**

Unique ID of the email.

**MIME-Version**

Used MIME version.

It will help to understand the delivered "non-text" contents and attachments.

**X-Headers**

The receiver mail providers usually add these fields.

Provided info is usually experimental and can be different according to the mail provider.

**X-Received**

Mail servers that the email went through.

**X-Spam Status**

Spam score of the email.

**X-Mailer**

Email client name.

Important Email Header Fields for Quick Analysis

Analysing multiple header fields can be confusing at first glance, but starting from the key points will make the analysis process slightly easier. A simple process of email analysis is shown below.  

**Questions to Ask / Required Checks** 

**Evaluation**

Do the "From", "To", and "CC" fields contain valid addresses?

Having invalid addresses is a red flag.

Do the "From" and "To" fields are the same?

Having **the same** sender and recipient is a red flag.

Do the "From" and "Return-Path" fields are the same?

Having **differen**t values in these sections is a red flag.

Was the email sent from the correct server?

Email should have come from the official mail servers of the sender.

Does the "Message-ID" field exist, and is it valid?

Empty and malformed values are red flags.

Do the hyperlinks redirect to suspicious/abnormal sites?

Suspicious links and redirections are red flags.

Do the attachments consist of or contain malware?

Suspicious attachments are a red flag.

File hashes marked as suspicious/malicious by sandboxes are a red flag.

You'll also need an email header parser tool or configure a text editor to highlight and spot the email header's details easily. The difference between the raw and parsed views of the email header is shown below.

Note: The below example is demonstrated with the tool "Sublime Text". The tool is configured and ready for task usage in the given VM.   

![raw vs highlighed header](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/f3e74d182c55d8c943609bbeaa99fcfd.png)  

You can use Sublime Text to view email files without opening and executing any of the linked attachments/commands. You can view the email file in the text editor using two approaches.

  

-   Right-click on the sample and open it with Sublime Text.
-   Open Sublime Text and drag & drop the sample into the text editor.

![Open email file in a text editor](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/60c3da93bf82ac7d5cda9d31116edd84.png)  

  

If your file has a **".eml"** or **".msg"** extension, the sublime text will automatically detect the structure and highlight the header fields for ease of readability. Note that if you are using a **".txt"** or any other extension, you will need manually select the highlighting format by using the button located at the lower right corner.  
  

![Change highlight syntax](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/59bdec958210eed28518803f3b6fe604.png)  

  

  

Text editors are helpful in analysis, but there are some tools that can help you to view the email details in a clearer format. In this task, we will use the "emlAnalyzer" tool to view the body of the email and analyse the attachments. The emlAnalyzer is a tool designed to parse email headers for a better view and analysis process. The tool is ready to use in the given VM. The tool can show the headers, body, embedded URLs, plaintext and HTML data, and attachments. The sample usage query is explained below.

  

**Query Details**

**Explanation**

**emlAnalyzer**

Main command

**-i** 

File to analyse  
-i /path-to-file/filename  
**Note:** Remember, you can either give a full file path or navigate to the required folder using the "cd" command.

**--header**

Show header

**-u**

Show URLs

**--text**

Show cleartext data

**--extract-all**

Extract all attachments

Sample usage is shown below. Now use the given sample and execute the given command.  

  

emlAnalyzer Usage

```shell-session
user@ubuntu$ emlAnalyzer -i Urgent\:.eml --header --html -u --text --extract-all
 ==============
 ||  Header  ||
 ==============
X-Pm-Content-Encryption.....end-to-end
X-Pm-Origin.................internal
Subject.....................Urgent: Blue section is down. Switch to the load share plan!
From........................[REDACTED]
Date........................[REDACTED]
Mime-Version................[REDACTED]
Content-Type................[REDACTED]
To..........................[REDACTED]
X-Attached..................[REDACTED]
Message-Id..................[REDACTED]
X-Pm-Spamscore..............[REDACTED]
Received....................[REDACTED]
X-Original-To...............[REDACTED]
Return-Path.................[REDACTED]
Delivered-To................[REDACTED]
 =========================
 ||  URLs in HTML part  ||
 =========================
[+] No URLs found in the html
 =================
 ||  Plaintext  ||
 =================
[+] Email contains no plaintext
 ============
 ||  HTML  ||
 ============
Dear Elves,.......
 =============================
 ||  Attachment Extracting  ||
 =============================
[+] Attachment [1] "Division_of_........
```

At this point, you should have completed the following checks.  

  

-   Sender and recipient controls
-   Return path control
-   Email server control
-   Message-ID control
-   Spam value control 
-   Attachment control (Does the email contains any attachment?)

Additionally, you can use some Open Source Intelligence (OSINT) tools to check email reputation and enrich the findings. Visit the given site below and do a reputation check on the sender address and the address found in the return path.

  

-   **Tool:** `https://emailrep.io/`

![Email reputation check](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/91616d2eb4be20f6118c056b0fce8a12.png)  

  

Here, if you find any suspicious URLs and IP addresses, consider using some OSINT tools for further investigation. While we will focus on using Virustotal and InQuest, having similar and alternative services in the analyst toolbox is worthwhile and advantageous.

  

**Tool**

**Purpose**

**VirusTotal**  

A service that provides a cloud-based detection toolset and sandbox environment.  

**InQuest**  

A service provides network and file analysis by using threat analytics.  

**IPinfo.io**  

A service that provides detailed information about an IP address by focusing on geolocation data and service provider.

**Talos Reputation**  

An IP reputation check service is provided by Cisco Talos.  

**Urlscan.io**  

A service that analyses websites by simulating regular user behaviour.  

**Browserling**  

A browser sandbox is used to test suspicious/malicious links.  

**Wannabrowser**  

A browser sandbox is used to test suspicious/malicious links.  

After completing the mentioned initial checks, you can continue with body and attachment analysis. Now, let's focus on analysing the email body and attachments. The sample doesn't have URLs, only an attachment. You need to compute the value of the file to conduct file-based reputation checks and further your analysis. As shown below, you can use the sha256sum tool/utility to calculate the file's hash value. 

  
**Note:** Remember to navigate to the file's location before attempting to calculate the file's hash value.  

  

emlAnalyzer Usage

```shell-session
user@ubuntu$ sha256sum Division_of....
0827bb9a.... 
```

  

![VirusTotal](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/c2c773b4d53b467950632c80649553f7.png)Once you get the sum of the file, you can go for further analysis using the **VirusTotal**.  

  

-   Tool: `https://www.virustotal.com/gui/home/upload`

Now, visit the tool website and use the `SEARCH` option to conduct hash-based file reputation analysis. After receiving the results, you will have multiple sections to discover more about the hash and associated file. Sections are shown below.

![Virustotal sections](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/d71085a61113f7bf87b31dcd0e40570c.png)

-   Search the hash value
-   Click on the `BEHAVIOR` tab.
-   Analyse the details.

After that, continue on reputation check on **InQuest** to enrich the gathered data.

  

-   **Tool:** `https://labs.inquest.net/`

Now visit the tool website and use the `INDICATOR LOOKUP` option to conduct hash-based analysis.

  

-   Search the hash value
-   Click on the SHA256 hash value highlighted with yellow to view the detailed report.
-   Analyse the file details.

  

![InQuest](https://tryhackme-images.s3.amazonaws.com/user-uploads/6131132af49360005df01ae3/room-content/f7a0ebef6bbe5b127fe4787a5caf9811.png)  

  

After finishing the shown steps, you are finished initial email analysis. The next steps are creating a report of findings and informing the team members/manager in the appropriate format.  

  
Now is the time to put what we've learned into practice. Click on the Start Machine button at the top of the task to launch the Virtual Machine. The machine will start in a split-screen view. In case the VM is not visible, use the blue Show Split View button at the top-right of the page. Now, back to elf McSkidy analysing the suspicious email that might have helped the Bandit Yeti infiltrate Santa's network.  

  

**IMPORTANT NOTES:**

-   **Given email sample contains a malicious attachment.**
-   **Never directly interact with unknown email attachments outside of an isolated environment.**