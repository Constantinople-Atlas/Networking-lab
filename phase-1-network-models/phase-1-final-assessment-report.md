# Phase 1 Final Assessment Report

* **Network Models and Encapsulation**
* **Status:** Completed 🏆

---

## 1. Assessment Overview

* **Phase:** Phase 1 — Network Models and Encapsulation
* **Assessment Type:** Multiple-choice knowledge assessment
* **Total Questions:** 15
* **Correct Answers:** 15
* **Incorrect Answers:** 0
* **Final Score:** 15/15 (100%)
* **Result:** Passed

This assessment evaluated my understanding of network models, encapsulation, networking devices, VLANs, trunk links, routing, and practical network troubleshooting.

---

## 2. Assessment Objectives

The assessment tested my ability to:
* Identify the functions of the seven OSI layers.
* Understand the TCP/IP model and how it relates to OSI.
* Explain encapsulation and decapsulation.
* Identify protocol data units at different layers.
* Understand the roles of switches, routers, and VLANs.
* Explain 802.1Q trunking.
* Understand Router-on-a-Stick inter-VLAN routing.
* Interpret basic network behaviour using Packet Tracer evidence.
* Identify common configuration errors.

---

## 3. Assessment Results

| No. | Knowledge Area | Result |
| :--- | :--- | :--- |
| 1 | OSI Network Layer | Correct |
| 2 | Ethernet and MAC Address Forwarding | Correct |
| 3 | Data Encapsulation | Correct |
| 4 | Network Layer PDU | Correct |
| 5 | Layer 2 Switching | Correct |
| 6 | VLANs and Broadcast Domains | Correct |
| 7 | 802.1Q Trunking | Correct |
| 8 | Router-on-a-Stick Subinterfaces | Correct |
| 9 | Inter-subnet Communication | Correct |
| 10 | VLAN Broadcast Behaviour | Correct |
| 11 | TTL and Router Hops | Correct |
| 12 | Unknown Unicast Flooding | Correct |
| 13 | Default Gateway Configuration | Correct |
| 14 | Verifying Trunk Configuration | Correct |
| 15 | Troubleshooting VLAN Encapsulation | Correct |

**Final result:** 15/15 — 100%.

---

## 4. Practical Knowledge Demonstrated

The assessment confirmed my understanding of several important networking principles.

### OSI and Encapsulation
* The Network layer is responsible for logical addressing and routing.
* Ethernet switches use MAC addresses to forward frames.
* Encapsulation adds protocol information as data moves down the network stack.
* A packet is the Network layer PDU.

### Switching and VLANs
* VLANs create separate Layer 2 broadcast domains.
* An access port normally belongs to one VLAN.
* An 802.1Q trunk carries traffic for multiple VLANs using VLAN tags.
* Unknown unicast frames are flooded within the relevant VLAN, excluding the incoming port.

### Routing
* Devices in different IP subnets communicate through a router or Layer 3 device.
* The default gateway provides a path outside the local subnet.
* Router-on-a-Stick uses router subinterfaces to route between VLANs over one physical trunk connection.
* Each subinterface must use the correct VLAN ID in its `encapsulation dot1Q` configuration.

### Troubleshooting
I demonstrated the ability to recognize problems involving:
* Incorrect default gateways.
* Incorrect VLAN assignments.
* Trunk configuration.
* Incorrect 802.1Q VLAN IDs.
* Communication between different subnets.

---

## 5. Connection to the Practical Lab

The final assessment reinforced the concepts applied in my Packet Tracer practical lab.

### Lab Components
* Three PCs.
* A Cisco Catalyst 2960 switch.
* A Cisco 2911 router.
* VLAN 10 and VLAN 20.
* An 802.1Q trunk between the switch and router.
* Router-on-a-Stick inter-VLAN routing.
* Separate IPv4 subnets for the two VLANs.

### Test Results
The practical tests demonstrated successful communication:

| Test | Result |
| :--- | :--- |
| **PC0 to PC1 (different VLANs)** | Successful after initial timeout |
| **PC1 to PC0** | 4/4 successful |
| **PC2 to PC0** | 4/4 successful |
| **PC1 to PC2 (same VLAN)** | 4/4 successful |

The successful tests provided practical evidence that the VLAN and inter-VLAN routing configuration was working.

---

## 6. Final Assessment Reflection

Achieving 15/15 demonstrates that I can correctly apply the fundamental concepts covered in Phase 1.

I can explain how network communication is organized through layers, how data is encapsulated, how switches and routers operate, and how VLANs and trunk links support network segmentation.

I also understand how to identify common configuration mistakes and relate theoretical concepts to practical Packet Tracer results.

---

## 7. Phase 1 Completion

* **Phase 1:** Network Models and Encapsulation — **COMPLETED**

### Topics Completed
1. Introduction to Network Models.
2. OSI Model.
3. TCP/IP Model.
4. OSI vs TCP/IP.
5. Data Encapsulation and Decapsulation.
6. Protocol Data Units (PDUs).
7. Networking Devices and Their Layers.
8. Practical Packet Tracer Lab and Final Assessment.

* **Final assessment score:** 100%.
* **Overall status:** Successfully completed Phase 1.

---

## 8. Next Learning Phase

The next stage of the Networking Engineering curriculum will build on these foundations and introduce the next set of networking concepts through progressive explanations, practical exercises, and knowledge checks.

---

*End of Phase 1 Final Assessment Report.*
