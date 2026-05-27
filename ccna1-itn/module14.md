# Module 14: Transport Layer (TCP, UDP; Ports)

**Relevant JITL Video**: Day 30 - TCP & UDP

- Terms
    - Transmission Control Protocol (TCP)
    - User Datagram Protocol (UDP)
    - File Transfer Protocol (FTP)
    - Simple Mail Transfer Protocol (SMTP)
    - Domain Name System (DNS)
    - Dynamic Host Configuration Protocol (DHCP)
    - Trivial FTP (FTFTP)
    - Hypertext Transfer Protocol (HTTP), Secure (HTTPS)
    - Post Office Protocol v3 (POP3)
    - Internet Message Access Protocol (IMAP)
    - Simple Network Management Protocol (SNMP)
- 14.1 Transportation of Data
    - Layer Responsibilities
        - Tracking Individual Conversations
            - Conversation = sets of data flowing source-destination
        - Segmenting Data and Reassembling Segments
        - Add Header Information (Enable protocols)
        - Identifying the Applications (Port Numbers)
            - Each software process assigned unique host port #
        - Conversation Multiplexing (Interleaving convos on net)
    - Protocols (TCP, UDP): how to transfer msg, manage reliability
        - **TCP**: reliable, full-feature, connection-oriented, flow control; ensures data arrives, divides into segments
        Operations include:
            - Number and track segments to a specific host from a specific application
            - acknowledge received data
            - retrans unacknowledged data after set time
            - sequence data that may arrive in wrong order
            - send data at an efficient rate acceptable by receiver
        - **UDP**: does not provide reliability or flow control (simpler, connectionless, stateless, best-effort); reassembles in order received; uses datagrams
    - Choosing the Right Protocols:
        - TCP (reliability): databases, web browsers (HTTP), 
        email clients (SMTP/IMAP), stream stored audio/video
        - UDP (speed): VoIP, DNS, real-time video
- 14.2 TCP Overview
    - Stateful, reliable, acknowledges data, resends lost data, delivers data in sequence; use for email and web (segments)
        - Stateful = track state of communication session by tracking sent and acknowledged data
    - Features (RFC 793)
        - Basic = Data Segmentation + Reassembly
        - Establishes a Session: negotiate and establish a permanent connection (session) b/w devices prior to forwarding traffic
        - Ensures Reliable Delivery
        - Provides Same-Order Delivery
        - Supports Flow Control: regulate data sent
    - TCP Header (10 fields, 20 bytes)
        - **Source Port** (16bit): source application port number
        - **Destination Port** (16bit): destination application port number
        - **Sequence Number** (32bit): use for data reassembly
        - **Acknowledgment Number** (32bit): indicate receipt, next byte expected from source
        - **Header Length** (4bit): data offset, indicates length of TCP segment header
        - **Reserved** (6bit): for future use
        - **Control Bits** (6bit): bit codes (flags) indicating purpose and function
        - **Window Size** (16bit): number bytes that can be accepted at one time
        - **Checksum** (16bit): error checking of segment header and data
        - **Urgent** (16bit): indicate if urgent
    - Applications that use TCP: HTTP, FTP, SMTP, SSH
- 14.3 UDP Overview
    - Best-effort, stateless, connectionless; fast, low overhead, doesn’t require acknowledgments, do not resend lost data, delivers data in the order it arrives; use for VoIP and DNS (datagrams)
    - Features (lightweight, best-effort; no reliability or flow control)
        - Basic = data segmentation + reassembly (order received)
        - Any segments lost not resent
        - No session establishment
        - Sender not informed about resources available (connectionless)
    - UDP Header (4 fields, 8 bytes)
        - **Source Port** (16bit)
        - **Destination Port** (16bit)
        - **Length** (16bit): datagram header length
        - **Checksum** (16bit): error checking header/data
    - Applications:
        - Live Video and Multimedia (VoIP, video conferencing)
        - Simple request/reply apps (DNS, DHCP)
            - Note: DNS and DHCP can use TCP (DNS > 512 bytes)
        - Apps handling reliability (SNMP, TFTP)
            - Net admin can config SNMP to use TCP
- 14.4 **Port Numbers**
    - Used by TCP/UDP to manage multiple, simultaneous conversations (multiplexing); ports associated with applications
    - Socket Pairs: combo of source/destination IP Address and Port #
        - ex. 192.168.1.5:1099
        IP Address : Port Number
        - Enable multiple processes and connections to distinguish themselves from each other
    - Port Number Groups
        - Need to know Well Known (specific), and Port Ranges
        - IANA responsible for establishing standard port numbers
        - **Well-Known Ports** (0 - 1,023): reserved for common services (web, email, remote access)
            
            
            | Port # | Protocol | Application |
            | --- | --- | --- |
            | 20 | TCP | FTP - Data |
            | 21 | TCP | FTP - Control |
            | 22 | TCP | Secure Shell (SSH) |
            | 23 | TCP | Telnet |
            | 25 | TCP | SMTP |
            | 53 | UDP, TCP | DNS |
            | 67 | UDP | DHCP - Server |
            | 68 | UDP | DHCP - Client |
            | 69 | UDP | TFTP |
            | 80 | TCP | HTTP |
            | 110 | TCP | POP3 |
            | 143 | TCP | IMAP |
            | 161 | UDP | SNMP |
            | 443 | TCP | HTTPS |
        - **Registered Ports** (1,024 - **49,151**): assigned to requesting entity to use with specific processes or applications (ex. Cisco port 1812 for RADIUS server auth process)
        - **Private/Dynamic Ports** (49,152 - 65,535): ephemeral ports; client OS assigns dynamically when service connection initiated
            - Some client OS may use registered ports instead of dynamic for assigning source ports
    - The **netstat** Command: determine active TCP connections
        - Default: attempt to resolve IP addresses to domain names and port numbers to well-known applications
        - -n option used to display IP addresses and port numbers in numerical form
- 14.5 TCP Communication Process
    - TCP Server Processes
        - Servers cannot have multiple processes assigned to the same port number within the same transport layer service
            - Connection =/= Process, multiple conversations can happen at the same port simultaneously
            - Process can handle multi-connections by making new sockets for each
            - Server handles multiple connections by calling accept() and assigning each new client to a file descriptor
        - Active server application assigned to a specific port = OPEN (transport layer accepts+processes segments addressed)
    - TCP Connection Establishment - ***Three-Way Handshake***
        - Validates destination host available (SYN, ACK+SYN, ACK)
        1. **SYN**: client requests session with server
        2. **ACK and SYN**: server acknowledge, request comm session
        3. **ACK**: client acknowledges
    - Session Termination: FIN control flag, ***two way handshake*** (FIN-ACK), terminate single convo = four exchanges 
    (Client FIN, Server ACK, Server FIN, Client ACK)
    - TCP Three-way handshake analysis
        - TCP is full-duplex, each connection = two one-way communication sessions
        - Functions of 3-way handshake:
            - dest device present on net
            - verify dest has active service, intended dest port accepting requests
            - inform dest device of source client intent to establish comm session on port
        - Control Bit Field (Flags):
            - **SYN** - synchronize sequence numbers used in connection establishment
            - **FIN** - no more data from sender, session termination
            - **ACK** - acknowledgment flag (connection establishment, session termination)
            - **URG** - urgent pointer field significant
            - **PSH** - push function
            - **RST** - reset connection when error/timeout
- 14.6 Reliability and Flow Control
    - Sequencing
        - **Initial Sequence Number (ISN)**: set during session setup, starting value of bytes transmitted (random number for security)
        - **Sequence Number** (TCP) incremented by number of bytes transmitted, enable reassembly
    - Data Loss and Retrans
        - Use sequence (SEQ) number and acknowledgement (ACK) number to confirm receipt of data
        - **Expectational Acknowledgment**: Receiver sends ACK number to indicate next byte expected
        - **Selective Acknowledgment (SACK)**: negotiated during 3-way; receiver can explicitly acknowledge segments received
        - TCP uses timers to know how long to wait before resending a segment
        - *Note*: TCP typically sends ACKs for every other packet
    - TCP Flow Control - Window Size and Acknowledgments
        - Flow Control - amount of data destination can receive and process reliably (uses Window Size header field)
        - **Window Size**
            - # bytes sent before expecting ACK (ACK is next # of expected bytes)
            - established during 3-way
            - “Sliding Windows”
                - window size included in every segment to allow for dynamic destination modification
                - destination does not need to wait for max bytes before sending ACK, allows for continual adjustment; receiver typically sends ACK after every other segment received
        - Maximum Segment Size (MSS)
            - Part of Options Field, specifies largest data amount (not including the TCP header); included in 3-way
            - 1460 bytes typically max receivable (IPv4)
                - Ethernet MTU (1500) - IPv4 Head (20) - TCP (20)
        - Congestion Avoidance
            - Source assumes congestion based on rate of unacknowledged TCP segments
            - If congestion, sender reduces number of bytes sent before receiving ACK from destination
            - If packet loss due to congestion, then retrans
            - TCP uses congestion handling *mechanisms, timers, algorithms*
- 14.7 UDP Communication
    - Connectionless, best-effort, unreliable
    - UDP Datagram Reassembly - simply reassembles data in the order it was received, then forwards to the application
    - UDP Server Processes and Requests: client randomly selects a source port number; UDP apps assigned well-known or registered ports (No 3-way handshake!)
    - UDP Client Processes: like TCP
  
