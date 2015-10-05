---
layout: post
title: "Network Security Recap"
modified:
category: blog
excerpt: "It is a damn good shit. A good material for catching up with network security stuffs."
tags: [sweden, education, chalmers]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
---


EDA491 | Network Security 
===================

---

### **April 2015**

**IP fragmentation** is problematic in many situations. Consider a firewall or IDS system inspecting the traffic and mention **three problems** related to fragmentation that may affect security. Describe the problems and what problems they may cause.

> IP fragmentation is a process to split up IP datagram into several packets of smaller size. Usually first fragment starts with ICMP message and the following fragmented packets are ICMP without header (like a beheaded datagrams).
 
> An IDS or firewall will have to check several informations found on the header, e.g. ID, Flag and Offsets and the size of the fragments. Small fragments could lead into an next fragment header overwrite, big fragments could lead into overlapping datagrams.

> **Teardrop attack** happens when there is a mismatch between offset length and the actual datagrams size. This caused an overlapping between datagrams and the overlapped part of the datagrams often used as a place to insert malicious information. When the reassembly is not properly concatenate all of the fragments, the attack will go undetected.
> 
> **Overrun DoS** happens when the complete reassembled datagrams size is more than allowed IP packet 65,535 bytes. Processing data this large could lead the system into network crash state and resulted on Denial-of-Service.
> 
> **Rose Attack** happens when there are too many incomplete fragments detected on the network. This kind of incomplete fragment usually is used to bypass IDR or to launch DoS attack. 

What is required by an attacker to do a **blind insertion** of a packet into an ongoing TCP session between two other users? What is the **countermeasure** we have in TCP against it? Is the solution good enough? How likely is the attacker to succeed?

> When we are dealing with TCP connection, there is an established connection between two sides. Both of them have to maintain the sequences of the communication using sequence number for their packets. 

> An attacker that wants to perform blind insertion, should have a knowledge about target IP address and try to catch the sequence number prediction as soon as the initial 3-way handshake is finished. However, when both sides are using random number to generate the sequence number, then the attacker is completely blind and it is very hard in practice. Sequence number has 32-bits of length in the TCP header, thus 2^32 chances are too much for the attacker.

Describe in some detail the process of how a **certificate** is used, i.e. how the owner’s identity can be verified by another party! What does the receiver of the certificate do? How is the certificate distributed to the receiver?

> When user receives developer's certificate, it contains the CA's private key that's signed by the developer. CA's public key that comes with the preloaded certificates within browser is used to check the integrity of the developer's certificate. A hash of this preloaded certificate is created and encrypted then compared to the one comes with the developer's certificate.

> To check the owner's identity, basic digital signature is used. User can challenge the owner to encrypt the data using its private key and later on user can use owner's public key to decrypt and make sure that the owner is the one it supposed to.

**Identity management** (IDM) is a relatively new field. What is the fundamental problem IDM tries to solve?

> Identity management put the security measure closer to the center and control oriented. It brings the consciousness of security aware in full control from the organization.

If a dishonest supplicant can both send and listen to the traffic between the Authenticator and the **Radius** server, it is possible to do a number of attacks. Give one example of an **attack against the shared secret** and one against someone else’s password!

> If the dishonest supplicant observed valid Access-Request (ARq) and associated Access-Accept (AA) or Access-Reject (ARj), then it is possible for the supplicant, now a potential attacker, to perform exhaustive attack on **shared_secret** within response_authenticator part of the response. The attacker will compute MD5 state for **code** + **ID** + **length** + **request_auth** + **attribute** which are known. The attacker will then resume the hash once for each shared secret **guess**.

> Radius uses stream cipher and good attacker can find information about the rate of authentication limit imposed by the client. First, attacker attempts to authenticate to the client using fake username and password. The authenticator will try to send AR to the **Radius** server. Attacker captures AR and replay the AR to the **Radius** using different variations of password. If the server does not impose rate authentication limit, then this attack can be performed in an exhaustive way to find correct password.

What is a **DMZ**, where is it located and what **purpose** does it have?

> DMZ or demilitarized zone, or perimeter network, is an external network outside the local network that runs several services, e.g. web servers, mail servers etc to avoid forwarding traffic to the internal (local) network.

You are now given the task to connect a small office network to the Internet and a firewall is needed. Assume that you are considering two different rather cheap firewall configurations:

* a stateless firewall, **OR**
* NAT gateway that takes care of security

compare the two solutions and discuss advantages and disadvantages with them. Discuss what types of attacks they may be vulnerable against, if any.

> **Stateless** firewall watch network traffic, working and decisions are taken by rules on source or destination addresses. This firewall does not aware about the state of the connection, e.g. TCP sequence number, traffic patterns or data flow. Lot of packets can be rogue and let this alone enough to lead the system prone to **spoofing attack**.

> **NAT** gateway works by looking at the translation table. Thus, it is as good as the table is. It works like a firewall with default setting set to 'not letting' any connections that are not originated from the network. However, NAT gateway still can be attacked, not by spoofing attack, but with **SYN flood** or ping-of-death that does not harm the main security measure of the system, but will lead to the denial-of-service (**DoS**) problem.

In the picture above, **two firewalls** not specially designed for this purpose, are placed in parallel by someone believing they could increase both reliability if one fails and improve performance by sharing the load. Does this work? List some problems with this approach!

> From **redundancy** point of view, in order to keep both of the firewalls at the same state everytime, the switch has to broadcast every packets. It is doable and easy, however the problem arise when we have two same state firewalls that forward exactly same packets. This will create duplicate of packets that is not good for the network. So, one of the firewall must halt the forwarding when the other firewall, let say the **primary firewall** works 100% well.

> From **performance** point of view, load sharing ideally performed by stripping packet per packet and let the packets forwarded to each of the firewalls in order. However, this doesn't really work since TCP is connection based and for each session, the connection has to be maintained between both sides.

SSL consists of several protocols. Describe the functionality of the record layer, change cipher, alert and handshake protocols! 

> **Record layer** performs fragmentation > compression > MAC > encryption. 
> **Change cipher** tells the other side about last security parameter negotiated and starts the encryption phase
> **Alert** protocol sends warning and error messages to the other side
> **Handshake** protocol negotiates cipher key, exchanging public key and certificate, and performs authentication.

The first **SSL** messages exchanged between the client and the server are the **HELLO** messages: 

* { verC || r1 || sid || ciphers || comps } -> server 
* { verS || r2 || sid || cipherA || compA } <- server

Explain clearly what each field **contains** and the purpose of it (what it is used for)!

> **verC** is the highest protocol that's supported by the browser (client side). 
> **r1** is the unique nonce from client side (**r2** is from server side) that is used for limited time (one occasion). It is a random number that will be used for key generation later. 
> **sid** or session ID is the number for identifying the session, new connection start from 0 and non-zero **sid** means that clients want to resume existing connection.
> **ciphers** is a list supported cipher suite by the client side, while **cipherA** is the cipher suite that is selected by the server.
> **comps** is a list of compression that can be used for compressing the payload. **compA** is the compression method that is selected by the server.

In **IPsec tunnel** mode, a new IP header is created. Why? Why is this not necessary in transport mode? Explain!

> Because in **tunnel** mode, header is needed due to the presence of gateway. Tunnel mode isn't like **transport** mode which the datas are completely decrypted and unpacked by end point, instead the gateway is the one who does that then forward the necessary packets (decrypted) to the end user. Thus, in **tunnel** mode, one of the advantage is the user doesn't have to implement **IPSec**.

Mention **five typical functions** that characterize a **security protocol,** i.e. things you would expect to find in a protocol that is supposed to be secure. For each such function, explain its purpose (why it is there) and give an example of how it can be solved/implemented.

> In practice, implementation of security protocol is quite close with each other and several functions are being used many times due to the performance, reliability and the practicality. From **algorithm negotiation**, **key exchange**, **data integrity**, **two-step verification** and **security protection** or measure.

> **Algorithm negotiation** usually is performed at early phase, e.g. SSL and SSH, to determine what kind of cipher suite or encryption algorithm to be used later on.

> **Key exchange** is necessary for both sides to get to know the master key to be used for the encryption. Usually it implements Diffie-Helmann or RSA key exchange algorithm.

> **Data integrity** means that the encrypted data should not be changed while transmitting. To maintain the data integrity, one way encryption, e.g. Hash MD5 or HMAC can be used.

> **Two-step verification** is needed when typical username & password combinations is not enough. Usually it is implemented by asking the user something using questions, e.g. watermark, captcha, etc. The answer for this kind of questions is precomputed and saved alongside the sessions in the server.

> **Security protection** has to be carried out to maintain the confidentiality, integrity and authenticity of the communications. We speak this because there is always a non-zero chance that a dishonest user or a man-in-the-middle that wants to exploit some of the weakness in our security protocols. Several specific protections are **replay-attack** protection by utilizing sessions token,  protection for **MiTM** using mutual-authentication and PKI

How does **WEP** prevent packets from being modified? Is this mechanism sufficient, or does it result in any (known) security problems? Give some details!

> No, WEP doesn't guarantee a modification of the packets. WEP uses **CRC** feature to enable data integrity check later on. Simple CRC implementation basically takes several bits of the datas and append them on the last part of the datas. It is possible to find out what **bit is changed on the checksum** when input is changed. Thus, it is possible for an attacker to resolve this and do some modifications on the datas.

WEP has at least one problem with its handling of IVs. Explain how an attacker may use this weakness! Is this problem likely to occur?

> WEP allows IV to be reused. That means two packets can be encoded/encrypted using stream cipher with same IV & shared keys. That means, **XOR** operations of those two ciphertexts will also affect **XOR** operations on the cleartexts.

> c1 **xor** c2=(p1 **xor** b) **xor** (p2 **xor** b) = p1 **xor** p2

> If one of the cleartext is longer than the other, then the longer part could be revealed. And since the IV is merely 24-bit, it is likely reused by busy AP (Access Points).

Some more advanced switches may have more **intelligent** functionality than a simple €10 switch, and can prevent some **link-layer** attacks. Describe two such possible functions and what kind of attacks they protect against!

> If we are on link-layer attack, mostly the protection is all about MAC addresses. We can **limit the number MAC addresses** on specific ports. By limiting this, it will prevent broadcast message flooding to all ports by specific MAC address. Other solution is to **implement trust** on specific ports. With this we are limiting what ports to answer specific requests. Going further and more detail, we can limit **one MAC one port** thus limiting the functionality as well. Also, MAC-spoofing protection is feasible to do by performing monitoring both for IP and MAC.

**DHCP spoofing** is another problem advanced switches can deal with. What is DHCP spoofing? And how can it be dealt with by a switch?

> An attacker may intercept the DHCP request message and give an answer. That means the attacker becomes **man-in-the-middle**. The trusted ports function implemented by the switch only allow associated ports to answer this DHCP request.

---
### **August 2014**

Give one example each of a **Link Layer**, **Network Layer** and **Transport Layer** attack! Clearly explain how each attack works and what the possible results of it may be

> In link layer, **ARP spoofing** is one of the attack example. ARP is like a question message about who has certain IP address. When there is a reply (ARP reply), ARP updates the ARP table with the IP address and its associated MAC address. Attacker could spoof the MAC address thus mismatch the IP-MAC relation in the table. By this, letting the traffic to be routed to attackers MAC. The **countermeasure** for this is *antidote* patch like in Linux. The host check the old MAC first before accept new MAC.

> In network layer, every abuse about IP header can be considered potentially harmful. For example, **IP spoofing** attack, is an attack on which the attacker send malicious packets using victim's IP address as source IP address. With this, the attacker can hide his identity and potentially exploit the trusted connection between two sides.

> In transport layer, anyone can insert malicious data in UDP packet. This leads an **UDP attack**. This attack is done by inserting malicious information within UDP datagram into the host. It is hard to detect because there is sequence number in UDP packets. Another good example is **SYN flooding** attack. Attacker sends SYN flooding from many zombie clients into one host. This makes the host to send SYN/ACK to any zombies in such a short time. Probably will lead into chaotic denial-of-service problem.

The goal of some **DoS attacks** is to try to exhaust the resources of the target, for example memory, internal tables, network or the CPU. Describe two possible DoS attacks targeting different resources, how they work and what weaknesses they make use of!

> DoS attack usually targets several different resources. **Reflection attack** is an example of DoS attack that target the bandwidth of the victim to be exhausted. This attack is initiated by an attacker doing an authentication with the victim A. After A sent a challenge request to the attacker, it opens new connection to the B and forward that **challenge request** from A to B. The response from B to the attacker, then forwarded back to A. Without careful inspection, if we have lot of B, then there will be lot of open connections between A and Bs.

> Another DoS attack that targets internal resource is **SYN flooding** attack. Attacker sends SYN flooding from many zombie clients into one host. This makes the host to send SYN/ACK to any zombies in such a short time and wasting lot of victim's resources. Probably will lead into chaotic denial-of-service problem.

Explain how **encryption** of data traffic is done in **WEP**! A figure together with explaining text is needed. You need to discuss how encryption is done, all input and output

> Well, WEP uses **stream cipher** that takes IV + key (seed) as an input to the RC4 symmetric cipher algorithm to produce keystream. The keystream bit length is exactly the same with the stream plaintext of datas. Then **XOR operation** is performed between the keystream and the plaintext. The ciphertext is produced and then used.

Even though WEP encrypted traffic contains an **encrypted checksum** or CRC, it cannot prevent an attacker from modifying the packets. Explain why this is possible and explain how WEP should have been designed to avoid this problem!

> WEP uses **CRC** feature to enable data integrity check later on. Simple CRC implementation basically takes several first bits (24-bits if we use CRC24) of the datas and append them on the last part of the datas. It is possible to find out what **bit is changed on the checksum** when input is changed. Thus, it is possible for an attacker to resolve this in exhaustive fashion and do some modifications on the datas.

Mention briefly two things **TKIP** (temporal key integrity protocol, present **in WPA**) does to enhance security?

>TKIP ensures the encryption key not static or changeing over time. It extends the regular IV into EIV by incorporating MAC address as a input for pseudo random generator. Another purpose is to make sure that the sequence number of the packets is unique thorough all the communication sessions.

Give six examples of different types of rules to filter traffic that a **screening router** likely would have! You may use text or your own “pseudo-language” for the filter rules as long as they are possible to understand (comments to the rules are necessary).

> Screening router is a stateless firewall. This means that the screening router doesn't have capabilities to check the state of connections. Several common filter that we should have are:

> **reserved IP address**
> source_addr : 0.x.x.x > DROP | **never used 0.x.x.x net**
> source_addr : 127.x.x.x > DROP **localhost  should be used internally**

> **broadcast and multicast address**
> source_addr : x.x.255.255 > DROP | **multicast source should be dropped**
> source_addr : x.x.x.x, y.y.y.y > DROP | **multicast source should be dropped**
> dest_addr : x.x.x.x, y.y.y.y > DROP |  **tcp doesn't utilize multicast**

> **malformed packet**
> flag SYN=1 **and** FIN=1 > DROP | **never ever SYN & FIN in same packet**
> if **tiny_fragment**=1, DROP | **tiny fragment is dangerous**

> **block ICMP reply**
> protocol : ICMP, flag: reply > DROP | **prevent smurf attack**

In the course we have discussed several ways to **evade IDS** systems and **firewalls**. One problem for the IDS system is to select proper timeouts for IP fragment reassembly. Why is this problematic? Give two examples of what would happen if the IDS system selects a too long or too short timeout!

> When fragmented packets are received, there has to be a timeout before unfinished fragment will be dropped. If the timeout duration in IDS is shorter, there is a chance that the fragments will be dropped while the target system may find the fragment is complete datagram. If the timeout duration in IDS is longer, then IDS will find that the fragments discarded in target system.

**TTL** can sometimes also be used to fool firewalls and intrusion detection (IDS) systems. How? Mention a possible protection mechanism against this attack.

> Low TTL value can cause fragments or packets to be discarded due to early timeout. This in turn may leads to a situation where IDS and target host receiving different meaning of what had been communicated through the communications. Border routers could normalize TTL value to, let say 20.

If you are given the task to test whether a **firewall** is **stateful** or **not**, how would you do this? Describe what you do (send) and the expected result! Explain clearly why this test would work!

> To check for the statefulness of a firewall, we may send ACK message and check for the result. If the firewall is stateless, it doesn't read the state of the connection and then will forward the ACK to the destination. Normal systems **will respond the ACK with RST** if the ACK doesn't belong to a known session. If the firewall is stateful, then the ACK message will be dropped.

**NAT gateways** are strictly speaking not firewalls, but they are still useful and can in many situations replace a conventional firewall. Why?

> **NAT** helps to **hide** the internal systems from the outer network. Any information that is not inside NAT's translation table is not visible. Also, NAT doesn't inspect any traversed packets, thus it grants better performance metrics. For the application, NAT gateways are suitable for small network or home users.

On the last page, there is a picture of an **IPsec** header. Explain what the next header, SPI and padding fields are used for and what purposes they have!

> **Next header** tells what upper layer protocol should receive the data.
> **SPI** is an index that tells what kind of SA (Security Association) to be used.
> **Padding** field is to disguise to an attacker about the actual amount of the datas being transmitted.

**IKE** is a protocol often used together with IPsec. What does it do (mention its three main functions)?

> IKE or Internet Key Exchange is used for establishing crypto session keys, negotiate ciphers suite and performing two-sides verification.

The figure on the last page shows the **SSL handshake** protocol and how a client and a server exchange information. Explain briefly the messages, what they contain and their purpose/function!

> In the **Hello** part, basically client and server are just exchanging information about **random numbers**, **ciphers suite** and **session ID**. Then server sends certificate that is signed and verified by the Certificated Authority (CA), alongside with server's Public Key. Then browser verifies the certificate with the one loaded into the browser. If it is verified that the certificate comes from the trusted CA. If there is no public key shared between client and the server, then extra step **server key exchange** is needed. Later on, client responded by sending **client key exchange** that consists shared key secret encrypted using server's public key. Then the server is authenticated, everything is done and client ends the phase sending **client finished**. Last phase is all about sending cipher spec where both sides negotiate about everything based on previously happened communication. This phase ends the handshake because everything will be encrypted after this.

SSH (Secure Shell) offers a concept called **port forwarding**. What is it? What functionality does it offer? Explain with 2-3 sentences, not more.

> The SSH client opens a local port (e.g. 80) and local applications can connect to it. All data written (and read) is **sent** encrypted **to** the **SSH server** which decrypts it and **forwards** it **to** the final **target** (in this case, a **web server** at the other end).

When port forwarding is used, SSH opens a number of ports. This may enable attacks by other users, local and remote. Explain!

> Basically, a port **doesn't offer access control.** Establishing port forwarding to certain port will make end user to that port vulnerable to an intruder. Thus that means that everyone can connect to the port and access the service (sending datas to the remote server). On multi user systems, this is a bad thing because there will be numerous of users that could communicate with corresponding port and access the remote server.

---
### **May 2014**

There are several ways to **scan** networks and systems, for example with **SYN** and **ACK** scans. Explain the difference(s) between these two types and describe what the expected results from them are. It should be obvious from your answer that there is a reason why scanning tools offer different methods to scan systems.

> Both SYN and ACK scans follow the TCP 3-way handshake sequences to conform specific purposes. SYN scan will **probe ports** on the target host with TCP message with **SYN flag** is **set to 1**. If there is a SYN/ACK result from the target host, then there is an open port in the host. If there is a RST result, then the port is closed. If there is no reply, there is a firewall in between.

> ACK scan is performed to **check the state** of the firewall. If you got the RST, then there is an either closed or open port on the target host. If there is no reply, then maybe the packet is **filtered** by stateful firewall or there is no host at all.

UDP scanning is harder to do than TCP scanning. Why? How can operating systems, at least to some degree, protect themselves against this type of scanning?

> Because UDP doesn't maintain the connection (like TCP, which maintain the connection state using TCP sequence number). Thus, we never really know whether the packet is dropped or silently accepted. If there is no host open, then we will get **ICMP Unreachable Host** reply. A protection can be implemented, e.g. limiting the number of ICMP reply rate (1 reply per second) to avoid ICMP flood.

Give two examples of attacks against IP (one sentence per attack is enough)

> First is **IP spoofing** attack. IP spoofing is when an attacker send a packet to a target host with the victim's IP address set as the source IP in the header. Attacker uses this to hide their true identity and to exploit trust connection between victim and the target host.

> Second is **LAND** attack. It is performed by sending a packet to the victim with victim's IP address set as both the source and destination IP in the message header. This attack will produce bug that leads to a system crash.

The allocation of **SYN cookies** is a possible defense against **SYN flooding** DoS attacks. The defense is to store a SYN cookie in the initial serial number (ISN) selected by the server:

* **ISN = hash ( src and dest IP & port addr + secret + client’s ISN )**

What is a **SYN flooding** attack? Also explain clearly why SYN cookies can offer protection and why it works! Are there any drawbacks or problems with this method?

> After receiving SYN message, target host will allocate internal resource to maintain the state of the connection and then return SYN/ACK message. When attacker keep sending SYN message in a very frequent fashion, then eventually the resource will be **exhausted**.

> The idea of **SYN cookies** is the target host will not save the state internally. Instead, they will let the client to save their own state. The state contained inside ISN that will be compared with what client sends back later on. If there is no ACK (unfinished handshake), then ISN is discarded. If there is an ACK reply from the client (either from attacker or from legitimate client), the ACK will be compared with the ISN to check the integrity.

> The **drawback** of  SYN cookies is all about performance. The hash function will take some computation time obviously that the attacker should consider.

---

> **Written** by [Yanuar T. Aditya](http://januaditya.com)