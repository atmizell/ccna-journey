# Module 13: ICMP

- Terms
    - Internet Control Message Protocol (ICMP)
- 13.1 ICMP Messages
    - TCP/IP suite provides for error messages, sent using ICMP - provide feedback about processing issues with IP packets (often not allowed in a network for security)
    - ICMP Message Types
        - **Host Reachability:** is a host reachable? Host A sends **ICMP Echo Request**, Host B responds with **ICMP Echo Reply** if available (basis of Ping)
        - **Destination or Service Unreachable**: if packet cannot deliver, host/gateway sends ICMP Destination Unreachable to notify sender; codes:
            - ICMPv4:
                - 0 - Net Unreachable
                - 1 - Host Unreachable
                - 2 - Protocol Unreachable
                - 4 - Port Unreachable
            - ICMPv6:
                - 0 - No route to destination
                - 1 - comms are admin prohibited (firewall)
                - 2 - beyond scope of source address
                - 3 - address unreachable
                - 4 - port unreachable
        - **Time Exceeded**: TTL (v4) or Hop Limit (v6) decremented to 0; Time Exceeded messages are used by the traceroute tool
    - ICMPv6 Messages
        - Includes four new protocols as part of Neighbor Discovery Protocol
        - Msg b/w IPv6 router-device: 
        Router Solicitation (RS)
        Router Advertisement (RA)
        - Msg b/w devices - Neighbor Discovery
        Neighbor Solicitation (NS, i.e. DAD)
        Neighbor Advertisement (NA)
        (Also includes the redirect message)
- 13.2 Ping and Traceroute Tests
    - Ping - Test Connectivity (ICMP Echo Request/Reply)
        - Provides time feedback, can measure net performance
        - Has a timeout value, could be error or security; common for first ping to timeout if ARP needs to be performed
        - Test Types (ping):
            - **local loopback**: test internal IP configuration
            Ping: 127.0.0.1 (IPv4), ::1 (IPv6)
            Response comes from Network Layer
            Does not indicate: address/mask/gateway configured; nor the status of the lower layer
            - **default gateway**: host comm w/ local network, indicate local host/router are operational
            If another host responds, but not router, likely router interface issue
            - **remote host**: comm across internetwork, verify network and remote host operations
            Note: many admin limit/prohibit ICMP entry to corporate network, so lack of ping may be security
    - Traceroute - Test the Path, generate list of hops (router interfaces)
        - Round Trip Time (RTT): time for response from remote host 
        (* = lost or unreliable packet); locate problem routers
        - IPv4 TTL / IPv6 Hop Limit: uses layer 3 header values and the ICMP Time Exceeded message; Traceroute increments the TTL to receive Time Exceeded msg from each router in chain to remote host (responds with either ICMP Port Unreachable or ICMP Echo Reply)
