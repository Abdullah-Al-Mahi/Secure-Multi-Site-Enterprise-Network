# 01 — Architecture & Physical Interface Map

## Business context
A fictional enterprise operates a Headquarters in Kuala Lumpur, plus Penang and Johor Bahru branches. HQ hosts centralized intranet and DNS services; branch endpoints reach approved services over simulated routed WAN links.

## Device inventory
- **Routers:** R-HQ, R-PEN, R-JHB (Cisco 2911)
- **Switches:** SW-HQ-ACCESS, SW-HQ-SERVER, SW-PEN-ACCESS, SW-JHB-ACCESS (Cisco 2960-24TT-L)
- **Clients:** PC-{HQ|PEN|JHB}-{IT|HR|FIN|MGT|GUEST} (15 PCs)
- **Servers:** SRV-HQ-DNS (`10.10.60.10`), SRV-HQ-WEB (`10.10.60.20`)

## Network structure
- **HQ:** Router-on-a-stick R-HQ Gi0/0 → SW-HQ-ACCESS Gi0/1; HQ access switch Gi0/2 → SW-HQ-SERVER Gi0/1 (trunk); two servers on access VLAN 60.
- **Penang:** R-PEN Gi0/0 → SW-PEN-ACCESS Gi0/1 (trunk); five PCs in VLANs 10/20/30/40/50.
- **Johor:** R-JHB Gi0/0 → SW-JHB-ACCESS Gi0/1 (trunk); five PCs in VLANs 10/20/30/40/50.
- **WAN hub-and-spoke:** R-HQ ↔ R-PEN; R-HQ ↔ R-JHB. There is no direct Penang↔Johor WAN link; inter-branch traffic transits HQ.

## Interface and cable map

| Device A | Interface | Device B | Interface | Function |
|---|---|---|---|---|
| R-HQ | Gi0/0 | SW-HQ-ACCESS | Gi0/1 | 802.1Q trunk, native VLAN 998 |
| SW-HQ-ACCESS | Gi0/2 | SW-HQ-SERVER | Gi0/1 | 802.1Q trunk, native VLAN 998 |
| R-HQ | Gi0/1 | R-PEN | Gi0/1 | WAN `172.16.0.0/30` |
| R-HQ | Gi0/2 | R-JHB | Gi0/1 | WAN `172.16.0.4/30` |
| R-PEN | Gi0/0 | SW-PEN-ACCESS | Gi0/1 | 802.1Q trunk, native VLAN 998 |
| R-JHB | Gi0/0 | SW-JHB-ACCESS | Gi0/1 | 802.1Q trunk, native VLAN 998 |
| SW-HQ-SERVER | Fa0/1 | SRV-HQ-DNS | Fa0 | Access VLAN 60 |
| SW-HQ-SERVER | Fa0/2 | SRV-HQ-WEB | Fa0 | Access VLAN 60 |

Each of the three access switches uses Fa0/1–Fa0/5 for IT, HR, Finance, Management, and Guest PCs respectively. Router-facing and inter-switch trunks are not subject to access-port port security.

## Routing design
- Single OSPF process **1**, area **0**
- Router IDs: `1.1.1.1` HQ; `2.2.2.2` Penang; `3.3.3.3` Johor
- Branches each form one OSPF adjacency with HQ; HQ forms two
- LAN-facing router subinterfaces passive; WAN interfaces non-passive
- Dynamic route test: Penang → Johor via HQ

## Design boundaries
The WAN uses Ethernet links as a **simulation** of private site connections. Network includes no ISP, NAT, firewall appliance, WAN encryption, failover path or proven carrier-grade performance. See the policy and limitations sections for details.

Editable and rendered diagrams: `../diagrams/Final_Topology.dot`, `.svg`, `.png`.
