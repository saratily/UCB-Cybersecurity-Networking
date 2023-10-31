# 11.1 Network Security/Firewall cheatsheet

day 1:      ufw     firewall            nmap
day 2:      onion   IDS         snort           NSM
day 3:

### Firewall & OSI model

```text
    L7: Application         _       
    L6: Presentation        |------> Application /Proxy / WAF (web application firewall)
    L5: Session             |       Circuit gateway
    L4: Transportation      |       Packet filtering (Stateful)
    L3: Network            _|       Packet Filtering (Stateless)
    L2: Data Link                  MAC layer filtering
    L1: Physical 
```

### ufw

```text
ufw version     
sudo ufw reset                      to reset all UFW rules
sudo ufw status                     to check the current status of the firewall
sudo ufw enable                     to start the firewall and update rules
sudo ufw default deny incoming      to block all incoming connections
sudo ufw default allow outgoing     to allow all outgoing connections
sudo ufw allow <port #>             to open specific ports
sudo ufw deny <port #>              to close specific ports
sudo ufw delete <port #>            to delete rules
sudo ufw disable                    to shut down the firewall
sudo ufw reload                     to reload (stop + start) the UFW firewall
```

### firewalld

```text
sudo systemctl status firewalld                         to check firewalld status
sudo systemctl start firewalld                          to start firewalld demon
/etc/init/d/firewalld start                              """
sudo firewalld-cmd --list-all
sudo firewalld-cmd --list-all-zone                      to list all current zone
sudo firewalld-cmd --zone=home --change-interface=eth0  to bind together u=interfaces
sudo firewalld-cmd --get-services                       to list currentlu configured services
sudo firewalld-cmd --zone=home --list-all               to list home related configured services
sudo firewalld-cmd --zone=home --permenant --add-rich-rule=<rule>   add rule to a zone 
--permanent - Optional - otherwise rules temp
<rule> -> 'rule family="ipv4" source address="10.10.10.10" reject'
            reject all traffic from 10.10.10.10 through ipv4
```

## nmap

```text
sudo nmap -O -p 1-500 --osscan-guess 192.168.68.187
-O                  -> OS fingerprint
-p                  -> ports 
1-500               -> port_range
--osscan-guess      -> guess open ports
192.168.68.187      -> ip address

-sO         -> IP protocol scan
-sV         -> enumerate service type
-A -T4      -> OS finger printing using fast execution
-a          -> OS type and version
-sA         -> enumberate the type of firewall in use
```
