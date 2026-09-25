========================================================================
   WAN / LAN NETWORK LAYOUT (LINEAR TOPOLOGY)
========================================================================

Logical flow:  ISP  ->  WAN  ->  Switch  ->  WAN  ->  Firewall  ->  LAN  ->  Router AP

This diagram represents a network architecture structured into 3
functional layers, where each device assumes a specific role
in the traffic-forwarding chain.

------------------------------------------------------------------------
 VISUAL DIAGRAM (ASCII)
------------------------------------------------------------------------

                          INTERNET
                              |
                              v
   +================================================================+
   |                         [ ISP ]                               |
   |   Internet Service Provider (Tier 1 - Internet Access)        |
   |   - Edge router / Gateway                                     |
   +================================================================+
                              |
                              |  WAN link (fiber / MPLS)
                              v
   +================================================================+
   |                         [ Switch ]                             |
   |                         (Tier 2)                               |
   |   - Dual-Core processor with dedicated L3 forwarding           |
   |   - 5G/4G WAN + Ethernet, QoS, integrated firewall             |
   +================================================================+
           |                                |
           | LAN IPV6 link                  |  WAN link (fiber / MPLS)
           v                                v
   +====================================================+
   |                 [ FIREWALL ]                       |
   |   Firewall / Router (Tier 3 - Control)             |
   |   - Stateful packet filtering                      |
   |   - Access policies and NAT                        |
   |   - IPS/IDS and segmentation control               |
   +====================================================+----|
   |                                                         |
   | LAN Interface link                                      | Virtual LAN link (Gigabit/Ethernet)
   |                                                         v
   |	+=================================================================+
   | 	|                       [ VMs / SERVERS ]                         |
   | 	|               - Docker Hosts | NAS                              |
   |	|               - Recursive DNS | AI Inference | Staging VPS      |
   | 	+=================================================================+
   v
 +================================================================+
 |                     [ Router AP ]                              |
 |   Access Point / AP (Tier 3 - End Devices)                     |
 |   - Last mile for hosts/segments                               |
 |   - DHCP, local routing, and Wi-Fi coverage                    |
 +================================================================+

------------------------------------------------------------------------
 LAYERS / DEVICES
------------------------------------------------------------------------

LAYER 1  -  ISP (Internet Service Provider)
    Function: Connects the internal network to Internet providers.
    Layer:    Network (L3)
    Functions: Edge routing, default gateway, WAN QoS.

LAYER 2  -  Switch
    Function: Dual-Core 1.4GHz processor with dedicated L3 forwarding.
    Layer:    Network (L3)
    Functions: WAN/LAN Ethernet + 5G WAN (4G/LTE), inter-VLAN routing,
               advanced QoS, integrated firewall, VLAN tagging, NAT, ACL.

LAYER 3  -  (Firewall / Router)
    Function: Protects the network by filtering and controlling traffic.
    Layer:    Network (L3/L4)
    Functions: Stateful firewall, NAT, IPS/IDS, segmentation.

------------------------------------------------------------------------
  NOTES
------------------------------------------------------------------------
- The topology follows a sequential (linear) trust flow:
  each device trusts the previous one and forwards traffic to the next.
- Traffic from end hosts arrives through the Router/AP (Wi-Fi/LAN).
- The VMs/Servers card accesses the Firewall through a virtual LAN link
  (Gigabit/Ethernet), receiving high-performance traffic for
  intensive workloads such as AI inference, staging VPS, and Docker.
- The WAN links (ISP<->Switch and Switch<->Firewall) carry trunk traffic;
  the LAN link (Firewall<->Router/AP) carries traffic to end devices.

-----------------------------------------------------------------
  ACRONYM / ABBREVIATION LEGEND
-----------------------------------------------------------------
| Acronym | Meaning                                             |
|---------|-----------------------------------------------------|
| ISP     | Internet Service Provider                           |
| WAN     | Wide Area Network                                   |
| LAN     | Local Area Network                                 |
| L3      | Network Layer (OSI)                               |
| L4      | Transport Layer (OSI)                             |
| QoS     | Quality of Service                                 |
| NAT     | Network Address Translation                         |
| ACL     | Access Control List                                 |
| DHCP    | Dynamic Host Configuration Protocol                 |
| IPS     | Intrusion Prevention System                         |
| IDS     | Intrusion Detection System                          |
| VPS     | Virtual Private Server                              |
| NAS     | Network Attached Storage                            |
| DNS     | Domain Name System                                  |
| API     | Application Programming Interface                   |
| CPU     | Central Processing Unit                             |
| GHz     | Gigahertz                                           |
========================================================================
