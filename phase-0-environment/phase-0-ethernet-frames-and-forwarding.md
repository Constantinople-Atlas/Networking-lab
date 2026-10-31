# Phase 0 — Ethernet Frames and Switch Forwarding (Simulation Mode)

## Lab Status

* **Status:** Completed
* **Tool:** Cisco Packet Tracer 9.0.1
* **Topology:** PC0 — Switch0 (Cisco 2960) — PC1
* **Track:** Networking Engineering

---

## 1. Objective

Understand how Ethernet frames travel through a Layer 2 switch and how the switch uses its MAC address table to forward frames to their destinations.

The exercise also demonstrates the relationship between MAC addresses, IP addresses, ICMP, and Ethernet frames.

---

## 2. Lab Topology

Three devices were used:
* PC0
* Switch0 — Cisco 2960
* PC1

### Connections

| Device | Interface | Connected to |
| :--- | :--- | :--- |
| **PC0** | FastEthernet0 | Switch0 `Fa0/1` |
| **PC1** | FastEthernet0 | Switch0 `Fa0/2` |

Both connections were operational, indicated by green link indicators.

### Topology representation
```text
PC0 -------- Switch0 -------- PC1
             Cisco 2960
```

---

## 3. IP Address Configuration

| Device | IPv4 address | Subnet mask |
| :--- | :--- | :--- |
| **PC0** | `192.168.1.10` | `255.255.255.0` |
| **PC1** | `192.168.1.11` | `255.255.255.0` |

Both devices belong to the same IPv4 subnet:
```text
192.168.1.0/24
```

No default gateway or DNS server was required for the direct IP communication tested in this isolated LAN.

---

## 4. Ethernet Frame Fundamentals

An Ethernet frame is the Layer 2 data unit used to transport data across an Ethernet network.

### Important Ethernet frame fields include:

| Field | Purpose |
| :--- | :--- |
| **Destination MAC** | Identifies the intended receiving interface |
| **Source MAC** | Identifies the sending interface |
| **EtherType** | Identifies the encapsulated protocol |
| **Payload** | Carries the encapsulated data |
| **FCS** | Used for frame error detection |

### How a switch uses these fields
* A switch learns source MAC addresses from incoming Ethernet frames.
* When forwarding a known unicast frame, it checks the destination MAC address against its MAC address table to determine the appropriate outgoing port.
* The switch does not use the destination IP address as its normal Layer 2 forwarding decision.

---

## 5. MAC Address Verification

The MAC addresses were inspected through each PC's:
```text
Config → FastEthernet0
```

### Observed MAC addresses

| Device | MAC address |
| :--- | :--- |
| **PC0** | `0001.43AC.2881` |
| **PC1** | `00D0.BA68.15B5` |

These matched the MAC addresses previously learned by Switch0.

---

## 6. Switch MAC Address Table

The following command was used:
```text
show mac-address-table
```

### Observed entries

| VLAN | MAC address | Type | Port |
| :--- | :--- | :--- | :--- |
| 1 | `0001.43ac.2881` | DYNAMIC | `Fa0/1` |
| 1 | `00d0.ba68.15b5` | DYNAMIC | `Fa0/2` |

### Interpretation
* Both devices were learned in VLAN 1.
* PC0's MAC address was associated with `Fa0/1`.
* PC1's MAC address was associated with `Fa0/2`.
* **DYNAMIC** means the switch learned the entries automatically from incoming traffic.

This table helps the switch forward Ethernet frames through the correct port when it knows the destination MAC address.

---

## 7. Simulation Mode: PC0 to PC1

Simulation Mode was enabled in Packet Tracer. The ICMP filter was selected, and the following command was executed on PC0:
```text
ping 192.168.1.11
```

The event was inspected using PDU Information.

### Outbound PDU Details
* **Source MAC:** `0001.43AC.2881`
* **Destination MAC:** `00D0.BA68.15B5`

The packet was advanced through the topology using Capture/Forward.

### Observed journey
1. PC0 generated the traffic.
2. The frame moved to Switch0.
3. Switch0 examined the destination MAC address.
4. Switch0 used its MAC address table to identify `Fa0/2`.
5. The frame was forwarded through `Fa0/2`.
6. PC1 received the frame.

This demonstrated known unicast forwarding.

---

## 8. Simulation Mode: PC1 to PC0

The reverse direction was tested using:
```text
ping 192.168.1.10
```

### Outbound PDU Details
* **Source MAC:** `00D0.BA68.15B5`
* **Destination MAC:** `0001.43AC.2881`

The frame was observed travelling through:
```text
PC1 → Switch0 → PC0
```

This confirmed that the source and destination MAC addresses reverse when the communication direction changes. The switch used its MAC address table to forward the frame towards PC0.

---

## 9. Important Networking Concepts Learned

* **Source MAC versus Destination MAC:** Source MAC identifies the sending interface; destination MAC identifies the intended receiving interface.
* **MAC Address Learning:** A switch learns the source MAC address of an incoming Ethernet frame and records the interface on which it arrived.
* **Frame Forwarding:** A switch consults its MAC address table to determine the appropriate outgoing interface for a known destination MAC address.
* **Unknown Unicast Flooding:** If a destination MAC address is not in the MAC address table, the switch floods the frame through eligible ports in the same VLAN, excluding the incoming port.
* **Layer 2 versus Layer 3:** Layer 2 uses MAC addresses for Ethernet frame delivery; Layer 3 uses IP addresses for logical addressing and routing between networks.
* **ICMP and Ethernet:** The ping command uses ICMP. The ICMP message is carried inside an IP packet, which is then encapsulated inside an Ethernet frame for transmission over the local Ethernet network.

---

## 10. Knowledge Check Results

| Question | Correct answer | Result |
| :--- | :--- | :--- |
| What does a switch use to forward a known frame? | Destination MAC | Correct |
| What happens when the destination MAC is unknown? | Flooding within the VLAN | Correct |
| Why did the switch use Fa0/2 for PC1? | MAC table association | Correct |
| Which address identifies the sender? | Source MAC | Correct |
| Which address identifies the intended receiver? | Destination MAC | Correct |

**Result:** 5/5 — All five knowledge checks answered correctly.

---

## 11. Limitations and Observations

* The Packet Tracer switch image did not accept the attempted `show mac address-table aging-time` command.
* The exact MAC address aging timer was therefore not verified.
* The simulation demonstrated frame forwarding in both directions.
* The exercise used a simulated isolated LAN, not the physical home network.
* The observed forwarding behaviour was consistent with the switch's MAC address table.

---

## 12. Phase 0 Progress

* [x] Environment readiness checks
* [x] Windows network configuration inspection
* [x] Gateway connectivity test
* [x] External IP connectivity test
* [x] DNS resolution test
* [x] Packet Tracer installation and launch
* [x] First LAN topology
* [x] Static IPv4 configuration
* [x] PC-to-PC connectivity
* [x] Switch MAC address learning
* [x] Ethernet frame fundamentals
* [x] Simulation Mode inspection
* [x] Two-way frame forwarding
* [x] ARP and address resolution practical
* [x] IPv4 addressing and subnetting practical
* [x] Phase 0 practical assessment
* [ ] Complete remaining documentation reviews
* [ ] Prepare Phase 0 portfolio materials
* [ ] Complete final Phase 0 review

Phase 0 remains in progress until documentation review, portfolio preparation, and final verification are complete.

---

## 13. Next Learning Topic: ARP — Address Resolution Protocol

The next lesson explains how a computer discovers the MAC address associated with an IPv4 address on its local network.

### Topics investigated:
* Why ARP is needed.
* ARP Requests and ARP Replies.
* Broadcast MAC addresses.
* ARP caches.
* How ARP and Ethernet work together.

The ARP practical exercise and its documentation have since been completed as a separate milestone.

---

*End of milestone report.*
**Document:** Phase 0 — Ethernet Frames and Switch Forwarding
**Status:** Completed learning milestone; portfolio review in progress.
