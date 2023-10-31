# UCB-Cybersecurity-Networking

## Commonly used ports

```text
HTTP    : 80
HTTPS   : 443
FTP     : 21
SSH     : 22
SMTP    : 25
POP3    : 110
IMAP    : 143
```

## OSI Model and tools

| OSI model      | Tools | Platform  | Filters                 | Issues | Solution | 
|:--------------:|:-----:|:---------:|:-----------------------:|:-----:|:---------:|
| 7.Application - Interface-API ||||||
|| dhcp  | wireshark | dhcp.option.dhcp == 1->deliver          | Starvation ||
||       |           | dhcp.option.dhcp == 2->offer            | spoofing   | snooping |
|  |       |           | dhcp.option.dhcp == 3->request        |
|  |       |           | dhcp.option.dhcp == 1->ack (lease IP) |
|-----------------------|-------|-----------|-------------------------|-------|-----------|
| 6.Presentation - formatting/encryption/compression||||||
||aircrack-ng|||||
|-----------------------|-------|-----------|-------------------------|-------|-----------|
| 5.Session - authentication/authorization ||||||
||nslookup| terminal | -type=A		domain->IP |||
||        |          | -type=PTR	IP->domain |||
||        |          | -type=Cname	domain alias |||
||        |          | -type=SOA	admin details(Email,TTL,last_updated
||        |          | -type=NS	multi server dns |||
||        |          | -type=MX	mail exchange |||
||        |          | -type=TXT	human readable (details about SPF: layer3) |||
||nbstat|||||
|-----------------------|-------|-----------|-------------------------|-------|-----------|
| 4.Transport - Reliability  ||||||
||nmap (SYN SCAN) |wireshark| ||
|| Port state:| open         |SYN -> SYN/ACK|||
||| closed         |SYN -> RST/ACK|||
||| filtered(firewall) |SYN 		Stat -> Conv -> TCP	tcp.stream.eq.2 |||
|||terminal| sudo nmap -sS -pI 500 stupidmachine.io|||
||TCP - HTTP(S)/FTP/SSH/SMTP |wireshark|tcp.flags.syn == 1 |||
||SYN -> SYN/ACK -> ACK  SYN -> ACK/RESET \ FIN->ACK   FIN->ACK || tcp.flags.ack == 1 |||
||UDP |wireshark| ||
||netstat |wireshark| -l = only services which are listening on some port |||
|| || -n = show port number, don't try to resolve the service name |||
|| || -t = tcp ports |||
|| || -u = udp ports |||
|| || -p = name of the program |||
|-----------------------|-------|-----------|-------------------------|-------|-----------|
| 3.Network - Addressing and routing ||||||
||ipconfig| terminal ||||
||ping ICMP based| terminal | sudo ping -c 4 google.com |||
||||-a -> audible ping |||
||||-I -> ping using MAC address|||
||||-c n -> limit line count|||
||fping - multiple IP/IP range| terminal |-g |||
||traceroute (tracert)| terminal | sudo traceroute -I pepsi.com |||
||NAT | | map pvtIP -> pubIP |||
|||||||
|-----------------------|-------|-----------|-------------------------|-------|-----------|
| 2.Data Link - Logical Link Control/ Media Access Control ||||||
||ARP |wireshark| arp.opcode == 1 (req) broadcast | Spoofing/cache poisoning | static IP |
||||arp.opcode == 2 (resp)|||
||||arp.duplicate-address-detected (dups) |||
|-----------------------|-------|-----------|-------------------------|-------|-----------|
| 1.Physical - - Transmission ||||||
|-----------------------|-------|-----------|-------------------------|-------|-----------|