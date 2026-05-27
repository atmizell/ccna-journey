# Module 16: Network Security Fundamentals

**Relevant JITL Video**: Day 48 - Security Fundamentals

- Terms
    - Simple Network Management Protocol (SNMP)
    - Simple Mail Transfer Protocol (SMTP)
- 16.1 Security Threats and Vulnerabilities
    - Threat Actors - intruders who gain access by modifying software or exploiting software vulnerabilities
    - Four Threat Types:
        - Information Theft
        - Data Loss and Manipulation
        - Identity Theft
        - Disruption of Service
    - Three Primary Vulnerabilities:
        - **Technological**
            - TCP/IP Protocol Weakness: HTTP, FTP, ICMP inherently insecure; SNMP/SMTP related to insecure TCP
            - OS Weakness: each have problems documented in the Computer Emergency Response Team (CERT) archives
            - Network Equipment Weakness: device vulnerabilities like password protection, lack of authentication, routing protocols, firewall holes
        - **Configuration**
            - Unsecure User Accounts: transmit info insecurely
            - System account easy passwords
            - Misconfigured internet services: JavaScript on untrusted sites, misconfigured terminal service, FTP, or web servers
            - Unsecured default settings
            - Misconfigured network equipment: access lists, routing protocols, SNMP community strings can create or enable holes in security
        - **Security Policy**
            - Lack of written security policy
            - Politics
            - Lack of authentication continuity
            - Logical access controls not applied: inadequate monitoring and auditing
            - Software/hardware installation and changes not following policy
            - Nonexistent disaster recovery plan: lack of response to a natural disaster provides attack opportunity
    - Four Classes of Physical Threat:
        - **Hardware** - physical damage to devices
        - **Environmental** - temperature/humidity extremes
        - **Electrical** - voltage spikes, insufficient supply voltage (brownouts), unconditioned power (noise), power loss
        - **Maintenance** - poor handling (electric discharge), lack of spare parts, poor cabling, poor labeling
    - Plan Physical Security: lock equipment, monitor and control entry with logs, use security cameras
- 16.2 Network Attacks
    - **Malware** (malicious software): code/software specifically designed to damage, disrupt, steal, or inflict “bad” or illegitimate action on data, hosts, or networks
        - **Virus** - become part of another program (typically an executable file) to propagate; some viruses operate within, or overwrite a host program
        - **Worm** - self-replicating, standalone; use system features to spread
        - **Trojan Horse** - looks like something else, known for creating backdoors for access; must spread through user interaction
    - **Network Attack Categories**:
        - **Reconnaissance** - discovery and mapping of systems, services, or vulnerabilities
            - Internet Queries - initial information (search, nslookup, whois)
            - Ping Sweeps - determine which addresses area active in a range (tools like fping or gping)
            - Port Scans - executed on the discovered ip addresses
        - **Access** - unauthorized manipulation of data, system access, or user privileges; exploit known vulnerabilities in authentication, FTP, and web services
            - Password - various password cracking attacks (Brute Force, Trojan Horse, Packet Sniffers)
            - Trust Exploitation - utilize a trusted system to gain unauthorized access
            - Port Redirection - use compromised system
            - Man-in-the-Middle (MITM) - positioned between two legitimate entities to read or modify data
        - **Disruption** - disabling or corruption of networks, systems, or services
            - Denial of Service (DoS) - single source, coordinated effort to overwhelm target
            - Distributed Denial of Service (DDoS) - multiple, coordinated sources (botnet = infected zombie hosts, uses Command and Control [CnC] program)
- 16.3 Network Attack Mitigation
    - Mitigate by securing devices (routers, switches, servers, hosts)
    - Defense-in-Depth Security (Organizations)
        - Utilize networking devices and services
        - Protect against TCP/IP threats:
            - **VPN** - router used to secure corporate sites; provide remote access using secure encrypted tunnels
            - **ASA Firewall** - reside b/w networks, control traffic, help prevent unauthorized access; utilize filtering methods (packet, application, URL; SPI)
                - Packet Filtering (IP or MAC)
                - Application Filtering (port based)
                - URL Filtering (web)
                - Stateful Packet Inspection (SPI) - block unsolicited packets; can help filter DoS
            - **Intrusion Prevention System (IPS)** - monitors traffic
            - **Email Security Application (ESA)** - filters spam and sus emails
            - **Web Security Appliance (WSA)** - filters known and suspicious internet malware sites
            - **AAA Server** - secure database of authorized users
                - Authenticate - who can access
                - Authorize - what can they do
                - Accounting - what actions they perform
                - *Note*: Servers accessible to the outside typically on special network (DMZ)
        - **Backups**: Infrastructure devices should have backups of config files and IOS images on an FTP; considerations:
            - Frequency - regular
            - Validation - ensure data integrity
            - Storage - secure, possibly offsite
            - Security - strong passwords
        - Mitigate Worm: download security updates from OS vendor and patch all vulnerable systems
            - **Patch** fixes something
            - **Update** is a group of patches
            - **Upgrade** is a group of updates
- 16.4 Device Security
    - Security settings must be updated from their default install values
        - Resource access should be restricted appropriately
        - Usernames and Passwords (passphrase)
            - Encrypt all plain text passwords on devices
            service password-encryption
            - Set minimum acceptable password length
            security passwords min-length <*length*>
            - Deter brute force password guessing attacks
            login block-for <*block time sec*> attempts <*attempt #*> within <*attempts within time*>
            - Disable an inactive privileged EXEC mode access after a specific amount of time
            exec-timeout <*minutes*> <*seconds*>
        - Configure appropriate devices to support SSH:
            1. Configure a unique device hostname
            hostname <*name*>
            2. Configure the IP domain name
            ip domain-name <*name*>
            3. Generate a key to encrypt SSH traffic
            crypto key generate rsa general-keys modulus <*bits*>
            bits determines size of key, b/w 360-2048
            min recommended 1024 bits
            4. Verify or create a local database entry
            username entry - username
            ex. username Bob secret Cisco
            5. Authenticate against the local database
            (config-ln)# login local
            6. Enable vty inbound SSH sessions
            transport input {ssh | telnet}
            *Note*: either ssh, telnet, or both can be used after input
        - Unnecessary services/applications should be turned off and uninstalled when possible
            - Ex. CPU Cycles, RAM; HTTP, telnet
            - Verify open ports: show ip ports all
            (Prior to IOS-XE) show control-plane host open-ports
            - Disable unused ports:
                Disable HTTP: no ip http server
                Disable Telnet: transport input ssh
    - Note: Cisco routers can utilize AutoSecure
