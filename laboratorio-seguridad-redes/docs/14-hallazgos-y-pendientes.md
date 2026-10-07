# 14 · Hallazgos y pendientes

Nada de esto se corrigió en silencio: se documenta lo observado y se conservan los archivos originales.

## A. Puntos por confirmar

| # | Observación | Qué confirmar |
|---|---|---|
| A1 | El bloqueo VLAN 10 → Inventario es *ACCEPT + Web Filter* y solo cubre HTTP (80). | Que es el diseño buscado. HTTPS y SSH al Inventario sí están bloqueados por `VLAN10-DMZ-DENY` (T15–T17); el servidor solo escucha en 80. |
| A2 | `PC2` (VPCS, VLAN 20) tiene captura DHCP y MAC sticky en Gi0/2, pero **no aparece en la topología**. | Si debe agregarse a la foto. |
| A3 | El nodo inferior de la topología (sin etiqueta) tiene un enlace con punto **rojo**; `port6` (192.168.99.1/24) aparece **Up**. | A qué puerto y equipo corresponde. |
| A4 | Las capturas de DMZ → LAN, DMZ → 8.8.8.8 y `wget` no muestran el nombre del servidor origen (solo el nombre de archivo "server caja" en el `wget`). | Servidor usado en cada una. |
| A5 | ✅ **Resuelto:** DMZ → VLAN 10 tiene captura (ping a 20.25.6.11, 3 paquetes, 100 % pérdida). | — |
| A6 | No visibles: valores de `NET-DMZ`, `SRV_CAJA`, `SRV_INVENTARIO`, `VLAN 20`, `VLAN10 address`; servicios de `SVC_UPDATES`; IDs de políticas (solo 5 y 6 en el log); rutas estáticas. | Capturas o export `.conf`. |
| A7 | Los servidores resolvieron `archive.ubuntu.com`, pero no hay captura de su DNS ni de una política DNS para la DMZ. | Cómo resuelven (resolv.conf / DNS en FortiGate). |
| A8 | `running-config` de SWITCH-USERS contiene mensajes de consola intercalados (`%PORT_SECURITY-2-PSECURE_VIOLATION`, `--More--`) que cortan `GigabitEthernet1/3` y el banner. Además no muestra `vlan 10/20` (sí aparecen en `show vlan brief`). | Re-exportar el archivo limpio si se desea. El original se conserva. |
| A9 | Las capturas `show interfaces trunk` / `show vlan brief` muestran `Switch#`, no `SWITCH-USERS`. | Se tomaron antes de asignar hostname (solo informativo). |
| A10 | La MAC `000c.29a0.bf69` (Windows VM) está sticky en Gi0/1 **y** Gi0/2: la VM se movió entre VLAN durante las pruebas. | Es coherente con Windows .11 (VLAN 10) y .139 (VLAN 20). |
| A11 | La topología rotula el nombre como "Crrasco"; el banner dice "CARRASCO". | Corregir el rótulo. |
| A12 | **Falta el enlace del video** (debe ir al inicio del README). | Proveer URL. |

## B. Hallazgos de seguridad (documentados, no corregidos)

| Sev. | Hallazgo | Recomendación |
|:-:|---|---|
| 🟠 | `VLAN20-SSH-DMZ` tiene **NAT activo**: los servidores ven el login desde `20.25.97.1` (FortiGate), no desde `20.25.6.139`. Se pierde trazabilidad en el servidor. | Desactivar NAT (los servidores ya enrutan por el FortiGate), como en `VLAN20-WEB-DMZ`. |
| 🟠 | Administración del FortiGate: WAN con HTTP/HTTPS/SSH/FMG-Access; `port6` con HTTP; `VLAN10` con PING/HTTPS/SSH. | Limitar a una interfaz de gestión, quitar HTTP, usar *trusted hosts*. |
| 🟠 | `ip http server` y `ip http secure-server` activos en ambos switches. | `no ip http server` / `no ip http secure-server`. |
| 🟡 | Sin DHCP Snooping ni Dynamic ARP Inspection. | Habilitar en VLAN 10/20 con el uplink como *trusted*. |
| 🟡 | Puertos libres sin `shutdown`; VLAN nativa 1 en el troncal; SW-DMZ sin VLAN propias. | Apagar puertos libres, cambiar VLAN nativa. |
| 🟡 | Autenticación SSH por contraseña en los servidores. | Llaves SSH. |
| 🟡 | Políticas permiten HTTPS a Caja/Inventario, pero los servidores solo escuchan 80. | Servir TLS o quitar HTTPS. |
| 🟡 | `secret 5` (MD5) y RSA 1024 bits en el script del SW-DMZ. | `secret 9`, RSA ≥ 2048. |
| 🟡 | VLAN 10 → Internet con `ALL`; VLAN 20 sin política hacia Internet. | Confirmar que es intencional. |
| 🔵 | Direccionamiento `20.25.x.x` no es RFC 1918. | Solo relevante fuera del laboratorio. |

## C. Sanitización aplicada al repositorio
- Contraseña en texto claro de `Config-switch-DMZ.txt` → `<REDACTED>`.
- Hashes `secret 5` de ambos `running-config` → `<REDACTED-HASH>`.
- Todo lo demás se conserva sin cambios. Tu nombre y matrícula siguen visibles (banner y topología); retíralos si el repo será público.

## D. Pruebas adicionales sugeridas
1. ~~VLAN 10 → Inventario HTTPS y SSH a `.3`/`.4`~~ ✅ hecho (T15–T17).
2. ~~DMZ → VLAN 10~~ ✅ hecho (T18).
3. ~~DMZ → Internet por TCP 443~~ ✅ hecho (T19, `wget` a google.com y facebook.com).
4. `security.ubuntu.com` y `dl-cdn.alpinelinux.org` (solo se probó `archive.ubuntu.com`).
5. VLAN 10 → VLAN 20.
