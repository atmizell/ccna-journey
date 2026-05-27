# Module 12: IPv6 Addressing

**Relevant JITL Videos**: 
> Days 13/14/15 - Subnetting
> 
> Day 24 - Dynamic Routing
> 
> Days 31/32/33 - IPv6

- **Terms**:
    - **SLAAC** = Stateless Address Autoconfiguration
    - **EUI** = Extended Unique Identifier
- 12.1 IPv4 Issues
    - Problems
        - Address exhaustion
        - ICMPv6 has address resolution and autoconfiguration not found in ICMPv4
    - IPv4 and IPv6 Coexistence - Migration Techniques
        - **Dual Stack** - allows coexistence on same network segment
            - known as **native IPv6**, the customer network has an IPv6 connection to their ISP and is able to access internet over IPv6
        - **Tunneling** - method of transporting an IPv6 packet over an IPv4 network (encapsulated inside an IPv4 packet)
        - **Translation** - Network Address Translation 64 (NAT64) allows IPv6-enabled devices to comm with IPv4-enabled devices, similar to NAT. Translates IPv4 and IPv6 packets between eachother.
        - *Note*: Tunneling and Translation are for transitioning to native IPv6 and should only be used where needed. Goal should be native IPv6 communications from source to destination.
- 12.2 IPv6 Address Representation
    - 128-bit length, 32 hexadecimal values (not case sensitive), 8x “hextets”
    - Preferred Format → x:x:x:x:x:x:x:x (each x is a hextet, 4x hex values)
    - Rules:
        - (1) Omit Leading Zeros (only applies to leading 0s in each hextet)
        - (2) Double Colon - replace the longest string of continuous zeros with a double colon (::)
- 12.3 IPv6 Address Types
    - Casting
        - **Unicast** - ID an interface on an IPv6-enabled device
            - Source IPv6 is always unicast
        - **Multicast** - send single packet to multiple destinations
        - **Anycast** - IPv6 unicast address that can be assigned to multiple devices; packet sent to anycast address is routed to the nearest device having that address
            - Subnet-Router Anycast Address: All 0s interface ID typically reserved for routers
        - *Note*: IPv6 does not have a broadcast address; however, there is an IPv6 all-nodes multicast address that gives the same result
    - **IPv6 Prefix Length** (Network Portion)
        - IPv4 - Identified by a dotted-decimal subnet mask or prefix length (slash notation)
        - IPv6 is only called the Prefix Length (slash notation, range 0 - 128); recommended prefix length in most networks is /64 (ex. 2001:db8:a::/24)
        - 64-bit interface recommended because **Stateless Address Autoconfiguration (SLAAC)** uses 64 bits for the Interface ID, makes subnetting easier to create and manage
    - **Unicast Types**:
        - **Global Unicast (GUA)**: globally unique, internet-routable (static or dynamic, like public IPv4 address)
            - Allocated by ICANN and IANA; Only GUAs with the first three bits of 001 or 2000::/3 are being assigned (Global Routing Prefix portion)
            - *Note*: 2001:db8::/32 reserved for documentation
            - Parts: recommend 64-bit prefix (GRP + Sub ID), total length of 128-bits
                - **Global Routing Prefix**: prefix/network portion, /48 common, determines size of subnet ID
                - **Subnet ID**: identify subnets (larger = more available); with /48 common GRP, this is 16 bits (makes typical 64-bit prefix length)
                - **Interface ID**: host portion
        - **Link-Local (LLA)**: required for every IPv6-enabled device; used to communicate with other devices on same local link (link = subnet).
            - Range: **fe80**::/10
            - Uniqueness only confirmed in link because they are not routable outside - routers will not forward packets with link-local source or destination addresses.
            - If not manually defined (static), device will create its own (dynamic) without communicating with a DHCP server (even without an assigned Global Unicast Address)
                - Dynamic uses the **Extended Unique Identifier (EUI)** method, which uses the client MAC address along with additional bits
            - Routers use LLA of neighbor routers to send routing updates
            - Hosts use the LLA of a local router as the default-gateway
        - Other Types:
            - **Loopback** (::1/128) - indicates that IP is properly installed
            - **Unspecified** (::)
            - **Embedded IPv4**
            - **Unique Local** (fc00::/7 - fdff::/7): not common, may be used to address devices that should not be accessible from outside, different from RFC 1918 private addresses for IPv4:
                - used for local addressing within a site(s)
                - used for devices that never need network access
                - not globally routed or translated to global IPv6
- 12.4 GUA and LLA Static Configuration
    - Static GUA Config on a Router - configuring interfaces
    IPv6 Address: ipv6 address *ipv6-address/prefix length*
    example: interface gigabitethernet 0/0/0
    ipv6 address 2001:db8:acad:1::/64
    no shutdown
    exit
    - Static GUA Config on a Windows Host:
        - Default gateway is the GUA of the connected router interface; alternatively, the default gateway can be configured to match the LLA of the respective router interface (best practice)
        - To support large environments, net admins enable dynamic assignment of IPv6 → SLAAC, Stateful DHCPv6 (LLA of router will be auto default gateway address)
    - Static Config of a Link-Local Unicast Address (LLA): typically only necessary on routers; beginning hextet b/w fe80 - febf
        - Cmd: ipv6 address *ipv6-link-local-address* link-local
        - Note: exact same LLA can be configured on each link, so long as it is unique on that link; best practice to create a different LLA on each router interface
- 12.5 Dynamic Addressing for IPv6 GUAs
    - RS and RA Messages
        - Devices obtain IPv6 GUAs dynamically using Router Advertisement (RA) and Router Solicitation (RS) messages
        - Routers send out Internet Control Message Protovol v6 (ICMPv6) RA messages, every 200 seconds or in response to a host RS message
        - Routers must have IPv6 routing enabled: ipv6 unicast-routing
        - ICMPv6 suggests how to obtain an IPv6 GUA; RA includes:
            - **Network Prefix + Prefix Length**: tells device network
            - **Default Gateway Address**: IPv6 LLA, source of RA
            - **DNS Addresses + Domain Name**
    - RA Message Methods:
        1. **SLAAC**: “I have everything - prefix, prefix length, default gateway address”
            1. Default, RA only, DHCPv6 server not required
            2. SLAAC is stateless = no central server allocating GUAs
            3. Created GUA: Prefix (in RA) + Interface ID (Uses EUI-64)
            4. EUI-64: generate random 64-bit number, depending on the device OS (see below)
        2. **SLAAC w/ Stateless DHCPv6 Server**: “Here is info, but you need to get DNS addresses from DHCPv6 server”
            1. RA msg suggests: SLAAC for IPv6 GUA; router LLA as default gateway; use stateless DHCPv6 for DNS Server Address and a Domain Name
            2. Stateless DHCPv6 server distributes DNS Server Addresses and Domain Names; doesn’t allocate GUAs
        3. **Stateful DHCPv6 (no SLAAC)**: “Here is default gateway; you need to ask stateful DHCPv6 server for other info”
            1. Stateful DHCPv6 similar to IPv4 DHCP; device receives GUA, prefix length, and DNS info from DHCP server
            2. RA msg suggests: router LLA, stateful DHCPv6 server
            3. Server allocates and maintains a list of IPv6 addresses
            4. *Note*: default gateway only obtained dynamically from RA
    - Extended Unique Identifier (EUI)-64 Process: uses 48-bit Ethernet MAC address of client, inserts 16-bits in the middle, to create interface ID
        - MAC = OUI + Device Identifier
        - EUI-64
            - 24-bit OUI from client MAC, reversing the 7th bit (Universally/Locally bit)
            - Inserted 16 bits (fffe) - *likely indicates EUI-64 creation*
            - 24-bit Device Identifier from client MAC
        - Users concerned that EUI address can allow for trace of physical device can opt to use the randomly generated interface ID instead
            - Note: to ensure uniqueness of any IPv6 unicast address, client may use Duplicate Address Detection (DAD) - similar to ARP Request (if no reply, address is unique)
- 12.6 Dynamic Addressing for IPv6 LLAs
    - LLA Creation = fe80::/10 Prefix + Interface ID (**EUI-64** / random)
    - Windows: typically same method as SLAAC
    - Dynamic LLAs on Cisco Routers
        - Auto created when GUA assigned to interface (default EUI-64)
        - Serial Ints - use MAC of Ethernet interface
        - Common to statically configure IPv6 LLAs on routers for ease of use
    - **Verify IPv6 Address Configuration**
        - Display IPv6 address of Ethernet interfaces:
        show ipv6 interface brief
        1st Address (fe80) = link-local unicast address (LLA)
        2nd Address = configured GUA
        - Verify IPv6 network - interface addresses installed:
        show ipv6 route
        C - directly connected (i.e. GUA)
        L - local route (i.e. GUA/128)
        LLAs not included because they are *not routable*
        - Verify layer 3 connectivity: ping
        When pinging LLA from a router, user will be prompted for an exit-interface, b/c destination can be on one or more links
- 12.7 IPv6 Multicast Addresses
    - Used to send to a multicast group; can only be destination addresses; prefix ff00::/8
    - Types:
        - **Well-known** multicast addresses (assigned)
            - ff02::1 All-Nodes multicast group: all IPv6-enabled devices; packets processed by all IPv6 interfaces on link (like broadcast)
            - ff02::2 All-Router multicast group: all IPv6 routers; joins with command ipv6 unicast-routing; packet processed by all routers on link
        - **Solicited node** multicast addresses: like all-node, mapped to a special Ethernet multicast address; allows Ethernet NIC to filter the frame via destination MAC (lvl 2)
- 12.8 Subnet an IPv6 Network
    - Use the Subnet ID - field in the GUA b/w Global Routing Prefix and the Interface ID
        - 16-bit subnet ID creates up to 65,536 subnets
        - 64-bit Interface ID: supports up to 18 quintillion hosts per subnet
        - Subnet ID bits = 64 - Global Routing Prefix bits
    - To determine next available subnet, just count up in hexadecimal, Example:
    Global Routing Prefix: 2001:db8:acad::/48
    (64-48) = Provides for a 16-bit Subnet ID
    Subnet 1: 2001:db8:acad:0000/64
    Subnet 2: 2001:db8:acad:0001/64
    Note: subnet prefix = bits left of slash (4 hextets = 64)
    - IPv6 Subnet Allocation - create a logical scheme, address space not an issue
    - Router Configuration: configure each for different IPv6 subnet
