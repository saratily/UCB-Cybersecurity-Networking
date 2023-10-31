# IP and Protocols
```
Overview
DHCP
DEMO
Attacks:
    DHCP starvation
    DHCP spoofing
    DHCP snooping
DEMO
NAT
Routing scheme and protocols
Routing Technique
    Static Routing
    Dynamic Routing
Routing Protocol
    Distance
    Speed
wireless - Remote networking concepts
    WiFi
    wireless access point (WAP)
    MAC address
    SSID
    1. WEP
    2. WPA
    3. WPA 2

Wireless attack
    Cybercriminal Tactics
    air-crack-ng + wireshark
```

# Overview
- DHCP and NAT
- Routing schemes and routing protocols
- wireless - Remote networking concepts

## Dynamic Host Configuration Protocol (DHCP)
- Layer 7: applciation layer protocol, using 67 (for server) & 68 (for client) port
- client/server based protocol for LAN, to manage and assign IP address, so device in LAN can communicate on WAN
- on small network, DHCP server is part of router
- server/router uses MAC address to track client
-  4 step process
    1. DHCP discover
        |_ Client needs to find server
        Client -> broadcast
        0.0.0.0 -> 255.255.255.255
        Note: both IPs are reserved: 0.0.0.0 is when IP is not assigned, 255.255.255.255 is for broadcast

    2. DHCP offer
        |_ server checks next available IP, and sends it out to the network.
        Server -> Client
        192.168.0.1 -> 192.168.0.10

    3. DHCP request
        |_ client sends a message back to the DHCP server
        Client -> broadcast
        0.0.0.0 -> 255.255.255.255

    4. DHCP ack
        |_ server lease IP address to the client. Once it expires, the DHCP server can give that IP to another device.
        Server -> Client
        192.168.0.1 -> 192.168.0.10

### DEMO
In VM box, download Resources/dhcp.pcap
In the terminal inside VM box, 
$ sudo wireshark dhcp.pcap

Filters:
dhcp.option.dhcp == 1       -> for deliver
dhcp.option.dhcp == 2       -> for offer
dhcp.option.dhcp == 3       -> for request
dhcp.option.dhcp == 5       -> for ack

## Attacks:
- DHCP server has limit IP addresses

### DHCP starvation
- Attacker can make too many calls, resulting in DHCP server running out of space
- type of denial of service (DoS) attack.
- impacts the availability aspect of the CIA triad. 
PREVENT: set maximum threshold - # of DHCP request server can accept per second

### DHCP spoofing
- After DHCP starvation attack, set up a fraudulent DHCP server
- start sending traffic to the malicious router.
- The attacker can then use the router to capture sensitive data.
SOLUTION: DHCP snooping
- process implemented on a network switch
- inspects packets to confirm that they’re legitimate DHCP offers, and blocks those it determines to be unauthorized

### DEMO
```
In VM box, download Resources/DHCPAttack.pcap
In the terminal inside VM box, 
$ sudo wireshark DHCPAttack.pcap

OBSERVATION:
All calls are made within a second, and all calls are discover only
Broadcast massage takes alot of network traffic
May be: power outage
```

### ACTIVITY - 04_DHCP_Attacks

0. Get packet capture file
```
In VM box, download Resources/DHCPAttack.pcap
In the terminal inside VM box, 
$ sudo wireshark DHCPAttack.pcap
```
1. Create a filter to determine the count for each DHCP activity:
DHCP Discover -     dhcp.option.dhcp == 1       -> 135 out of 150 
DHCP Offer -        dhcp.option.dhcp == 2       -> 15 out of 150 
DHCP Request -      dhcp.option.dhcp == 3       -> none

2. Based on these results, summarize what type of attack may have occurred, and why you believe Acme Corp's employees are having network issues.
Either DHCP server is not configured properly and sending 255.255.255.255 (broadcast message instead of IP address to client, resulting in exhausting network traffic)
OR 
An attacker has starved DHCP server and spoofing as DHCP server 

#### Bonus
3. Analyze the source MAC addresses of the DHCP activities and summarize what the attacker is doing.
Attacker DHCP server is spoofing different MAC address for each deliver message, to look legit request

## Network Address Translation (NAT)
- Layer 3: Network layer tool
- method to map private IP address to public IP address and vice versa
- NAT table limit: # of available IP address * open prots
```
    e.g.    laptop                  ->          google.com
            source                              destination
            10.0.0.5:49200                      74.0.0.1:80
            
            laptop                      NAT table                       google.com
            10.0.0.5:49200  --->     replace 10.0.0.5:49200
                                     with 32.0.0.1:49200         --->    74.0.0.1:80
            google response:
                                    map 32.0.0.1:49200           <---    74.0.0.1:80
            10.0.0.5:49200  <---    to 10.0.0.5:49200
```    
## Routing scheme and protocols
- Routing is the act of choosing the path that traffic takes in or across networks.
- Routing schemes
1. Unicast - single device - e.g. phone call
disadv: can't send msg to more than one device
2. Broadcast - 1:all - send msg to entire LAN
disadv: cause unnecessary traffic
3. Multicast - 1:group of devices - subscription based service
disadv: have to maintain subscripton list

### Routing technique
1. Static IP address
    |_ Manually configured by network admin
    Adv. lower CPU
    Disadv: fault tolerance, if device goes down, can't be ajusted automatically 

2. Dynamic IP address
    |_ primary routing technique used over the internet
    |_ uses routing protocols to determine the best route to direct the traffic
    |_ if device goes down, re-route automatically
    Adv. The network is adaptive
        data gets forwarded based on network conditions.

### Network protocol
- Dynamic routing protocols look at two primary criteria to determine the optimal path:
                                                  B
                                              6 /   \ 6
                                              A ______C
                                                  8
#### 1. Distance 
- \# of hops takes to go from src to dest
- 2 Distance-Vector Routing Protocols
-- Routing Information Protocol (RIP)
    |_ dynamic protocols that uses the hops count as its main criteria for choosing the route.
    |_ A-C optimal path: A -> C = 8
-- Enhanced Interior Gateway Routing Protocol (EIGRP)
    |_ A more efficient distance-vector routing protocol than RIP.

#### 2. Speed
- time it takes to go form src to dest, (what if a hop is really slow in response, penalizing the route) 
    |_ Open Shortest Path First (OSPF)
        |_ If using a link-state routing protocol such as OSPF, speed would be the key factor.
        |_ \# b/w devices indicate time to get from src device to dest.
        |_ A-C optimal path: A -> B -> C = 6

### ACTIVITY - 07_Routing_Schemes_and_Protocols
```
            New York office         St Loius                Miami
A to C      A-B-C           = 3     A-C             = 1     A-D-C               = 3
A to I      A-B-C-E-I       = 8     A-C-G-K-J-I     = 6     A-D-C-E-F-J-I       = 7
A to K      A-B-C-D-G-K     = 8     A-C-G-K         = 4     A-D-C-E-F-J-K       = 8
A to M      A-B-C-E-H-L-M   = 9     A-C-G-K-J-N-M   = 8     A-D-C-E-F-J-I-M     = 10
                                                            A-D-C-E-F-J-K-N-M   = 10
E to N      E-H-L-M-N       = 5     E-I-J-N         = 5     E-F-J-K-N           = 5
```

## wireless - Remote networking 
- WiFi - type of wireless technology that uses radio waves to provide wireless internet and network connections.

- wireless access point (WAP)
    |_ network device connecting a wireless network to a wired network.
    |_ sends broadcast signal, called beacon
    |_ uses a Basic Service Set Identifier (BSSID) to identify its MAC address in a beacon signal.

- MAC address:
    |_ 6 hexadecimal octate
    00  :    A4    :    22            :     01  :   F3  :   45  
    Organization unique identifier          Network interface controller specific
    |_ Hard to recignize a device, SOLUTION: SSID

- SSID
    |_ Service Set Identifier
    |_ use more recognizable format.
    |_ admin can configure this SSID.
    |_ Use: when select “View Available Wireless Networks” on your device, the SSIDs are listed, e.g. “Airport WiFi,” “Cafe_Public,” etc.

- Wireless security
1. WEP
    |_ Wired Equivalent Privacy - 1999
    |_ The first kind of WiFi security, WEP was created as a security protocol using encryption to provide protection and privacy to wireless traffic.
2. WPA
    |_ WiFi Protected Access - 2003
    |_ Due to the vulnerabilities
    |_ a more secure and sophisticated wireless security protocol than WEP

3. WPA 2
    |_ 2006
    |_ An even more secure wireless protocol was created. 
    |_ commonly used security protocol in most WAPs today

### DEMO - Visualizing Wireless in Wireshark
- In VM box, download Resources/Beacon.pcapng
```
$ sudo wireshark Beacon.pcapng
```
OBSERVATION: all rows has 802.11 protocol

- Get SSID
```
IEEE 802.11 Wireless Management
    |_ Tagged parameter
        |_ Tag: SSID parameter set: 
            SSID -> right click -> Apply as Column
```
- Get BSSID
```
IEEE 802.11 Beacon Frame, Flags ..... c
    |_ BSS Id: -> right click -> Apply as Column
```

- Get Type of wireless security
```
IEEE 802.11 Wireless Management
    |_ Tagged parameter
        |_ Tag: Vendor Specific:     WPA Information Element
            |_ Type -> right click -> Apply as Column
            OR
            |_ WPA Version -> right click -> Apply as Column
```

### ACTIVITY - 11_Analyzing_Wireless_Security
Similar to demo
1. Determine how many wireless routers are in the Kansas City Office.

2. For each wireless router, determine:
SSID
BSSID
Type of wireless security

## Wireless attack
air-crack-ng
    |_ a free wireless decryption tool 
    |_ to decrypt WEP-encrypted wireless traffic

## Cybercriminal Tactics
- Cybercriminals can also create a fake wireless access point, called an evil twin.
- An attacker can make a fake SSID to trick unsuspecting users into connecting to the attacker’s wireless access point.
- For example: An attacker can set up a fake WAP with the SSID Starbucks_FreeWifi in a Starbucks coffee shop. Once the user is connected, the attacker can capture and view their traffic.

- Cybercriminals have several methods of finding weak wireless security routers:0
1. Wardriving
    |_ Driving around an area with a computer and a wireless antenna to find wireless LANs that may be vulnerable

2. Warchalking
    |_ Marking locations with chalk so sites can be exploited at these access points at a later time.

3. Warflying
    |_ Using drones to find vulnerable access points.

### DEMO - Decrypting with Aircrack-ng
- In VM box, download Resources/WEP.pcap

- if aircrack-ng is not install, then install it
$ sudo apt install aircrack-ng

- In the terminal inside VM box, 
```
sudo wireshark WEP.pcap
goto protocols
    |_ wireless
        |_ WAN traffic
            |_ last column WEP
```

- To capture live traffic
```
$ aircrack-ng
```

- To get decryption key of a recorded traffic, e.g. WEP.pcap file
```
$ aircrack-ng WEP.pcap 
Opening WEP.pcap
Read 133068 packets.

   #  BSSID              ESSID                     Encryption

   1  00:23:69:61:00:D0  Ment0rNet                 WEP (29719 IVs)

Choosing first network as target.

Opening WEP.pcap
Attack will be restarted every 5000 captured ivs.
Starting PTW attack with 29719 ivs.


                                Aircrack-ng 1.2 rc4


                [00:00:01] Tested 938 keys (got 26805 IVs)

   KB    depth   byte(vote)
    0    3/  4   D0(33536) 1F(33024) 27(33024) BC(33024) 2F(31744) 
    1    0/  1   E5(38656) 82(33024) 0C(32256) 3C(32000) EB(31744) 
    2    0/  6   9E(34048) 27(33792) 7A(32768) E9(32512) 8B(31744) 
    3    0/  4   B9(35328) D4(35072) 2E(34048) B9(33024) 00(32768) 
    4    8/ 10   6D(31488) 10(31232) B9(31232) 7A(30976) 95(30976) 

                         KEY FOUND! [ D0:E5:9E:B9:04 ] 
	Decrypted correctly: 100%
```

- Add above session key (D0:E5:9E:B9:04) to wireshark to decrypt traffic
```
$ sudo wireshark WEP.pcap
Edit -> Preference -> Protocol -> IEEE 802.11 
OR
View -> Wireless Toolbar -> '802.11 Preference' will appear as toolbar
Check 'Enable decryption'
Decryption key :Edit

Click + at the bottom to add decryption key
WEP : D0:E5:9E:B9:04               -> get it from the terminal
Hit OK
```
- All the traffic is decrypted now

### ACTIVITY - 14_Wireless_Attacks
Similar to demo
1. Open the kansascityWEP.pcap file in wireshark.
2. Use Aircrack-ng on the packet capture to determine the secret key.
3. Use the key to decrypt the traffic.
4. Analyze the decrypted traffic and determine the associated security risks.