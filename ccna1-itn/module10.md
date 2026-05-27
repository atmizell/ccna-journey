# Module 10: Basic Router Configuration (THE BILLS)

- 10.1 Configure Initial Router Settings
    - Boot up process of a Cisco Router:
        - Phase 1 - Perform the POST and load the bootstrap program
        - Phase 2 - Locate and load the Cisco IOS software
        - Phase 3 - Locate and load the startup configuration file
    - Basic Configuration Steps:
        - Device Name: hostname *name*
        - Secure Priv EXEC mode: enable secret *password*
        - Secure EXEC mode: line console 0 → password *password →* login
        - Secure remote Telnet/SSH access: line vty 0 4 → password *password* → login → transport input {    ssh    | telnet}
            - *Enable Telnet/SSH*: transport input ssh telnet
        - Secure all passwords in config: service password-encryption
        - Provide legal notification: banner motd *delim msg delim*
            - Can do multi-line messages (start/end with delimiter #): banner motd #
            
            **************************************************
            
            **WARNING: Unauthorized access is prohibited!**
            
            **************************************************
            
            #
            
        - Save config: copy running-config startup-config
- 10.2 Configure Interfaces
    - Routers are not reachable by end devices until the interfaces are configured; router interfaces are very similar to switch interfaces
    - **Configuration Commands**: interface *type-and-number*
        - description *text*
        - ip address *ipv4-address subnet-mask*
        - ipv6 address *ipv6-address/prefix-Length*
        - no shutdown
            - activates the interface (must be connected)
        - exit
            - allows for configuration to occur
    - **Verify Interface Configuration**
        - Display all interfaces, ip addresses, and status; configured and connected should display “up / up”
            - show ip interface brief
            - show ipv6 interface brief
        - Display contents of IP routing tables stored in RAM
            - show ip route
            - show ipv6 route
        - Display interface statistics: show interfaces
        - Display IPv4 statistics: show ip interface
        - Display IPv6 statistics: show ipv6 interface
- 10.3 Configure the Default Gateway
    - Local Network: if one router, it will be gateway; if multiple routers, must select one of them to be default gateway; devices only use routers to communicate outside LAN
    - Switch Gateway Config: ip default-gateway *ip-address*
        - Switch does not need IPv6 manually configured; it will receive the default gateway from the **ICMPv6 Router Advertisement** message from the router
    - *Note*: packets originating from host computers connected to the switch must already have the default gateway address configured on their host computer operating systems
