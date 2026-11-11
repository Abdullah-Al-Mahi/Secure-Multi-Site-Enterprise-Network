# Secure Multi-Site Enterprise Network
### VLAN Segmentation, OSPF Routing, Centralized Network Services & Layered Security

**Simulation:** Cisco Packet Tracer 9.0.0.0810 · Cisco 2911 routers · Cisco 2960-24TT-L switches  
**Scenario:** Meridian Technologies Malaysia (fictional enterprise)  
**Sites:** Headquarters — Kuala Lumpur; branches — Penang and Johor Bahru  
**Project scope:** Educational, simulated network. Not a production deployment or Internet-connected infrastructure.

![Logical network topology](diagrams/Final_Topology.png)

## Project overview

Designed and implemented a three-site enterprise topology using subnetted departmental VLANs, 802.1Q trunks, router-on-a-stick inter-VLAN routing, single-area OSPF, per-site DHCP, and shared DNS/HTTP services hosted at HQ. Enforced routed ACLs, SSH-only management restricted to HQ IT, sticky-MAC port security, and unused-port isolation.

## At a glance

| Component | Implemented |
|---|---|
| Sites | 3 (HQ, Penang, Johor Bahru) |
| Routers | 3 × Cisco 2911 |
| Switches | 4 × Cisco 2960-24TT-L |
| Endpoints | 15 PCs, 2 servers |
| VLAN design | 10 IT, 20 HR, 30 Finance, 40 Management, 50 Guest; HQ VLAN 60 servers; 99 management; 998 native reserve; 999 parking |
| WAN | HQ–Penang and HQ–Johor Bahru, each simulated via a /30 point-to-point Ethernet link |
| Routing | OSPF process 1, area 0; neighbor adjacencies across WAN only |
| Addressing | Local /24 subnets, /30 WANs; router-based client DHCP |
| Services | HQ DNS `10.10.60.10`; HQ HTTP intranet `10.10.60.20` |
| Security | Extended router ACLs, SSH v2 and VTY ACL, access-port sticky-MAC policy, shutdown unused ports |
| Port security | 17 PC/server access interfaces configured and verified |
| Unused ports | 82 planned interfaces assigned VLAN 999 and shutdown |

## Key outcomes

- Demonstrated OSPF adjacency and dynamic HQ↔branch routing; Penang-to-Johor traceroute traverses HQ (172.16.0.1) then Johor (172.16.0.6).
- Verified DNS-name resolution and HTTP intranet access from allowed departments across offices.
- Demonstrated blocked inter-department traffic: Penang HR → Johor Finance, as well as guest isolation and management restrictions in prior live tests.
- Verified allowed HQ IT administrative SSH and denied unauthorized branch IT SSH according to the management source policy.
- Verified sticky MAC learning on all 17 access ports; shutdown behavior is documented as a separate controlled security test.
- Evaluated OSPF route cost on R-PEN: metric **3 → 52 → 3** after change/restoration, with the neighbor maintaining FULL adjacency.
- Demonstrated an ICMP round trip in Packet Tracer Simulation Mode from Penang IT to HQ DNS via the HQ network.

See [validation and evidence](docs/04_Validation_and_Evidence.md) for which results are backed by files in this package and which were previously confirmed interactively.

## Architecture and documentation

| Resource | Location |
|---|---|
| Logical network diagram, editable Graphviz source | [`diagrams/Final_Topology.dot`](diagrams/Final_Topology.dot) |
| Architecture and device/cable map | [`docs/01_Architecture.md`](docs/01_Architecture.md) |
| IP addressing and VLAN reference | [`docs/02_IP_Addressing_and_VLANs.md`](docs/02_IP_Addressing_and_VLANs.md) |
| Security policy and implementation | [`docs/03_Security_Controls.md`](docs/03_Security_Controls.md) |
| Validation matrix and test proof | [`docs/04_Validation_and_Evidence.md`](docs/04_Validation_and_Evidence.md) |
| Export and publishing safety | [`docs/05_Safe_Configuration_Export.md`](docs/05_Safe_Configuration_Export.md) |
| Phase 9 completion checklist | [`docs/06_Phase9_Completion_Checklist.md`](docs/06_Phase9_Completion_Checklist.md) |

## Open in Packet Tracer

1. Install **Cisco Packet Tracer 9.x** (original network built in 9.0.0.0810).
2. Open the final validated `.pkt` file locally from the project `02_Implementation` folder.
3. Switch to Realtime mode for ordinary operation; use Simulation mode for packet-flow evidence.
4. For key tests, see the matrix in `docs/04_Validation_and_Evidence.md`. Device configuration must be saved (`copy running-config startup-config`) before final `.pkt` saving.

**Important:** The `.pkt` file is **not bundled in this documentation-only package** because Packet Tracer projects may contain account hashes, passwords, RSA private keys or other credential material. Do not publish the original simulation or raw device configuration exports until secrets and keys have been sanitized and checked. The full simulation is intended for controlled local demonstrations.

## Scope and limitations

- WAN connections use Ethernet links to simulate private carrier circuits; this does not replicate real Malaysian intercity latency or service reliability.
- There is no ISP, Internet connectivity, NAT, VPN encryption, redundant WAN, or dedicated stateful firewall in the verified baseline.
- IPv4 router ACLs are stateless; demonstrated policy controls apply to routed flows, not same-VLAN device-to-device switching.
- Packet Tracer's event timestamps are simulated timing, **not** benchmarks of throughput, real-world delay, jitter, or SLA compliance.


## Project status

**Status: Completed — Phases 0–10**

Designed, implemented, secured, tested, and documented a three-site enterprise network using Cisco Packet Tracer.

The project includes VLAN segmentation, OSPF routing, DHCP, DNS, HTTP, ACL enforcement, SSHv2 administration, port security, and network validation.

The original Packet Tracer `.pkt` file is kept private because it contains administrative credential material and cryptographic keys.

This public repository contains the network architecture, technical documentation, validation results, and test evidence.

