# Networking Concepts

## Concepts
- MAC address & ports
- IP, Network/Subnetwork, mask, gateway
- OSI model
- Transport protocols
- TCP/IP model
- Devices
- Firewall
- Proxy & Reverse Proxy
- VPN
- Troubleshooting tools


## MAC address & ports


## IP, Network/Subnetwork, mask
- internet protocol address
- IPv4 and IPv6
- IPv4:
  - 32 bit, 8 bit fields
    - network part & host part
    - subnet mask:
      - network address (reserved, not usable) (first address in the network, used to identify the network)
      - broadcast address (reserved, not usable) (used to send packages to all hosts in the network)
      - a number host addresses (usable)
      - example: 192.168.5.21/29;
        - network address 192.168.5.16, 
        - broadcas address 192.168.5.23; 
        - number of hosts available: 6 (192.168.5.17-192.168.5.22)
    - IP classes: 
      - A: subnet mask 255.0.0.0; range 1-128
      - B: subnet mask 255.255.0.0; range 128-191
      - C: subnet mask 255.255.255.0; range 192-223
      - D: range 224-239 (used for multicasting)
      - E: range 240-254 (research & development)
  - private IPs:
    - 10.0.0.0/8
    - 172.16.0.0/12
    - 192.168.0.0/16
  - public IPs: the rest
- IPv6:
  - 128 bit alphanumeric
- Gateway:
  - a network node which enables communications between 2 networks using different transmission protocols; an entry/exit point for the traffic


## OSI model
- Open System Interconnection model
- first standard model for network communications, adopted by all major computer and telecommunication companies in the early 1980s
- 7 layers:

  ![alt text](1.gif "Title")
- Layer 1 Physical layer: transports data using electrical, optical interfaces or radio waves; bits into signals
- Layer 2 Data link: data frames using MAC addresses
- Layer 3 Network: data packages with network addresses
- Layer 4 Transport: HOW the transfer of the data packages is made across the network
- Layer 5 Session: set up "conversations" between applications
- Layer 6 Presentation: format the data based on what the receiving application accomodates
- Layer 7 Application: end user interaction


## Transport protocols
- provide a mechanism for packages of data to be sent accross a network
- TCP:
  - Transmission Control Protocol
  - error checking communication
  - 3 way handshake:
    - client send SYN message to server
    - server respond to client by sending a SYN ACK to client
    - client respond to server by sending ACK to server and start the data transfer
    - client send FIN to server
    - server respond to client by sending a FIN ACK to client
    - client respond to server by sending ACK to server and connection is safe disconnect
- UDP:
  - User Datagram Protocol
  - no error checking
  - no 3 way handshake, the data is directly transferred


## TCP/IP model
- framework for organizing communication protocols in a network
- 4 layers:
  
  ![alt text](2.webp "Title")
- Layer 1 Network Interface: HOW the data should be sent over the network
- Layer 2 Network: internet layer; send&receive packets in the network
- Layer 3 Transport: rules of sending&receiving the data packets in the network
- Layer 4 Application: end user interaction


## Devices


## Firewall
- your first ever anti-virus
- filters the network traffic and decides which traffic is allowed to pass and which is not
- scans the incoming packets and decides if they are allowed to pass or they are considere threats
- Firewall rules:
  
  ![alt text](3.png "Title")


## Proxy & Reverse Proxy
- Proxy:
  - server that sits in front of the user and captures the traffic request and forwards it as its own
  - usage:
    - control internet usage of employees / students / etc.
    - bandwidth savings and improved speeds (cache if all the requests are for the same website)
    - privacy: user IP address will not be visible on the internet (similar to VPN)
    - improved security: block some un-wanted website in schools/companies, etc
- Reverse Proxy:
  - server that sits in front of the webserver and intercepts the traffic requests from clients
  - usage:
    - protects the website (another layer of abstraction between client and the webserver)
    - load balancing
    - caching


## VPN


## Troubleshooting tools

- ping:

- telnet: