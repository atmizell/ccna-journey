# Module 11: IPv4 Addressing

**Relevant JITL Videos**: 
- Days 7/8 - IPv4 Addressing
- Days 13/14/15 - Subnetting
- Day 24 - Dynamic Routing

- 11.1 IPv4 Address Structure
    - IP addresses = 32 bits, 4x octets; Network + Host Portions
        - Bits in Network are identical for all devices on same network
        - Bits in Host are unique to identify the specific device
        - **Subnet Mask** utilized to identify Network v. Host portions 
        (i.e. 255.255.255.0)
        - *Note*: D**efault Gateway** IPv4 address required to reach remote networks; **DNS server** IPv4 address are required to translate domain names to IP
    - **Subnet Mask**:
        - Ex. 255.255.255.0 → 11111111  11111111  11111111  |  00000000
        To ID Net / Host portions, compare subnet mask bit-for-bit to IPv4 address from left to right
        - **ANDing Process** = Subnet Mask does not actually contain the Net v Host portions, it just tells the computer where to look for the parts of the IPv4 address
    - **Prefix Length**: The number of bits set to 1 in the subnet mask, written in slash notation (i.e., /1 to /32)
        - Subnet Mask  |  32-bit Address  | Prefix Length
        255.0.0.0          |  11111111.00000000.00000000.00000000  |  /8
        255.255.0.0      |  11111111.11111111.00000000.00000000        | /16
        255.255.255.0  |  11111111.11111111.11111111.00000000             | /24
        - Mask-to-Length (Last Octet): 128 (25), 192 (26), 224 (27), 240 (28), 248 (29), 252 (30), 254 (31), 255 (32)
        - Note: A network address is also referred to as a prefix or network prefix; therefore, prefix length = number of 1 bits in the subnet mask
    - Determining the Network - Logical AND (1 AND 1 = 1, else 0)
        - To ID the network address of an IPv4 host, the IPv4 address is logically ANDed, bit by bit, with the subnet mask.
    - Network, Host, and Broadcast Addresses:
        - Network = first address in range, represents network
            - Device belongs to network if: same subnet mask as network address, same network bits, located in the same broadcast domain as other hosts with the same network address
        - Broadcast = last address in range, used to reach all devices on the IPv4 network; has all 1 bits in the host portion
        - Usable Addresses = all other between first and last (i.e., 2^n - 2)
- 11.2 IPv4 Unicast, Broadcast, Multicast
    - **Unicast** - one-on-one communication (all comms unicast in this course unless indicated otherwise); Range: 1.0.0.1 - 223.255.255.255
    - **Broadcast** - comms with all network devices; destination IP with all 1s in the host portion (IPv4 has broadcast packets; no broadcast packets in IPv6)
        - Routers do not forward broadcasts (subdivide networks to improve performance); every host on network processes the broadcast packets - therefore, broadcasts should be limited
        - Device can send an **IP Directed Broadcast** to a remote network, then the router will broadcast it to the network; Cisco defaults to turning off this ability (no ip directed-broadcasts)
    - Multicast - send to select hosts; range: 224.0.0.0 - 239.255.255.255; routing protocols such as OSPF use multicast transmissions (OSPF multicast 224.0.0.5)
- 11.3 Types of IPv4 Addresses
    - **Public Address**: globally routed between ISP routers
    - **Private Address**: Used to assign IPv4 addresses to internal hosts; defined in **RFC 1918** (Private Address = “RFC 1918 Address Space”)
        - Network Address + Prefix  |  RFC 1918 Private Address Range
        10.0.0.0/8                              |  10.0.0.0 - 10.255.255.255
        172.16.0.0/12                         | 172.16.0.0 - 172.31.255.255
        192.168.0.0/16                       | 192.168.0.0 - 192.168.255.255
    - Routing to the Internet
        - Private IP packets routed across networks need to be filtered (discarded) or translated to public IP addresses
        - **Network Address Translation (NAT)**: translates b/w private and public IPv4 addresses
        - **Demilitarized Zone (DMZ)**: Devices in an organizations network that have resources available to the internet, or have public IP addresses
        - IETF does not consider Private Addresses or NAT as effective security measures
    - **Special Use IPv4 Addresses**
        - **Loopback Addresses** (127.0.0.0/8 or 127.0.0.1 - 127.255.255.254): used by a host to direct traffic to itself; used to test if the TCP/IP configuration operational
        - **Link-Local Addresses** (169.254.0.0/16 or 169.254.0.1 - 169.254.255.254): known as **Automatic Private IP Addressing (APIPA)** addresses, or **Self-Assigned** addresses; used by a Windows DHCP client to self-configure in the event there are no DHCP servers available; can be used in a peer-to-peer connection (uncommon)
    - **Legacy Classful Addressing** (RFC 790 - Assigned Numbers): Customers assigned network address based on three classes (A,B,C)
        - **Class A** (0.0.0.0/8 - 127.0.0.0/8): Designed to support extremely large networks with more than 16 million host addresses; accounted for 50% of the IPv4 networks
        - **Class B** (128.0.0.0/16 - 191.255.0.0/16): Designed to support moderate networks, with ~65,000 host addresses
        - **Class C** (192.0.0.0/24 - 223.255.255.0/24): Designed to support small networks with a max of 254 hosts
        - **Class D** (224.0.0.0 - 239.0.0.0) - Multicast Block
        - **Class E** (240.0.0.0 - 255.0.0.0) - Experimental Address Block
        - Note: in mid-1990s, introduction of WWW, classful addressing replaced with Classless Addressing
    - Assignment of IP Addresses: managed by the Internet Assigned Numbers Authority (IANA), allocates blocks of IP to the Regional Internet Registries (RIR):
        - African Network Information Centre (AfrNIC)
        - Asia Pacific Network Information Centre (APNIC)
        - American Registry for Internet Numbers (ARIN)
        - Regional Latin-American and Caribbean IP Address Registry (LACNIC)
        - Reseaux IP Europeens Network Coordination Centre (RIPE NCC)
    - Quiz Notes:
        - *Correct about Private IPv4 Addresses*: Private addresses are assigned to devices within an organizations intranet (internal network); any organization can use the 10.0.0.0/8 address
        - *Correct about Public IPv4 Addresses*: to access a device over the internet, the destination IPv4 address must be public; public IPv4 address exhaustion is a reason why there are private IPv4 addresses and why organizations are transitioning to IPv6
        - *Which organization receives IP addresses from IANA and allocated them*: RIRs
- 11.4 Network Segmentation
    - Broadcast Domains and Segmentation
        - In an Ethernet domain, devices use broadcast and ARP to locate other devices, or by using services (Dynamic Host Configuration Protocol)
        - Switches propagate broadcasts out of all interfaces except receiver
        - Routers do not propagate broadcasts; therefore, each router interface connects to a broadcast domain
        - Large Broadcast Domains can generate excessive broadcasts, resulting in slower network operations
        - **Subnetting** creates smaller broadcast domains by creating smaller network spaces → Use Host Bits to create additional subnets (moving from /16 to /24); subnets use longer prefix lengths to ID networks
            - Available addresses: 2^n - 2, where n = # Host Bits
    - *Reasons for Segmenting Networks*: reduce network traffic, improve performance; allow admin to implement security policies, reduce abnormal broadcast traffic; can also segment based on location, group/function, or device type
- 11.5 Subnet an IPv4 Network
    - IPv4 subnets created by using host bits as network bits (extending the subnet mask, reduces the number of hosts per network)
    - Subnet on an Octet Boundary: easiest to use the octet boundaries of 8, 16, 24
    - Subnet within an Octet Boundary: any octet not on the boundaries (i.e. 25)
    - Subnet with Magic Number:
        - Moving from Subnet 24 by borrowing host bits:
        24 → 25, 7x 0s = 2^7-2 = 126 hosts possible (2 nets)
        24 → 26 = 6 host bits = 2^6 - 2 = 62 hosts possible (4 nets)
        24 → 27 = 5 host = 2^5 - 2 = 30 hosts possible (8 nets)
        - On a LAN, we do not go higher than a /30 subnet (not enough hosts)
        - Magic Number = Place value of last 1 (Left to Right), tells us how to find our Networks; Examples:
        10000000 → Magic Number = 128 (/25)
        11000000 → Magic Number = 64 (/26)
        11110000 → Magic Number = 16 (/28)
        00000000 → Magic Number = 1 (/24)
        - 11111110 00000000 → Magic Number = 2 (/23)
- 11.6 Subnet a Slash 16 and a Slash 8 Prefix
    - To calculate the number of subnets created by borrowing host bits, start with the number of original network bits: 
    2^n, where n = # of host bits borrowed from left to right
    2^1 = 2 subnets
    2^7 = 128 subnets
    - Example problem: how to create 1000 subnets with a Slash 8 prefix:
    10.0.0.0/8 = nnnnnnnn.hhhhhhhh.hhhhhhhh.hhhhhhhh
    Need to borrow 10 Host Bits: 2^10 = 1024
    Resulting Subnet Mask: 255.255.192.0 or 10.0.0.0/18
- 11.7 Subnet to Meet Requirements
    - Private v Public IPv4 Address Space
        - Intranet - internal network, accessible within organization (pvt)
            - Private: can subnet at octets 8, 16, 24
        - DMZ - network containing internet available resources (pub)
            - Public: due to address exhaustion, must use **Variable Subnet Length Masking (VSLM)** to maximize available addresses
    - Minimize Unused Host IPv4 Addresses and Maximize Subnets (VLSM)
        - Consider: Number of Hosts Required, Number of Subnets needed
        - See Professor Messer method for quick 25 → 32 table reference, providing number of subnets and hosts
        - See VLSM below for detailed method, review of class notes
- 11.8 VLSM
    - IPv4 Address Conservation - making the most of available host addresses is a primary concern when subnetting IPv4 networks (driving factor for IPv6 change)
    - VLSM developed to avoid wasting addresses by enabling us to subnet a subnet
    - This is the method we learned in class: Given a network address and variable numbers of Hosts Required, calculate the respective subnets:
    **Host Req | INC | Net ID | 1st Avail | Last Avail | Broadcast | CSM | CIDR**
    # | X.X | x.x.x.x (IP) | x.x.x.x | x.x.x.x | x.x.x.x | (subnet mask) | (/xx prefix)
        - First Step: Always order the Host Req largest to smallest
        - Use the Host Req to find INC, use INC to write next Net ID (up to the “Ghost Net ID”
        - Broadcast = Next Net ID - 1
        - Last Avail = Broadcast - 1
        - First Avail = Net ID + 1
        - CSM = Subnet Mask based on # Host Bits
        - CIDR = Prefix Length based on Net/Host Bits (/1 → /32)
- 11.9 Structured Design
    - **IPv4 Network Address Planning** - develop an address scheme for entire network (that allows for growth) before subnetting; need to know: # subnets, # hosts per subnet, devices in each, public v private addresses
        - Perform a Network Requirement Study: analyze both the intranet and DMZ to determine how each will be segmented; where is address conservation needed (usually DMZ)
            - Where conservation required (DMZ), how many subnets/hosts needed?
            - Conservation likely not required in intranet (private)
    - **Device Address Assignment** - based on different devices
        - **End User Client**: most networks allocate addresses to clients dynamically (DHCP)
            - Reduces network staff burden, eliminates entry errors
            - In DHCP, addresses are leased for a period of time and can be reused when the lease expires (important for transient users and wireless devices)
            - Changing subnetting scheme requires DHCP reconfiguration, clients must renew IPv4 addresses (IPv6 clients obtain address info using DHCPv6 or SLAAC)
        - **Servers and Peripherals**: should have predictable static IP addresses, using consistent numbering scheme
        - **Servers accessible from the internet**: need public IPv4 addresses, often accessed using the NAT; may have private address internally, requiring users to create a **Virtual Private Network (VPN)**
        - **Intermediary Devices**: assigned addresses for network management, monitoring, and security (predictable, static)
        - **Gateway**: routers and firewall devices, IP address that serves as gateway for hosts; typically the lowest or highest address in the network
