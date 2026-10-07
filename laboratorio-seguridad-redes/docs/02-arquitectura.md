# 02 · Arquitectura

![Topología](../topology/topologia-general.png)

| Zona | Elementos (según la topología) |
|---|---|
| WAN | Nube `WINOTELECON` → FortiGate `port1` |
| LAN USERS | `SWITCH-USERS` (conectado a `port2`), `PC1`, `windows10-1` |
| SERVIDORES (DMZ) | `SW-DMZ-0697` (conectado a `port3`), `Caja-WEB`, `db-server-lab-1`, `Inventario` |
| Firewall | `FortiGate7.0.9-1` (nombre del nodo en GNS3) |

## Diagrama lógico

```mermaid
flowchart LR
  WAN(["Internet / WAN<br/>192.168.42.138/24"])
  FG{{"FortiGate"}}
  subgraph USERS["LAN USERS"]
    SWU["SWITCH-USERS"]
    V10["VLAN 10<br/>20.25.6.0/25"]
    V20["VLAN 20<br/>20.25.6.128/25"]
  end
  subgraph DMZ["DMZ 20.25.97.0/28"]
    SWD["SW-DMZ-0697"]
    S["Caja .2 / Inventario .3 / DB .4"]
  end
  WAN --- FG
  FG ---|"port2 trunk"| SWU
  SWU --- V10
  SWU --- V20
  FG ---|"port3"| SWD
  SWD --- S
```

Más diagramas: [`diagrams/README.md`](../diagrams/README.md). Inconsistencias de la topología (PC2 ausente, nodo inferior sin etiqueta): ver [14](14-hallazgos-y-pendientes.md).
