# Module 5: Number Systems

- 5.1 Binary Number System
    - Binary and IPv4 Addresses
        - Hosts, servers, network devices use binary addresses
        - Binary IPv4 Addresses (32-bits): four octets, ex. 11000000.10101000.00001010.00001010
        - Dotted Decimal notation: 192.168.10.10
    - Conversions:
        - Radix = Number Base; Binary = Base 2
        - Positional Notation: Base^n
        - Binary Octet: 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1
- 5.2 Hexadecimal Number System
    - Hex = Base 16 number system (0-9, A-F)
        
        
        | Decimal | Binary | Hexadecimal |
        | --- | --- | --- |
        | 0 | 0000 | 0 |
        | 1 | 0001 | 1 |
        | 2 | 0010 | 2 |
        | 3 | 0011 | 3 |
        | 4 | 0100 | 4 |
        | 5 | 0101 | 5 |
        | 6 | 0110 | 6 |
        | 7 | 0111 | 7 |
        | 8 | 1000 | 8 |
        | 9 | 1001 | 9 |
        | 10 | 1010 | A |
        | 11 | 1011 | B |
        | 12 | 1100 | C |
        | 13 | 1101 | D |
        | 14 | 1110 | E |
        | 15 | 1111 | F |
    - Used for IPv6 and Ethernet MAC Addresses
        - IPv6 = 128 bits, 4 bits = 1 hex val; 32 hex vals total
        - IPv6 Notation: x:x:x:x:x:x:x:x (x = 4 hex vals = hextet)
    - Decimal to Hexadecimal Conversion:
        - (1) Convert decimal to 8-bit binary string
        - (2) Divide the binary string into groups of 4
        - (3) Convert each four binary numbers to corresponding hexadecimal digit
        - Ex. 168 = 10101000 → 1010 | 1000 → A | 8
    - Hex to Decimal Conversion:
        - (1) Convert the hex to a 4-bit binary string
        - (2) Create 8-bit binary grouping from the rightmost position
        - (3) Convert each 8-bit binary grouping into their equivalent decimal digit
        - Ex. D2 → 1101 | 0010 → 11010010 → 128+64+16+2 = 210
    - Practice:
        - 202 → 128+64+8+2 → 11001010 → CA
        - 254 → 11111110 → FE
        - A9 → 1010 1001 → 128+32+8+1 = 169
        - 7D → 0111 1101 → 64+32+16+8+4+1 = 125
