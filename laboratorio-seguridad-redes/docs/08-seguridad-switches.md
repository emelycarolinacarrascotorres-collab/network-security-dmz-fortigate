# 08 · Seguridad de switches

Configuraciones reales: [`SW-DMZ-0697`](../configs/sw-dmz-0697/running-config.txt) · [`SWITCH-USERS`](../configs/switch-users/running-config.txt). Secretos ocultados (`<REDACTED>`); los comandos no se modificaron.

## SWITCH-USERS

| Puerto | Modo | VLAN | Seguridad |
|---|---|---|---|
| Gi0/0 | Trunk 802.1Q (`TRUNK-A-FORTIGATE`) | permitidas 10,20 | `nonegotiate` |
| Gi0/1 | Access | 10 | port-security máx. 2, `restrict`, sticky · PortFast · BPDU Guard |
| Gi0/2 | Access | 20 | port-security máx. 2, `restrict`, sticky · PortFast · BPDU Guard |
| Gi0/3 y resto | sin configurar | 1 | — |

Sticky aprendidas: Gi0/1 → `0050.7966.6800` (PC1) y `000c.29a0.bf69` (Windows VM); Gi0/2 → `0050.7966.6801` (PC2) y `000c.29a0.bf69`.

**Evidencia de violación:** el archivo contiene líneas de `%PORT_SECURITY-2-PSECURE_VIOLATION` por la MAC `0050.56c0.0002` en `Gi0/2` (puerto ya con 2 MAC aprendidas). Con `restrict` el switch descarta esas tramas y registra el evento sin apagar el puerto.

## SW-DMZ-0697

| Puerto | Modo | Seguridad |
|---|---|---|
| Gi0/0 `UPLINK-FORTIGATE-PORT3` | Access | — |
| Gi0/1 | Access | sticky `0242.41f2.b600` (Caja) · PortFast · BPDU Guard |
| Gi0/2 | Access | sticky `0242.eaf2.8c00` (DB) · PortFast · BPDU Guard |
| Gi0/3 | Access | sticky `0242.62e7.6d00` (Inventario) · PortFast · BPDU Guard |

Las MAC coinciden con los `ip a` de los servidores ([09](09-servidores.md)). Sin `violation` definido, el modo es el predeterminado (**shutdown**). Sin VLAN propias: todo en VLAN 1.

## Medidas comunes (ambos)
- `service password-encryption`, `enable secret` y usuario local `Admin` (priv 15, `secret 5`).
- SSH v2 (`ip domain-name redlocal.com`), cifrados `aes128/192/256-ctr`, timeout 60 s.
- `line vty 0 4`: `login local`, `transport input ssh`, `exec-timeout 5 0`. Consola con `exec-timeout 5 0`.
- `no ip domain-lookup`, banner MOTD de acceso restringido.



## Riesgo que reducen
| Control | Riesgo |
|---|---|
| Port-security sticky | Conexión de equipos no autorizados / MAC flooding |
| BPDU Guard + PortFast | Switches o loops introducidos en puertos de usuario |
| Trunk acotado + nonegotiate | VLAN hopping |
| SSH v2 + login local | Administración en texto claro (Telnet) |
