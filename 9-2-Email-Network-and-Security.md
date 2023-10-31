# Email, Network and Security
```OverviewDNS erverDNS zone file    A - domain -> ip
    PTR - pointer ip -> domain
    Cname - canonical name
    SOA - start of authority
    NS - name server
Domains and subdomains
DNS records for email
    MX - mail exchange
    TXT - text files
        SPF record
nslookup
whois
Email Networking
    MTA
    SMTP
    POP3
    IMAP
Email Security Issues
```

## Overview
- Validate DNS record using nslookup
- Email - Processes, protocols and headers 
- Analyze email header
- email Security Issues

## DNS server
|_ maps IP to URL     and also URL to IP

## DNS zone file
    |_ actual file containing all the DNS records for a particular domain
    |_ lives in DNS server
    |_ contains a Time to Live (TTL) indicating how long a DNS cache before fetching latest copy from server
    
## DNS records
1. A record
|_ text file, having domain -> IP address
2. PTR record (pointer)
    |_ same as whois
    |_ text file, having IP address -> domain
3. Cname record (canonical name)
    |_ dns record for only one domain e.g. domain1,
    |_ onther dns records are maintained using aliases
    |_ maintain alias -> domain1 -> domain2
4. SOA record (start of authority)
    |_ certain admin details, (contact email, TTL, domain last updated, etc.)
5. NS record (name server)
    |_ Indicates which server contains actual DNS records for a domain
    |_ display multiple dns records

## Domains and subdomains
1. domain
    |_ A domain can have multiple NS records for redundancy (if one fails, another is available).
    |_ For example:
    widgets.com has two name servers containing its DNS records:
    ? ns1.dnscompany.com
    ? ns2.dnscompany.com
    if ns1 fails, it fails over ns2

2. Subdomain
    |_ NS records can also be used to point subdomains to name servers.
    |_ drives.google.com  is a subdomain of google.com

## DNS records for emails
1. MX record 
    |_ Mail exchagne
    |_ Uses separate mail server to send and receive emails
    |_ domains can have multiple MX records for redundancy, in case one goes down or can�t handle a large amount of traffic.
    |_ sets priority to set mail server preference, lower #, higher preference

2. TXT record
    |_ Text records
    |_ human-readable notes, such as associated company name.
`   |_ also have some computer-readable data
    |_ e.g. SPF 

### SPF record (sender policy framework)

    |_ determines trusted email server.
    |_ mail servers that can send emails on behalf of a domain
    |_ domains might use 3rd party to send emails, e.g. office 365, outlook, gmail, etc.
    |_ main purpose is to prevent spam, phishing, and email spoofing, by detecting emails that may have a forged sender email.
    
    v=spf1 ip4:192.41.100.193
    spf version     Mail server IP
            ip version 
    
    |_ steps performed by spf:
        |_ 1. verify sender IP address
        |_ 2. validate DNS record - SPF to confirm that the sending mail server's IP address
        |_ 3. spf pass: send to inbox
              spf fail: send to spam folder

### nslookup

    |_ command to explore dns record of any domain
    |_ e.g. If emails aren�t reaching their final destination
            use nslookup -type=MX google.com

### DEMO

nslookup is mostly available on all OS (linux, windows, MAC)
```
nslookup gadget.com

nslookup -type=NS gadget.com

nslookup -type=MX gadget.com

whois

nslookup -type=SOA gadget.con
```


### ACTIVITY - 03_DNS_Record_Types

1. Acme Corp owns the following domains:
splunk.com
fireeye.com
nmap.org
For each website, determine the following DNS records and document your findings:
A record
NS record
MX record

nslookup -type=A splunk.com
```
Name:	splunk.com
Address: 52.5.196.118
```

nslookup -type=NS splunk.com
nslookup -type=MX splunk.com
nslookup -type=A fireeye.com
```
Non-authoritative answer:
Name:	fireeye.com
Address: 162.159.246.125
```


nslookup -type=NS fireeye.com
nslookup -type=MX fireeye.com

nslookup -type=A nmap.com
```
Non-authoritative answer:
Name:	nmap.com
Address: 45.33.49.119




```
nslookup -type=NS nmap.com
nslookup -type=MX nmap.com

Answer the following questions:
a. Did any of the domains have more than one MX record?  If so, why do you think that is?

b. For nmap.org, list the mail servers, from the highest to lowest priority.
```
$ nslookup -type=MX nmap.org
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
nmap.org	mail exchanger = 5 ALT2.ASPMX.L.GOOGLE.COM.
nmap.org	mail exchanger = 5 ALT1.ASPMX.L.GOOGLE.COM.
nmap.org	mail exchanger = 10 ASPMX3.GOOGLEMAIL.COM.
nmap.org	mail exchanger = 10 ASPMX2.GOOGLEMAIL.COM.
nmap.org	mail exchanger = 1 ASPMX.L.GOOGLE.COM.


1 ASPMX.L.GOOGLE.COM
5 ALT2.ASPMX.L.GOOGLE.COM.
5 ALT1.ASPMX.L.GOOGLE.COM.
10 ASPMX3.GOOGLEMAIL.COM.
10 ASPMX2.GOOGLEMAIL.COM.

```


Bonus

Look up the SPF record for nmap.org and explain what it indicates.

## Email Networking
```
           forward                   SMTP -> port 25
send email -------> sender mail server ------------> reciver mail server -------> mailbox
                        MTA                             POP3/IMAP            - 
````
### MTA
    |_ Mail transfer Agent
    |_ use DNS lookup to determine receiver mail server

### SMTP 
    |_ Simple Mail Transfer Protocol
    |_ Layer 7: APllication layer protocol ->  port 25

### POP3 (Post Office Protocol)
    |_ Layer 7: Application layer -> port 110
    |_ Once download, remove email - do NOT retain copy
    |_ Not able to download second time
    |_ security benefit: email not exist on a server the recipient doesn�t control.

### IMAP (Internet Message Access Protocol)
    |_ Layer 7: APplication layer -> port 143
    |_ Keeps a copy after download
    |_ can be downloaded multiple times
    |_ benefit: preventing data lost, as it backs up emails on a server

## Email header
From: (R)
To: (R)
Subject (O)
message body (O)
Date/Time

Gmail email ---> : ---> SHow Original
Delivered-To     ----> To
Return-Path:     ----> From 
Received: list of mail servers, src -> dest
Received-SPF: 
fields with x: (O) ext of standard header, e.g. x-mailer, x-Google-Smtp-Source.
    |_ Custom fields, for tracking, reporting, and authentication

### ACTIVITY - 06_Email_Networking/ 09_Email_Security

Email 1:
Delivered-To: juliejones@acme.com
Return-Path:  <jonathanthomas@microsoft.com>
IP address of source domain: 40.76.4.15
Message-ID: 1689837351.2998569.1568044304435@mail.microsoft.com

Received-SPF: pass (google.com: domain of jonathanthomas@microsoft.com designates 40.76.4.15 as permitted sender) client-ip=40.76.4.15


Email 2:
Delivered-To: juliejones@acme.com
Return-Path: xzvvvret34344@yahoo.com
IP address of source domain: 74.6.130.41
Message-ID: 1689837351.2998569.1568044304435@mail.yahoo.com
SUSPICIOUS: sender email address 

Received-SPF: pass (google.com: domain of xzvvvret34344@yahoo.com designates 74.6.130.41 as permitted sender) client-ip=74.6.130.41;

Email 3:
Delivered-To: juliejones@acme.com
Return-Path: timmytom@widgets.com
IP address of source domain: 34.86.130.49
Message-ID: 1gytrdd9837351.987987abs9.1568044304435@mail.widgets.com
SUSPICIOUS: spf failed 
ask for wire transfer

Received-SPF: fail (google.com: domain of timmytom@widgets.com does not designate 34.86.130.49 as permitted sender) client-ip=34.86.130.49 ;

Recommendation to prevent future emails

Email 4:
Delivered-To: juliejones@acme.com
Return-Path: return@irs.org
IP address of source domain: 64.71.74.115
Message-ID: 404021289698796556000_11022028878758757117@jdq.hentersss.com
SUSPICIOUS: spf failed 
ask for bitcoins

Received-SPF: fail (google.com: domain of return@irs.org does not designate 64.71.74.115 as permitted sender) client-ip=64.71.74.115;

Recommendation to prevent future emails

Email 5:
Delivered-To: juliejones@acme.com
Return-Path: billybob@companyA.com
IP address of source domain: 209.85.210.51
Message-ID: CACLG$52234Ygm1FNJZWXQc=dR-cv-jw+KRbRPf455BU6E-Xp_xDEA@mail.gmail.com

Received-SPF: pass (domain of companyA.com designates 209.85.210.51 as permitted sender)


ACTIVITY - Received-SPF: pass (domain of companyA.com designates 209.85.210.51 as permitted sender)
X-Originating-IP: [209.85.210.51]

## Email Security Issues
### 1. Spam
    |_ 60% of mails are spam
    |_ not dangerous, just hard to organize
    |_ SPF is use to identify, matching lists of known spam senders, and keyword identification.

### 2. Unencrypted Channels
    |_ mostly emails are encrypted
    |_ if intercept and viewd by unauthorized users.
    |_ encryption/decryption requires cordination b/w sender nd receiver
    |_ encrypt using:
        |_ PGP (Pretty Good Privacy)
        |_ S/MIME (Secure/Multipurpose Internet Mail Extensions)

### 3. Email Spoofing/Phishing
    |_ when an email is designed to trick the receiver into believing it is coming from a trusted source
    |_ pretend to be some bank OR
    |_ ask for credentials
    |_ link to fake website
    |_ spoofing detection methods
        |_ 1. From Email Header
            |_ raw email in From or Return-Path
            |_ random username (34839abc@yahoo.com)
            |_ simlar to a valid domain (cittibank instead of citibank) 

        |_ 2. Received-SPF Email Header
            |_ SPF record - to identify mail servers - authorized to send emails on domain's behalf.
            Received-SPF: pass (Accept)
            Received-SPF: fail  (reject)

        |_ 3. Received Email Header
            |_ received field has sender IP address
            |_ use arin.net to get details about sender IP address
            |_ arin.net shows the location of the IP

### ACTIVITY - 09_Email_Security 
DONE above
