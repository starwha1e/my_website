---
title: "System and Network"
date: 2019-11-14T11:33:56+08:00
draft: true
---

# Lecture 2 Fundamentals
## 1.Distributed system
 **Client-Server Model**
Client \<- -\> Server

**Sending data between computers**
**Identifying computers**: Types of address	
Hostname address
IP adress
Hardware address ( is not the same as IP address ), also known as MAC address.
**Layered Model**
Application -\> Transport -\> Internet -\> Net. Interface 
**Identifying Applications**: Port numbers
Used to identify an application as an end point

**Type of IP(V4) Address (Comer, 2009)**
Unicast
Multicast
Broadcast

# Lecture 7 Signals and transmission media
Sign waves and signal characteristics

## Type of transmission media
Forms of energy:
1. Electrical signals 
	- Copper wire: great conductivity and cheap, but deteriorates as distance become longer because of interference
	- Twisted Pair: UTP(wired telephone), STP(shield twist pair). Different twist rate is needed to avoid interference.
	- Coaxial cable
2. Light signals 
	- Optical Fibre
		Advantage: no electrical interference, carry a signal much further than copper
		Disadvantage: hard to find breaks
	- Infrared
	- Laser
3. EM Signals (electromagnetic signals)
	Radio, satellite, microwave 

# Lecture 8 Reliability and channel coding

## Transmission errors: Categories
1. Interference: e.g. EM radiation from other device
2. Distortion: physical systems distort signals
3. Attenuation: Weakening of signal with distance

## Tradeoffs with error detection/correction
1. Error detection adds overheads
2. Whether to use error detection or not
3. Consider a single bit error
	- Not important for image transmission
	- Important for bank transfer

## Errors and their causes (Cormer, 2015)
1. Single bit error
2. Burst error (multiple bits in a block)
3. Erasure (Ambiguity, neither 1 nor 0)

## Handling channel errors: Channel coding - Two schemes
1. **Forward error correction mechanisms (FEC)**
	- **Single parity bit checking**: add parity bit 1/0 to make the number of the 1 bit odd or even
		- Even parity checking: Total number of 1 must be even
		- Odd parity checking
		Introduces additional costs, only detects limited types of errors, cannot correct errors

	- **Row and column parity checking (RAC)**
		- parity for each row and column
			can only correct single-bit errors

	- **Checksums**
		- Interpret data as if it were a sequence of integers and add them together, and in any carry bits too
		- Append the check sum to the frame
		- Data overhead, computational overhead, periodic reversal bits undetected

	- **Cyclic redundancy checks**
		- Requires simple hardware
		- Sender and receiver agree a generator G
		- Sender appends an additional sequence of bits to the data (the CRC bits)
		- So that the resulting sequence is exactly divisible by G
		- Receiver divide the received bit pattern by G and checks whether the remainder is 0

2. **Automatic repeat reQuest mechanisms (ARQ)**
	- Sender and receiver communicate meta-information
	- Sender sends data, receiver sends acknowledgement (ack) back
	- If no ack after a timeout, sender retransmit copy of original data

# Lecture 9 Long Distance Communication: Modulation, Modems and Multiplexing
## Modulation and shift keying
- Modulation: term used for analogue signals 
	- Shift Keying: term used for digital signals

## Modulation
- Amplitude modulation
- Frequency modulation

## Shift Keying
- Amplitude shift keying
	- Frequency shift keying
	- Phase shift keying:
		no shift, 1/4 shift, 1/2shift, 3/4shift — 00, 01, 10, 11. Each phase shift carries 2 bits

## MODEM (modulator-demodulator)
- Optical, Radio and Dialup Modems

## Multiplexing
- Frequency division multiplexing
	- Wavelength division multiplexing (uses prisms to combine beams of light of different wavelengths into a sing beam)
	- Time division multiplexing (Statistical TDM): TDM is an alternative to FDM where the sources sharing the medium take turns
	- Inverse multiplexing: commonly used in the internet.
		When service providers need higher bit rates than are available on a single medium:
		Uses multiplexing in reverse
		Spread a high-speed digital input over multiple lower-speed circuits for transmission
		Combine them at the receiving end
		Sender and receiver have to agree on how data arriving form the input will be distributed 			over the lower-speed connections

## Lecture 10 Local Area Networks: Packets, Frames and Technologies
### Circuit Switching
A communication mechanism that establishes a path between sender and receiver
Each circuit doesn’t necessarily correspond to a physical path
Paradigm:
Point-to-point communication 
Separate steps for circuit creation, use and termination
Performance equivalent to an isolated physical path

### Packet Switching
Uses statistical multiplexing: Communication from multiple sources competes for use of shared media
Paradigm:
Arbitrary, asynchronous communication
No set-up required before communication begins
Performance varies due to statistical multiplexing among packets

### LAN topologies
**Star**: More robust but hub may be a bottleneck
**Ring**: Easy coordination but is sensitive to a cable being cut 
**Mesh**: Poor scalability 
**Bus**: Less wiring required but is also sensitive to a cable being cut

### Framing
Each type of hardware defines its own format/wrapping for a packet called a frame
SOH: start of heading; EOT: end of transmission

### Byte Stuffing
Control character can be included in data.
Transmitter scans data block and replaces all occurrences of control characters.
Receiver do the reverse mapping.

## Lecture 11 Coordination of Access to LANS (The IEEE MAC Sub-layer)
LLC - Logical Link Control: Addressing and multiplexing
MAC - Media Access Control: Access to shared media

### Controlled access protocols
**Polling:**
- Network uses a centralised controller
	Cycles through stations on network
	Gives an opportunity to transmit a packet
**Reservation:**
- Typically uses a centralised controller
	- Each round of packet transmissions is planned in advance
	- Two-step process:
		Each potential sender specifies whether they have a packet to send during next round
		Controller transmits a list that will be transmitting
		Stations use the list to know when they should transmit
**Token Passing:**
Mostly associated with ring topologies
Token: allow to send a certain amount of data
At any instant, exactly one computer has the token
Whilst no station has any packets to send, token circulates continuously 
When a station has a packet to send, it waits to receive the token and holds onto it while it send its packet (Once it finished, it sends the token on again)

### Random access protocols (How computers access the medium)
Carrier sense with multiple access (CSMA)
the computers can detect when a single is on the ether - carrier sense
can only transmit when the receiver is free

1. Ethernet: **CSMA/CD** with collision detection
	Each computer also senses for garbled transmission - a collision
	**Collision recovery**: 
	Computer but wait a random delay, d, after collision before retransmission, double it for each 		sub sequent collision

2. Wireless networks: **CSMA/CA** with collision avoidance
Collision detection does not work because a transmission from one computer may only be received by its immediate neighbours.

Sender sends small request message to receiver
receiver responds with a ‘clear’ to send message that is received by all adjacent computer
-\> Request to 2
Clear to send  	\<- -\>	 Clear to send
-\> Packet transmission to 2
Computer 1		Computer 2 		Computer 3

- Handling of collisions 
	If transmit a packet at the same time
	Control message will collide
	Computer 2 will detect collision won’t be ‘clear to send’ message
	Random backoff is applied

## Lecture 15 WAN Technologies
### Packet Switches
In order to grow, WANs use many switches
Basic component is the packet switch that can connect to local computers and to other packet switches

### Next Hop Forwarding
only has information about how to reach the next switch - next hop

### WANs and Graphs

### Computing the shortest path
Dijkstra’s Algorithm

# Lecture 13-14 Wireless Networking Technologies
## Spread spectrum techniques
Uses multiple frequencies to send data
Sender spreads data across multiple frequencies
Receiver combines information form these frequencies to rebuild the original data
Different techniques use different ways of doing this

Two alternative reasons for using spread spectrum
Increase overall perforative
Make transmission more immune to noise

## Different wireless LAN standards
Each standard specifies: frequency range, modulation technique, multiplexing technique, data rate
Different standards have different advantages, e.g. security, high data rate, high reliability

## Three key parts to a wireless LAN
Access point (base station)
Switch or router: used to connect access points
Set of wireless hosts (wireless nodes or wireless stations)

## Personal Area Networks (PANs)
Bluetooth
Short distance wireless connection technology
Operates at 2.45GHz
Architecture	
Basic unit - piconet
Master node, up to 7 active slaves, up to 255 parked nodes
Centralised TDM system
All communication is between master and slave
InfraRed
ZigBee

ISM wireless

## Lecture 12 Extending LANs
### Fibre modems
Fibre modems extend connection between computer and transceiver/hub

### Repeaters
- Join Ethernet cables (segments) together
- Amplified signal, deals with signal strength but not delay
- No knowledge of frames
Ethernet standard says no more than four repeaters between two computers
Fibre modems can be used between repeaters for long distance extensions

Problem: Transmit all signals including collisions and the noise which limits scalability

### Bridges
- Connect two segments, but work at the frame level
- Use promiscuous mode and so receive all frames
- Don’t forward erroneous frames (e.g. collisions and noise)

- Only forward a frame if necessary:
	destination is on the other segment
	broadcast/multicast address is used
- Bridge learns which segment a computer is on when that computer sends a frame
- When a frame arrives at the bridge:
	Extracts source address and updates knowledge
	Inspects destination address for forwarding

**Planning a bridged network**
- A bridge will only forward frames as far as is necessary
- Bridges allow communication on separate segments to occur at the same time
- Plan the net work so that computers that communicate frequently are on the same segment
- May be possible to improve an existing LAN’s


### Switches
- A single electronic device that transfers frames between computers
- Whereas a hub simulates a shared medium, a switch simulates a bridged LAN with on computer per segment
	Hub - operates as an analogue device that forwards signals among computers
	Switch - digital device that forward frames
- Advantage:
	Greater data rate due to parallelism
	Hub can support one transmission at a time
	Switch can potentially support multiple transmissions at a time
- Some organisation combine hubs and switches to reduce cost

## Lecture 16-17 IP Classes and Addressing
### The IP addressing scheme
Each host is assigned a unique binary number: An IP address
All communication with a host use this address
IPv6: 128-bit addresses
IPv4: 32-bit addresses
Each 32 bit address is divided into **Prefix** and **Suffix**

### Arbitrary boundary
Address masks (subnet masks): A value to specify the exact prefix/suffix boundary.
For IPv4, 32-bits long
Has 1 bits to mark a network prefix and 0 bits to mark the host portion

CIDR Notation used with IPv4:
Classless Inter-Domain Routing
192.5.48.69/26: the left most 26 bits are the network prefix

## Lecture 18 Datagram Forwarding

### IPv6 Base and Extension Headers

### Forwarding an IP Datagram
Each router forwards a virtual packet by using a local forwarding table
**Each entry corresponds to a network, not a host.**
Each entry contains:
- Destination address
	- Mask
	- Next hop: IP address of a router or deliver direct
**Example IPv4 Routing Table**
Destination	Mask		Next Hop
30.0.0.0		255.0.0.0	         40.0.0.7
Longest Prefix Match
Forwarding tables can contain a default entry
Internet forwarding uses the longest prefix match

### Best-Effort Delivery
IPv4 and IPv6 attempt best effort delivery and do not guarantee to deal with:
- Datagram duplication
	- Delayed or out of order delivery
	- Corruption of data
	- Datagram loss
These issues are dealt with other protocol layers 

### MTU and Datagram Size
- MTU - Maximum Transmission Unit: Max of data that a frame can carry on a given network
- A packet may have to cope with different MTU sizes as is passes over an internet
*IPv4 and IPv6 do fragmentation differently*
**IPv4 Fragmentation**  
An IPv4 datagram that is larger than MTU is fragmented into smaller datagrams
Fragments can be fragmented
Destination doesn’t need to wait for all sub-fragments of a fragment to arrive before whole datagram can be reassembled
- Reduces CPU time
	- Less data needs to be stored in fragment headers
In the Internet, designers try to avoid networks being arranged in a sequence of decreasing MTUs

**IPv6 Fragment Size**
Fragment is chosen to be the MTU of the underlying network.
Done at the original source, NOT at routers. Hosts are expected to choose a datagram size that won’t need fragmentation
If a router along the way receives a datagram larger than MTU it will throw it away and send an error message

Host therefore has to learn the MTU of each network along the path to the destination
*Path MTU* is minimum MTU along a path
*Path MTU discovery* is the process of learning the path MTU
Iterative process of trying a size and seeing if an error gets returned.

### Reassembly
**Done at the final host**
- routers require less state information
	- fragments can take different routes
Header fields indicate when the data is a fragment and also where it belongs
Each datagram contains fragment
Whole datagram is lost if any fragment lost

**Identification field**
Used along with the IP source address to determine *which original datagram* the fragment belongs to 
**Fragment offset field**
Allows receiver to determine *where in the original datagram* the payload of the fragment belongs
**Fragment Loss**
When receiver receives a fragment it starts a timer (reassembly timer)
Receiver must buffer fragments in memory until either all fragment arrive or timer expires
If timer expires before all fragments have been received then those that have been received are discarded
## Lecture 19-20 Support Protocols and Technologies
### Address Resolution Protocol (ARP)
*Used for address binding*
- IP addresses are globally unique, hardware independent.
- Mapping between IP address and MAC address is known as **Address Resolution** (Only ever done for computer on the same physical network)
- Hardware MAC address are needed to transport data across link layer
- ARP translates between IPv4 address and hardware address.
- Can be used on any kind of network that supports broadcasting.

A sender will use ARP to find the hardware address of 
- Destination if on same network
	- Next-hp router if on another network
ARP forms a conceptual boundary
- Protocols above ARP use IP addresses
	- Protocols below ARP use MAC addresses

IPv4 ARP Discovery Packets and Caching
Requesting host broadcasts a discovery packet
- ARP request message is put into the payload of the hardware frame
	- Type field is set to 0x806 (denote ARP message)
	- Operation field in ARP request denotes request or response
All host receive it but only the one who has that IP address replies
Host updates its ARP cache with requestor’s addressing binding
ARP cache at each host stores a number of mappings
- When a response arrive and the table is full, the oldest entry is removed
	- When an entry has not been updated for a long time (e.g. 20 mins) the entry is removed
### Internet Control Message Protocol (ICMP)

### Dynamic Host Configuration Protocol (DHCP)

### Network Address Translation (NAT)

## Lecture 21 Transport Layer Protocols - UDP
TCP/IP suite contains:
User Datagram Protocol (UDP)
- The only connectionless transport service
	- Best effort delivery
Transport Control Protocol (TCP)
- Connection- based transport service
	- Provides delivery guarantees

### Transport Protocols and End-to-End Communication
IP passes traffic across the Internet, but it cannot distinguish between multiple applications running on a network host

Field in the datagram header only identify computers
- IP addresses don’t identify programs running on computers
IP (Layer 3) - treats a computer as an endpoint of communication
Layer4 above - UDP/TCP - known as end-trend protocols because they allow an individual application to be an endpoint of communication

### UDP - The connectionless paradigm
An application using UDP
- Does not need to establish a connection before sending data
	- Does not need to inform the network when finished
	- Can generate and send data any time
	- Can delay an arbitrarily long time between transmission of two messages
UDP
- Does not maintain state
	- Does not send extra control messages
	- Communication consists only of data messages themselves
	- Extremely low overhead

### Message-Oriented Interface
When an application requests UDP to transmit a block of data
- UDP places data in a single message for transmission
	- UDP does not divide a message into multiple packets
	- UDP does not combine messages for delivery
Data therefore has to fit into one message or application has to coordinate sending multiple message (up to the programmer)

**Consequence for programmers**
Applications that use UDP can depend on protocol to preserve data boundaries

### UDP Communication Semantics
UDP provides applications with exactly the same best-effort delivery semantics as IP
An application must either be immune to those problems, or the programmer must take additional steps to detect and correct problems
Choice of using UDP depends on type pf application

### Endpoint Identification with Protocol Port Numbers
UDP defines an abstract set of identifiers called protocol port numbers
Independent of the underlying Operating System
Each computer using UDP must provide a mapping between port number and program identifiers that the Operating System uses
All computers running UDP recognise the standard port numbers
Therefore, they know which program to pass the message on the to based on port number

### UDP Datagram Format
Each UDP message is called a user datagram
Datagram consists of Header and Payload
**UDP header**
No identification of the sender or receiver other than protocol port numbers
UDP assumes that the IP source and destination address are contained in the IP datagram that carries UDP
**UDP Encapsulation**
   | UDP Hdr |	   UDP payload     |
 |  IP Header | 		IP Payload			|
| Frame Header | 				Frame Payload			|

## Lecture 22 Transport Layer Protocols - TCP
Transmission Control Protocol (TCP)
Operates at layer 4
Users IP’s unreliable datagram service, but provides a reliable transport service
Data must be delivered:
- In same order as it was sent
	- Without packet loss
	- Without packet duplication

### End-to-End Service and Virtual Connections
TCP is an end-to-end protocol (like UDP)
Connection oriented
Connections provided by TCP are called *virtual connections*
- Because the connections are provided by software
	- Illusion of a connection

### Techniques that Transport Protocols use
An end-to-end transport protocol needs carful design to achieve efficient reliable transfer
Problem it has to deal with:
- Unreliable communication
	- End system reboot
	- Heterogeneous end systems
	- Congestion in the Internet
Use techniques to repair or circumvent problems, not just CRC, parity bits, checksums

### Solutions to Common Networking Issues
**Sequencing to handle duplicate and out-of-order delivery**
- Connectionless network with dynamic routing may deliver packets out of order or more than once
	- Transport protocols solve this with sequencing
		- Each packet is given a sequence number
		- The receiver notes the number of the last packet that arrived in sequence and stores additional out of order packets
		- The packets are delivered in sequence to the next layer up
		- Duplicates are discarded

**Retransmission to handle lost packets**
- Packet loss is a fundamental problem due to transmission errors
	- One approach to reliable transmission involves *positive acknowledgement with retransmission* 
		- Sender transmits and starts a timer
		- Receiver receives and acknowledges
		- On time-out the sender transmits again
		- The receiver must watch out for duplicates
		- Limit number of attempts before giving up
	- Retransmission can cause duplicate packets

**Techniques to avoid replay cause by excessive delay**
- A duplicate packet might turn up in a later session (queued in a switch for a long time)
	- May be confused with a packet from the later session that uses the same sequence number, the correct packet may be discarded as duplicate
	- Replay can occur with control packets too
	- Solution is to include a unique session identifier in the packet e.g. Timestamp

**Flow control to prevent data overrun**
Data overrun occurs when the sender sends faster than the receiver can receive
Simple solution is to acknowledge each packet before sending the next (’stop and go”)
However, this can be wasteful of bandwidth
*Sliding Window Protocols*
Much more efficient flow control technique (Higher throughput rates)
Sender and receiver agree a window size (number of packets)
Receiver must allocate a buffer to fit the whole window size
Initially a whole window is sent
But sender keeps a copy of data in case it has to retransmit until it gets an ACK
After that, each packet is acknowledged and the another can be sent
[image:175EE01C-44E5-4BDE-937D-AB01DCEEC921-361-0001BDD9A15468DC/Screen Shot 2020-01-02 at 15.35.09.png](#)
[image:EB6CD72D-A29C-4E03-BF57-134B5BDCCCC2-361-0001BDE92CEF34F4/Screen Shot 2020-01-02 at 15.36.19.png](#)

**Techniques to avoid network congestion**
Congestion arises due to too much traffic and/or bottlenecks in the network
Limited storage in switches means that packets get dropped
In the Internet congestion usually occurs in touters
**Detecting congestion**
- Intermediate system such as routers can inform senders
	- E.g. routers can send a special message to the source
		- Or routers can set a bit in each packet header to indicated congestion
	- Increased delay or packet loss can be used as an estimate of congestion
		- The computer that receives the packet can include info in the ACK to inform the original sender
		- Works well because in the Internet most delay and loss is due to congestion, not hardware failure
	- Solution is rate control

### Retransmission
Techniques used in TCP to handle packet loss
- Sender sets a timer
	- Receiver sends an acknowledgment 
	- Timeout results in retransmission

**Adaptive Retransmission**
Sensible timer values vary greatly on an Internet
TCP monitors the delay on a connection and adapts the timer
- notes time take to receive acknowledgments
	- computers weighted average and variance over many transmissions and uses these to set the timer
	- i.e. estimates round trip delay for each active connection
When a delay remains constant TCP will increase the retransmission timeout to be slightly longer than the round trip delay
When a delay drops back down, TCP rests timer to a lower value

### Flow Control
TCP uses a window mechanism (measured in bytes)
Each end of the connection allocates a buffer and notifies the other one of its size
Receiver sends available window size in each acknowledgement (*window advertisement*)
Receiver send window advertisement when the application consumes some data
Positive window advertisement when the application consumes some data
Positive window advertisement if receiver can read data as quickly as sender sends it
*Zero window* advertisement tells the sender to stop transmitting until further notice as receiver’s buffer is full.

### Three-way Handshake
Use special control messages
- Synchronization segment (SYN)
	- Finish segment (FIN)
	- Also confirms that all data has been received at both sides
During startup 3-way handshake, each side sends a control message specifying initial buffer size and a sequence number
Sessions need to be identifiable so that old delayed packets, or on end crashing, does not cause confusion
Each end of each new connection generates a random 32bit sequence number
This becomes the initial sequence for data being sent
[image:FFB1AC7C-CFEC-4FB8-A075-05CC47319866-361-0001C53E23A7C5FC/Screen Shot 2020-01-02 at 17.50.42.png](#)

### TCP Congestion Control
To avoid congestion collapse in a network, TCP must adapt flow according to congestion level
- TCP measures congestion by changes in delay
	- Responds by reducing the window size and thus rate of retransmission
When starting a new connection, TCP uses a special congestion control mechanism, *slow start*:
- First sends a single message containing data
	- If and ACK received ok then TCP double the amount of data being sent to two messages
	- TCP exponentially increase its data rate until half of the window size is reached
	- TCP the slows the rate of increase and, whilst congestion does not occur, increases the window size linearly
One connection is up and running:
- First lost message, TCP backs right off and sends just one small message
	- Alleviates congestion
As long as TCP implementations follow the rules, congestion in the internet is handled really well
Other schemes have been proposed as improvements to TCP, e.g.
- Selective Acknowledgement (SACL) by a receiver which specifies which pieces of data are missing
	- Explicit Congestion Notification (ECN) whereby each router can mark each TCP segment that passes over a congested network
But these have not been as successful as hoped

### TCP Segment Format
Single format for all messages
TCP messages are called segments
A segment header contains information about
- Outgoing data: Sequence number
	- Incoming data: Acknowledgement number and Window

## Lecture23 Disks and File Systems
### Storing Files on Disk - Inodes (Unix)
Index Nodes are structures used to store information about each file
Inode of a regular file or directory file: Locations of its disk blocks
Inode of special file: Information which identifies the peripherals
Every file has on inode
Each inode has a unique inode number
Inodes also store:
- Type of file
	- File permissions 
	- Owner and group lds
	- Hard link count
	- Value of symbolic link if appropriate
	- Last modification time and last access time
Inodes stored in the inode list

### UNIX File system Layout
Boot block
- First logical block of disk
	- Contains executable code used when UNIX first activated
Superblock
- Second logical block of disk
	- Contains information about the disk plus bitmap of free blocks, 1 - free, 0 - used
Inode list
- Fixed size set of blocks
	- Contains all the inodes associated with the files on disk
Remaining blocks
- Contain directories and user files

### Partitioning and Volume Management
Traditionally disks used to be partitioned
- Partitions divide disk into segments
	- Different partitions used for different purposes
	- Once a partition is full, data cannot overflow into another petition
	- A physical volume referred to a partition, a disk, ect..
Volume Management - Logical Volumes
- Can span across more than one physical hard drive
	- Several physical hard drives, seen as one big disk
	- So can have LVs stretched over several disks
	- LVs can be resized easily

### RAID - Array of Independent Disks
Multiple physical disk drive components form a logical unit of storage
Different levels of RAID 0 -\> 6
Different complexities from disk mirroring to striping and parity blocks
Purpose:
Reliability, performance, capacity, availability 

### Distributed Systems
- Distributed Systems are systems which involve more than one host on a network
- Components on each host communicate and cooperate to achieve a common goal
- They do so by message passing
- Many Distributed Systems rely on the ability to remotely call a procedure on another network host - *Remote Procedure Call (RPC)*

**Mounting**
File systems on other devices can be attached to the original directory hierarchy
File systems can be automatically mounted when booting
Large UNIX file systems are usually spread over many devices
Each device holds a subtree of the overall hierarchy
Purpose: To allow user to access files seamlessly, no matter where they are stored

**NFS**
Uses *RPC*
Clients can mount partitions of a server
As though partitions physically connected
Low cost computers can share the same high-capacity disk simultaneously
Users can have access to files on different platforms
- Easy data interchange
Mostly transparent
- user at a workstation logs on
	- can access files as if locally stored
Set-up
- Workstations can be set up to mount the files automatically at  boot time
	- Can be set up to mount files when referenced

**File Handles**
Objects (e.g. Files) on NFS-mounted filesystems are represented by file handles
File handles are data objects used by NFS client and servers to reference files and directories
Client cannot interpret content of a file handle, it is opaque
Server can interpret contents of a file handle, it has meaning
File handle used to uniquely identify every file and directory on the server
Three elements of particular importance:
- *file identifier* e.g. can be an inode number
	- *file system identifier* refers to partition containing the file
	- *Generation count* incremented each time a file is unlinked and recreated
File handle does not include a path name
- does not have to 
	- pathnames can change while a file is being accessed

### NFS is based on two protocols: MOUNT and NFS (both use file handles)
**MOUNT Protocol**
Used fo initial negotiation between NFS client and server
MOUNT enables:
- Client to determine which filesystems are available for mounting
	- Client to obtain a token (the file handle) - used to access the root directory
Can use MOUNT to export only a part of a local partition to a client
- Specify that the root is a directory on the partition (i.e. not the actual root)

**NFS Protocol**
Allows users to perform file and directory operations:
Create, read, modify files
List contents of exported filesystem’s directories
Get file handled for other directories and files
Operations use RPC function
- E.g. READADDR - reads the contents of a directory
	- There for `ls` in shell would equate to “READADDR” RPC function being executed on server of exported filesystem

### Samba
Allows UNIX filesystems to be shared with Windows systems
Samba makes UNIX filesystems look like shared Windows filesystems
Can therefore be accessed using normal Windows facilities and commands
Can also allow Unix systems to access Windows filesystems

## Lecture 24 Directory Services
Distributed information held on a logical database
A generalised name service
Provided by a collection of open systems
Information stored about objects in the real world
Aimed at requests for information, not a read/write database

**Directory Information Tree**
Partitioned into smaller regions, each with a connected subtree
No overlapping between subtree partitions
Various cooperating authorities maintain the data
Replication services enhance availability and provide redundancy

### LDAP - Lightweight Directory Access Protocol
An Internet open standard
Gradually being integrated (or replacing) vendor specific systems
**What is stored in a Directory Service?**
User authentication details
Phone numbers, addresses
Can provide a complete identity management service for an organisation 
The information stored in DNS records can be stored in LDAP
Software which enables the organisation and management of an organisations resources and users

### Examples of Directory Services (do not necessarily use LDAP)
- **Network Information Service (NIS)**
Distributed database to allow computers to share:
- password files
	- group files
	- host tables
	- other files/information
Stored on the NIS master server
NIS allows easier management of a large network
All account and configuration information can be stored on a single computer

NIS Tables
NIS creates network databases called tables
used to store information about computers and users in an organisation
NIS database has a number of tables which store information that would otherwise
entail huge files on a UNIX system

NIS Domains
Specify an administrative group of machines
Have to specify a NIS domain when configuring a NIS server
You can have more than one NIS server for an organisation

NIS Net groups
All the specification of groups for users or machines on the network
Net groups intended to simplify configuration files
Allow increased security by limiting the individuals and machines that have access to critical resources
Net group data base kept on NIS master server
So this enables easier administration of privileges and policies

**Domain Name System - DNS**
For translating between symbolic names and IP addresses
Used by applications
An application becomes a client of a DNS server
Structured as a hierarchical list of domains with the most significant on the right
mersey.cs.nott.ac.uk
Number of domains is not fixed

Each organisation registers under a top level domain with the internet authority
It can then manage its own name space, possibly further devolving authority to sub-units of the organisation 

Distributed DNS Servers
DNS database is distributed among servers
Local server contacts other servers if necessary
Root servers are authorities for top-level domains
Other server are authorities for a part of the hierarchy and also hold references to servers lower down and root servers
Each organisation can decide how to partition authority among servers

Client-server Interactions
An application acting as a client requests recursive resolution
A server acting as a client requests iterative resolution
Replication of servers (especially root servers) optimists performance
Caching exploits temporal locality of reference

# Lecture 25 Distributed applications
## File Transfer Protocol (FTP)
Has to handle a number of complications:
Heterogeneous computers
Characteristics:
- Arbitrary file contents
	- Bidirectional transfer
	- Support for authentication and ownership
	- Ability to browse folders
	- Textual control messages
	- Accommodates heterogeneity
Users can use FTP directly but many don’t
When user requests a download through a browser, browser often uses FTP

A Typical FTP Session
Two types of connections:
- Control connection
	- Data connection
Client sets up a control connection to an FTP server
Sends requests(commands)
Server responds with a numeric status over control connection
Server opens a new data connection to the client

FTP Communication Paradigm
Once a control connection is established client has to login to there server
e.g. normal username/password or as a guest/anonymous (for public files)
Response form server is a numeric status message (OK, or user not found/access denied)
After this:
Client-server relationship is inverted
When client asks to down/upload a file (via the control connection), server opens new data connection
- Server acts like a client
	- Client acts like a server and waits for the data connection
Every time the server need to download/upload a file it opens a new connection
After that connection has beed used for on transfer, data connection is closed
FTP server listens for control connections on a well-known port number
Before connecting, client decides on a port number to listen on for the data connection
Client binds to that port number
Client then transmits the port number to the server over the control connection and waits for server to connect
Can cause problems in some situations e.g.NAT

Email
Involves two separate pieces of software:
- An email interface application
	- A mail transfer Program
Three main categories of protocols used with email:
- Transfer
	- Access
	- Representation

SMTP - Simple Mail Transfer Protocol
Standard protocol used by a mail transfer program
- Stream paradigm
	- Uses textual control messages
	- Only transfers text
	- Allow sender to specify recipients’s names and check each name
	- Sends one copy of a given message
Ability to send a single message to multiple recipients on a given computer
- Client can list users one at a time
	- Single copy sent to all users on the list
		- client send a message "I have a message for user A”
		- server replies “250 OK" or “550 No such user here”

Mail Access
Distinct from transfer
Access only involves a sing user interacting with a single mailbox
- Provides access to a user's mailbox
	- Permit a user to view headers, download, delete or send individual messages
	- Client runs on user's personal computer
	- Server runs on a computer that stores user’s mailbox

ISPs, Mail servers and Mail Access
ISPs offer email services:
- Because most users now do not leave their computer running all the time
	- A lot of users do not know how to configure email
ISP runs an email server
Provides a mailbox for each user
Interface for users to access their mailboxes
Either special-purpose email interface application
Or Web browser
Can read email on any device, no special software needed
Special purpose interface applications for mobile devices

**Mail Access Protocols - *POP and IMAP***
Post Office Protocol and Internet Mail Access Protocol
Offer the same basic services
user can access their mailbox
can view header, download, delete, send
Email client runs on user's personal computer/device
Server runs on computer where user’s mailbox is stored
Each provides own authentication mechanism

**Cloud Computing**
Use of resources/software provided by someone else
Computing power, software and hardware offers as a service
Usually pay per use, customers billed
So effectively “renting" software/hardware/service use instead of buying it
Means the service provider manages more
No knowledge needed of the service you are using other than what it does for you
Do not need to know what hardware is used

SaaS - Software as a Service
Run software without installation and processing power
GoogleDocs

PaaS - Platform as a Service
Provides computing platforms, computational resources
Can only use the tools/platform/language provided by the service
Google App Engine

IaaS - Infrastructure as a Service
Provides high performance computing infrastructure, storage, networking, servers 
Enable development of programs for running on a high performance infrastructure, maintained by service provider
Amazon Web Service (AWS), Microsoft Azure

