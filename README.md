# Network-3-layers

Example of network layers for security and scalability.
``` text
========================================================================
   LAYOUT DE REDE WAN / LAN (TOPOLOGIA LINEAR)
========================================================================

Fluxo logico:  ISP  ->  WAN  ->  Switch  ->  WAN  ->  Firewall  ->  LAN  ->  Router AP

Este diagrama representa uma arquitetura de rede estruturada em 3
camadas funcionais, onde cada dispositivo assume um papel especifico
na cadeia de encaminhamento de trafego.

------------------------------------------------------------------------
 DIAGRAMA VISUAL (ASCII)
------------------------------------------------------------------------

                          INTERNET
                              |
                              v
   +================================================================+
   |                         [ ISP ]                               |
   |   Internet Service Provider (Tier 1 - Acesso a Internet)      |
   |   - Roteador de borda / Gateway                               |
   +================================================================+
                              |
                              |  Link WAN (fibra / MPLS)
                              v
   +================================================================+
   |                         [ Switch ]                             |
   |                         (Tier 2)                               |
   |   - Processador Dual-Core com encaminhamento L3 dedicado       |
   |   - WAN 5G/4G + Ethernet, QoS, firewall integrado              |
   +================================================================+
           |                                |
		       | Link LAN IPV6                  |  Link WAN (fibra / MPLS)
           v                                v
   +====================================================+
   |                 [ FIREWALL ]                      |
   |   Firewall / Roteador (Tier 3 - Controle)         |
   |   - Filtragem de pacotes (stateful)               |
   |   - Politicas de acesso e NAT                     |
   |   - IPS/IDS e controle de segmentacao             |
   +====================================================+----|
   |                                                         |
   | Link LAN Interface                                      | Link LAN Virt (Gigabit/Ethernet)
   |                                                         v
   |	+=================================================================+
   | 	 |                      [ VMs / SERVIDORES ]                     |
   | 	 |              - Docker Hosts | NAS                             |
   |	 |              - DNS Recursive | AI Inference | Staging VPS     |
   | 	+=================================================================+
   v
 +================================================================+       
 |                     [ Router AP ]                             |     
 |   Access Point / AP (Tier 3 - Dispositivos Finais)            |
 |   - Ultima milha para hosts/segmentos                         |
 |   - DHCP, roteamento local e cobertura Wi-Fi                  |
 +================================================================+

------------------------------------------------------------------------
 CAMADAS / DISPOSITIVOS
------------------------------------------------------------------------

CAMADA 1  -  ISP (Internet Service Provider)
    Funcao:   Conecta a rede interna a provedores de Internet.
    Camada:   Rede (L3)
    Funcoes:  Roteamento de borda, gateway default, QoS WAN.

CAMADA 2  -  Switch
    Funcao:   Processador Dual-Core 1,4GHz com encaminhamento L3 dedicado.
    Camada:   Rede (L3)
    Funcoes:  WAN/LAN Ethernet + WAN 5G (4G/LTE), roteamento inter-VLAN, QoS avançada, firewall integrado, VLAN tagging, NAT, ACL.

CAMADA 3  -   (Firewall / Roteador)
    Funcao:   Protege a rede filtrando e controlando o trafego.
    Camada:   Rede (L3/L4)
    Funcoes:  Firewall stateful, NAT, IPS/IDS, segmentacao.

------------------------------------------------------------------------
  OBSERVACOES
------------------------------------------------------------------------
- A topologia segue um fluxo sequencial (linear) de confianca:
  cada dispositivo confia no anterior e encaminha ao subsequente.
- O trafego dos hosts finais chega via Router/AP (Wi-Fi/LAN).
- O card de VMs/Servidores acessa o Firewall por link LAN virtual
  (Gigabit/Ethernet), recebendo tráfego de alto desempenho para
  cargas intensivas como AI inference, staging VPS e Docker.
- Os links WAN (ISP<->Switch e Switch<->Firewall) transportam o trafego
  tronco; o link LAN (Firewall<->Router/AP) leva o trafego aos finais.

-----------------------------------------------------------------
  LEGENDA DE SIGLAS / ABREVIACOES
-----------------------------------------------------------------
| Sigla   | Significado                                         |
|---------|-----------------------------------------------------|
| ISP     | Internet Service Provider                           |
| WAN     | Wide Area Network (Rede de Longa Distancia)         |
| LAN     | Local Area Network (Rede de Area Local)             |
| L3      | Camada de Rede (Network Layer - OSI)                |
| L4      | Camada de Transporte (Transport Layer - OSI)        |
| QoS     | Quality of Service (Qualidade do Servico)           |
| NAT     | Network Address Translation                         |
| ACL     | Access Control List (Lista de Controle de Acesso)   |
| DHCP    | Dynamic Host Configuration Protocol               	|
| IPS     | Intrusion Prevention System (Sistema de Preventao)	|
| IDS     | Intrusion Detection System (Sistema de Deteccao)  	|
| VPS     | Virtual Private Server (Servidor Privado Virtual)  	|
| NAS     | Network Attached Storage                           	|
| DNS     | Domain Name System                                 	|
| API     | Application Programming Interface                  	|
| CPU     | Central Processing Unit                            	|
| GHz     | Gigahertz                                          	|
========================================================================
```
