# Phase 0 — Network Connectivity Checks

## 1. Objective

Learn how to inspect a Windows network configuration and test local connectivity, external IP connectivity, and DNS resolution.

---

## 2. Environment

* **Operating system:** Windows 10 Pro 22H2, 64-bit
* **Connection type:** Wi-Fi
* **Wireless adapter:** Intel(R) Dual Band Wireless-AC 7260
* **Lab stage:** Phase 0 — Environment Readiness and Foundations

---

## 3. Network Configuration Observed

> *The following values use documentation-safe example addresses rather than the original home-network addresses.*

| Setting | Example value |
| :--- | :--- |
| **DHCP enabled** | Yes |
| **IPv4 address** | `192.168.10.25` |
| **Subnet mask** | `255.255.255.0` (`/24`) |
| **Default gateway** | `192.168.10.1` |
| **DHCP server** | `192.168.10.1` |
| **Configured DNS server** | `192.168.10.1` |
| **IPv6 link-local address** | Present |

> **Privacy note:** Private network addresses and device identifiers have been replaced with illustrative values for public documentation.

---

## 4. Tests Performed

### Test 1 — Inspect IP Configuration
* **Command:** `ipconfig`
* **Result:** The computer's IPv4 address, subnet mask, and default gateway were identified.

### Test 2 — Test the Default Gateway
* **Example command:** `ping 192.168.10.1`
* **Result:**
  * Replies received: 4 out of 4
  * Packet loss: 0%
  * Approximate average round-trip time: 0 ms, as reported by Windows
* **Conclusion:** The original gateway responded to this short ping test, confirming successful ICMP communication with the local gateway during the test.

### Test 3 — Test an External IP Address
* **Command:** `ping 1.1.1.1`
* **Result:**
  * Replies received: 4 out of 4
  * Packet loss: 0%
  * Average round-trip time: 182 ms
* **Conclusion:** The external IP address responded to this short ping test, demonstrating successful IP connectivity to that destination at the time of testing.

### Test 4 — Test DNS Resolution
* **Command:** `nslookup example.com`
* **Result:**
  * DNS server: represented as `Home-Router` in this public document
  * DNS server address: `192.168.10.1` (illustrative)
  * IPv4 and IPv6 addresses were returned for `example.com`.
* **Conclusion:** The original DNS lookup succeeded. The configured DNS server successfully returned address records for the requested domain.

### Test 5 — Inspect Detailed Network Configuration
* **Command:** `ipconfig /all`
* **Result:**
  * DHCP was enabled.
  * A DHCP server was reported.
  * The default gateway and configured DNS server were reported.
  * A DHCP lease obtained time and expiration time were displayed.
  * An IPv6 link-local address was present.
* **Conclusion:** Windows was configured to obtain network settings automatically through DHCP, including its IPv4 address and other network configuration options.

---

## 5. Concepts Learned

* **IPv4 Address:** An IPv4 address logically identifies a network interface within an IP network.
* **Subnet Mask:** A subnet mask identifies the network prefix and host portion of an IPv4 address.
* **Default Gateway:** A default gateway forwards traffic toward destinations outside the local network.
* **Ping:** Ping uses ICMP Echo Request and Echo Reply messages to test whether a destination responds. A successful response confirms that the destination responded to those requests, but it does not guarantee that every network service is available.
* **DNS:** The Domain Name System resolves domain names into IP addresses, such as looking up the address associated with `example.com`.
* **DHCP:** Dynamic Host Configuration Protocol automatically provides network configuration information, such as IPv4 address, subnet mask, default gateway, and DNS server.
* **DORA:** DORA describes the four main stages of DHCP address assignment:
  1. Discover
  2. Offer
  3. Request
  4. Acknowledge
* **IPv6 Link-Local Address:** IPv6 link-local addresses use the `fe80::/10` address range and support communication on the local network segment. They are not globally routable internet addresses.

---

## 6. Limitations

* Successful ping tests do not guarantee that every network service is working.
* A successful DNS lookup does not guarantee that a website can be reached.
* The tests were short snapshots of connectivity, not continuous monitoring.
* DHCP messages were not captured; the DHCP process was studied conceptually.
* No network settings were changed during these tests.

---

## 7. Troubleshooting Notes

No connectivity failures were observed in the tests recorded above.

If a future test fails, record:
1. The command used.
2. The exact output or error.
3. The time of the test.
4. What was checked next.
5. The confirmed cause, if identified.
6. The solution and verification result, if applicable.

---

## 8. Progress

* [x] Inspect basic IP configuration.
* [x] Test gateway reachability.
* [x] Test external IP reachability.
* [x] Test DNS resolution.
* [x] Inspect detailed Windows network configuration.
* [x] Complete the remaining Phase 0 learning and practical assessment.
* [x] Complete Phase 0 documentation review.
* [x] Review and sanitize documentation for the public portfolio.

---

## 9. Safety and Change-Control Notes

* No router settings were changed during these checks.
* No virtual machines were started during these checks.
* No network configuration changes were made.
* All tests were limited to observing the existing network configuration and testing connectivity.

---

**Document:** Phase 0 — Network Connectivity Checks
**Track:** Networking Engineering
**Status:** Completed learning milestone; portfolio preparation in progress.

> *Documentation note: Example local addresses are used in this public version. Original test outcomes are retained, but the example addresses should not be interpreted as the exact addresses used during the original tests.*
