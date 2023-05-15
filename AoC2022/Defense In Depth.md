The Story

![Banner for Day 23](https://tryhackme-images.s3.amazonaws.com/user-uploads/60c1834f577d63004fdaec50/room-content/4efae3f1894e77bb72afcd58791b2961.png)

Check out Marc's video walkthrough for Day 23 [here](https://youtu.be/eib8zGMTook)!  

Every effort you have put through builds on top of each other to bring you right at this moment. Santa and the security team are so proud of you for sticking around and being with us until now. You’re practically a member of the SSOC team already! There’s just one more thing left to learn: a lesson that may completely change how you look at and approach security.

Throughout all the previous tasks, layering defenses and reducing attack surfaces have been touched upon at least once. Writing secure code and being able to respond and analyse different parts of the attack chain, among others, are all essential in maintaining Santa's Security Posture defensible. In this task, we will focus on what is formally known in security circles as Defense in Depth, which is a more general and encompassing topic than the prior ones on their own.

Core Mindset

The core mindset that Defense in Depth is founded on is the idea that there is no such thing as a silver bullet that would defeat all of an organisation’s security woes. No single defence mechanism can protect you from the bad world out there.

Contrasting the Past and the Modern Takes on Defensive Security

Castle walls are built to withstand sieges and barrages and are fortified and manned well to protect from pillagers and marauders. Yet despite all this effort and diligence at maintaining and protecting this security measure, attackers will breach it sooner or later, and depending on the defenders' response within the castle walls, may mark the start of their end.

![Security Elf](https://tryhackme-images.s3.amazonaws.com/user-uploads/60c1834f577d63004fdaec50/room-content/fb56361ab25a143e789048eb19f62860.png)For the longest time, and maybe even until today, a lot of organisations have looked at their security posture in the same way medieval lords did: a strong focus on securing the castle walls - the perimeter, so to speak. However, like medieval lords, after the perimeter is breached and depending on the organisation’s response, it’s pretty much done for them too.

Fret not, though! Modern defensive security teams are moving on from this mindset and are shifting to a more robust approach. Being mindful that the castle wall, while important, is not the only way to secure the organisation, acknowledging the reality that at some point, gunpowder will be discovered and a single point of failure consequently exploited, and having additional defensive layers, especially for the specific _crown jewels_ that the bad guys may be targeting - these are some of the foundations that make up the modern security posture of defensible organisations.

Disrupting Adversarial Objectives

Defense in Depth is mainly focused on disrupting adversarial objectives; that is, the shift of focus from ‘just’ securing the perimeter to securing everything in the path that the adversary will have to take from the perimeter to the crown jewels.

Let’s look at it at three varying levels of defense:

1.  The first level is having a focus on perimeter security. There are great prevention mechanisms present in the perimeter and essentially complete trust within it; thus, once the perimeter is bypassed, the organisation is pretty much at the mercy of the adversary.
2.  The second level has defensive layers in place; however, the emphasis is solely on prevention. It doesn’t leverage ‘knowing your environment’; even though adversarial objectives may be prevented to some degree, there’s a missed opportunity in terms of detection and consequently, alerting and response. Prevention is good, but the key to defeating the bad guys is having visibility into what they are doing.
3.  The third level has well-rounded defensive layers in place, leveraging the strategic application of sensors, effective creation of analytics, and efficient alerting and response capabilities of the security team. Preventative measures here are not only coupled by detection and alerting but also by immediate and efficient response.

The first level above can be thought of as an organisation that employs great perimeter defenses in place, such as Web Application Firewalls (WAFs), Perimeter Network Firewalls, and even a Demilitarized Zone (DMZ), but is yet to implement internal network security, and zero trust mechanisms are not yet in place.

The second level can be thought of as an organisation that employs the first level of defenses but with more capable internal security measures, such as network segmentation, zero trust principle implementation, least privileged access principle implementation, and even hardened hosts and networks. Having this level is actually really good; however, forgetting that preventative appliances may be used for detective capabilities, too, is a wasted opportunity.

The third level can be thought of as using the advantages of the first and second levels to ramp up the detection and response capability of the organisation via effective log collection and well-crafted analytics. This is where it goes full circle. We are not only expected to be good at layering preventive measures against attacks, but we should also be capable of responding to them if and when these defensive capabilities are bypassed.

![Bandit Yeti: Abominable for a Day](https://tryhackme-images.s3.amazonaws.com/user-uploads/60c1834f577d63004fdaec50/room-content/9c4b9cf210bca525421b44a0ac19cd7a.png)

Let’s have the following scenario as an example: let’s say an adversary was able to penetrate our perimeter defenses via a successful spear phishing campaign. He would need to navigate a hardened environment full of tripwires and traps.

He may be able to take over a specific user’s account, but since we have implemented the principle of least privilege access properly, he would be limited in terms of what he can work with. He may be able to move laterally to another user with better privileges via pass-the-hash. Still, since we have good logging mechanisms and detection capabilities, our analytics would know exactly what pass-the-hash looks like, so we will pick this activity up. Our cavalry will be alerted to respond and remediate this particular breach immediately.

Remember that the main difference between levels 2 and 3 is the jiving together of these defensive layers and detection and response mechanisms, allowing for a coherent and well-rounded security posture.

A goal of layering defenses is to limit the room for mistakes that an adversary can have. In that sense, the bad guys need to get everything correct, but we only need them to make a mistake once.

To drive this point home, we will explore what it’s like to be in the bad guy’s shoes.

We have prepared a little game for you. The game's objective is simple: get your hands on the Naughty or Nice list in Santa’s vault. There will be three levels; each consequent one builds a defensive layer on top of the previous one and will be a little bit harder. Remember that you’re playing as the Bad Yeti here, so make sure you don’t get caught!

Click the View Site button at the top of the task to launch the static site.  

Game summary / post-game discussion

After completing the game and experiencing the different levels of difficulty that an adversary may face when layering defensive measures, it must be clear to you by now that layering defenses is cool!

The levels of the game are an analogy to the three varying levels of defenses discussed earlier in this task. Further, the attack chain within the game can be interpreted as follows: Santa’s Executive Assistant (EA) receives a spear phishing email. Upon establishing a foothold, the Bad Yeti immediately realises that the EA doesn’t have access to the Naughty or Nice list. So after some enumeration and even assuming Santa’s identity at one point, the Bad Yeti was able to get ahold of the coveted list and actually ruin Christmas. Good thing it was just a game!

While the reason behind it is to show the varying effects of having defensive layers from the bad guy's perspective, on the flip side, the reality of implementing it for defensive security teams is more iterative and cyclic. Breaches and true-positive detections show areas that require improvement and the extent of our visibility in the organisation, respectively. Attacks, whether or not successful, should further inform defensive steps and approaches. If we learned that an adversary was able to exploit a specific vulnerability, then rest assured that the vulnerability will be patched and the application introducing it will be hardened.

Summary and Conclusion

Modern security practices are composed of layers. Stopping attacks from the get-go is very helpful - ideal even, but it’s not always possible. Everyone can and will be compromised - it’s just a matter of when, and it’s up to every one of us to disrupt them from being able to do what they intend to do and in every step of the way.