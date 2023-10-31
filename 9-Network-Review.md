# 13_Networking_Review

- Open reviewpackets.pcap.
- Filter for HTTP traffic.
- Make sure Name Resolution for Resolving Network Addresses is enabled.
- There should be four HTTP packets.

A. Answer the following questions on HTTP:

1. What does HTTP stand for?
Hypertext Transfer Protocol

2. What is the port number for HTTP?
80 or 8080

3. What types of services does HTTP provide?
accessing unencrypted website

4. Which OSI layer does HTTP exist in?
Layer 7: Application layer

5. What website is being accessed?
wireshark -> view -> Name resolution -> Resolve Network Addresses
example.com

6. What is the source port being used?
58424

7. What is the the port number range that this port is part of?
 Dynamic / private ports ranging 49152� 2^23 (65535)

B. Select packet number 419, which should be the first HTTP packet. View the packet details to answer the following questions:
Under Ethernet II is a value of Destination: Technico_65:1a:36 (88:f7:c7:65:1a:36)

1. What does this value represent?
Mac Address

2. Which OSI layer does this exist in?
Layer 2: Data Link layer

3. What networking devices use these values?
all devices are identified by MAC address

## Part Two: ARP

Continue viewing the same PCAP.
Filter for ARP traffic. There should be 115 ARP packets.
A. Answer the following questions on ARP:

1. What does ARP Stand for?
Address Resolution Protocol

2. What service does ARP provide?
maps MAC -> IP address within the LAN

3. Which OSI layer does ARP exist in?
Layer 2: Data Link layer

4. What type of networking request does ARP first make?
broadcast

B. Use a filter to find the count of ARP responses, and answer the following questions:

1. What is the IP of the device that is responding?
10.0.0.32
2. To what IP is the device responding to?
10.0.0.31
3. Write out in simple terms what has taken place, describing the request and response.
a0:a4:c5:10:ac:c0 is telling 10.0.0.31, that 
	10.0.0.32 is at a0:a4:c5:10:ac:c0

Part Three: DHCP
Continue viewing the same PCAP.

1. Filter for DHCP traffic.
type dhcp in Filter

2. There should be four DHCP packets.
DHCP Deliver - dhcp.option.dhcp == 1
DHCP offer - dhcp.option.dhcp == 2
DHCP Request - dhcp.option.dhcp == 3
DHCP Ack - dhcp.option.dhcp == 5

A. Answer the following questions on DHCP:

1. What does DHCP stand for?
Dynamic Host Configuration Protocol

2. What service does DHCP provide?
Assign IP address to a device, so a device in LAN can communitcate on WAN

3. Which OSI layer does DHCP exist in?
Layer 7: Application layer

4. What are the four steps of DHCP?
DHCP Deliver - dhcp.option.dhcp == 1
DHCP offer - dhcp.option.dhcp == 2
DHCP Request - dhcp.option.dhcp == 3
DHCP Ack - dhcp.option.dhcp == 5

B. Use a filter to view the DHCP Discover, and answer the following questions on that packet:

1. What is the original source IP?
0.0.0.0 

2. Why does it have that value?
NO IP assigned to device originally

3. What is the original destination IP?
255.255.255.255

4. What does that value signify?
A broadcast msg - no specific destination

C. Use a filter to view the DHCP ACK, and answer the following questions on that packet.
dhcp.option.dhcp == 5

1. Explain in simple terms what is happening in this packet.
10.0.0.1 is confirming that 10.0.0.32 will be leased to  a0:a4:c5:10:ac:c0

2. Define the term "DHCP lease."
DHCP lease means an IP address to assign to a device until it expires

3. What is the DHCP lease time provided in this packet?
604800 - 7 days

Part Four: TCP and UDP

Continue viewing the same PCAP.

1. Filter for the following IP Address: 185.42.236.155. There should be five packets.
ip.addr == 185.42.236.155
A. Answer the following questions on TCP:
1. What does TCP stand for?
Transmission Control Protocol

2. Is TCP connection-oriented or connection-less?
connection-oriented

3. Which OSI layer does TCP exist in?
Layer 4: Transport layer

4. What are the steps in a TCP connection?
SYN
SYN/ACK
ACK

5. What are the steps in a TCP termination?
FIN
ACK
FIN
ACK

6. What steps appear in the packets displayed?
SYN, SYN/ACK, ACK

7. What type of activity/protocol is TCP establishing a connection for?
HTTP

8. What is the website name being accessed after the TCP connection?
sportingnews.com

B. Answer the following questions on UDP:

1. What does UDP stand for?
User Datagram Protocol

2. Is UDP Connection oriented or connection-less?
connection-less

3. What type of services would UDP provide a benefit for?
when it is OK to loss some pkts, e.g. video streaming

Part Five: Network Devices, Topologies, and Routing
Open reviewdoc.pdf and answer the following questions:

A. Topologies
1. What are the Topologies for A, B, C?
A -> Tree
B -> Hybrid (Bus+star)
C -> Ring

2. What are the advantages and disadvantages for each?
A. If one node fails, its depend devices are not reachable
B. If one of the devices, connect to bus, fails, then all devices in its star are not reachable
C. Latency

## B. Network Devices

1. In the network devices illustration, what are numbers one through four?
1. Cloud/WAN
2. Firewall
3. Router
4. Switch

2. What does the dashed line represent in number five?
Separation b/w WAN and LAN

3. What is a load balancer?
Balances incoming traffic across all servers, to avoid DoS attack

4. Where would you place a load balancer?
Just after firewall

## Network Routing

1. Which routing protocols use distance as criteria?
RIP or EIGRP

2. Which routing protocols use speed as criteria?
OSPF

3. Using the shortest number of hops, determine the shortest path from A to O.
A -> C -> F -> J - M -> O

4. Using the least time, determine the shortest path from A to O.
A -> D -> C -> E -> F -> J -> K -> N -> S -> R -> Q -> P -> O

Part Six: Network Addressing:
Answer the following questions.

1. Define binary.
0/1 representation of a number (in decimanl, headecimal or octal number)

2. What are the two binary states?
0 - Off
1-On

3. What are IP addresses used for?
to identify a device on LAN or WAN

4. What are the two primary versions of IP addresses?
IPv4 and IPv6

5. How many octects are in a IPV4 address?
4

5. Use a web tool to determine the IP of the following binary representation: 11000000.10101000.00100000.00101011
https://www.browserling.com/tools/bin-to-ip
192.168.32.43

6. What is the difference between primary and public IP addresses?
A public IP address can be accessed through the internet, while a private IP address is assigned to a device in a private space such as an office or home. Typically, private IP addresses are not directly exposed to the internet, so other people cannot navigate to your personal device.

7. What is CIDR?
Range of IP addresses

8. What is the range of IP addresses in: 192.18.65.0/24?
192.18.65.0 - 192.18.65.255
