# Lecture 24 - Denial of Service (DoS)

Denial of Service (DoS) is an attack against **availability**. Examples include:
- **BGP attack**
- **Physical disruption of infrastructure**
- **Exploiting weaknesses in software** to induce crashes
- **Resource depletion** (the most common type - overloads the target system)

## Types of Resource Depletion

Local Resource Depletion
- **Fork bomb**: `while(1) { fork(); }`
- **Filling disks**: Consuming all available disk space.

Remote Resource Depletion
- **TCP SYN flood attack**
- **Bandwidth saturation**: Overloading the network bandwidth.

## TCP SYN Flood Attack
- **TCP** is a stateful protocol that uses a **3-way handshake** for connection establishment.
- When a server receives a SYN packet from a client, it allocates a **Transmission Control Block (TCB)** to track the connection.
- Operating systems limit the number of TCBs available.
- An attacker sends many SYN packets, causing the server to exhaust its TCBs, preventing legitimate clients from establishing connections.

### Classes of SYN Flood Attacks
1. **Direct Attack**
    - Attacker - single machine - single real IP address.
    - Not a major concern since the server can block the IP address.
    - However, if a single IP sends too many SYN packets:
      - The IP may represent multiple users or processors.
      - Allowing a limited number of connections (e.g., 2 dozen) is reasonable, but excessive connections are suspicious and can be blocked.

2. **Spoofed Attack**
    - Attacker - single machine - many fake IP addresses.
    - This is the primary focus of mitigation efforts.

3. **Distributed Attack (DDoS)**
    - Attacker - many machines (e.g., via a botnet) with real IP addresses.
    - This is a form of **Distributed Denial of Service (DDoS)** and is very difficult to mitigate.
    ## SYN Flood Defense

    ### Challenges with Spoofed Attacks
    - In spoofed attacks, the attacker does not receive the SYN-ACK response from the server.
    - To mitigate this, the server can delay allocating a **Transmission Control Block (TCB)** until it receives the client's ACK.
        - The server must verify if `client ACK == server sequence number + 1`.
        - This approach reduces memory usage since a sequence number consumes less memory than a TCB.
    - However, can this verification be done without using server-side memory?

    ### TCP SYN Cookies
    - **SYN cookies** are a defense mechanism that avoids allocating server-side memory prematurely.
    - The server uses a **pseudorandom sequence number** generated as:
        - `Hash(key || client IP & port || server IP & port)`
        - This makes guessing the sequence number highly improbable (probability: 1 in 2³²).
    - Modern operating systems implement SYN cookies. In practice, the sequence number includes:
        - A 5-bit (slow) timestamp.
        - A 3-bit maximum segment size (MSS).
        - A 24-bit truncated hash.

    ---

    ## Reflection Attacks with Amplifiers

    ### Attack Mechanism
    - The attacker generates requests to public servers while spoofing the victim's IP address.
    - These servers reply to the victim, overwhelming their system with traffic.
    - Reflection attacks often exploit **amplification effects**, where small requests result in disproportionately large replies.
    - Commonly used protocols for such attacks include **UDP** and **ICMP**.

    ### Example: NTP DoS Attack
    - **Network Time Protocol (NTP)** is used for synchronizing system clocks and is widely deployed on servers, workstations, and network devices.
    - Normal usage involves:
        - Small queries and responses.
        - One exchange every 1 to 12 minutes.
    - However, older NTP servers supported the **"monlist"** diagnostic command, which was highly vulnerable:
        - A single request (~100 bytes) could trigger a reply of ~40 packets (~20 KB), resulting in a **200x amplification**.
        - Some servers exhibited amplification factors as high as **60,000,000x**.

    ---

    ## Case Study: Google Cloud DDoS Attack (2023)
    - In 2023, Google Cloud reported a significant Distributed Denial of Service (DDoS) attack.
    - This attack serves as a reminder of the evolving sophistication and scale of modern DDoS threats.
    - Effective mitigation strategies require a combination of:
        - Advanced detection mechanisms.
        - Scalable infrastructure.
        - Collaboration with upstream providers to filter malicious traffic.
