# 01 · Propósito y alcance

## Objetivo
Demostrar, en un laboratorio GNS3 con FortiGate, que una infraestructura segmentada puede **aislar servidores en una DMZ**, **limitar quién los administra** y **controlar el acceso por VLAN**, con evidencia de cada control.

## Requisitos del laboratorio y estado

| # | Requisito | Estado | Evidencia |
|---|---|:-:|---|
| 1 | LAN de servidores configurada como DMZ | ✅ | [port3 rol DMZ](../evidence/fortigate/02-interfaz-dmz-port3.png) |
| 2 | Sin fuga de tráfico DMZ → LAN | ✅ | [políticas DENY](../evidence/firewall/01-politicas-firewall.png), [ping bloqueado](../evidence/dmz/dmz-a-lan-ping-bloqueado.png) |
| 3 | DMZ sin acceso abierto a Internet | ✅ | `DMZ-to-WAN-DENY`, [ping 8.8.8.8](../evidence/dmz/dmz-a-internet-ping-bloqueado.png) |
| 4 | DMZ solo a endpoints de actualización | ✅ | `DMZ-to-WAN-UPDATES`, [wget](../evidence/dmz/dmz-a-endpoint-actualizacion-wget.png) |
| 5 | Ningún otro tráfico DMZ → Internet | ✅ | `DMZ-to-WAN-DENY` |
| 6 | VLAN 20 única con SSH a servidores | ✅ | [SSH VLAN 20](../evidence/ssh/vlan20-tcp22-tres-servidores.png) / [SSH VLAN 10 bloqueado](../evidence/ssh/vlan10-ssh-caja-bloqueado.png) |
| 7 | VLAN 10 restringida al Web de Inventario | ✅ | `VLAN10-INVENTARIO-BLOCK` + [Web Filter](../evidence/firewall/04-perfil-web-filter-inventario.png) |
| 8 | Demostración de política violada/bloqueada | ✅ | [página de bloqueo](../evidence/vlan/vlan10-inventario-bloqueado-navegador.png), [log](../evidence/firewall/06-log-vlan10-inventario-bloqueado.png) |
| 9 | Evidencias de políticas y pruebas | ✅ | [`evidence/`](../evidence/README.md) |
| 10 | 2 switches con VLAN y seguridad básica | ✅ | [08](08-seguridad-switches.md) |
| 11 | 3 servidores /28 | ✅ | [09](09-servidores.md) |
| 12 | 2 redes de usuarios /25 con DHCP | ✅ | [10](10-dhcp.md) |


