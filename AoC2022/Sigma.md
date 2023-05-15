The Story

![Task banner for day 18](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/ad32970704bf0d7932de3b91a747afa6.png)

Check out Cybrites' video walkthrough for Day 18 [here](https://www.youtube.com/watch?v=4Zqd_FlkEu8)!

Compromise has been confirmed within the Best Festival Company Infrastructure, and tests have been conducted in the last couple of weeks. However, Santa’s SOC team wonders if there are methodologies that would help them perform threat detection faster by analysing the logs they collect. Elf McSkidy is aware of Sigma rules and has tasked you to learn more and experiment with threat detection rules.

## Threat Detection

Cyber threats and criminals have advanced tactics to ensure that they steal information and cause havoc. As you have already seen through the previous days, there are many ways in which this can be done. There are also ways for security teams to prepare their defences and identify these threats. What would be evident is that most of the blue-team activities will require proactive approaches to analysing different logs, malware and network traffic. This brings about the practice of threat detection.

Threat detection involves proactively pursuing and analysing abnormal activity within an ecosystem to identify malicious signs of compromise or intrusion within a network.

## Attack Scenario

Elf McBlue obtained logs and information concerning the attack on the Best Festival Company by the Bandit Yeti. Through the various analysis of the previous days, it was clear that the logs pointed to a likely attack chain that the adversary may have followed and can be mapped to the Unified Kill Chain. Among the known phases of the UKC that were observed include the following:

-   **Persistence**: The adversary established persistence by creating a local user account they could use during their attack.
-   **Discovery**: The adversary sought to gather information about their target by running commands to learn about the processes and software on Santa’s devices to search for potential vulnerabilities.
-   **Execution**: Scheduled jobs were created to provide an initial means of executing malicious code. This may also provide them with persistence or be part of elevating their privileges. 

The attack chain report included indicators of compromise (IOCs) and necessary detection parameters, as listed below. Additionally, the attack techniques have been linked to the MITRE ATT&CK framework for further reading.

Attack Technique

Indicators of Compromise

Required Detection Fields

[Account Creation](https://attack.mitre.org/techniques/T1136/)

-   EventID: 4720
-   Service: Security

-   Service
-   EventID

[Software Discovery](https://attack.mitre.org/techniques/T1518/)

-   Category: Process Creation
-   EventID: 1
-   Service: Sysmon
-   Image: C:\Windows\System32\reg.exe
-   CommandLine: `reg query “HKEY_LOCAL_MACHINE\Software\Microsoft\Internet Explorer” /v svcVersion`

-   Category
-   EventID
-   Image
-   CommandLine strings

[Scheduled Task](https://attack.mitre.org/techniques/T1053/005/)  

-   Category: Process Creation
-   EventID: 1
-   Service: Sysmon
-   Image: C:\Windows\System32\schtasks.exe
-   Parent Image: C:\Windows\System32\cmd.exe
-   CommandLine: `schtasks /create /tn "T1053_005_OnLogon" /sc onlogon /tr "cmd.exe /c calc.exe"`

-   Category
-   EventID
-   Image
-   CommandLine strings

Before we proceed to the next section, deploy the attached machine and give it up to 5 minutes to launch its services. Once launched, use the AttackBox or VPN to access the link [http://MACHINE_IP](http://machine_ip/) to our Sigma application, which constitutes the following features:

-   **Run** - Submit your Sigma rule and see if it detects the malicious IOC.
-   **Text Editor** - Write your Sigma rule in this section.
-   **Create Rule** - Create a Sigma rule for the malicious IOC.
-   **View Log** - View the log details associated with the malicious IOC.

## Chopping Logs with Sigma Rules

Sigma is an open-source generic signature language developed by Florian Roth & Thomas Patzke to describe log events in a structured format. The format involves using a markup language called [YAML](http://yaml.org/), a designed syntax that allows for quick sharing of detection methods by security analysts. The common factors to note about YAML files include the following:

-   YAML is case-sensitive.
-   Files should have the `.yml` extension.
-   Spaces are used for indentation and not tabs.
-   Comments are attributed using the `#` character.
-   Key-value pairs are denoted using the colon `:` character.
-   Array elements are denoted using the dash `-` character.

Sigma makes it easy to perform content matching based on collected logs to raise threat alerts for analysts to investigate. Log files are usually collected and stored in a database or a Security Information and Event Management (SIEM) solution for further analysis. Sigma is vendor-agnostic; therefore, the rules can be converted to a format that fits the target SIEM.

Sigma was developed to satisfy the following scenarios:

-   To make detection methods and signatures shareable alongside IOCs and Yara rules.
-   To write SIEM searches that avoid vendor lock-in.
-   To share signatures with threat intelligence communities.
-   To write custom detection rules for malicious behaviour based on specific conditions.

## Sigma Rule Syntax

![Sigma Rule Structure](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/ab03bb7d76129d0b7b20bd5b5120a553.png)Sigma rules are guided by a given order of required/optional fields and values that create the structure for mapping needed queries. The attached image provides a skeletal view of a Sigma rule.

Let’s use the first attack step challenge to define the syntax requirements, fill in the details into our skeletal rule, and detect the creation of local accounts. Use the text editor section of the SigHunt application to follow along.

-   **Title:** Names the rule based on what it is supposed to detect.
    
-   **ID:** A globally unique identifier that the developers of Sigma mainly use to maintain the order of identification for the rules submitted to the public repository, found in UUID format.
    
-   **Status:** Describes the stage in which the rule maturity is at while in use. There are five declared statuses that you can use:
    
    -   _Stable_: The rule may be used in production environments and dashboards.
    -   _Test_: Trials are being done to the rule and could require fine-tuning.
    -   _Experimental_: The rule is very generic and is being tested. It could lead to false results, be noisy, and identify exciting events.
    -   _Deprecated_: The rule has been replaced and would no longer yield accurate results. 
    -   _Unsupported_: The rule is not usable in its current state (unique correlation log, homemade fields).

-   **Description:** Provides more context about the rule and its intended purpose. Here, you can be as detailed as possible to provide information about the detected activity.
    

local_account_creation.yml

```shell-session
title: Suspicious Local Account Creation
id: 0f06a3a5-6a09-413f-8743-e6cf35561297 
status: experimental
description: Detects the creation of a local user account on a computer.
```

-   **Logsource:** Describes the log data to be used for the detection. It consists of other optional attributes:
    
    -   _Product_: Selects all log outputs of a certain product. Examples are Windows, Apache
    -   _Category_: Selects the log files written by the selected product. Examples are firewalls, web, and antivirus.
    -   _Service_: Selects only a subset of the logs. Examples are _sshd_ on Linux or _Security_ on Windows.
    -   _Definition_: Describes the log source and its applied configurations.

local_account_creation.yml

```shell-session
logsource:
  product: windows
  service: security
```

-   **Detection:**  A required field in the detection rule describes the parameters of the malicious activity we need an alert for. The parameters are divided into two main parts:
    
    -   _The search identifiers_ are the fields and values the detection should search for.
    -   _The condition expression_ - sets the action to be taken on the detection, such as selection or filtering. The critical thing to look out for account creation on Windows is the Event ID associated with user accounts. In this case, Event ID: 4720 was provided for us on the IOC list, which will be our search identifier.

local_account_creation.yml

```shell-session
detection:
  selection:
    EventID:  # This shows the search identifier value
      - 4720    # This shows the search's list value
  condition: selection
```

  
The search identifiers can be enhanced using different modifiers appended to the field name with the pipe character `|`. The main type of modifiers are known as **Transformation modifiers** and comprise the values: _contains_, _endswith_, _startswith_, and _all_. Some of these modifiers will be vital in writing rules against the other IOCs.

random_rule.yml

```shell-session
detection:
  selection:
    Image|endswith:
      - '\svchost.exe'
    CommandLine|contains|all: 
      - bash.exe
      - '-c '   
  condition: selection
```

-   **FalsePositives:** A list of known false positives that may occur based on log data.
    
-   **Level:** Describes the severity with which the security team should take the activity under the written rule. The attribute comprises five levels: Informational -> Low -> Medium -> High -> Critical
    
-   **Tags:** Adds information that can be used to categorise the rule. Common tags are associated with tactics and techniques from the MITRE ATT&CK framework. Sigma developers have defined a list of [predefined tags](https://github.com/SigmaHQ/sigma/wiki/Tags).
    

local_account_creation.yml

```shell-session
falsepositives: 
    - unknown
level: low
tags:
   - attack.persistence # Points to the MITRE Tactic
   - attack.T1136.001 # Points to the MITRE Technique
```

  

Voila!! We have written our first Sigma rule and can run it on the AoC Sigma Hunting application to see if we can get a match. As mentioned before, Sigma rules are converted to fit the desired SIEM query, and in our case, it should be known that they are being transformed into Elastic Queries on the backend. Various resources perform this task, with the native tool being [Sigmac](https://github.com/SigmaHQ/sigma/blob/master/tools/README.md), which is being deprecated and replaced with a more stable python library, [pySigma](https://github.com/SigmaHQ/pySigma). Another notable tool to check out is [Uncoder.io](https://uncoder.io/). Uncoder.IO is an open-source web Sigma converter for numerous SIEM and EDR platforms. It is easy to use as it allows you to copy your Sigma rule on the platform and select your preferred backend application for translation.