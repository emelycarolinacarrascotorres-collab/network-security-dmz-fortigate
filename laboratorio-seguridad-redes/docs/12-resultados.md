# 12 · Resultados

## Matriz de comunicación (según políticas reales)

| Origen | Destino | Servicio | Permitido | Política / motivo |
|---|---|---|:-:|---|
| VLAN 10 | Caja (20.25.97.2) | HTTP/HTTPS | ✅ | `VLAN10-CAJA` |
| VLAN 10 | Inventario (20.25.97.3) | HTTP | ❌ | `VLAN10-INVENTARIO-BLOCK` + Web Filter (L7) |
| VLAN 10 | Inventario | otros puertos (incl. HTTPS, SSH) | ❌ | `VLAN10-DMZ-DENY` |
| VLAN 10 | DMZ | SSH / cualquiera | ❌ | `VLAN10-DMZ-DENY` |
| VLAN 10 | Internet | Cualquiera | ✅ | `VLAN10-to-WAN` |
| VLAN 20 | Servidores DMZ | SSH | ✅ | `VLAN20-SSH-DMZ` |
| VLAN 20 | Caja, Inventario | HTTP/HTTPS | ✅ | `VLAN20-WEB-DMZ` |
| VLAN 20 | Internet | — | ❌ | Sin política visible → denegación implícita |
| VLAN 10 ↔ VLAN 20 | — | — | ❌ | Sin política visible → denegación implícita |
| DMZ | VLAN 10 / VLAN 20 | Cualquiera | ❌ | `DMZ-to-VLAN10-DENY` / `DMZ-to-VLAN20-DENY` |
| DMZ | `GRP_UPDATES` (3 FQDN) | `SVC_UPDATES` | ✅ | `DMZ-to-WAN-UPDATES` |
| DMZ | Internet (resto) | Cualquiera | ❌ | `DMZ-to-WAN-DENY` |

### Diferencias respecto a la matriz esperada del enunciado
1. **VLAN 10 → Inventario** no se bloquea con una política `DENY`, sino con *ACCEPT + Web Filter*. El resultado es el requerido, pero el mecanismo es L7.
2. **VLAN 10 → Internet** está permitido (el enunciado no lo restringe).

## Resumen

| Total pruebas | PASS | FAIL |
|:-:|:-:|:-:|
| 19 | 19 | 0 |

Los 12 requisitos del laboratorio están cubiertos ([01](01-proposito-y-alcance.md)). Los hallazgos de endurecimiento y las evidencias faltantes están en [14](14-hallazgos-y-pendientes.md).
