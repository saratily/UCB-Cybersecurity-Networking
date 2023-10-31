# 11.1 Introduction to Firewalls and Network Security

 - [11.1 Introduction to Firewalls and Network Security](#111-introduction-to-firewalls-and-network-security)
  * [Overview](#overview)
  * [Network security](#network-security)
  * [Firewall](#firewall)
    + [Firewall & OSI model](#firewall---osi-model)
      - [1. MAC layer firewall](#1-mac-layer-firewall)
      - [2. Packet Filtering Firewall](#2-packet-filtering-firewall)
        * [- Stateless](#--stateless)
        * [- Stateful](#--stateful)
    + [3. Circuit gateway](#3-circuit-gateway)
    + [4. Application/Proxy firewall](#4-application-proxy-firewall)
  * [ufw](#ufw)
    + [ufw Features](#ufw-features)
    + [DEMO - ufw](#demo---ufw)
    + [ACTIVITY: Configuring UFW](#activity--configuring-ufw)
      - [Set Up](#set-up)
      - [Activity Instructions](#activity-instructions)
      - [Bonus:](#bonus-)
  * [firewalld](#firewalld)
    + [DEMO](#demo)
    + [ACTIVITY - Firewalld ()](#activity---firewalld---)
  * [nmap](#nmap)
    + [DEMO - nmap - TODO](#demo---nmap---todo)
    + [ACTIVTY - Testing Firewall Rules with Nmap - TO DO](#activty---testing-firewall-rules-with-nmap---to-do)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

==================================================================================

## Overview 
- explore linux based cmd tools to implement firewall
-- ufw 
-- firewalld
-- iptable (not discussed)
- 
- nmap

## Network security
-  restricts unauthorized access, misuse, malfunction, modification, destruction, or improper disclosure
-  DiD - Defense in Depth - 
-- reduce attack progression
```
Perimeter
        Network 
            Host
                Application
                    Data
```

## Firewall
Distinguish b/w trusted and untrusted traffic/connection
Two types of firewat

- Network firewall 
|_ Usually placed in front of router
|_ Inspect src/dest addr & ports, TCP flags, and incoming packets features
|_ only allow trusted src

- Host-based firewall
|_ runs on machine to protect that machine


### Firewall & OSI model
```
    L7: Application         _       
    L6: Presentation        |------> Application /Proxy / WAF (web application firewall)
    L5: Session             |       Circuit gateway
    L4: Transportation      |       Packet filtering (Stateful)
    L3: Network            _|       Packet Filtering (Stateless)
    L2: Data Link                  MAC layer filtering
    L1: Physical 
```

#### 1. MAC layer firewall
    |_ (MAC) address is a unique hardware ID that helps devices communicate with each other.
    |_  filter based on source and destination MAC addresses.
    |_ Routers compare the MAC address of a device against an approved list. If there is a match, the traffic is forwarded to that device
    |_ Adv:
        |_ Can secure the network from novice attackers
    |_ Disadv:
        |_ Can be easily bypassed by MAC spoofing

#### 2. Packet Filtering Firewall
    |_ User can create their own firewall (for both stateless and stateful)
    |_ Use rules based on individual packets:
        ? Source and destination IP address
        ? Source and destination port information
        ? IP protocols
        ? Ingress and egress interface

##### - Stateless
    |_ operate at Layer 3: Network layer
    |_ These firewalls statically evaluate the contents of packets and do not keep track of the state of a network connection (aka stateless).
    |_ create checkpoints within a router and examine packets as they progress through an interface. If the information does not pass the inspection, it is dropped. 
    |_ NO session control
    |_ Adv: 
        |_ Not resource intensive, meaning they are low-cost and do not have a significant impact on system performance.
        |_ Work best with small networks. 
    |_ Disadv:
        |_ someone can easily spoof
        |_ Easy to subvert compared to more robust firewalls.
        |_ Only operate at the network layer.
        |_ They are vulnerable to spoofing and do not support custom based rule sets. 

##### - Stateful
    |_ keep track of communication, 3-way handshake
    |_ examine the connection as whole, looking at streams of packets.
    |_ They inspect the packets’ conversation and routing tables and use a combination of TCP handshake verification and packet inspection technology.
    |_ can identify different protocol, such as HTTP but can't distinguish file vs img
    |_  MAC: Built-in firewall
        Windows: windows defender
        Cloud: choose from a list
    |_ packets in Stateful firewalls can have:
        ? new state -> Trying to establish a new connection
        ? established state -> part of an existing connection
        ? rogue packet -> neither new nor existing connection
    |_ Adv:
        |_ Offer transparent mode, which allows direct connections between clients and servers
    |_ Disadv:
        |_ Are resource-intensive systems.
        |_ Don't understand application layer, hence don't understand underlying traffic. An attacker can encrypt a malware, and these firewalls can't detect it 

### 3. Circuit gateway
    |_ Operates on Layer 5: session layer
    |_ only look at the header of a packet. Once the circuit is allowed to establish an end-to-end connection, all data is tunneled between parties
    |_ work by verifying the three-way TCP handshake.
    |_ TCP handshake checks can verify the following about a source:
        ? Unique session identifier
        ? State of the connection (handshake established, closed)
        ? Sequencing information
    |_ Adv: 
        |_ Quickly and easily approve and deny traffic without consuming significant computing resources. 
        |_ Relatively inexpensive and provide anonymity to the private network. 
    |_ Disadv:
        |_ Do not check the contents of the packet. 
        |_ If a packet contains malware but has the correct TCP information, the data is allowed to pass through.

### 4. Application/Proxy firewall
    |_ Works on Layer 3 through Layer 7 
    |_ inspect the actual contents of the packet, including authentication and encryption components. 
    |_ Proxy firewalls use deep packet inspection and stateful inspection to determine if incoming traffic is safe or harmful.
    |_ can intercept all traffic on its way to its final destination, without the data source knowing.
    |_ A connection is established to the proxy firewall, which inspects the traffic and forwards it if it’s determined to be safe, or drops it if it’s determined to be malicious.
    |_ extra layer of protection b/w src & dest  by hiding dest from src, provides anonymity and hence protection for the network
    |_ Adv:
        |_ More secure than other implementations and provide simple log and file audit management for incoming traffic. 
    |_ Disadv:
        |_ Resource intensive. Requires robust modern hardware and high costs. Bypassed with encryption.

## ufw 
    |_ Uncomplicated Firewall
    |_ Layer 3 L& 4 firewall
    |_ multifunctional firewall, providing stateless and stateful packet filtering.
    |_ works on all kinds of network address and port translation,
        |_ such as Network Address Translation (NAT) and 
        |_ Network Address Protection (NAP).
        |_ It is a standard Linux firewall, avaialbe on Ubuntu, debian by default

### ufw Features
    Host-based          UFW is most commonly used on hosts.
    Logging             UFW has the ability to generate multi-level logs, providing great insight into attacks.
    Remote management   Firewalls can be remotely managed. e.g. SSH -> 22.
    allow/deny Rules    src & dest IP, port, packet types, all without opening the packet. Also uses TCP handshake, packet inspection.
    Rate-limiting       Supports rate-limited connections to protect from brute force attacks.

### DEMO - ufw
```
$ man ufw

# start fresh
$ sudo ufw reset

# check status
$ sudo ufw status

# to start the firewall and update rules
$ sudo ufw enable

# start denying both incoming and outgoing service
$ sudo ufw default deny incoming
$ sudo ufw default deny outgoing

# check status - use verbose flag will add detailed view
$ sudo ufw status verbose 

# open ports 22, 80, and 443.
$ sudo ufw allow 22
$ sudo ufw allow 80
$ sudo ufw allow 443

# deny 110 - pop3 mail 
$ sudo ufw allow 110

# remove 'deny 110 rule' - more specific, better
$ sudo ufw delete deny 110
$ sudo ufw status verbose

# stop ufw firewall 
$ sudo ufw disable

#start ufw firewall
$ sudo ufw enable

# reload is stop and start in one line
$ sudo ufw reload
```

### ACTIVITY: Configuring UFW
In this activity, you will use UFW to harden your system and ensure that your organization complies with PCI DSS (Payment Card Industry Data Security Standard)

#### Set Up
Complete the following steps before beginning the activity:
1. Log into your Ubuntu UFW virtual machine with the following credentials:
    Username: sysadmin
    Password: cybersecurity

2. Reset all the UFW rules to their factory defaults. This will allow us to customize UFW with our own rule sets.
    Run sudo ufw reset
When prompted, enter the letter y and press Enter.

3. Make sure firewalld is not running. (We'll learn more about firewalld in the next section.)
    Run sudo service firewalld stop
    Run sudo service firewalld status

#### Activity Instructions
1. Test that you can SSH into the Ubuntu UFW using Ubuntu firewalld (Other VM). The firewalld VM will be used to test connections to the UFW VM.
- Log into the firewalld VM with the following credentials:
        Username: sysadmin
        Password: cybersecurity
- Attempt to use SSH to log into the UFW VM.
- See if you can use SSH to gain access to your Ubuntu UFW. Make sure to use the IP address for the Eth0 interface of the Ubuntu UFW.

- Type the command to SSH.
- Type exit to terminate the connection.
- Now that you know you can log in using SSH, you'll use UFW to stop that from happening.

2. Switch back to the Ubuntu UFW. Enable UFW and set up default rules that block all incoming and allow all outgoing traffic.
3. Block all incoming SSH connections.
4. The protocol Telnet sends unencrypted traffic and is therefore a vulnerability. Configure the firewall to block Telnet.


5. Open the following ports to allow incoming information from web servers and mail applications.
    - 80: For HTTP connections
    - 143: For IMAP (Internet Message Access Protocol)
    - 587: For SMTP (Simple Mail Transfer Protocol)
    - 110: For POP (Post Office Protocol)
    - 443: For HTTPS (HTTP over SSL)

6. Stop and restart UFW then verify the UFW status.
- Check the UFW status to see if the rules are still there.

7. Use the SSH protocol to try to connect to the UFW VM from firewalld VM.
- Type the SSH command to connect to the UFW machine.
- Press Ctrl+c to stop your terminal from processing.
- Does this confirm that the Ubuntu UFW VM will block all incoming SSH traffic?

8. Recall that you blocked Telnet but opened up port 80. What will happen if you try to use Telnet on port 80?
- Type the command to telnet to port 80.
- What do you see? What happens if you type a letter and then hit enter?
- You will see the web server contacted on port 80 and the HTTP headers retrieved.
-- Why is it able to make the connection with Telnet even though you blocked Telnet?

9. Switch back to the UFW VM. Delete your initial rule blocking SSH. It is no longer needed because you are actively blocking all incoming connections.
- Type the command that deletes the first rule only. 'sudo ufw delete #' (It goes by the line the rule is on)
-- If you did not block SSH as your first rule, enter sudo ufw status and determine the line number of your deny 22 rule. Add that line number to the end of the command.
- You will be prompted to delete your deny 22 rule.

#### Bonus:
10. You have blocked all incoming traffic except for the protocols deemed essential. Try to SSH once more from the firewalld VM into UFW. What happens? (We'll learn more about firewalld in the next section.)
- You'll notice you're still blocked even though we deleted the ssh rule, this is because we are still blocking all incoming traffic. We will now add a rule to just allow only the firewalld VM to SSH into UFW.

- Now you will allow the firewalld's IP address to access to your VM via SSH.
--Type the command that allows traffic to ufw vm on port 22 from firewalld VMs IP address.

11. To recap, you now have a rule in place that blocks all incoming traffic and a rule allowing firewalld VM's IP address to connect on port 22 to UFW VM.
Switch back to the firewalld VM and test using SSH again:
- From the firewalld VM, try to SSH back into the UFW VM.
- Type the command to SSH into UFW VM.
- Exit out of the active SSH connection.

## firewalld
    |_ dynamically manage firewall
    |_ comes with pre-defined zones(e.g. home, work, etc.) then can add new zones or customize exisiting ones 
    |_ zone logical partition (jsut like group)
    |_ e.g. guest zone, HR zone, mkt zone
    |_ runtime config -> unless specify as permanent
    
### DEMO
```
- To check whether firewalld demon is running or not
$ sudo systemctl status firewalld

- To start firewalld demon
$ sudo systemctl start firewalld
OR 
$ /etc/init/d/firewalld start

- 
firewalld-cmd --list-all
sudo firewalld-cmd --list-all-zone                      to list all current zone
sudo firewalld-cmd --zone=home --change-interface=eth0  to bind together u=interfaces
sudo firewalld-cmd --get-services                       to list currentlu configured services
sudo firewalld-cmd --zone=home --list-all               to list home related configured services
sudo firewalld-cmd --zone=home --permenant --add-rich-rule=<rule>   add rule to a zone 
--permanent - Optional - otherwise rules temp
<rule> -> 'rule family="ipv4" source address="10.10.10.10" reject'
            reject all traffic from 10.10.10.10 through ipv4


```
### ACTIVITY - Firewalld ()
Firewalld Configuration - In this activity, you will use firewall to add rules to various zones. 



## nmap

### DEMO - nmap - TODO 

### ACTIVTY - Testing Firewall Rules with Nmap - TO DO 
- In this activity, you will perform various network scans to test firewall integrity.