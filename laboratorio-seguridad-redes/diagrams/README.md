# Diagramas

Fuentes en formato Mermaid (se renderizan automáticamente en GitHub). Representan **solo** lo que aparece en las evidencias.

## Arquitectura general

```mermaid
flowchart LR
  WAN(["WINOTELECON / Internet<br/>WAN port1 192.168.42.138/24"])
  FG{{"FortiGate (FortiGate7.0.9-1)<br/>port2 | port3 | port1"}}
  MG["Nodo sin etiqueta<br/>enlace inferior de la topologia<br/>(port6 192.168.99.1/24 - por confirmar)"]
  subgraph USERS["LAN USERS"]
    SWU["SWITCH-USERS<br/>Gi0/0 trunk VLAN 10,20"]
    H1["PC1 / windows10-1<br/>VLAN 10 Gi0/1 - VLAN 20 Gi0/2"]
  end
  subgraph DMZ["SERVIDORES - DMZ 20.25.97.0/28"]
    SWD["SW-DMZ-0697<br/>Gi0/0 uplink a port3"]
    CAJA["Caja-WEB<br/>20.25.97.2"]
    INV["Inventario<br/>20.25.97.3"]
    DB["db-server-lab-1<br/>20.25.97.4"]
  end
  WAN --- FG
  FG ---|"port2 (VLAN 10 / VLAN 20)"| SWU
  SWU --- H1
  FG ---|"port3 DMZ-SERVER 20.25.97.1"| SWD
  SWD ---|"Gi0/1"| CAJA
  SWD ---|"Gi0/2"| DB
  SWD ---|"Gi0/3"| INV
  FG -.-|"por confirmar"| MG
  classDef fw fill:#c0392b,color:#fff,stroke:#7b241c
  classDef dmz fill:#d6f3ff,stroke:#2980b9
  classDef usr fill:#d5f5e3,stroke:#1e8449
  class FG fw
  class SWD,CAJA,INV,DB dmz
  class SWU,H1 usr
```

## Segmentación

```mermaid
flowchart TB
  FG{{"FortiGate"}}
  subgraph P2["port2 - troncal 802.1Q"]
    V10["VLAN 10 - Usuarios<br/>20.25.6.0/25<br/>GW 20.25.6.1<br/>DHCP .10 - .120"]
    V20["VLAN 20 - Usuarios/Admin<br/>20.25.6.128/25<br/>GW 20.25.6.129<br/>DHCP .138 - .248"]
  end
  subgraph P3["port3 - DMZ-SERVER"]
    D["DMZ 20.25.97.0/28<br/>GW 20.25.97.1<br/>.2 Caja / .3 Inventario / .4 DB"]
  end
  FG --- V10
  FG --- V20
  FG --- D
  classDef v10 fill:#fdebd0,stroke:#d68910
  classDef v20 fill:#d5f5e3,stroke:#1e8449
  classDef dmz fill:#d6f3ff,stroke:#2980b9
  class V10 v10
  class V20 v20
  class D dmz
```

## DMZ

```mermaid
flowchart LR
  subgraph LAN["Redes internas"]
    V10["VLAN 10"]
    V20["VLAN 20"]
  end
  FG{{"FortiGate<br/>DMZ-SERVER (port3)"}}
  subgraph DMZ["DMZ 20.25.97.0/28"]
    CAJA["Caja .2"]
    INV["Inventario .3"]
    DB["DB .4"]
  end
  UPD(["GRP_UPDATES<br/>archive.ubuntu.com<br/>security.ubuntu.com<br/>dl-cdn.alpinelinux.org"])
  NET(["Internet (resto)"])
  V20 -->|"SSH, HTTP/HTTPS permitido"| FG
  V10 -->|"HTTP/HTTPS a Caja permitido<br/>Inventario bloqueado (web filter)"| FG
  FG --> DMZ
  DMZ -. "DMZ a VLAN 10/20: DENY" .-> LAN
  DMZ -->|"SVC_UPDATES permitido"| UPD
  DMZ -. "DMZ-to-WAN-DENY" .-> NET
```

## Flujo de tráfico

```mermaid
flowchart LR
  V10["VLAN 10<br/>20.25.6.0/25"]
  V20["VLAN 20<br/>20.25.6.128/25"]
  DMZ["DMZ<br/>20.25.97.0/28"]
  CAJA["Caja .2"]
  INV["Inventario .3"]
  ALL["Servidores .2 .3 .4"]
  WAN(["Internet"])
  UPD(["Endpoints de actualizacion"])
  V10 -->|"OK HTTP/HTTPS - VLAN10-CAJA"| CAJA
  V10 -->|"BLOQUEADO HTTP - web filter"| INV
  V10 -->|"BLOQUEADO SSH - VLAN10-DMZ-DENY"| ALL
  V10 -->|"OK - VLAN10-to-WAN"| WAN
  V20 -->|"OK SSH - VLAN20-SSH-DMZ"| ALL
  V20 -->|"OK HTTP/HTTPS - VLAN20-WEB-DMZ"| CAJA
  V20 -->|"OK HTTP/HTTPS - VLAN20-WEB-DMZ"| INV
  DMZ -->|"OK - DMZ-to-WAN-UPDATES"| UPD
  DMZ -->|"BLOQUEADO - DMZ-to-WAN-DENY"| WAN
  DMZ -->|"BLOQUEADO - DMZ-to-VLAN10/20-DENY"| V10
```

## Controles de seguridad por capa

```mermaid
flowchart TB
  subgraph L2["Capa 2 - Switches"]
    A1["VLAN 10 / 20 + trunk 802.1Q (solo 10,20, nonegotiate)"]
    A2["Port-security sticky + PortFast + BPDU Guard"]
    A3["SSH v2 / login local / exec-timeout 5 min"]
  end
  subgraph L3["Capa 3-4 - FortiGate"]
    B1["Interfaz port3 rol DMZ (solo PING administrativo)"]
    B2["DMZ a VLAN 10/20: DENY"]
    B3["DMZ a Internet: solo GRP_UPDATES, resto DENY"]
    B4["SSH a DMZ: solo VLAN 20"]
    B5["VLAN 10 a DMZ: solo Caja HTTP/HTTPS, resto DENY"]
  end
  subgraph L7["Capa 7 - UTM"]
    C1["Web Filter WF_BLOQUEO_INVENTARIO (URL filter local)"]
  end
  L2 --> L3 --> L7
```
