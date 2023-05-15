The Story

![Banner image showing a Christmas tree branch with the number 22 and different types of Christmas Candy](https://tryhackme-images.s3.amazonaws.com/user-uploads/61306d87a330ed00419e22e7/room-content/de458d6095e39ca645b5eac109ca1a5b.png)  

Check out Simply Cyber's video walkthrough for Day 22 [here](https://www.youtube.com/watch?v=1i4-Qq6tM2Q)!

McSkidy wants to improve the security posture of Santa's network by learning from the recent attempts to disrupt Christmas. As a first step, she plans to implement low-effort, high-value changes that improve the security posture significantly.

## Learning Objectives

To help McSkidy with her improvements, we will learn some concepts and evaluate some steps to take. These will include:

-   Understand what an attack vector is.
-   Understand the concept of the attack surface.
-   Some practical examples of attack surface reduction techniques that McSkidy can utilize to strengthen Santa's network.

So let's start.

## Attack Vectors![An image showing the Bandit Yeti wielding a candy as a weapon. The weapon is marked as attack vector](https://tryhackme-images.s3.amazonaws.com/user-uploads/61306d87a330ed00419e22e7/room-content/6bcf4931e66fb13c1bdb493e0e4fe354.png)

An attack vector is a tool, technique, or method used to attack a computer system or network. If we map the attack vectors to the physical world, attack vectors would be the weapons an adversary uses, like, swords, arrows, hammers, etc. A non-exhaustive list of examples of attack vectors in cybersecurity includes the following: 

-   Phishing emails; Deceptive emails that are often impersonating someone and asking the victim to perform an action that compromises their security.
-   Denial of Service (DoS) or Distributed Denial of Service (DDoS) attacks; Sending so many requests to a website or web application that it reaches its limits and can no longer serve legitimate requests.
-   Web drive-by attacks; Flaws in web browsers that compromise the security of the victim by merely visiting a website.
-   Unpatched Vulnerability exploitation; A flaw in the internet-facing infrastructure, such as the web server or the network interface, that is exploited to take control of the infrastructure.

## Attack Surface![An image showing a Blue Team Elf in armor and with a shield. The unshielded part of the Elf's body is marked as Attack surface](https://tryhackme-images.s3.amazonaws.com/user-uploads/61306d87a330ed00419e22e7/room-content/5dd98afd83d1a5dae96af3bc9701cdd4.png)

The attack surface is the surface area of the victim of an attack that can be impacted by an attack vector and cause damage. Taking forward our example of the physical world, the attack surface will include the unarmoured body of a soldier, which an attack of a sword, an arrow, or a hammer, etc., can damage. In cybersecurity, the attack surface will generally contain the following: 

-   An email server that is used for sending and receiving emails.
-   An internet-facing web server that serves a website to visitors.
-   End-user machines that people use to connect to the network.
-   Humans can be manipulated and tricked into giving control of the network to an attacker through social engineering.

## Attack Surface Reduction

As we might notice, the attack surface can not be eliminated short of running away from the battlefield. It can only be reduced. The Greek Phalanx is an excellent example of attack surface reduction, as seen in the picture. In the picture, the front of the defending army is covered by their shields, whereas the walls of a pass cover the sides, leaving no room for an attacker to inflict damage on the defenders without running into their defenses. This is how the Spartan army could hold back a much larger Persian army for several days in the battle of Thermopylae.

![An image showing Blue team Elfs in a close Greek Phalanx formation, surrounded by mountains, showing a reduced attack surface due to shields in the front and mountains on the side](https://tryhackme-images.s3.amazonaws.com/user-uploads/61306d87a330ed00419e22e7/room-content/438752f129cb45eb534bcccd6809806b.png)  

However, this attack surface reduction works for the weapons of that time. This technique will not impact the attack surface against modern weapons. 

In cybersecurity, the most secure computer is the one that is shut down and its cables removed. However, that is not feasible for running critical operations dependent on computers. Therefore, cybersecurity leaders aim to keep the operations running with the lowest possible attack surface. We can consider the goal as creating the digital equivalent of the Greek Phalanx.

## Examples of Attack Surface Reduction

Now that we have understood the concept behind attack surface reduction let's identify the attack vectors that could be used against Santa's infrastructure. Let's help McSkidy devise a Greek Phalanx defense to minimize the attack surface. 

Close the ranks:  
Santa's website was defaced earlier. When investigating that attack, McSkidy found that an SSH port was open on the server hosting the website. This led to the attacker using that open port to gain entry. McSkidy closed this port.  

Put up the shields:  
Although the open SSH port was protected by a password, the password was not strong enough to resist a brute-forcing attempt. McSkidy implemented a stronger password policy to make brute-forcing difficult. Moreover, a timeout would lock a user out after five incorrect password attempts, making brute-force attacks more expensive and less feasible.

Control the flow of information:  
McSkidy was informed by her team about the GitHub repository that contained sensitive information, including some credentials. This information could be an attack vector to target Santa's infrastructure. This information was made private to block this attack vector. Moreover, best practices were established to ensure credentials and other sensitive information are not committed to GitHub repositories.

Beware of deception:  
Another attack vector used to intrude into Santa's network was phishing emails. McSkidy identified that no phishing protection was enabled, which led to all such emails landing in the inbox of Santa's employees. McSkidy enabled phishing protection on Santa's email server to filter out spoofed and phishing emails. All emails identified as phishing or spoofed were dropped and didn't reach the inbox of Santa's employees.

Prepare for countering human error:  
The phishing email that targeted Santa's employees contained a document containing malicious macros. To mitigate the risk of malicious macro-based documents compromising Santa's infrastructure, McSkidy disabled macros on end-user machines used by Santa's employees to avoid malicious macro-based attacks.

Strengthen every soldier:  
McSkidy wanted the attack surface reduced from every endpoint's point of view. So far, she had taken steps to strengthen the network as a whole. For strengthening each endpoint, she took help from [Microsoft's Attack Surface Reduction rules](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/attack-surface-reduction-rules-reference?view=o365-worldwide). Though these rules were built into the Microsoft Defender for Endpoint product, she took help from these rules and created a similar set of rules for her own EDR platform. 

Make the defense invulnerable:  
To further strengthen the infrastructure, McSkidy carried out a vulnerability scan highlighting some vulnerabilities in the internet-facing infrastructure. McSkidy patched these vulnerabilities found on Santa's internet-facing infrastructure to avoid exploitation.

After implementing these steps, McSkidy was a little relieved. However, she understood that though her attack surface was reduced, she still needed to be vigilant to avoid any incidents. Come back tomorrow to see what other steps McSkidy takes to strengthen her defenses. See ya!