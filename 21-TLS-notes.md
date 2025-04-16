# Transport

Understand basic concepts of TCP
- Explore how TCP stacks up against the network security threat models
- Evaluate defenses against TCP hijacking attacks

IP provides best-effort delivery between hosts
• TCP/UDP between processes (IP address, port)
• TCP (Transmission Control Protocol)
– In-order delivery, reliable delivery, …
• UDP (User Datagram Protocol)
– None of the above, security is similar to IP
• We will focus on TCP

Port 
Each process is identified by a port number. Some destination port numbers used for
specific applications by convention
80   | HTTP (Web)
443  | HTTPS (Web)
23   | SMTP (mail)
22   | SSH (secure shell)
514  | RSH (remote shell)

## TCP - Transmission Control Protocol
- Reliable, Ordered, Slower
- used in emails, web browsing, file transfer

TCP/IP was designed in the 1970s by Vint Cerf and Bob Kahn, funded by the U.S. Department of Defense’s DARPA. RFC 793 (September 1981) officially defined TCP — the protocol you’re asking about.

- A TCP segment = **header** + **Data**
    - Header: Control info
        contains source port, destination port, sequence number, acknolwedgement number, offset, reserved, flags, window size, checksum, urgent pointer, options, padding
    - Data: The actual payload (e.g., a chunk of a file or webpage)
- header - 
![TCP Header](image-15.png)

- Sender may send several segments before receiving acknowledgement
- Receiver always acknowledges with sequence number of next expected byte


1. TCP three-way handshake
```
1️⃣ Client ➝ Server: SYN, Seq = X
    → "I want to start a connection. My starting sequence number is X."

2️⃣ Server ➝ Client: SYN + ACK, Seq = Y, Ack = X + 1
    → "Okay! My sequence number is Y. I got your SYN."

3️⃣ Client ➝ Server: ACK, Seq = X + 1, Ack = Y + 1
    → "Got your SYN. Let’s go!"
```

2. CLient sends data 
```
Client ➝ Server:
  Seq = 1001, Data = "HELLO"         // Seq = 1001 → 1001 to 1005
Server ➝ Client:
  ACK = 1006                        // ACKs next expected byte

Client ➝ Server:
  Seq = 1006, Data = "WORLD"         // Seq = 1006 → 1006 to 1010
Server ➝ Client:
  ACK = 1011                        // All 10 bytes received

```
3. COnnection termination (4-way FIN)
```
Client ➝ Server: FIN, Seq = 1011
Server ➝ Client: ACK = 1012

Server ➝ Client: FIN, Seq = 3001
Client ➝ Server: ACK = 3002

```
•	Seq = X means “this segment starts with byte X”
•	ACK = Y means “I’ve received everything up to byte Y-1”


- What if the first packet is dropped in network?
    - Receiver always acknowledges with sequence number of next expected byte
    - Sender retransmits lost packet
    - Receiver acknowledges
- TCP Seq and Ack numbers
    - Initial sequence number is random
    -  32-bit sequence numbers (0 to 2³² - 1 = 0 to 4,294,967,295) that wrap around to 0 when you reach end
    - 
- 8 one-bit flags in TCP header
    SYN Synchronize: Initiates a connection and synchronizes sequence numbers  
    ACK Acknowledgment: Acknowledges received data (or a SYN)  
    FIN Finish: Gracefully closes a connection  
    RST  Reset: Abruptly terminates a connection due to error or invalid state  
    PSH Push: Tells the receiver to immediately pass data to the application layer  
    URG Urgent: Marks data as urgent (rarely used)  
    ECE ECN-Echo: Used for Explicit Congestion Notification (see below)  
    CWR Congestion: Window Reduced Acknowledges receipt of ECE (for congestion control)  
    
SYN
Connection initiation
SYN + ACK
Response to SYN (step 2 of 3-way)
ACK
All normal data packets
FIN + ACK
Graceful connection close
RST + ACK
Connection reset due to error


Security Properties - CIA an Authenticity
Availability Ensuring services are up and accessible  
Confidentiality Preventing unauthorized access to data  
Integrity Preventing tampering of data  
Authenticity Ensuring identities are genuine (not spoofed or impersonated)  



## UDP - User Datagram Protocol
- Unreliable, Unordered, Faster (used for video streaming, gaming)

