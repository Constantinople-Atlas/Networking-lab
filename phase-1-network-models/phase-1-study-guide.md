# Phase 1 --- Network Models and Encapsulation Study Guide

## Phase Overview

**Curriculum:** Networking Engineering\
**Phase:** 1 --- Network Models and Encapsulation\
**Level:** Beginner\
**Practical software:** Cisco Packet Tracer

### Phase Objectives

By the end of this phase, a learner should be able to:

1.  Explain why network models are used.
2.  Identify the seven layers of the OSI model in order.
3.  Identify the four layers of the TCP/IP model.
4.  Compare the OSI and TCP/IP models.
5.  Explain encapsulation and decapsulation.
6.  Identify the Protocol Data Units (PDUs) used at different layers.
7.  Describe the roles and layers of common networking devices.
8.  Interpret basic Packet Tracer events and verify a small VLAN-based
    network.

------------------------------------------------------------------------

## Topic 1 --- Introduction to Network Models

### What is a network model?

A network model is a conceptual framework that organizes the tasks
involved in network communication into layers.

Each layer has particular responsibilities and works with the layers
above and below it.

### Why are network models useful?

Network models help people:

-   Understand how data moves between devices.
-   Separate complex communication tasks into manageable parts.
-   Troubleshoot network problems systematically.
-   Explain how protocols and devices work together.
-   Design and maintain networks using common terminology.

### Key idea

A network model is a way to understand and organize communication. It is
not a physical device or a single networking protocol.

------------------------------------------------------------------------

## Topic 2 --- The OSI Model

The Open Systems Interconnection (OSI) model divides network
communication into seven layers.

  -----------------------------------------------------------------------
  Layer             Name              Main              Common Examples
                                      Responsibility    
  ----------------- ----------------- ----------------- -----------------
  7                 Application       Provides network  HTTP, DNS, SMTP
                                      services to       
                                      applications      

  6                 Presentation      Handles data      Data formats,
                                      representation,   encryption
                                      translation,      
                                      encryption, and   
                                      compression       

  5                 Session           Establishes,      Session
                                      manages, and      management
                                      terminates        
                                      communication     
                                      sessions          

  4                 Transport         Provides          TCP, UDP
                                      end-to-end        
                                      transport,        
                                      segmentation, and 
                                      reliability where 
                                      supported         

  3                 Network           Handles logical   IP, routers
                                      addressing and    
                                      routing between   
                                      networks          

  2                 Data Link         Handles local     Ethernet,
                                      frame delivery,   switches
                                      MAC addressing,   
                                      and media access  

  1                 Physical          Transmits raw     Cables, radio
                                      bits through a    signals, hubs
                                      physical medium   
  -----------------------------------------------------------------------

### The layers in order

From top to bottom:

1.  Application
2.  Presentation
3.  Session
4.  Transport
5.  Network
6.  Data Link
7.  Physical

From bottom to top, the order is reversed.

### Important distinctions

-   The **Network layer** is responsible for logical addressing and
    routing.
-   The **Data Link layer** handles local delivery using frames and MAC
    addresses.
-   The **Physical layer** transmits bits as electrical, optical, or
    radio signals.
-   The **Transport layer** supports communication between processes on
    end systems.

### Example

When a computer sends data to a destination on another IP network:

-   The Transport layer prepares transport data.
-   The Network layer uses IP addressing to determine where the packet
    should go.
-   The Data Link layer prepares a frame for the next local network hop.
-   The Physical layer transmits the bits.

------------------------------------------------------------------------

## Topic 3 --- The TCP/IP Model

The TCP/IP model describes the communication architecture used by the
Internet and many modern networks.

A common four-layer version is:

  -----------------------------------------------------------------------
  TCP/IP Layer            Main Responsibility     Example Protocols or
                                                  Technologies
  ----------------------- ----------------------- -----------------------
  Application             Provides network        HTTP, HTTPS, DNS, SMTP
                          services to             
                          applications            

  Transport               End-to-end transport    TCP, UDP
                          between processes       

  Internet                Logical addressing and  IPv4, IPv6, ICMP
                          routing between         
                          networks                

  Network Access          Local network delivery  Ethernet, Wi-Fi
                          and access to the       
                          transmission medium     
  -----------------------------------------------------------------------

### Important points

-   TCP provides connection-oriented transport with mechanisms for
    reliable, ordered delivery.
-   UDP provides connectionless transport without TCP's built-in
    reliability and ordering guarantees.
-   IP provides logical addressing and packet forwarding between
    networks.
-   Ethernet and Wi-Fi provide common ways to carry data across local
    network links.

The TCP/IP model is commonly presented with four layers, although some
references divide the Network Access layer into separate Data Link and
Physical layers.

------------------------------------------------------------------------

## Topic 4 --- OSI Model vs TCP/IP Model

Both models help explain network communication, but they organize the
responsibilities differently.

  OSI Model      TCP/IP Model
  -------------- ----------------
  Application    Application
  Presentation   Application
  Session        Application
  Transport      Transport
  Network        Internet
  Data Link      Network Access
  Physical       Network Access

### Main differences

**OSI model**

-   Has seven layers.
-   Provides a detailed conceptual framework.
-   Helps explain and troubleshoot networking responsibilities.

**TCP/IP model**

-   Commonly has four layers.
-   Describes the architecture used by Internet protocol networking.
-   Groups some responsibilities that the OSI model separates.

### Key lesson

The models are not competing sets of networking devices. They are
different ways of organizing and describing communication functions.

------------------------------------------------------------------------

## Topic 5 --- Data Encapsulation and Decapsulation

### Encapsulation

Encapsulation happens when data moves down the layers of a sending
device's network stack.

Each layer adds the control information it needs. This information is
commonly placed in a header; the Data Link layer also adds a trailer in
Ethernet framing.

A simplified process is:

1.  Application data is created.
2.  The Transport layer adds its header, producing a TCP segment or UDP
    datagram.
3.  The Internet or Network layer adds an IP header, producing an IP
    packet.
4.  The Data Link layer adds a frame header and trailer, producing a
    frame.
5.  The Physical layer transmits the frame as bits.

### Decapsulation

Decapsulation happens at the receiving device.

The receiving device processes the incoming data up the network stack.
Each layer examines and removes the control information intended for
that layer before passing the remaining data upward.

### Important lesson

Encapsulation prepares data for transmission. Decapsulation processes
received data and makes the original application data available to the
receiving application.

### Example

When a PC sends an IP packet to another network, the Ethernet frame is
addressed to the next local-hop device, often the default gateway. The
IP packet retains its source and destination IP addresses as it travels
through routers, although fields such as the TTL are changed and the
Layer 2 frame is replaced at each routed hop.

------------------------------------------------------------------------

## Topic 6 --- Protocol Data Units (PDUs)

A Protocol Data Unit is the name given to data at a particular layer of
a network model.

  Layer                Common PDU Name
  -------------------- ---------------------------------
  Application          Data
  Transport            Segment (TCP) or Datagram (UDP)
  Network / Internet   Packet
  Data Link            Frame
  Physical             Bits

### Remember these distinctions

-   A **segment** is associated with TCP at the Transport layer.
-   A **UDP datagram** is the corresponding common term for UDP
    transport data.
-   A **packet** is associated with the Network or Internet layer.
-   A **frame** is associated with the Data Link layer.
-   **Bits** are transmitted over the physical medium.

The term PDU is a general term. The exact name depends on the protocol
and layer being discussed.

------------------------------------------------------------------------

## Topic 7 --- Networking Devices and Their Layers

Networking devices perform different roles in moving data through a
network.

  -----------------------------------------------------------------------
  Device                  Common OSI Layer        Main Role
                          Association             
  ----------------------- ----------------------- -----------------------
  Hub                     Layer 1 --- Physical    Repeats incoming
                                                  signals to other ports

  Repeater                Layer 1 --- Physical    Regenerates or repeats
                                                  signals

  Switch                  Layer 2 --- Data Link   Forwards Ethernet
                                                  frames using MAC
                                                  address information

  Router                  Layer 3 --- Network     Forwards IP packets
                                                  between networks

  Wireless access point   Primarily Layer 2 in a  Connects wireless
                          basic bridged WLAN      clients to a local
                                                  network

  Firewall                Depends on              Enforces traffic
                          implementation; may     security policies
                          operate across multiple 
                          layers                  
  -----------------------------------------------------------------------

### Switch

A typical Layer 2 switch learns source MAC addresses and associates them
with switch ports.

When forwarding a known unicast frame, it uses its MAC address table to
select the appropriate port within the relevant VLAN.

When it receives an unknown unicast frame, it generally floods the frame
within that VLAN, excluding the incoming port.

### Router

A router forwards IP packets between different IP networks. It uses
routing information to select a next hop or outgoing interface.

### VLANs

A Virtual Local Area Network (VLAN) creates a separate Layer 2 broadcast
domain on a VLAN-aware switched network.

Devices in different VLANs generally need a Layer 3 device, such as a
router or Layer 3 switch, to communicate with one another.

------------------------------------------------------------------------

## Topic 8 --- Practical Packet Tracer Lab

### Lab objective

Configure two VLANs on a switch and use a router with subinterfaces to
enable communication between them.

### Devices used

-   Three PCs: PC0, PC1, and PC2.
-   One Cisco Catalyst 2960 switch.
-   One Cisco 2911 router.

### VLAN and port plan

  VLAN   Name             Switch Access Ports
  ------ ---------------- ---------------------
  10     vlan10-PC0       Fa0/1
  20     vlan20-PC1-PC2   Fa0/2 and Fa0/3

The switch's GigabitEthernet0/1 port connected to router
GigabitEthernet0/0 and was configured as an 802.1Q trunk.

### IP addressing plan

  Device     VLAN IP Address     Mask              Default Gateway
  -------- ------ -------------- ----------------- -----------------
  PC0          10 192.168.1.10   255.255.255.192   192.168.1.1
  PC1          20 192.168.1.70   255.255.255.192   192.168.1.65
  PC2          20 192.168.1.71   255.255.255.192   192.168.1.65

### Subnet calculations

**VLAN 10 --- 192.168.1.0/26**

-   Network address: 192.168.1.0
-   Usable range: 192.168.1.1--192.168.1.62
-   Broadcast address: 192.168.1.63
-   Default gateway: 192.168.1.1

**VLAN 20 --- 192.168.1.64/26**

-   Network address: 192.168.1.64
-   Usable range: 192.168.1.65--192.168.1.126
-   Broadcast address: 192.168.1.127
-   Default gateway: 192.168.1.65

### Router-on-a-Stick

The router used two logical subinterfaces on its physical
GigabitEthernet0/0 interface:

  Subinterface     VLAN Tag IP Address
  -------------- ---------- -----------------
  G0/0.10                10 192.168.1.1/26
  G0/0.20                20 192.168.1.65/26

Each subinterface was configured to recognize its corresponding 802.1Q
VLAN tag.

### Verification

The lab used switch and router verification commands, including:

-   `show vlan brief`
-   `show interfaces trunk`
-   `show ip interface brief`

The reported `show interfaces trunk` output confirmed that
GigabitEthernet0/1 was trunking with 802.1Q and that VLANs 1, 10, and 20
were active and forwarding.

The reported `show ip interface brief` output showed:

-   GigabitEthernet0/0: up/up
-   GigabitEthernet0/0.10: up/up, address 192.168.1.1
-   GigabitEthernet0/0.20: up/up, address 192.168.1.65

### Connectivity test results

  Test         Observed Result
  ------------ -------------------------------------
  PC0 to PC1   3 replies out of 4; 25% packet loss
  PC1 to PC0   4 replies out of 4; 0% packet loss
  PC2 to PC0   4 replies out of 4; 0% packet loss
  PC1 to PC2   4 replies out of 4; 0% packet loss

The first PC0-to-PC1 ping timed out, while the next three succeeded.
This may be consistent with initial address resolution, but the precise
cause was not independently confirmed.

The successful tests demonstrated same-VLAN communication and
communication between VLANs through the router.

------------------------------------------------------------------------

## Phase 1 --- Key Terms

  -----------------------------------------------------------------------
  Term                                Meaning
  ----------------------------------- -----------------------------------
  Network model                       A framework for organizing network
                                      communication functions

  Protocol                            Rules that govern communication
                                      between devices or software

  Encapsulation                       Adding protocol control information
                                      as data moves down the stack

  Decapsulation                       Processing and removing protocol
                                      control information as data moves
                                      up the stack

  PDU                                 Protocol Data Unit; the name for
                                      data at a particular layer

  MAC address                         A link-layer address used for local
                                      network delivery

  IP address                          A logical address used to identify
                                      an interface on an IP network

  VLAN                                A logical Layer 2 broadcast domain

  Access port                         A switch port assigned to an access
                                      VLAN

  Trunk port                          A link that carries traffic for
                                      multiple VLANs

  802.1Q                              A standard for identifying VLANs on
                                      Ethernet trunk links

  Subinterface                        A logical interface associated with
                                      a physical router interface

  Default gateway                     The router address a host uses to
                                      reach destinations outside its
                                      local IP subnet
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## Phase 1 --- Final Review Checklist

Use this checklist to review the concepts covered in this phase.

-   [ ] Explain why network models are useful.
-   [ ] Name all seven OSI layers in the correct order.
-   [ ] Explain the main responsibility of each OSI layer.
-   [ ] Name the four commonly taught TCP/IP layers.
-   [ ] Compare the OSI and TCP/IP models.
-   [ ] Explain encapsulation and decapsulation.
-   [ ] Distinguish segments, UDP datagrams, packets, frames, and bits.
-   [ ] Explain the basic roles of switches and routers.
-   [ ] Explain VLANs, access ports, and trunk ports.
-   [ ] Explain the purpose of 802.1Q tags.
-   [ ] Describe Router-on-a-Stick inter-VLAN routing.
-   [ ] Interpret basic switch and router verification output.
-   [ ] Explain what successful same-VLAN and inter-VLAN ping tests
    demonstrate.

## Conclusion

Phase 1 introduced the models, terminology, and processes used to
understand network communication. The practical Packet Tracer lab
applied these concepts through VLAN segmentation, 802.1Q trunking,
subnet addressing, and Router-on-a-Stick inter-VLAN routing.

**Phase status:** Completed\
**Final assessment:** 15/15 (100%)\
**Practical lab:** Completed
