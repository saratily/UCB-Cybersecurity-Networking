# Networking_Attacks_Review

1. ARP Attack
2. DHCP Attacks
3. TCP Attacks
4. Wireless Attacks
5. Email Attacks

## Part One: ARP Attacks

Open the network_attack_review.pcap.
Filter by ARP packets.
type arp in Filter

1. Review the packets captured, and explain in simple terms what is taking place in each of the three packets.

- A device (VMware_1d:b3:b1) sent a Broadcast, asking Who has 192.168.47.254? Tell 192.168.47.171 
- VMware_f9:f5:54 replies that 192.168.47.254 is at 00:50:56:f9:f5:54
- VMware_1d:b3:b1 also reply that 192.168.47.254 is at 00:0c:29:1d:b3:b1
(2 devices have same IP address)

2. What type of attack is this?

Cache poisoning

3. What is the MAC address of the good device?

00:50:56:f9:f5:54

4. What is the MAC address of the hacker's device?

 00:0c:29:1d:b3:b1 (similar to sending device)

5. What negative impact might this type of attack have?

an attacker device can intercept traffic and pretend to be another device and traffic will be routed to attackers machine instead of valid device

## Part Two: DHCP Attacks

Continue in the same PCAP.

1. Filter by DHCP packets.
dhcp

2. Review the packets captured, and explain in simple terms what is taking place.
all the request are DHCP Discover request, made form DHCP server

3. What type of attack is this?
2 step attack
starvation
then spoofing

4. Why is the destination IP 255.255.255.255 for all packets?
broadcast

5. What negative impact might this type of attack have?
no more ip address available 

## Part Three: TCP Attacks

Continue in the same PCAP.

1. Filter by TCP packets.
type tcp in filter

2. Review the packets captured, and explain in simple terms what is taking place.
checking open ports

3. What type of attack is this?
SYN Scan

4. Is this type of activity always an attack? In other words, can a security professional benefit from what is taking place?

5. What negative impact might this type of attack have?
Attacker can use open ports to atack server

## Part Four: Wireless Attacks

Answer the following questions:

1. What are the different security types available for Wireless communications? List them in order from least to most secure.
WEP
WPA
WPA 2

2. What is 802.11?

Stqandards for wireless security

3. What is an SSID?

name of wireless network

4. What is the name of the the signal a WAP sends out identifying its SSID?

Beacon

5. If a user has WEP encrypted wireless, what is a potential negative outcome?

## Part Five: Email Attacks

Open email_reviews.pdf.

1. Review the two emails and their headers and determine if the emails are legitimate or spoofed.

1st email: looks legit:

1. From email address looks valid: customercare@turbotax.com
2. SPF pass
3. arin.net: ip is assigned to INTUITS

2nd email: Looks suspicious:

1. From email does not look valid: asdc789asd@yahoo.com
2. SF fail
3. arin.net: location somewhere is Africa

2. Document your findings and summarize why you believe the emails are legitimate or spoofed.
