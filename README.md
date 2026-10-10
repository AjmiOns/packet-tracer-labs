# Packet Tracer Labs

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer%208.x-1BA0D7?logo=cisco&logoColor=white)
![Protocols](https://img.shields.io/badge/protocols-eBGP%20%7C%20OSPFv2-2E75B6)
![Status](https://img.shields.io/badge/status-in%20progress-yellow)
![Purpose](https://img.shields.io/badge/purpose-educational-lightgrey)

Hands-on networking labs built with **Cisco Packet Tracer**, covering inter-domain routing with **BGP** and intra-domain routing with **OSPFv2**. Each lab ships with the topology file, the original statement when available, annotated screenshots, and a written report (*compte rendu*) containing the exact commands used.

---

## Table of contents

- [Repository structure](#repository-structure)
- [Labs overview](#labs-overview)
- [Lab 1 - eBGP basics](#lab-1---ebgp-basics)
- [Lab 2 - BGP across three autonomous systems](#lab-2---bgp-across-three-autonomous-systems)
- [OSPF - Single-area OSPFv2](#ospf---single-area-ospfv2)
- [Getting started](#getting-started)
- [Verification cheat sheet](#verification-cheat-sheet)
- [Key takeaways](#key-takeaways)
- [Conventions](#conventions)
- [Author](#author)

---

## Repository structure

```
packet-tracer-labs/
├── README.md
├── .gitignore
├── configurationPackTracer/          # Single-Area OSPFv2 Packet Tracer activity
│   └── 2.7.1 ... Single-Area OSPFv2 ...
├── lab-bgp-config/
│   └── BGP_lab2.pkt                  # BGP topology (Lab 2 base file)
├── lab1-ebgp-basic/
│   ├── Screenshots/                  # R1, R2, ISP-1 captures
│   ├── topology.png                  # Topology screenshot (used in README)
│   ├── BGP-Lab1.pkt                  # Packet Tracer topology
│   ├── EnonceLab1.pdf                # Original lab statement (French)
│   └── Lab1_eBGP_Compte_Rendu.pdf    # Lab report with commands + screenshots
└── lab2-bgp-configuration/
    ├── Screenshots/                  # Router configs, routing tables, pings
    ├── topology.png                  # Topology screenshot (used in README)
    ├── Lab2_BGP_Compte_Rendu.pdf     # Lab report with commands + screenshots
    └── Practice Lab - BGP Configuration 2 v1.pkt
```

## Labs overview

| Lab | Topic | Routers | Autonomous systems | Deliverables |
|---|---|---|---|---|
| [Lab 1](lab1-ebgp-basic) | eBGP between a company and its ISP | R1, R2, ISP-1 (Cisco 1941) | AS 65000, AS 65001 | `.pkt`, statement, report, screenshots |
| [Lab 2](lab2-bgp-configuration) | eBGP chain across three ASes | nw_R1, nw_R2, nw_R3 (Cisco 2811) | AS 100, AS 200, AS 300 | `.pkt`, practice file, report, screenshots |
| [OSPF](configurationPackTracer) | Single-area OSPFv2 | see activity | Area 0 | `.pkt` |

---

## Lab 1 - eBGP basics

A company (**AS 65000**) connects to an ISP (**AS 65001**). The ISP originates a default route and R2 learns it over eBGP. R1 reaches the outside world through a static default route towards R2.

**Topology**

<p align="center">
  <img src="lab1-ebgp-basic/topology.png" alt="Lab 1 topology: R1 - R2 - ISP-1 over serial links" width="700">
  <br><em>Lab 1 - R1 (AS 65000) - R2 (AS 65000) - ISP-1 (AS 65001)</em>
</p>

**Addressing**

| Device | Interface | IP address | Mask |
|---|---|---|---|
| R1 | Serial0/1/0 (DCE) | 198.133.219.1 | 255.255.255.248 |
| R2 | Serial0/1/0 | 198.133.219.2 | 255.255.255.248 |
| R2 | Serial0/1/1 (DCE) | 209.165.200.2 | 255.255.255.252 |
| ISP-1 | Serial0/1/1 | 209.165.200.1 | 255.255.255.252 |
| ISP-1 | Loopback0 (web server) | 10.10.10.10 | 255.255.255.255 |

**What it demonstrates**

- Enabling BGP and declaring an eBGP neighbor (`router bgp`, `neighbor ... remote-as`)
- Advertising a prefix with `network ... mask ...`
- Receiving a default route from the ISP (`B*` route with administrative distance 20)
- End-to-end test from R1 to the simulated web server

Full commands and evidence: [`Lab1_eBGP_Compte_Rendu.pdf`](lab1-ebgp-basic/Lab1_eBGP_Compte_Rendu.pdf)

## Lab 2 - BGP across three autonomous systems

Three routers in a line, each in its own AS, with a LAN and a loopback per router. R2 sits in the middle and relays routes between R1 and R3.

**Topology**

<p align="center">
  <img src="lab2-bgp-configuration/topology.png" alt="Lab 2 topology: nw_R1 (AS 100), nw_R2 (AS 200), nw_R3 (AS 300) with one PC each" width="850">
  <br><em>Lab 2 - AS 100, AS 200 and AS 300, one LAN and one PC behind each router</em>
</p>

**What it demonstrates**

- Two eBGP sessions on a transit AS (R2)
- Explicit `bgp router-id` and loopback advertisements
- Route propagation across ASes, visible as `B` entries in `show ip route`
- Full connectivity between the three LANs, validated with pings (TTL 126 across two routers, 125 across three)

Full commands and evidence: [`Lab2_BGP_Compte_Rendu.pdf`](lab2-bgp-configuration/Lab2_BGP_Compte_Rendu.pdf)

## OSPF - Single-area OSPFv2

Packet Tracer activity on single-area OSPFv2 (area 0). Open the `.pkt` file in [`configurationPackTracer`](configurationPackTracer) and follow the activity instructions embedded in the file.

---

## Getting started

**Requirements**

- [Cisco Packet Tracer](https://www.netacad.com/) 8.x (free with a Cisco Networking Academy account)
- Any PDF reader for the reports

**Clone and open**

```bash
git clone https://github.com/<your-username>/packet-tracer-labs.git
cd packet-tracer-labs
```

Open any `.pkt` file with Packet Tracer (`File > Open`). Router configurations are already applied; you can also replay them from the command blocks in each report.

> Packet Tracer interface names depend on the modules installed. Lab 1's statement uses `S0/0/x` while the topology file uses `Serial0/1/x`; addressing is identical.

## Verification cheat sheet

| Command | Where | What it tells you |
|---|---|---|
| `show ip route` | any router | Routing table; `B` = BGP route, `B*` = BGP default route |
| `show ip bgp` | any BGP router | BGP table: prefixes, next hop, AS path |
| `show ip bgp summary` | any BGP router | Neighbor state and number of prefixes received |
| `ping <ip>` | routers and PCs | End-to-end reachability |
| `copy running-config startup-config` | any router | Persist the configuration |

## Key takeaways

- **eBGP neighbors must be reachable** and each side needs the correct `remote-as`; a typo in the neighbor address leaves a session stuck instead of failing loudly.
- **`network` statements must match the routing table exactly** (prefix and mask), otherwise the route is not advertised.
- **A single-homed company does not need BGP.** A static default route towards the ISP is enough. BGP is justified in multi-homed designs with several ISPs, where path selection and redundancy matter.
- **TTL is a quick hop counter** when validating multi-router paths with ping.

## Conventions

- **Folders**: `labN-short-topic`, lowercase, hyphenated.
- **Files**: `.pkt` for topologies, `.pdf` for reports and statements; Lab 2's report and Lab 1's files use underscores or hyphens.
- **Commits**: [Conventional Commits](https://www.conventionalcommits.org/) (`feat`, `docs`, `chore`, `fix`), one logical change per commit, scoped by lab (for example `docs(lab1): add R2 screenshots`).

## Author

Ons, engineering student at TEK-UP University, Tunisia.
[GitHub](https://github.com/<your-username>) · [LinkedIn](https://www.linkedin.com/in/ons-ajmi-0ab2982a2)

Labs completed as part of the routing protocols course. Feedback and suggestions are welcome through issues.
