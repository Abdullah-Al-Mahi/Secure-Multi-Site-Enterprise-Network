# Validation Results and Evidence

I checked the network in stages as I configured routing, services and security. The table below distinguishes screenshots included in this repository from tests that were completed earlier but do not have a separate image attached.

| Test | What I checked | Evidence / result |
|---|---|---|
| T01 | HQ IT receives a DHCP lease | Verified during setup; no screenshot included |
| T02 | Penang HR receives a DHCP lease | Verified during setup; no screenshot included |
| T03 | HQ VLANs, trunks and router interfaces | Verified during setup; no screenshot included |
| T04 | HQ OSPF neighbors | Verified during setup; no screenshot included |
| T05 | Penang learns OSPF routes | Verified during setup; no screenshot included |
| T06 | Penang Finance opens the intranet by hostname | [PASS — screenshot](../evidence/phase7/T06_PEN_FIN_Intranet.png) |
| T07 | Johor HR opens the intranet by hostname | [PASS — screenshot](../evidence/phase7/T07_JHB_HR_Intranet.png) |
| T08 | HQ HR traffic to HQ Finance is restricted | Verified during security testing; no screenshot included |
| T09 | Penang HR cannot ping Johor Finance | [PASS — screenshot](../evidence/phase7/T09_PEN_HR_to_JHB_FIN_Blocked.png) |
| T10 | HQ Guest cannot access the protected HQ intranet | Verified during security testing; no screenshot included |
| T11 | Johor Guest cannot access protected HQ services | Verified during security testing; no screenshot included |
| T12 | HQ IT can use authorized SSH management | Verified during security testing; no screenshot included |
| T13 | Branch IT is denied unauthorized SSH management | Verified during security testing; no screenshot included |
| T14 | Unauthorized MAC causes port-security shutdown | Test reported as passed; screenshot not included |
| T15 | Unused Penang switch port Fa0/6 is shut down | [PASS — port-status screenshot](../evidence/phase7/T15_Unused_Port_Shutdown.png); connectivity test also reported as passed |
| T16 | Penang-to-Johor traceroute follows the HQ path | [PASS — screenshot](../evidence/phase7/T16_PEN_to_JHB_Traceroute.png) |
| T17 | OSPF cost changes and returns to normal | [PASS — CLI results](../evidence/phase8/T17_OSPF_Cost_Evaluation.txt) |
| T18 | ICMP request/reply follows the expected route | [PASS — simulation events](../evidence/phase8/T18_ICMP_Simulation_EventList.png) and [reply PDU](../evidence/phase8/T18_ICMP_Reply_PDU.png) |

## OSPF route-cost test (T17)

On `R-PEN`, the route to `10.30.30.0/24` continued through HQ (`172.16.0.1`). I changed the OSPF cost on `GigabitEthernet0/1` to 50, observed the route metric change from **3 to 52**, then removed the override and confirmed it returned to **3**. The HQ OSPF neighbor stayed in FULL state.

The path did not change because this topology has no alternate Penang-to-Johor link.

## ICMP simulation test (T18)

Packet Tracer showed an ICMP exchange between Penang IT and the HQ DNS server:

`PC-PEN-IT → SW-PEN-ACCESS → R-PEN → R-HQ → SW-HQ-ACCESS → SW-HQ-SERVER → SRV-HQ-DNS`

The reply returned across the same network. This demonstrates packet forwarding in the simulator, not a real-world latency or throughput benchmark.

## Final topology

The [actual Packet Tracer workspace screenshot](../evidence/topology/PacketTracer_Full_Topology.png) is included, alongside the [logical network diagram](../diagrams/Final_Topology.png).

The original `.pkt` file and full raw device configuration exports are kept private because they can contain credential and key material. No test evidence has been invented to fill missing screenshots.
