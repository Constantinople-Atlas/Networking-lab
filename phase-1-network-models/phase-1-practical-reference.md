# Packet Tracer Practical Guide: Understanding Encapsulation and Packet Headers

## 1. Introduction

During Phase 1 of the Networking Engineering curriculum, we used Cisco Packet Tracer to understand how data is encapsulated as it travels through a network.

Instead of simply memorising the OSI and TCP/IP models, we observed actual packets moving between devices and inspected their headers.

We used Packet Tracer's Simulation Mode to examine the information carried at different network layers.

The objective was to understand:
- How Ethernet frames carry network-layer packets.
- How to identify an Ethernet II frame's EtherType.
- How to identify IPv4 and its protocol field.
- How to distinguish ICMP Echo Requests from Echo Replies.
- How to recognise whether an IPv4 packet carries TCP, UDP or ICMP.
- How encapsulation and decapsulation happen.

---

## 2. Switching Packet Tracer into Simulation Mode

To inspect packets:
1. Open the Packet Tracer project.
2. Select Simulation at the bottom-right of the Packet Tracer interface.
3. Generate traffic between two devices.
4. For example, open a PC's Command Prompt and run:
   ```bash
   ping 192.168.1.70
   ```
5. Observe the events appearing in the Simulation Event List.
6. Select an event or packet envelope.
7. Open PDU Information.
8. Examine the OSI Model and Inbound PDU Details / Outbound PDU Details tabs.

The Simulation Mode lets us follow the packet as it travels through the network.

The OSI Model tab helps us understand what happens at each layer, while the PDU Details tabs expose the actual protocol information.

---

## 3. Understanding Ethernet II Headers

Ethernet operates at Layer 2 of the OSI model.

An Ethernet II frame contains important information, including:

| Field | Purpose |
| :--- | :--- |
| **Destination MAC** | Identifies the receiving network interface |
| **Source MAC** | Identifies the sending network interface |
| **EtherType** | Identifies the protocol carried in the Ethernet payload |
| **Payload** | Contains the encapsulated network-layer packet |
| **FCS** | Used for error detection |

### Example Ethernet II header
An Ethernet II header might contain:
* Destination MAC: `0060.7034.A540`
* Source MAC: `00D0.BA68.15B5`
* EtherType: `0x0800`

*(These MAC addresses are examples from our earlier Packet Tracer practical work.)*

### What is EtherType?
EtherType is a field in an Ethernet II frame that identifies the type of protocol carried inside the frame.

Common EtherType values include:

| EtherType | Protocol |
| :--- | :--- |
| `0x0800` | IPv4 |
| `0x86DD` | IPv6 |
| `0x0806` | ARP |

For example:
```text
EtherType: 0x0800
```
This tells us that the Ethernet payload contains an IPv4 packet.

> **Important:** EtherType does not tell us whether the IPv4 packet carries TCP, UDP or ICMP. We must inspect the IPv4 header to identify that.

---

## 4. Understanding the IPv4 Header

IPv4 operates at Layer 3 of the OSI model.

When an Ethernet frame contains an IPv4 packet, we can inspect the IPv4 header.

Important fields include:

| IPv4 Field | Purpose |
| :--- | :--- |
| **Version** | Identifies the IP version |
| **Header Length** | Indicates the size of the IPv4 header |
| **Total Length** | Indicates the size of the entire IPv4 packet |
| **Identification** | Helps identify fragments belonging to a packet |
| **Flags** | Controls fragmentation behaviour |
| **Fragment Offset** | Indicates a fragment's position |
| **TTL** | Limits how many router hops a packet can take |
| **Protocol** | Identifies the next protocol carried by IPv4 |
| **Header Checksum** | Checks the IPv4 header for errors |
| **Source IP** | Identifies the source IP address |
| **Destination IP** | Identifies the destination IP address |

### The most important field for identifying TCP, UDP and ICMP
The IPv4 Protocol field identifies the protocol carried inside the IPv4 packet.

Common values:

| Protocol Number | Protocol |
| :--- | :--- |
| **1** | ICMP |
| **6** | TCP |
| **17** | UDP |

### Examples:
* `Protocol: 1` — The IPv4 packet carries ICMP.
* `Protocol: 6` — The IPv4 packet carries TCP.
* `Protocol: 17` — The IPv4 packet carries UDP.

This distinction is important:
* EtherType identifies IPv4 inside the Ethernet frame.
* The IPv4 Protocol field identifies ICMP, TCP or UDP inside the IPv4 packet.

---

## 5. Identifying ICMP Echo Requests and Echo Replies

We used the `ping` command to generate ICMP traffic.

ICMP is a network-layer control and diagnostic protocol.

When a computer sends a ping to another computer, it normally sends an ICMP Echo Request.

The receiving computer responds with an ICMP Echo Reply, provided it is configured to respond.

### ICMP message types

| ICMP Type | Message |
| :--- | :--- |
| **0** | Echo Reply |
| **8** | Echo Request |

### Example: Echo Request
When `PC0` sends a ping to `PC1`:
* Source IP: `192.168.1.71`
* Destination IP: `192.168.1.70`
* Protocol: `1` (ICMP)
* ICMP Type: `8` (Echo Request)

This means `PC0` is requesting an echo response from `PC1`.

### Example: Echo Reply
When `PC1` responds:
* Source IP: `192.168.1.70`
* Destination IP: `192.168.1.71`
* Protocol: `1` (ICMP)
* ICMP Type: `0` (Echo Reply)

This means `PC1` has responded to `PC0`'s request.

### How we interpreted the results
We learned that:
* Type 8 means Echo Request.
* Type 0 means Echo Reply.
* The source and destination IP addresses reverse when the reply travels back.
* Both messages use ICMP, identified by IPv4 Protocol number 1.

---

## 6. Identifying TCP and UDP

Not every IPv4 packet carries ICMP.

Applications use different transport-layer protocols depending on their communication requirements. Two major transport-layer protocols are TCP and UDP.

### TCP – Transmission Control Protocol
TCP provides reliable, connection-oriented communication.

It uses mechanisms such as:
- Connection establishment.
- Sequence numbers.
- Acknowledgements.
- Retransmission.
- Flow control.

The TCP protocol number in the IPv4 header is **`6`**.

When inspecting a TCP packet, look for:
- IPv4 Protocol: 6
- TCP source port.
- TCP destination port.
- Sequence number.
- Acknowledgement number.
- TCP flags such as SYN, ACK, FIN and RST.

> **Example:** `IPv4 Protocol: 6 (TCP)` | `Source Port: 50000` | `Destination Port: 80` (an illustrative example of a TCP packet directed towards a service using port 80).

### UDP – User Datagram Protocol
UDP provides connectionless communication.

It does not establish a TCP-style connection before sending data. It has a smaller transport header and does not provide TCP's built-in retransmission and acknowledgement mechanisms.

The UDP protocol number in the IPv4 header is **`17`**.

When inspecting a UDP packet, look for:
- IPv4 Protocol: 17
- UDP source port.
- UDP destination port.
- UDP length.
- UDP checksum.

> **Example:** `IPv4 Protocol: 17 (UDP)` | `Source Port: 53000` | `Destination Port: 53` (an illustrative example of a UDP packet directed towards a DNS service).

---

## 7. How to Identify the Protocol in the Correct Order

This is one of the most important lessons from our encapsulation practical.

When inspecting an Ethernet frame carrying IPv4, follow this order:

### Step 1 – Examine the Ethernet II header
Look at EtherType.
```text
EtherType: 0x0800
```
*Conclusion:* The Ethernet payload contains IPv4.

### Step 2 – Examine the IPv4 header
Look at the Protocol field.
* `Protocol: 1` — The IPv4 packet carries ICMP.
* `Protocol: 6` — The IPv4 packet carries TCP.
* `Protocol: 17` — The IPv4 packet carries UDP.

### Step 3 – Examine the next protocol header
Depending on the IPv4 Protocol field:
* For ICMP, inspect the ICMP Type and Code.
* For TCP, inspect the TCP ports, flags and sequence information.
* For UDP, inspect the UDP ports, length and checksum.

This allows us to identify what the packet contains without guessing from the application or the command that generated it.

---

## 8. Understanding Encapsulation

Encapsulation happens when data moves down the network stack before transmission.

Each layer adds information needed for communication.

For example, when an application sends data using TCP over IPv4 and Ethernet:
```text
Application Data
       |
       v
TCP Header + Application Data
       |
       v
IPv4 Header + TCP Segment
       |
       v
Ethernet Header + IPv4 Packet + Ethernet Trailer
```

The resulting units are commonly described as:

| Layer | Data Unit |
| :--- | :--- |
| **Application** | Data |
| **Transport** | TCP Segment or UDP Datagram |
| **Network** | IP Packet |
| **Data Link** | Ethernet Frame |
| **Physical** | Bits |

For ICMP, the ICMP message is carried directly inside the IPv4 packet. It does not use a TCP or UDP header.

---

## 9. Understanding Decapsulation

Decapsulation happens at the receiving device.

The receiving device processes the information from the outside inward:
1. Ethernet processes the frame and examines its destination MAC address and EtherType.
2. IPv4 processes the IP packet and examines its destination IP and Protocol field.
3. TCP, UDP or ICMP processes the information intended for that protocol.
4. The data is passed to the appropriate higher-layer process.

This is the reverse of encapsulation.

---

## 10. Important Distinctions to Remember

| Question | Where to Look | Example |
| :--- | :--- | :--- |
| **Is the Ethernet payload IPv4?** | Ethernet EtherType | `0x0800` |
| **Is the Ethernet payload IPv6?** | Ethernet EtherType | `0x86DD` |
| **Is the Ethernet payload ARP?** | Ethernet EtherType | `0x0806` |
| **Does IPv4 carry ICMP?** | IPv4 Protocol | `1` |
| **Does IPv4 carry TCP?** | IPv4 Protocol | `6` |
| **Does IPv4 carry UDP?** | IPv4 Protocol | `17` |
| **Is this an ICMP Echo Request?** | ICMP Type | `8` |
| **Is this an ICMP Echo Reply?** | ICMP Type | `0` |

> **Remember:** EtherType and the IPv4 Protocol field are not the same field. They operate at different levels of encapsulation.

---

## 11. Packet Tracer Practical Inspection Checklist

When inspecting a packet in Simulation Mode:
- [ ] Identify the source and destination devices.
- [ ] Open the packet's PDU Information.
- [ ] Examine the OSI Model tab.
- [ ] Examine the Ethernet II header.
- [ ] Identify the source and destination MAC addresses.
- [ ] Identify the EtherType.
- [ ] Examine the IPv4 header.
- [ ] Identify the source and destination IP addresses.
- [ ] Identify the IPv4 Protocol number.
- [ ] If ICMP, identify the message Type.
- [ ] If TCP, inspect the TCP header.
- [ ] If UDP, inspect the UDP header.
- [ ] Follow the packet through the network.
- [ ] Observe the response and compare the return packet.

---

## 12. Knowledge Review

Before moving on, make sure you can answer these questions:
1. What does EtherType identify in an Ethernet II frame?
2. What does EtherType `0x0800` mean?
3. Which IPv4 Protocol number identifies ICMP?
4. Which IPv4 Protocol number identifies TCP?
5. Which IPv4 Protocol number identifies UDP?
6. What ICMP Type identifies an Echo Request?
7. What ICMP Type identifies an Echo Reply?
8. Why can't we identify TCP or UDP by examining EtherType alone?
9. What is the difference between encapsulation and decapsulation?
10. Where would you look to identify a TCP destination port?

---

## 13. Final Summary

Our Packet Tracer exercises taught us to inspect actual network traffic instead of relying only on theory.

The key identification sequence is:
```text
Ethernet II Header
       |
       |-- EtherType: 0x0800
       |   IPv4
       |
       v
IPv4 Header
       |
       |-- Protocol: 1  = ICMP
       |-- Protocol: 6  = TCP
       |-- Protocol: 17 = UDP
       |
       v
Next Protocol Header
       |
       |-- ICMP Type 8 = Echo Request
       |-- ICMP Type 0 = Echo Reply
       |-- TCP Header
       |-- UDP Header
```

### The most important principle:
Follow the headers from the outside inward. Each header tells you what protocol comes next.

This practical skill connects the OSI model, TCP/IP model, Ethernet, IP addressing and transport protocols. It will also be useful when we later study routing, troubleshooting and packet analysis.

---

* **Document:** Networking Engineering – Phase 1 Practical Reference
* **Subject:** Packet Tracer, Encapsulation and Packet Header Inspection
* **Purpose:** A reusable reference for future networking labs and troubleshooting.
* **Status:** Completed
