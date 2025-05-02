# Agenda
- Wireshark
- Initial exploration of MP network and traffic
- `dig` and `curl` commands
- Port scanning
- TCP SYN scan
- `nmap` and `scapy`

## Network Interface Card (NIC)
A NIC is used to connect a device to a network. For this task, you will use an interface called a switch, which captures all packets passing through it.

---

## Wireshark
Wireshark is a tool for passively capturing and analyzing packets on a host network interface.

### Key Features:
- Launch Wireshark by typing `wireshark` in the terminal.
- Useful filters:
    - `ip.addr` - Filters packets by IP address.
    - `tcp.port == 80` - Displays all packets using port 80 (HTTP traffic).
    - `udp.port == 53` - Useful for observing DNS traffic (helpful in Checkpoint 2).

> **Note:** On shared computers or terminals, you can potentially sniff packets from other users on the same network.

---

## `dig` Command
The `dig` (Domain Information Groper) command is a Linux utility for performing DNS lookups. It is the go-to tool for querying DNS records, such as IP addresses, mail servers, and nameservers.

### Example:
```shell
dig www.bankofbailey.com
```

### Common DNS Record Types:
- **A**: Maps a domain to an IPv4 address.
- **AAAA**: Maps a domain to an IPv6 address.
- By default, an A record is created for a domain.

---
## Scapy

Scapy is a powerful Python-based interactive packet manipulation tool and library. It is used for crafting, sending, receiving, and analyzing network packets.

### Example:
```python
from scapy.all import *

# Create and send a simple ICMP packet
packet = IP(dst="8.8.8.8")/ICMP()
send(packet)
```
### Common Scapy Commands

#### Sending Packets
- `send(packet)`: Sends a packet at layer 3 (IP layer).
- `sendp(packet)`: Sends a packet at layer 2 (Ethernet layer).

```python
from scapy.all import *

# in terminal bash
netstat -rn | grep default

# Send an ICMP packet
send(IP(dst="8.8.8.8")/ICMP())

# Sniff packets
sniff(count=10).summary()
sniff(filter="tcp", count=5).summary()

# Display packet details
packet = IP(dst="8.8.8.8")/TCP(dport=80)
packet.show()

# Craft and modify packets
packet = Ether()/IP(dst="8.8.8.8")/TCP(dport=80)
packet[IP].src, packet[IP].ttl = "192.168.1.1", 64
packet.show()

# Save and load packets
wrpcap("packets.pcap", sniff(count=5))
rdpcap("packets.pcap").summary()

# Perform ARP scanning
answered, _ = srp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst="192.168.1.0/24"), timeout=2)
answered.summary()

# Send a DNS query
sr1(IP(dst="8.8.8.8")/UDP()/DNS(rd=1, qd=DNSQR(qname="example.com"))).show()
```

## Spoofing Attack via Scapy

A spoofing attack involves crafting packets with a forged source IP address to impersonate another device. Scapy can be used to simulate such attacks for educational and testing purposes.

### Example: IP Spoofing
```python
from scapy.all import *

# Craft a packet with a spoofed source IP
packet = IP(src="192.168.1.100", dst="8.8.8.8")/ICMP()

# Send the spoofed packet
send(packet)
```

### Example: ARP Spoofing
```python
from scapy.all import *

# Craft an ARP reply to poison the ARP cache
arp_response = ARP(op=2, pdst="192.168.1.1", hwdst="ff:ff:ff:ff:ff:ff", psrc="192.168.1.254")

# Send the ARP reply
send(arp_response, loop=1, inter=2)
```

> **Warning:** These examples are for educational purposes only. Unauthorized use of spoofing techniques is illegal and unethical.


# CIDR - Classless intern-domain routing
`10.4.22.0/24` This is called CIDR notation — short for Classless Inter-Domain Routing
It means the **first 24 bits** of the IP address are the **network part**. The **remaining 8 bits are for host addresses**.
it basically acts as a subnet mask - 255.255.255.0

IPv4 - 32 bits - 4 bytes - a.b.c.d - where a, b, c, d range from 0-255 in hex or 192.168.1.5 → 11000000.10101000.00000001.00000101 cuz 2^8 = 256
IPv6- 128 bits - 16 bytes - a.b.c.d.e.f.g - were they range from 0-65,025 (2 bytes each)
**subnet** - A smaller logical group of IP addresses within a larger network
its like a neighbour hood (subnet) inside a city(larger network)
All houses (IP addresses) share a common network prefix of (neighborhood)
£
Why are subnets useful?
	•	Split big networks into smaller, manageable chunks
	•	Improve security (e.g., guest Wi-Fi vs employee LAN)
	•	Reduce broadcast traffic
	•	Allow IP address reuse in different private networks


The pipe operator (|) was introduced by Douglas McIlroy (at Bell Labs) in the early 1970s.He described the concept in 1964 and it was implemented in the Unix shell by Ken Thompson in 1973.
	•	The idea: Connect programs like plumbing pipes, letting the “flow” of data go from one to the next.

“We should have some ways of connecting programs like garden hose–screw in another segment when it becomes necessary to massage data in another way.”
— Douglas McIlroy (the inventor of Unix pipes)

grep stands for “Global Regular Expression Print”.
	•	It was written by Ken Thompson at Bell Labs, for the original Unix operating system.
he name comes from the ed (Unix text editor) command: `g/re/p`
    •	g = global
	•	/re/ = regular expression
	•	p = print


✅ 1. Network Address
	•	Definition: The first address in a subnet.
	•	All host bits are 0.
	•	Used to identify the network itself.

     Broadcast Address
	•	Definition: The last address in a subnet.
	•	All host bits are 1.
	•	Used to send messages to all devices in the subnet.

    	All addresses between network and broadcast
	•	Usable range for devices (e.g., computers, printers, phones)

Example for 192.168.1.0/24:
	•	Usable: 192.168.1.1 through 192.168.1.254 there are 254 usable addresses

Company net


# CP2 - Discussion
what is an infinite header in packet - sendp() works for these infinite frames
![Arp spoofing demo](image-26.png)
basically a MiTM attaack - 
potocol designs have checks - they know 
TCP interception - reqrite - packets should still be valid at each layer - there will be fields that you need to modify - in each layer - https, TCP,
TCP - modify both sequence number and acknowledge number
revision:
- req num and ack num offer reliable delivery 
- whenever one byte is transferred after the other, tcp ensures that the previous byte is sent after the first bye
three way handshake - syn ( seq - 0, ack - na), synack (seq - 9000, ack - 1), ack(seq - 1, ack - 9001), http req 135 bytes (seq:1, Ack: 9001)
in TCP - 1 byte correspondings to 1 sequence number - this 1-to-1 alignment. ack gives a plus 1 to the seq, so basically 1 to 135 is already taken 
ack is allowed to piggy back off an http request - but they can also be separate packets. since there is no


bit flip - drop - in MiTM - server detects that there was a bit flip - and it drops 
so attacker when thye do script injection in http request - they need ot change the ack number - there will be a nice fin ack at the end
you need to do http injection - to get 4 credits - that was the easiest test case

other http interception cases:
- what if http response data exceeds on tcp repacket
what if there are multiple requests in a single connection? if there are multiplee, you need to consider multiple requests ack updates in a single connection
- what if injection occurs on packet segmentation boundary? 
	- either move some of the packets int a new packet to create space
http interception clarification:
	you need to perform the attack by performing MitM of a **single** connection between the client and server. **not acceptable**: attacker spoof as a server, 
	attacker actually does arp spoofing - its quite easy to do - but its incomplete - but server gets info about who the attacker is and their IP. its like connection between client and attacker and attacker and server. rather, do it in a single connection bia http interception.


Mitnick Xmas Day attack
1994 - san diego supercomptuer center attack on christmas

rhs server-  osiris - rimmons is an entre server - osiris only trusts rimmons - the goal is to log int oosiris - you want ot spoof a connection to osiris acting as rimmons as an attacker. at the time, solaris OS - same as osiris - has a predictable intial seq number - that enables that you as an attacker - guess the ack and gain the connection - 

step 1 - SYN to find ISN state
step 2 - spoof IP as rimmons

dont do a DoS rimmons - 

captuire on the shumomura interface 
demo

inductive way - do TCP SYNs probe - and observe the pattern - you see consistently

### ISN Pattern

The ISN (Initial Sequence Number) pattern refers to the predictable sequence of numbers used by older operating systems (like Solaris) to initialize TCP connections. In the context of the Mitnick Xmas Day attack, the attacker exploits this predictability to guess the sequence number and spoof a connection.

#### Steps to Identify the ISN Pattern:
1. **Observe ISN Generation**: Monitor multiple TCP connections to the target system and record the sequence numbers.
2. **Analyze the Pattern**: Look for a predictable increment or algorithm in the sequence numbers.
3. **Exploit the Pattern**: Use the identified pattern to craft packets with the correct sequence numbers, enabling the attacker to impersonate a trusted host.

> **Note:** Modern systems use randomized ISN generation to mitigate this vulnerability.


