# 8.2 Ports, Protocol and OSI Model
```
Overview
Protocol
    Common protocols
    Network packet
    Packet structure
    DEMO
    ACTIVITY - 03_protocols
PORTS
    Common ports
    Virtual ports
    ACTIVITY - 06_ports
OSI model
    Application - app can understand, 
    Presentation - encrypt/decrypt, translation, format
    Session - connection - handles dataflow
    Transportation - port - TCP traffic
    Network - IP address
    Data Link - MAC address
    Physical - raw data, b/w devices
Wireshark
```
## Overview
- analyze header, payload and trailer
- understand ports and their role
- associate protocols to ports
- encapsulation/decapsulation - allowing different protocol interact with one another
-OSI model layers - identify sources of problems on a network
- Use wireshark to capture and analyze live network traffic


## 1. Protocol
- set of strict rules to impose structure
- specifying the precise meaning of keywords, and its location in message
- Networks use protocols to ensure messages are fully sent and understood.
- send FIN to end transmission

### Common protocols
- Different protocols often contain different header and payload fields
HTTP/HTTPS            web traffic
FTP/FTPS             file transfer
PAP
    |_ Password Authentication Protocol
    |_ Used for authenticating a user.
    |_ Uses 2-way handshake (username/pwd -> accept/reject <-)
    Req: 
    1 Byte      1   2 Bytes     1 Byte          Variable    1 Byte      Variable
    Code: 1     ID  Length      username len    username    pwd len     pwd

SMB (Server Message Block)              A Windows-based protocol used for sharing files.
NetBIOS (Network Basic Input/Output System) Allows computers to communicate on a local network

## Network Packet
- msg having protocol rules to impose communication structure
- Packet size - upto 2 ^ 32 
-- Header (R) - 20-60 bytes
-- Payload (R)
-- trailer (O)

### Packet structure
header  - help the receiver tp understand how to handle the packet
        - allows recipient to interpret the contents/direction of the communication.
        - IPv4 and IPv6 headers are different
        - First 4 bit - version (0100 for IPv4. 0110 for IPv6)
 
### DEMO 
use string-functions.com/binary-string.aspx - to convert binary to human readable
```
01110000011000010111000000100000
01110011011001010110111001110100
00101101011101010111001101100101
01110010011011100110000101101101
01100101001000000101000001000001
01010000010101010101001101000101
01010010001000000111000001100001
01110011011100110111011101101111
01110010011001000010000000110111
```

Decoded string: pap sent-username PAPUSER password 7

### ACTIVITY - 03_protocols

<details>
  <summary>Log File 1 -> HTTP</summary>

GET / HTTP/1.1
Host: widgets.com
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.117 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,nb;q=0.8
</details>

#### Log File 2 - FTP
File Transfer Protocol (FTP)
    230 Login successful.\r\n
        Response code: User logged in, proceed (230)
        Response arg: Login successful.

#### Log File 3 - HTTPS
TLSv1.2 Record Layer: Application Data Protocol: http-over-tls
    Content Type: Application Data (23)
    Version: TLS 1.2 (0x0303)
    Length: 56
    Encrypted Application Data: d03ff41452da9e9c3ec76cbeb35e8ffc1f64bf80f512924a?

#### Log File 4
Domain Name System (query)
    Transaction ID: 0x18b6
    Flags: 0x0100 Standard query
        0... .... .... .... = Response: Message is a query
        .000 0... .... .... = Opcode: Standard query (0)
        .... ..0. .... .... = Truncated: Message is not truncated
        .... ...1 .... .... = Recursion desired: Do query recursively
        .... .... .0.. .... = Z: reserved (0)
        .... .... ...0 .... = Non-authenticated data: Unacceptable
    Questions: 1
    Answer RRs: 0
    Authority RRs: 0
    Additional RRs: 0
    Queries
        applegate.com: type A, class IN
    [Response In: 623]

#### Log File 5
Address Resolution Protocol (request)
    Hardware type: Ethernet (1)
    Protocol type: IPv4 (0x0800)
    Hardware size: 6
    Protocol size: 4
    Opcode: request (1)
    Sender MAC address: Technico_65:1a:36 (88:f7:c7:65:1a:36)
    Sender IP address: 10.0.0.1
    Target MAC address: 00:00:00_00:00:00 (00:00:00:00:00:00)
    Target IP address: 10.0.0.6


#### Bonus Log Record
<details>
HCI H4
    [Direction: Unspecified (0xffffffff)]
    HCI Packet Type: HCI Command (0x01)
HCI Command - Read Local Supported Features
    Command Opcode: Read Local Supported Features (0x1003)
    Parameter Total Length: 0
    [Response in frame: 4]
    [Command-Response Delta: 4.181ms]
</details>

## PORTS 
Building #          Room #
IP address          Ports
    |_ The Internet Assigned Numbers Authority (IANA)
        |_ entity officially responsible for assigning port # for a designated purpose.
        
### Virtual ports
    |_ Computers don't have enough space for all ports, 
    |_ use s/w to create ports
    software to create virtual ports.
    |_ every protocol ->  numerical virtual port number.
    |_ destination port - where other machines send data to communicate
    |_ port # -> 0 - 2^32 ()
        |_ System ports - 0 - 2^10 - restricted - bind to OS services 
        |_ Registered ports - 2^10 - 49151 - users launching their own services
        |_ Dynamic / private ports - 49152– 2^23 (65535) - source port, selected from dynamic range, to send msg from a machine
        |_ e.g. 
            |_ source ->    188.4.4.72     Port 50158 (e.g. browser)
            |_ dest ->      151.20.0.145    Port 80

### Common ports
80      -> HTTP     - Sending web traffic.
443     -> HTTPS    - Sending encrypted web traffic.
21      -> FTP      - Sending files.
22      -> SSH      - Securely operating network services.
25      -> SMTP     - Sending emails.
53      -> DNS      - Translating domains into IP addresses.

### ACTIVITY - 06_ports
1. Open the log file provided to you, and note that each log record is distinguished with a title: Log Record 1, Log Record 2, etc.
2. Determine the source and destination port for each request.
3. Determine the protocol associated with each destination port.
4. Research each protocol and summarize what Sally Stealer might be doing based on your findings.

```
Log Record 1
Source port             : 50152
Destination port        : 80
Destination protocol    : HTTP
Protocol Summary        : accessing unencrypted website.

Log Record 2
Source port             : 53367
Destination port        : 443
Destination protocol    : HTTPS
Protocol Summary        : accessing encrypted website.

Log Record 3
Source port             : 64836
Destination port        : 21
Destination protocol    : FTP
Protocol Summary        : using FTP to transfer files.
```


## OSI model
https://en.wikipedia.org/wiki/OSI_model

### Application - app can understand, 
### Presentation - encrypt/decrypt, translation, format
### Session - connection - handles dataflow
### Transportation - port - TCP traffic
### Network - IP address
### Data Link - MAC address
### Physical - raw data, b/w devices
### ACTIVITY - 10_osi

Identify layer
1. A networking cable was cut in the Data Center and now no traffic can go out.
Physical layer

2. A code injection was submitted from an administrative website, and it's possible that an attacker can now see unauthorized directories from your Linux server.
Application layer

3. The MAC address of one of your network interface cards has been spoofed and is preventing some traffic from reaching its destination.
Data link layer

4. Your encrypted web traffic is now using a weak encryption cipher and the web traffic is now vulnerable to decryption.
Presentation layer

5. The destination IP address has been modified and traffic is being routed to an unauthorized location.
Network layer

6. A flood of TCP requests is causing performance issues.
Transport layer

7. A SQL injection attack has been detected by the SOC. This SQL injection may have deleted several database tables.
Application layer

8. A switch suddenly stopped working and local machines aren't receiving any traffic.
Data link layer

9. An ethernet cable was disconnected and the machine connected isn't able to receive any external traffic.
Physical layer

10. Traffic within the network is now being directed from the switch to a suspicious device.
Data link layer


## Wireshark
