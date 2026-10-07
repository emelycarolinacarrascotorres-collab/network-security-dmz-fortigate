# Índice de evidencias

| Archivo | Qué demuestra | Requisito |
|---|---|---|
| `fortigate/01-interfaces-fortigate.png` | Interfaces, IP y acceso administrativo | Direccionamiento |
| `fortigate/02-interfaz-dmz-port3.png` | port3 con rol DMZ, `20.25.97.1/28`, solo PING | DMZ |
| `fortigate/03-subinterfaces-vlan-dhcp.png` | VLAN 10/20 con gateway y rangos DHCP | VLAN / DHCP |
| `firewall/01-politicas-firewall.png` | Tabla completa de políticas | Políticas |
| `firewall/02-objetos-fqdn-actualizaciones.png` | FQDN de actualización | DMZ → Internet |
| `firewall/03-address-groups.png` | `GRP_UPDATES`, `NET_LAN_ALL` | DMZ → Internet |
| `firewall/04-perfil-web-filter-inventario.png` | Perfil `WF_BLOQUEO_INVENTARIO` | VLAN 10 → Inventario |
| `firewall/05-log-trafico-vlan20-ssh-web.png` | Log de `VLAN20-SSH-DMZ (5)` y `VLAN20-WEB-DMZ (6)` | SSH solo VLAN 20 |
| `firewall/06-log-vlan10-inventario-bloqueado.png` | Log `blocked` 20.25.6.11 → http://20.25.97.3/ | Política bloqueada |
| `switches/01-show-interfaces-trunk.png` | Troncal 802.1Q VLAN 10,20 | VLAN |
| `switches/02-show-vlan-brief.png` | VLAN 10 y 20 por puerto | VLAN |
| `servidores/*-ip-a.png` | IP de cada servidor | Servidores |
| `servidores/*-puertos-escucha.png` | Servicios en escucha (22/80; DB solo 22) | Servidores |
| `dhcp/pc1-vlan10-dhcp.png`, `pc2-vlan20-dhcp.png` | Concesiones DHCP | DHCP |
| `ssh/vlan20-*.png` | SSH desde VLAN 20 a .2, .3, .4 | SSH solo VLAN 20 |
| `ssh/vlan10-ssh-caja-bloqueado.png` | SSH desde VLAN 10 a Caja fallido | SSH solo VLAN 20 |
| `ssh/vlan10-ssh-db-bloqueado.png` | SSH desde VLAN 10 a DB fallido | SSH solo VLAN 20 |
| `vlan/vlan10-caja-*.png` | VLAN 10 accede a Caja | Acceso autorizado |
| `vlan/vlan10-inventario-bloqueado-navegador.png` | Página de bloqueo de FortiGuard | Política violada |
| `vlan/vlan10-inventario-https-y-ssh-bloqueado.png` | VLAN 10 → Inventario :443, :22 y DB :22 fallidos | Restricción VLAN 10 |
| `vlan/vlan20-caja-web.png`, `vlan20-inventario-web.png` | VLAN 20 accede a ambas webs | Acceso autorizado |
| `dmz/dmz-a-lan-ping-bloqueado.png` | DMZ → LAN sin respuesta | Sin fuga a LAN |
| `dmz/dmz-a-endpoint-actualizacion-wget.png` | `HTTP 200` a archive.ubuntu.com | Actualizaciones |
| `dmz/dmz-a-internet-ping-bloqueado.png` | DMZ → 8.8.8.8 sin respuesta | Sin Internet abierto |
| `dmz/dmz-a-internet-https-bloqueado.png` | `wget` HTTPS a google/facebook con timeout | Sin Internet abierto |
| `dmz/dmz-a-vlan10-ping-bloqueado.png` | DMZ → 20.25.6.11 sin respuesta | Sin fuga a LAN |
