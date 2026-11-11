# 03 — Security Design and Verified Controls

## Security policy (simulation scope)

| Source | HQ DNS (53) | HQ HTTP (80) | Cross-department private routed networks | Device SSH |
|---|---|---|---|---|
| HQ IT, VLAN 10, `10.10.10.0/24` | Allow | Allow | Authorized administrative troubleshooting | Allow |
| Penang/Johor IT (VLAN 10) | Allow | Allow | Deny other internal departments | Deny |
| HR / Finance / Management (20/30/40) | Allow | Allow | Deny unrelated departments | Deny |
| Guest (50) | Deny | Deny | Deny private corporate networks | Deny |

The deployed policy includes narrow DHCP/ICMP gateway exceptions where required for client operation and diagnostics. **These are routed-traffic policies**; same-VLAN switching cannot be controlled by an ACL installed on a router subinterface.

## Router extended ACLs

- `CORPORATE_STAFF_POLICY`: applied inbound to non-admin departmental router subinterfaces; enables DHCP, required DNS and intranet access, limited diagnostic traffic, denies protected private networks as configured.
- `GUEST_ISOLATION_POLICY`: inbound on guest VLAN subinterfaces; permits DHCP and restricted gateway diagnostics, denies private networks.
- `HQ_ADMIN_ONLY`: standard ACL permitting `10.10.10.0/24` as the authorized VTY SSH source, applied to VTY management lines across routers and switches.
- ACL order and application direction determine behavior; inspect `show ip interface` and `show access-lists` when troubleshooting. These ACLs do not provide stateful firewall capabilities.

## Device administration

- SSH version 2 on all 7 routers and switches in the Phase 6 final audited project
- Local administrative credentials; enable secrets configured
- VTY connections restricted to SSH (`transport input ssh`), local login, and HQ IT source ACL
- Telnet management disabled in the verified project
- **Never publish** usernames plus secrets, cryptographic keys, hash strings, or private credentials from full Packet Tracer configurations

## Switch access security

- **17 active PC/server access ports** use `switchport port-security` with one maximum secure MAC address per interface, sticky MAC learning and `violation shutdown`
- **82 unused switch interfaces** are placed into VLAN 999 and administratively shut down
- Trunk native VLAN 998 reserved and unused by endpoints; trunks carry only needed VLANs
- A separate controlled port-security violation test was reported as completed; its final screenshot should be added to evidence before public claims rely on it

## Server hardening

- `SRV-HQ-DNS`: required DNS service enabled
- `SRV-HQ-WEB`: required HTTP service enabled
- Unnecessary FTP, TFTP, email and HTTPS server toggles disabled during Phase 6 correction
- Note: **HTTP is plaintext**. This is an intentionally local lab intranet, not an HTTPS-secured production website.

## Explicit limitations

No VPN/IPsec, no stateful firewall, no intrusion detection, no MFA for device management, no external Internet access, and no validated switch DHCP-snooping or dynamic ARP inspection implementation. Do not imply otherwise in technical reports or resumes.
