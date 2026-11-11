# 02 - IP Addressing and VLAN Reference

## VLAN roles

| VLAN | Name | Usage |
|---:|---|---|
| 10 | IT | IT clients (only HQ IT authorized for device SSH) |
| 20 | HR | Human Resources clients |
| 30 | FINANCE | Finance clients |
| 40 | MANAGEMENT | Corporate management staff clients |
| 50 | GUEST | Restricted guest clients |
| 60 | SERVERS | HQ DNS and HTTP servers only |
| 99 | NET-MGMT | Network device management addresses |
| 998 | NATIVE-UNUSED | Reserved trunk native VLAN; no user endpoints |
| 999 | PARKING | Disabled unused access ports |

**Subnet mask:** client, server, and management VLANs `/24` (`255.255.255.0`); WAN `/30` (`255.255.255.252`). VLAN IDs repeat across sites but do not imply a shared Layer 2 broadcast domain.

## Routed VLANs

| Site | VLAN | Subnet | Gateway | Allocation |
|---|---|---|---|---|
| HQ | 10 (IT) | `10.10.10.0/24` | `10.10.10.1` | Router DHCP (`.100`–`.254`) |
| HQ | 20 (HR) | `10.10.20.0/24` | `10.10.20.1` | Router DHCP (`.100`–`.254`) |
| HQ | 30 (FINANCE) | `10.10.30.0/24` | `10.10.30.1` | Router DHCP (`.100`–`.254`) |
| HQ | 40 (MANAGEMENT) | `10.10.40.0/24` | `10.10.40.1` | Router DHCP (`.100`–`.254`) |
| HQ | 50 (GUEST) | `10.10.50.0/24` | `10.10.50.1` | Router DHCP (`.100`–`.254`) |
| PEN | 10 (IT) | `10.20.10.0/24` | `10.20.10.1` | Router DHCP (`.100`–`.254`) |
| PEN | 20 (HR) | `10.20.20.0/24` | `10.20.20.1` | Router DHCP (`.100`–`.254`) |
| PEN | 30 (FINANCE) | `10.20.30.0/24` | `10.20.30.1` | Router DHCP (`.100`–`.254`) |
| PEN | 40 (MANAGEMENT) | `10.20.40.0/24` | `10.20.40.1` | Router DHCP (`.100`–`.254`) |
| PEN | 50 (GUEST) | `10.20.50.0/24` | `10.20.50.1` | Router DHCP (`.100`–`.254`) |
| JHB | 10 (IT) | `10.30.10.0/24` | `10.30.10.1` | Router DHCP (`.100`–`.254`) |
| JHB | 20 (HR) | `10.30.20.0/24` | `10.30.20.1` | Router DHCP (`.100`–`.254`) |
| JHB | 30 (FINANCE) | `10.30.30.0/24` | `10.30.30.1` | Router DHCP (`.100`–`.254`) |
| JHB | 40 (MANAGEMENT) | `10.30.40.0/24` | `10.30.40.1` | Router DHCP (`.100`–`.254`) |
| JHB | 50 (GUEST) | `10.30.50.0/24` | `10.30.50.1` | Router DHCP (`.100`–`.254`) |
| HQ | 60 (SERVERS) | `10.10.60.0/24` | `10.10.60.1` | Static DNS `.10`, web `.20` |
| HQ | 99 (NET-MGMT) | `10.10.99.0/24` | `10.10.99.1` | Static switches `.2`, `.3` |
| PEN | 99 (NET-MGMT) | `10.20.99.0/24` | `10.20.99.1` | Static switch `.2` |
| JHB | 99 (NET-MGMT) | `10.30.99.0/24` | `10.30.99.1` | Static switch `.2` |

## Infrastructure IP addresses

| Device | IP address | Location |
|---|---|---|
| SW-HQ-ACCESS (VLAN 99) | `10.10.99.2/24` | HQ |
| SW-HQ-SERVER (VLAN 99) | `10.10.99.3/24` | HQ |
| SW-PEN-ACCESS (VLAN 99) | `10.20.99.2/24` | Penang |
| SW-JHB-ACCESS (VLAN 99) | `10.30.99.2/24` | Johor |
| SRV-HQ-DNS | `10.10.60.10/24` | HQ, VLAN 60 |
| SRV-HQ-WEB | `10.10.60.20/24` | HQ, VLAN 60 |

## WAN point-to-point addressing

| Link | Network | Headquarters IP | Branch IP |
|---|---|---|---|
| HQ — Penang | `172.16.0.0/30` | `172.16.0.1` | `172.16.0.2` |
| HQ — Johor | `172.16.0.4/30` | `172.16.0.5` | `172.16.0.6` |

## Central services

- DNS A record: `intranet.corp.example` → `10.10.60.20`
- DNS service: `10.10.60.10` (UDP/TCP port 53)
- HTTP service: `10.10.60.20` (TCP port 80)
- Router DHCP pools for five client VLANs on each router; `.1–.99` excluded, normally assigning `.100+` per VLAN
- DHCP guest pools exist; guest clients are not granted corporate DNS/HTTP access by the routed guest ACL
