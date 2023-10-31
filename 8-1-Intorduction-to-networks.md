# 8.1: Intorduction to networks
```
Overview
Network
    1. Players of network
    2. Client  Server model
    ACTIVITY - 03_netsec
    3. Network security
    4. Network structure
        |_ LAN
        |_ WAN

Network Topology
    1. Ring
    2. Linear
    3. Star
    4. Bus
    5. Tree
    6. Fully-connected
    7. Mesh
    8. Hybrid
    ACTIVITY - 06_netstr - TODO 

Network Devices
    1. Router
    2. Switch
    3. Hub - depricated 
    4. Bridge
    5. Network Interface Controller (NIC)
    6. Modem
    7. Wireless Access Point (WAP)
    8. All-in-One Device

Network security Devices
    1. Firewall
    2. Load Balancers
    3. Demilitarized Zone (DMZ)
    Network Design Demo - TODO
    ACTIVITY - 09_netdev - TODO

Network Address
    Binary data
    IP Address
        IPv4
        IPv6
    Public IPs
    Private IPs
    Subnetting
    CIDRs
    MAC Address
    ACTIVITY - 13_netadr
```

## Overview
- identify clients/servers and requests/responses
- Identify network topologies; adv /disadv
- Design a conceptual network made of various network and network security devices.
- Convert binary -> IP addresses and identify servers the IP addresses belong to.
- Modify host file to circumvent DNS and redirect access of a website

## Network
### 1. Players of network
```
    -- Services
        |_ Amazon, ebay
        |_ GPS
        |_ online gaming
        |_ video streaming
    
    -- Resources
        |_ Web pages
        |_ mail
        |_ images
        |_ data files
    
    -- Devices (nodes or client)
        |_ Computers
        |_ laptops
        |_ cell phones
        |_ printers
        |_ servers
```

### 2. Client  Server model
- defines how resources and service are shared across a network.
- 1: m -> server: client
- request/response method - 2-way conversation b/w client and server
    |_ request - client send msg
    |_ response - server process request and send back msg
    |_ response msg - resource / ack / err

### Network security
- practices and policies used to protect and monitor a computer network’s resources against threats and risks, including:
    |_ Unauthorized access into networks
    |_ Denial of service (DoS) attacks
    |_ Eavesdropping
    |_ Data modification

### ACTIVITY - 03_netsec

1. A hacker logged into Microsoft Outlook with the stolen username and password of Acme's CFO.  The hacker sent an email to the head of accounting asking them to wire $10,000 to a foreign account owned by the hacker.
```
Client: Hacker / MS Outlook
Server: Mail exchange server
Request: 
Response:
```
2. A hacker used Firefox to visit the administrative website of Acme Corp, where they attempted to log into the CFO's account multiple times, until they correctly guessed the password.
```
Client: Browser / hacker
Server: web server - hosting admin pages
Request: login
Response: login attemp failed/sucessful
```

3. A hacker stole the Acme CFO's mobile phone. Login credentials were saved on the phone, allowing the hacker to log into Acme Corp's mobile admin application.
```
Client: mobile phone
Server: 
Request: login attempt
Response: accept or denied
```

### 3. Network structure
#### Local Area Network (LAN)
    |_ a private computer network that connects devices in smaller physical areas.
    |_ e. g multiple computers connected to router
ADV: 
    |_ Network speed and performance
    |_ Network security- easy to control data and access to resources.
    |_ Versatility - add/remove device easily

#### Wide Area Network (WAN)
    |_ network connecting multiple LANs.
    |_ e.g. in a large company, connect different dept (e.g. FInance, mkting, etc) LAN
    | initially used at MIT to communicate amongst dept 
ADV: 
    |_ share/access resources across larger geographic area.
DisADV:
    Security issues: Traffic/communication needs encryption
    Troubleshooting: challenging - hard to identify what is causing issue, due to lack of control (e.g. can't access portal, hard to know, either web site is down or site is blocked or router is not working)


## Network Topology
    |_ design how machines will be connected to each other
    |_ ideally should not have single point of failure
    |_ If an attacker takes control of a device on a topology in which that device is connected to other devices, the attacker may also be able to move from the compromised device to any other device on the network, which would have considerably more impact on the business.
    
### 1. Ring
    |_  each device is connected to the next device in the chain
    Types:
        |_ Bidirectional
            |_ allows traffic to move in either direction.
        |_  Unidirectional
            |_ flows traffic in a single direction.
    Adv:
        |_ Simple to build
        |_ Adding/removing device easy
        |_ No central node to manage
    DisAdv: 
        |_ single point of failure - one machine breaks down, affect entire network.
        |_ Latency
        
### 2. Linear
    |_ each device is connected to the adjacent device by a two-way link
    |_ just like ring, except two end machines are not connected
    Adv:
        |_ Add/remove device is easy
    DisAdv:
        |_ single point of failure - one machine breaks down, affect entire network.
        |_ Latency

### 3. Star
    |_  all devices in the network are attached to a central node
    Adv:
        |_ easy to extend the network
        |_ consistent communication amongst all devices
        |_ If one device fails, doesn't impact network
    DisAdv:
        |_ if central node fails, whole network goes down
        |_ # of devices -> # of nodes, central node allowed to connect
        |_ Hard to conenct a device which is physically far

### 4. Bus
    |_ every device is attached to a central data link
    |_ device transmits data through link -> all devices receives it simulteneously
    Adv: 
        |_ Easy expansion
        |_ data transmitted quickly to all devices
    Disadv:
        |_ waste of bandwidth - send data to all devices
        |_ two devices can't communicate
        |_ 
### 5. Tree
    |_ many connected devices are arranged like the branches of a tree.
    |_ there can be only one connection between any two connected devices.
    Adv:
        |_ easy to expand
    Disadv:
        |_ if top node is impacted, all devices below are impacted

### 6. Fully-connected
    |_ every device on the network is directly connected to every other
    Adv:
        |_ fast data trasmission b/w two devices
        |_ if single connection b/w two machine fails, they can still communicate
    Disadv:
        |_ very expensive to establsh
        |_ too complicated to setup
        |_ # of links - exponential

### 7. Mesh
    |_ similar to a fully connected, except, not every device is directly connected. | Slight less exoensive than fully-connected
    |_ Adv/DisAdv same as fully-connected

### 8. Hybrid
    |_ any combination of the above topologies.
    |_ Adv and DisAdv depends on the types of networks combined

### ACTIVITY - 06_netstr

## Network Devices
    |_ primary devices used in LAN and WAN

### 1. Router
    |_ Connect 2 networks (2 LANs, 2 WANs, or LAN to WAN)

### 2. Switch
    |_ forwards resources within a network
    |_ connect devices within a LAN.
    |_ can be programmed to direct resources to certain computers

### 3. Hub - depricated 
    |_ Same as switch, can't program (not inteligent)
    |_ less secure, connect all devices
### 4. Bridge
    |_  switch that only has two connections, one in and one out.
    |_ Usually connect 2 LANs

### 5.  Network Interface Controller (NIC)
    |_ a circuit board or chip installed on a computer.
    |_ Each computer must have an NIC in order to receive or send resources.
    |_  connects a computer to a computer network.

### 6.  Modem (modulator-demodulator)
    |_ converts resource data (digital -> analog, analog -> digital)
    |_ Computer -> digital 
    |_ Internet service provider -> analog
    |_ Computer and ISP can understand each other

### 7.  Wireless Access Point (WAP)
    |_  give wireless devices the ability to connect to a wired network.

### 8.  All-in-One Device (Pineapple)
    |_ can have modems, WAPs, routers, and more all built into a single device.
    Adv:
        |_ easy to use
        |_ less equipment, set up and maintained.
    DisAdv:
        |_ Single point of failure
        |_ hard to troubleshoot

## Network security Devices
    |_ devices used to protect resources on the network.

### 1. Firewall
    |_ intelligent device (programmable)
    |_ monitors incoming and outgoing traffic based on security rules.
    |_ placed right at the entry point of a LAN - to protect the confidentiality and integrity of resources within that LAN.

### 2. Load Balancers
    |_ intelligent device - placed right after a firewall.
    |_  distributes incoming network traffic across multiple servers.
    |_  ensures no single server has to handle too much traffic.
    |_ protects the availability of resources.
    
### 3. Demilitarized Zone (DMZ)
    |_ A DMZ is a smaller subnetwork within a LAN used to add an additional layer of security to an organization’s LAN, protecting secure data within the internal networks.
    |_ A DMZ typically has its own network security devices, such as firewalls, that attempt to detect network attacks before they access the internal networks

### Network Design Demo
1. Making networks more efficient, since proximity of certain devices can reduce latency.
2. Avoiding the creation of a “single point of failure.”
3. Ensuring private resources are protected from unauthorized sources.

### ACTIVITY - 09_netdev

# Network Address
## Binary data
    |_ 0/1 -> off/on
    Conversions:
        |_ Binary to ASCII -> 01101000 01101001 = hi
        |_ Binary to Hexadecimal -> 11000111 00000110 10100110 11100110 11110110 01000110 =  C7 6 A6 E6 F6 46.
        |_ Binary to Octal ->  11000111 00000110 = 307 6.
    |_ used by networks to identify network addresses to determine where to send data.

## IP Address
    |_ is a numerical network address associated with a device (e.g. computer, printer, router or server)
    |_ www.whatsmyip.org -> to find out IP address
    |_  managed by a global organization known as the Internet Assigned Numbers Authority (IANA)

### Primary Versions (2):
#### 1. IPv4
    |_ four octets separated by decimals
    |_ each octat -> 8 bits or 1 byte -> decimal numbers.
    |_ e.g. 10.0.3.254 ( 00001010    00000000    00000011    11111110)
    |_ Lowest value: 00000000 -> 0
    |_ Higest value: 11111111 -> 255

#### 2. IPv6
    |_ created becasue of IPv4 limited availibilty of address
    |_ Not widely adopted, needs device update to receive/transmit data
    |_ 8 groups of 2 bytes, in hex format
    |_ e.g 2001:0db8:85a3:0000:0000:8a2e:0370:7334

#### Converting IP 
IP to Binary: browserling.com/tools/ip-to-bin
Binary to IP: browserling.com/tools/bin-to-ip

### Categorize of IP (2)
#### 1. Public IPs
    |_ can be accessed over the internet.
    |_ typically assigned in IP ranges: (108.0.0.1 – 108.0.0.10)
    DisAdv: nOt all devices should be available over internet

#### 2. Private IPs
    |_ not exposed to the internet
    |_ typically located within a LAN
    |_ 3 ranges:
        start IP              ending IP             IP add available
        10.0.0.0            10.255.255.255          16,777,216
        172.16.0.0          172.31.255.255          1,048,576
        192.168.0.0         192.168.255.255         65,536
    Adv:
        |_ more secure
        |_ can be reused, within different LANs.
        |_ can't conflict across different networks
    DisAdv:
        |_ not directly accessible over the public internet.
        |_ assigned by a network administrator of the LAN they belong to.

### Subnetting
    |_ process of breaking up an IP address range into smaller, more specific networks of grouped devices
    |_ e.g. out of 100 IP address, assign 50 to mkt, & 50 to finance

### CIDRs - Classless Inter-Domain Routing
    |_ CIDR-IP Range Calculator: ipaddressguide.com/cidr
    |_ 2 parts: prefix & suffix
    |_ prefix -> The first 3 octets would be the Network ID
    |_ suffix -> The range of IPs and the number of IPs available.
    |_ e.g. 192.243.3.0 /24
        192.243.3.0  -> IP address
        24->    everything after the first 24 bits is variable. 
                the first 24 bits (three octets) are static for a given network.

### MAC Address
    |_ assigned to computer NIC 
    |_ unique to each NIC located on the same network
    |_ six sets of alphanumeric characters, separated by colons.
    |_ The first 24-bits identify the vendor/manufacturer/organization associated with the NIC.
    |_ e.g. 00:0a:95:9d:68:16

### ACTIVITY - 13_netadr

11000000101010000100010110010001
192.168.69.145
Private
Trade Secrets Server

00001010000000000000000000101010
10.0.0.42
Private
Trade Secrets Server

11000000101011000100010110010001
192.172.69.145
Private
Trade Secrets Server

00101001001011011011011000100000
41.45.182.32
Public
Intellectual Property Secret

00001010000000000000000001001100
10.0.0.76
Private
Trade Secrets Server