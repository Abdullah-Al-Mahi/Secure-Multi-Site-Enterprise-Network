<<<<<<< HEAD
<<<<<<< HEAD
# Validation Results and Evidence
=======
# 04 - Validation Results & Evidence Registry
>>>>>>> c5159ad1b214393349e6ec3c3edd435c629c0210
=======
# 04 — Validation Results & Evidence Registry
>>>>>>> parent of ac86c79 (Improved project presentation and evidence documentation.)

## Evidence policy

This document separates **attached screenshot proof** from **previously reported successful tests**. All tests have been performed during the project workflow, but not every screenshot was supplied for inclusion in this package. The available screenshot files are copied into the evidence folder; missing evidence is not fabricated. Full machine-readable details: `Validation_Matrix.csv`.

## Test matrix

| Test | Scenario | Verified status / evidence |
|---|---|---|
| T01 | HQ IT DHCP lease | Previously verified; source evidence not bundled |
| T02 | Penang HR DHCP lease | Previously verified; source evidence not bundled |
| T03 | HQ VLANs, trunks, router interfaces | Previously verified; source evidence not bundled |
| T04 | HQ OSPF neighbors | Previously verified; source evidence not bundled |
| T05 | Penang learned OSPF routes | Previously verified; source evidence not bundled |
| T06 | Penang Finance opens intranet by name | PASS — screenshot |
| T07 | Johor HR opens intranet by name | PASS — screenshot |
| T08 | HQ HR → HQ Finance routed traffic | Previously verified; source evidence not bundled |
| T09 | Penang HR → Johor Finance ping | PASS — screenshot, gateway unreachable response |
| T10 | HQ Guest → HQ intranet | Previously verified; source evidence not bundled |
| T11 | Johor Guest → HQ protected service | Previously verified; source evidence not bundled |
| T12 | HQ IT SSH to Penang switch | Previously verified; source evidence not bundled |
| T13 | Branch IT SSH to branch router/switch | Previously verified; source evidence not bundled |
| T14 | Unauthorized MAC on secure access port | User-reported PASS; screenshot not bundled |
| T15 | Unused Penang switch Fa0/6 | PASS — port-status screenshot; user reported connectivity test passed |
| T16 | Penang → Johor route traceroute | PASS — screenshot |
| T17 | OSPF route-cost evaluation | PASS — user-shared CLI outputs |
| T18 | ICMP simulation Penang IT → HQ DNS | PASS — simulation screenshots |

## Phase 8 OSPF cost demonstration (T17)

From `R-PEN` route `10.30.30.0/24`, via HQ next hop `172.16.0.1`:

| State | OSPF metric | Neighbor |
|---|---:|---|
| Before change | 3 | FULL/BDR |
| R-PEN Gi0/1 `ip ospf cost 50` | 52 | FULL/BDR |
| `no ip ospf cost` restored | 3 | FULL/BDR |

The WAN path did not change because no alternate Penang–Johor route exists. The test demonstrates metric calculation and reconvergence, not load balancing or redundancy.

## Phase 8 simulation (T18)

Packet Tracer event sequence records ICMP request/reply:

`PC-PEN-IT → SW-PEN-ACCESS → R-PEN → R-HQ → SW-HQ-ACCESS → SW-HQ-SERVER → SRV-HQ-DNS`, returning by the reverse path. A return ICMP reply (ICMP type 0) is visible in the PDU inspection screenshot. This proves simulated forwarding; the event time values are not real network latency benchmarks.

## Recommended documentation screenshots still to add locally

1. A full screenshot of the **final Phase 8 network topology** (`evidence/topology/PacketTracer_Full_Topology.png`).
2. The controlled unauthorized-MAC port-security violation proof from the **separate test copy** (T14), if retained.
3. Any final administrative SSH restriction screenshots if you want to show extra interview evidence.

**Do not include screenshots exposing passwords or secret hashes in a public repository.**
