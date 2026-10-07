# 11 · Pruebas de seguridad

Formato: origen → destino · servicio · esperado / obtenido · evidencia · conclusión.

## VLAN 10

| ID | Prueba | Origen → Destino | Servicio | Esperado | Obtenido | Estado | Evidencia |
|---|---|---|---|---|---|:-:|---|
| T01 | Acceso autorizado a Caja | 20.25.6.11 → 20.25.97.2 | TCP 80 | Permitido | `TcpTestSucceeded: True` | ✅ PASS | [captura](../evidence/vlan/vlan10-caja-tcp80-permitido.png), [web](../evidence/vlan/vlan10-caja-web.png) |
| T02 | Acceso al Inventario | 20.25.6.11 → 20.25.97.3 | HTTP | **Bloqueado** | Página *FortiGuard – Web Page Blocked* (`Local URLfilter Block`) | ✅ PASS | [navegador](../evidence/vlan/vlan10-inventario-bloqueado-navegador.png), [log](../evidence/firewall/06-log-vlan10-inventario-bloqueado.png) |
| T03 | SSH a Caja | 20.25.6.11 → 20.25.97.2 | TCP 22 / ICMP | Bloqueado | `TcpTestSucceeded: False`, ping `TimedOut` | ✅ PASS | [captura](../evidence/ssh/vlan10-ssh-caja-bloqueado.png) |
| T15 | HTTPS al Inventario | 20.25.6.11 → 20.25.97.3 | TCP 443 | Bloqueado | `TcpTestSucceeded: False` | ✅ PASS | [captura](../evidence/vlan/vlan10-inventario-https-y-ssh-bloqueado.png) |
| T16 | SSH al Inventario | 20.25.6.11 → 20.25.97.3 | TCP 22 | Bloqueado | `TcpTestSucceeded: False` | ✅ PASS | [captura](../evidence/vlan/vlan10-inventario-https-y-ssh-bloqueado.png) |
| T17 | SSH a DB | 20.25.6.11 → 20.25.97.4 | TCP 22 | Bloqueado | `TcpTestSucceeded: False` | ✅ PASS | [captura](../evidence/ssh/vlan10-ssh-db-bloqueado.png), [captura 2](../evidence/vlan/vlan10-inventario-https-y-ssh-bloqueado.png) |

**Conclusión:** VLAN 10 solo alcanza Caja. El Inventario por HTTP se bloquea en capa 7 (log `blocked` de `http://20.25.97.3/`, 358 B enviados / 0 B recibidos); HTTPS y SSH a los tres servidores fallan por `VLAN10-DMZ-DENY`.

## VLAN 20

| ID | Prueba | Origen → Destino | Servicio | Esperado | Obtenido | Estado | Evidencia |
|---|---|---|---|---|---|:-:|---|
| T04 | Puerto SSH abierto | 20.25.6.139 → .2, .3, .4 | TCP 22 | Permitido | `True` ×3 | ✅ PASS | [captura](../evidence/ssh/vlan20-tcp22-tres-servidores.png) |
| T05 | Sesión SSH | 20.25.6.139 → .2, .3, .4 | SSH | Permitido | Login Ubuntu 22.04 en Caja-WEB, Inventario, db-server-lab-1 | ✅ PASS | [Caja](../evidence/ssh/vlan20-ssh-caja.png), [Inv + DB](../evidence/ssh/vlan20-ssh-inventario-y-db.png) |
| T06 | Web Caja e Inventario | 20.25.6.139 → .2 y .3 | HTTP | Permitido | Ambas páginas cargan | ✅ PASS | [Caja](../evidence/vlan/vlan20-caja-web.png), [Inv](../evidence/vlan/vlan20-inventario-web.png) |
| T07 | Registro en FortiGate | — | — | Log de políticas | `VLAN20-SSH-DMZ (5)` y `VLAN20-WEB-DMZ (6)` | ✅ PASS | [log](../evidence/firewall/05-log-trafico-vlan20-ssh-web.png) |

## DMZ

| ID | Prueba | Origen → Destino | Servicio | Esperado | Obtenido | Estado | Evidencia |
|---|---|---|---|---|---|:-:|---|
| T08 | DMZ → LAN | servidor DMZ → 20.25.6.139 | ICMP | Bloqueado | 4 enviados / 0 recibidos | ✅ PASS | [captura](../evidence/dmz/dmz-a-lan-ping-bloqueado.png) |
| T09 | DMZ → endpoint permitido | servidor DMZ → archive.ubuntu.com (185.125.190.83) | HTTP 80 | Permitido | `HTTP/1.1 200 OK` | ✅ PASS | [captura](../evidence/dmz/dmz-a-endpoint-actualizacion-wget.png) |
| T10 | DMZ → Internet no autorizado | servidor DMZ → 8.8.8.8 | ICMP | Bloqueado | 4 enviados / 0 recibidos | ✅ PASS | [captura](../evidence/dmz/dmz-a-internet-ping-bloqueado.png) |
| T18 | DMZ → VLAN 10 | servidor DMZ → 20.25.6.11 | ICMP | Bloqueado | 3 enviados / 0 recibidos | ✅ PASS | [captura](../evidence/dmz/dmz-a-vlan10-ping-bloqueado.png) |
| T19 | DMZ → Internet por HTTPS | servidor DMZ → www.google.com y www.facebook.com | TCP 443 | Bloqueado | `download timed out`, código 1 (×2) | ✅ PASS | [captura](../evidence/dmz/dmz-a-internet-https-bloqueado.png) |


## Switches y DHCP

| ID | Prueba | Obtenido | Estado | Evidencia |
|---|---|---|:-:|---|
| T11 | Troncal solo VLAN 10,20 | `Gi0/0 trunking 802.1q, 10,20` | ✅ PASS | [captura](../evidence/switches/01-show-interfaces-trunk.png) |
| T12 | VLAN por puerto | VLAN 10→Gi0/1, VLAN 20→Gi0/2 | ✅ PASS | [captura](../evidence/switches/02-show-vlan-brief.png) |
| T13 | Port-security | Violaciones registradas por MAC `0050.56c0.0002` en Gi0/2 | ✅ PASS (indirecta) | [running-config](../configs/switch-users/running-config.txt) |
| T14 | DHCP | PC1 y PC2 obtienen IP | ✅ PASS | [10](10-dhcp.md) |

Comandos usados: [`scripts/comandos-de-prueba.md`](../scripts/comandos-de-prueba.md).
