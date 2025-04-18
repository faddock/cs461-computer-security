# Agenda
- wreshark
- first look at MP network and traffic
- dig and curl
- port scannin
- tcp SYN scan
- nmap and scapy

network Interface card - NIC

for this you using interface called switch
switch will have all the packets captured 

## Wireshark
A tool for passively capturing and viewing packets on a host network interface
- type `wireshark` in terminal to open
- ip.addr - 
- tcp.port == 80 - gives all packets that use posrt 80
- udp.port == 53 - helpful in checkpoint 2 - you want to observe teh TCP packets
> you can sniff your labmates packets if you use a shared computer / terminal

### dig
```shell
dig www.bankofbailey.com

```
- Domain Information Groper - Linux command
dig is the defacto tool to perform DNS lookups, which means finding the IP addresses associated with domain names. It can also be used to find other types of DNS records, like mail servers, nameservers, and more.

dns record names - A 
AAAA = IPv6
default record created is A - 

