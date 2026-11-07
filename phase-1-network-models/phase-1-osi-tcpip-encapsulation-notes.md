# Phase 1 — OSI, TCP/IP, and Encapsulation Notes

## 1. Purpose of These Notes

These notes provide a focused reference for the network models and data-handling processes covered in Phase 1.

They cover:
- The seven-layer OSI model.
- The four-layer TCP/IP model.
- How the two models relate.
- Encapsulation and decapsulation.
- Protocol Data Units (PDUs).
- How data travels across a routed Ethernet network.

---

## 2. The OSI Model

The Open Systems Interconnection (OSI) model is a seven-layer conceptual model used to describe the functions involved in network communication.

| Layer | Name | Main Function | Examples |
| :--- | :--- | :--- | :--- |
| 7 | Application | Provides network services to applications | HTTP, DNS, SMTP |
| 6 | Presentation | Handles data representation, translation, encryption, and compression | Data formats, encryption |
| 5 | Session | Establishes, manages, and terminates communication sessions | Session management |
| 4 | Transport | Supports communication between processes on end devices | TCP, UDP |
| 3 | Network | Provides logical addressing and routing between networks | IP, routers |
| 2 | Data Link | Provides local link delivery using frames and MAC addresses | Ethernet, switches |
| 1 | Physical | Transmits bits through a physical medium | Copper, fibre, radio |

### Layer Responsibilities

* **Layer 7 — Application:** Provides network-related services to applications. Examples include HTTP, DNS, and SMTP.
* **Layer 6 — Presentation:** Handles how data is represented. This can include data translation, encryption, and compression.
* **Layer 5 — Session:** Manages the establishment, maintenance, and termination of communication sessions.
* **Layer 4 — Transport:** Supports communication between processes on end devices. TCP and UDP are common transport protocols.
* **Layer 3 — Network:** Handles logical addressing and routing between IP networks. Routers commonly make forwarding decisions at this layer.
* **Layer 2 — Data Link:** Handles delivery across a local link. Ethernet frames and MAC addresses are common Layer 2 concepts. A typical Layer 2 switch forwards frames using its MAC address table.
* **Layer 1 — Physical:** Transmits raw bits through a medium such as copper cable, fibre-optic cable, or radio.

### OSI Layer Order
From top to bottom:
1. Application
2. Presentation
3. Session
4. Transport
5. Network
6. Data Link
7. Physical

Data generally moves down the layers at the sending device and up the layers at the receiving device.

---

## 3. The TCP/IP Model

The TCP/IP model describes the protocol architecture used by the Internet and many private networks. A commonly taught version has four layers.

| Layer | Main Function | Examples |
| :--- | :--- | :--- |
| **Application** | Network services used by applications | HTTP, HTTPS, DNS, SMTP |
| **Transport** | Communication between processes | TCP, UDP |
| **Internet** | Logical addressing and routing between networks | IPv4, IPv6, ICMP |
| **Network Access** | Delivery over the local network link | Ethernet, Wi-Fi |

### Important TCP/IP Protocols

* **TCP (Transmission Control Protocol):** TCP is connection-oriented. It provides mechanisms for reliable, ordered delivery, including acknowledgements and retransmission.
* **UDP (User Datagram Protocol):** UDP is connectionless and has less built-in transport overhead than TCP. It does not provide TCP's built-in reliability and ordering guarantees.
* **IP (Internet Protocol):** IP provides logical addressing and supports forwarding packets between networks.
* **ICMP (Internet Control Message Protocol):** ICMP supports network control and diagnostic messages. For example, ping commonly uses ICMP Echo Request and Echo Reply messages.

> **Note About Model Variations:** Some learning resources show TCP/IP as a five-layer model by separating Network Access into Data Link and Physical layers. These notes use the four-layer version.

---

## 4. Mapping OSI to TCP/IP

The OSI and TCP/IP models organize related communication functions differently.

| OSI Layer | Corresponding TCP/IP Layer |
| :--- | :--- |
| Application | Application |
| Presentation | Application |
| Session | Application |
| Transport | Transport |
| Network | Internet |
| Data Link | Network Access |
| Physical | Network Access |

### Key Differences
* OSI has seven layers, while the commonly taught TCP/IP model has four layers.
* OSI separates Application, Presentation, and Session functions; TCP/IP groups those functions into its Application layer.
* OSI's Network layer corresponds broadly to TCP/IP's Internet layer.
* OSI's Data Link and Physical layers correspond broadly to TCP/IP's Network Access layer.

The models are conceptual frameworks. Real protocols do not always fit perfectly into a single layer in every explanation or implementation.

---

## 5. Encapsulation

Encapsulation is the process of adding protocol control information as data moves down the network stack at the sending device.

A simplified sequence is:
1. An application produces data.
2. The Transport layer adds a transport header.
3. The Network or Internet layer adds an IP header.
4. The Data Link layer adds a frame header and trailer.
5. The Physical layer transmits the resulting frame as bits.

### Encapsulation Terms

| Stage | Resulting Data Unit |
| :--- | :--- |
| **Application** | Data |
| **Transport** | TCP segment or UDP datagram |
| **Network / Internet** | IP packet |
| **Data Link** | Frame |
| **Physical** | Bits |

Headers contain information needed by the corresponding protocols.

### Examples
* A TCP header includes source and destination port numbers.
* An IP header includes source and destination IP addresses.
* An Ethernet header includes source and destination MAC addresses.
* An Ethernet frame also has a trailer used for error detection.

---

## 6. Decapsulation

Decapsulation is the receiving process in which protocol information is examined and processed as data moves up the network stack.

A simplified sequence is:
1. The receiving interface obtains bits.
2. The Data Link layer processes the frame and its addressing information.
3. The Network layer processes the IP packet.
4. The Transport layer processes the TCP segment or UDP datagram.
5. The application receives the data intended for it.

### Encapsulation vs Decapsulation

| Encapsulation | Decapsulation |
| :--- | :--- |
| Occurs as data moves down the sender's stack | Occurs as data moves up the receiver's stack |
| Adds protocol control information | Processes and removes protocol control information |
| Prepares data for transmission | Makes received data available to the appropriate upper layer |

---

## 7. What Happens When Traffic Crosses a Router?

Consider a PC sending data to a destination in another IP network:
1. The PC determines that the destination IP address is outside its local subnet.
2. The PC sends the traffic toward its configured default gateway.
3. The PC creates a local Ethernet frame addressed to the gateway's MAC address.
4. The router receives and processes the frame.
5. The router examines the IP packet and selects a route or next hop.
6. The router prepares a new Layer 2 frame for the outgoing link.
7. The packet continues toward its destination.

### Important Distinction
When a router forwards an IP packet over another Ethernet link:
* The Layer 2 frame is removed and a new frame is created for the next link.
* The source and destination IP addresses normally remain the same across the routed path, unless a feature such as Network Address Translation changes them.
* The IPv4 TTL is reduced by at least one at each router hop.

This explains why the Ethernet MAC addresses used on one local link are not necessarily the MAC addresses used on the next link.

---

## 8. Protocol Data Units (PDUs)

A Protocol Data Unit is the name given to data at a particular layer.

| Layer | PDU Name |
| :--- | :--- |
| **Application** | Data |
| **Transport** | TCP segment or UDP datagram |
| **Network / Internet** | Packet |
| **Data Link** | Frame |
| **Physical** | Bits |

### Important Distinctions
* A TCP segment belongs to TCP at the Transport layer.
* A UDP datagram belongs to UDP at the Transport layer.
* An IP packet belongs to the Network or Internet layer.
* An Ethernet frame belongs to the Data Link layer.
* Bits are transmitted through the physical medium.

PDU terminology can vary slightly by protocol and reference, but these are the standard beginner-level distinctions.

---

## 9. Applying the Concepts to the Packet Tracer Lab

The Phase 1 practical lab used two VLANs and Router-on-a-Stick routing.

### VLAN and Subnet Assignments

| Device | VLAN | IP Address | Default Gateway |
| :--- | :--- | :--- | :--- |
| `PC0` | 10 | `192.168.1.10/26` | `192.168.1.1` |
| `PC1` | 20 | `192.168.1.70/26` | `192.168.1.65` |
| `PC2` | 20 | `192.168.1.71/26` | `192.168.1.65` |

### What Happens When PC0 Sends Traffic to PC1?
1. PC0 checks whether PC1's IP address is in its own subnet.
2. PC1 is in a different subnet, so PC0 sends the traffic to its default gateway, `192.168.1.1`.
3. PC0 uses the gateway's MAC address as the destination MAC address in the local Ethernet frame.
4. The switch carries VLAN 10 traffic over its trunk to the router using an 802.1Q VLAN tag.
5. The router receives the frame on its VLAN 10 subinterface.
6. The router routes the IP packet toward VLAN 20.
7. The router creates a new Ethernet frame for VLAN 20 and sends it through the trunk.
8. The switch forwards the frame to PC1's access port in VLAN 20.

This example connects several Phase 1 concepts:
* IP addressing determines whether the destination is local or remote.
* The default gateway provides a path to another subnet.
* Ethernet frames provide local link delivery.
* 802.1Q identifies VLAN traffic on the trunk.
* The router forwards the IP packet between networks.
* The switch forwards frames within each VLAN.

---

## 10. Troubleshooting Questions

When network communication fails, use the layered approach to narrow down the problem.

### Physical and Data Link Checks
* Are the cables and interfaces connected?
* Are the relevant switch ports up?
* Is the PC connected to the intended access VLAN?
* Is the switch-to-router link trunking?
* Are the required VLANs active and allowed on the trunk?

### Network Checks
* Does the host have the correct IP address and subnet mask?
* Is the default gateway correct?
* Are the router interfaces and subinterfaces up?
* Does each router subinterface use the correct VLAN tag?
* Does the router have a route to the destination network?

### Transport and Application Checks
* Is the required service listening?
* Are the correct transport protocols and port numbers being used?
* Could a firewall or application configuration be blocking the traffic?

A layered troubleshooting method helps avoid changing unrelated settings before identifying the likely source of a problem.

---

## 11. Knowledge Review

Use these questions to test your understanding:

1. Which OSI layer is responsible for logical addressing and routing?
2. Which OSI layer is associated with Ethernet frames and MAC addresses?
3. What is the common PDU name at the Network layer?
4. What is the difference between encapsulation and decapsulation?
5. Which TCP/IP layer corresponds broadly to the OSI Network layer?
6. Why does a host use a default gateway when sending traffic to another subnet?
7. Why does a router create a new Layer 2 frame when forwarding a packet over another Ethernet link?
8. What does an 802.1Q VLAN tag identify on a trunk?

### Answer Key
1. Layer 3 — Network.
2. Layer 2 — Data Link.
3. Packet.
4. Encapsulation adds protocol control information as data moves down the sender's stack; decapsulation processes that information as data moves up the receiver's stack.
5. Internet.
6. The default gateway provides a next hop toward destinations outside the host's local IP subnet.
7. Layer 2 addressing is specific to a local link, so the outgoing frame must use the addressing appropriate for the next link.
8. The VLAN associated with the tagged Ethernet frame.

---

## Conclusion

The OSI and TCP/IP models provide structured ways to understand network communication. Encapsulation and decapsulation explain how protocol information is added, transmitted, and processed. PDU terminology identifies the form of data at each layer.

The Phase 1 Packet Tracer lab applied these ideas through Ethernet frames, VLAN tags, IP packets, default gateways, and router subinterfaces.

* **Phase:** 1 — Network Models and Encapsulation
* **Status:** Completed
