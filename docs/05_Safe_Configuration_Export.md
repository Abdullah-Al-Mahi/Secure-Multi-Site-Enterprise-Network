# 05 - Safe Configuration Export & Public Sharing

## Preferred safe deliverable
Keep your verified `.pkt` file locally for demonstrations. The included documentation/diagrams/evidence files are a separate, **public-friendly starting point**, not a claim that all content is automatically safe. Perform a final visual and text security review before sharing.

## Why not publish a raw .pkt or show running-config?
A Packet Tracer project and raw device configurations may contain:
- Usernames and hashed or reversible passwords (including lab-only credentials)
- RSA private key material or SSH host keys
- SNMP strings, authentication settings, local account data
- Other infrastructure secrets introduced during future work

Even hashed passwords and generated keys should be treated as sensitive for public exports. Making a repository public does **not** sanitize these files.

## How to export private working configurations (for local archive only)

For each of the seven routers/switches:

1. Open device → **CLI** → `enable`.
2. Run `show running-config` and save output into an **offline private** `.txt` file.
3. Run `show startup-config` to verify persistence; save privately if needed.
4. Inspect for secret/username/password/key material. **Do not copy raw output to a public Git repository.**

## How to make sanitized public excerpts

- Prefer short commands such as `show ip ospf neighbor`, `show interfaces trunk`, `show vlan brief`, `show port-security`, `show ip route` or output focused on behavior.
- If publishing sample configurations, create *separate reconstructed snippets* with placeholder values for passwords and remove all real hashes, private keys and key-chain material.
- Never modify/erase the working configuration just to sanitize a public sample.
- Inspect `.pkt`, config backups, screenshots, Markdown, ZIP archives, and Git commit history for embedded secrets before public release.
- Check whether any fictional company name or educational course requirements should be credited; do not claim corporate deployment.

## Recommended publication practice

1. Keep the **private working folder** with final `.pkt` and original configs.
2. Keep a separate **publication folder** with README, documentation, rendered diagrams, selected redacted evidence, and optional sanitized example configs.
3. Review the publication folder file by file before uploading.
4. If you later wish to share a usable `.pkt`, create and inspect a **separate sanitized copy** and test that it still runs; until then, keep it private.

## Evidence naming convention

`T06_PEN_FIN_Intranet.png`, `T07_JHB_HR_Intranet.png`, `T09_PEN_HR_to_JHB_FIN_Blocked.png`, `T15_Unused_Port_Shutdown.png`, `T16_PEN_to_JHB_Traceroute.png`, `T17_OSPF_Cost_Evaluation.txt`, `T18_ICMP_Simulation_EventList.png`.
