# 03 · Direccionamiento IP

| Segmento | Interfaz FortiGate | Red | Máscara | Gateway | Hosts útiles | Evidencia |
|---|---|---|---|---|---|---|
| VLAN 10 | `Usuarios (VLAN10)` sobre port2 | 20.25.6.0/25 | 255.255.255.128 | 20.25.6.1 | .1–.126 | [captura](../evidence/fortigate/03-subinterfaces-vlan-dhcp.png) |
| VLAN 20 | `Usuarios (VLAN-20)` sobre port2 | 20.25.6.128/25 | 255.255.255.128 | 20.25.6.129 | .129–.254 | [captura](../evidence/fortigate/03-subinterfaces-vlan-dhcp.png) |
| DMZ | `DMZ-SERVER (port3)` | 20.25.97.0/28 | 255.255.255.240 | 20.25.97.1 | .1–.14 | [captura](../evidence/fortigate/02-interfaz-dmz-port3.png) |
| WAN | `WAN (port1)` | 192.168.42.0/24 | 255.255.255.0 | — | — | [captura](../evidence/fortigate/01-interfaces-fortigate.png) |
| Otra | `port6` | 192.168.99.0/24 | 255.255.255.0 | — | — | [captura](../evidence/fortigate/01-interfaces-fortigate.png) |

## Hosts

| Host | IP | Máscara | Gateway | Origen del dato |
|---|---|---|---|---|
| FortiGate WAN | 192.168.42.138 | /24 | — | Interfaces |
| Caja-WEB | 20.25.97.2 | /28 | 20.25.97.1 | [config](../scripts/configurar-ip-servidores.txt), [`ip a`](../evidence/servidores/caja-ip-a.png) |
| Inventario | 20.25.97.3 | /28 | 20.25.97.1 | [config](../scripts/configurar-ip-servidores.txt), [`ip a`](../evidence/servidores/inventario-ip-a.png) |
| db-server-lab-1 | 20.25.97.4 | /28 | 20.25.97.1 | [config](../scripts/configurar-ip-servidores.txt), [`ip a`](../evidence/servidores/db-ip-a.png) |
| PC1 (VPCS) | 20.25.6.10 (DHCP) | /25 | 20.25.6.1 | [captura](../evidence/dhcp/pc1-vlan10-dhcp.png) |
| PC2 (VPCS) | 20.25.6.138 (DHCP) | /25 | 20.25.6.129 | [captura](../evidence/dhcp/pc2-vlan20-dhcp.png) |
| Windows 10 en VLAN 10 | 20.25.6.11 (DHCP) | /25 | 20.25.6.1 | [captura](../evidence/ssh/vlan10-ssh-caja-bloqueado.png) |
| Windows 10 en VLAN 20 | 20.25.6.139 (DHCP) | /25 | 20.25.6.129 | [captura](../evidence/ssh/vlan20-tcp22-tres-servidores.png) |

> Los switches no muestran IP de gestión en los `running-config` (sin interfaz VLAN con IP). Se gestionan por consola.
> Las redes `20.25.x.x` **no son RFC 1918**; es válido en un laboratorio aislado, pero en producción debe usarse espacio privado.
