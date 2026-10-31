# Phase 0 Milestone: ARP and Address Resolution

## 1. Overview

This milestone explored the Address Resolution Protocol (ARP) using Cisco Packet Tracer 9.0.1.

The objective was to understand how devices discover the MAC address associated with an IPv4 address on their local network before communicating over Ethernet.

The practical exercise demonstrated ARP Requests, ARP Replies, broadcast delivery, switch flooding, ARP cache entries, and successful communication after address resolution.

---

## 2. Lab Topology

The simulated network contained:
* PC0
* PC1
* PC2
* Switch0 — Cisco 2960

All three PCs connected to the same switch and belonged to the same IPv4 subnet.

### IPv4 configuration

| Device | IPv4 address | Subnet mask |
| :--- | :--- | :--- |
| **PC0** | `192.168.1.10` | `255.255.255.0` |
| **PC1** | `192.168.1.11` | `255.255.255.0` |
| **PC2** | `192.168.1.12` | `255.255.255.0` |

All devices belonged to:
```text
192.168.1.0/24
```

Default gateways and DNS addresses were left blank because this exercise involved communication within the same subnet.

---

## 3. What Is ARP?

ARP stands for Address Resolution Protocol.

It allows an IPv4 device to discover the MAC address associated with another IPv4 address on its local network.

For example, PC0 knows PC2's IPv4 address:
```text
192.168.1.12
```

However, PC0 also needs PC2's MAC address to construct the Ethernet frame for local delivery. ARP helps PC0 discover that address.

### Important distinction:
* **IPv4 addresses** provide logical addressing at Layer 3.
* **MAC addresses** provide local Ethernet addressing at Layer 2.
* **ARP** helps connect these two addressing systems.

---

## 4. How ARP Works

The ARP exchange follows these steps:
1. PC0 checks its ARP cache for an existing mapping for PC2.
2. If the mapping is missing, PC0 broadcasts an ARP Request.
3. Switch0 floods the broadcast through eligible ports in the same VLAN, excluding the incoming port.
4. PC2 recognizes its IPv4 address in the request.
5. PC2 sends an ARP Reply containing its MAC address.
6. Switch0 forwards the reply towards PC0.
7. PC0 learns and stores the IP-to-MAC mapping in its ARP cache.

### ARP Request
The request asks:
> *"Who has 192.168.1.12?"*

The Ethernet destination MAC address is the broadcast address:
```text
FFFF.FFFF.FFFF
```
This allows the request to reach devices in the local broadcast domain.

### ARP Reply
PC2 responds with its address information. The observed PDU details were:

| Field | Observed value |
| :--- | :--- |
| **Sender IP** | `192.168.1.12` |
| **Sender MAC** | `0060.7034.A540` |
| **Target IP** | `192.168.1.10` |
| **Target MAC** | `0001.43AC.2881` |

The ARP Reply was addressed to PC0. Unlike the broadcast ARP Request, the reply in this observed exchange was directed to the requesting device.

### Observed ARP exchange
```text
PC0
 |
 | ARP Request (Broadcast)
 v
Switch0
 |
 | Floods through eligible ports
 v
PC2
 |
 | ARP Reply (Unicast)
 v
Switch0
 |
 | Forwards reply towards PC0
 v
PC0
```

---

## 5. ARP Cache Verification

After the ARP exchange, PC0's ARP table displayed:

```text
C:\>arp -a

Internet Address      Physical Address      Type
192.168.1.11          00d0.ba68.15b5        dynamic
192.168.1.12          0060.7034.a540        dynamic
```

This confirmed that PC0 had learned PC2's MAC address.

### What does dynamic mean?
A dynamic ARP entry is learned automatically through ARP communication. The device stores the mapping temporarily so it can reuse it for subsequent communication without necessarily sending another ARP Request.

### Important observation
PC0 already had an entry for PC1, but did not initially have an entry for PC2. The PC0-to-PC2 communication therefore provided an opportunity to observe a new ARP resolution exchange.

---

## 6. Connectivity Verification

The following command was executed from PC0:
```text
C:\>ping 192.168.1.12
```

### Result
* **Packets sent:** 4
* **Packets received:** 4
* **Packet loss:** 0%
* **Connectivity:** Successful

This confirmed successful communication between PC0 and PC2. The successful ping followed the ARP resolution process that allowed PC0 to obtain PC2's MAC address.

---

## 7. Important Networking Concepts Learned

* **ARP:** Resolves an IPv4 address to a MAC address for local network delivery.
* **Broadcast:** A frame addressed to all devices in the local broadcast domain.
* **Unicast:** Communication directed to one specific destination.
* **ARP Cache:** A temporary table containing learned IPv4-to-MAC mappings.
* **Switch Flooding:** A switch forwards broadcast frames through eligible ports in the same VLAN, except the port on which the frame arrived.
* **Ethernet and IP:**
  * IP addresses identify devices logically at Layer 3.
  * MAC addresses are used for local Ethernet delivery at Layer 2.
  * ARP connects IPv4 addressing with Ethernet addressing.

---

## 8. Important Limitations Observed

The Packet Tracer PC command prompt did not support the following command:
```text
arp -d 192.168.1.11
```

It returned:
```text
Invalid Command.
```

The simulated PC supports a limited command set. A fresh PC2 device was therefore introduced to observe a new ARP exchange without relying on unsupported ARP cache deletion commands.

The exact ARP cache aging timer was not tested.

> **Practical lesson:** Not every command available on a real operating system is necessarily supported by Packet Tracer's simulated command prompt.

---

## 9. Knowledge Check Results

All three questions were answered correctly.

| Question | Correct answer | Result |
| :--- | :--- | :--- |
| What does ARP resolve? | B — IPv4 address into MAC address | Correct |
| Why does PC0 broadcast an ARP Request? | A — It does not know the destination MAC address | Correct |
| What happens after PC0 learns PC2's MAC address? | A — It stores the mapping in its ARP cache | Correct |

**Score:** 3/3 — 100%.

---

## 10. Lessons Learned

By completing this exercise, I demonstrated that:
* Devices need destination MAC addresses for local Ethernet delivery.
* ARP Requests use broadcast addressing.
* ARP Replies are normally sent using unicast addressing.
* Switches flood broadcast frames within the same VLAN.
* Devices store learned mappings in their ARP caches.
* ARP resolution enables subsequent IPv4 communication over Ethernet.
* Packet Tracer may not support every command available on a real operating system.

---

## 11. Project Files

### Packet Tracer project
* `phase-0-first-lan.pkt`

### Previous milestone reports
* `phase-0-network-checks.md`
* `phase-0-first-lan-report.md`
* `phase-0-switching-and-mac-learning.md`
* `phase-0-ethernet-frames-and-forwarding.md`

### Current milestone
* `phase-0-arp-and-address-resolution.md`

---

## 12. Phase 0 Progress

* [x] Environment readiness checks
* [x] Windows network configuration inspection
* [x] Gateway connectivity test
* [x] External IP connectivity test
* [x] DNS resolution test
* [x] First simulated LAN
* [x] Static IPv4 configuration
* [x] PC-to-PC connectivity
* [x] Switch MAC address learning
* [x] Ethernet frames and forwarding
* [x] Simulation Mode inspection
* [x] ARP Request and ARP Reply
* [x] ARP cache verification
* [x] Successful PC0-to-PC2 connectivity test
* [x] IPv4 addressing and subnetting practical
* [x] Phase 0 practical assessment
* [ ] Complete remaining documentation reviews
* [ ] Prepare Phase 0 portfolio materials
* [ ] Complete final Phase 0 review

Phase 0 remains in progress until documentation review, portfolio preparation, and final verification are complete.

---

**Networking Engineering Learning Portfolio — Phase 0**
**Milestone:** ARP and Address Resolution
**Status:** Completed learning milestone; portfolio review in progress.
