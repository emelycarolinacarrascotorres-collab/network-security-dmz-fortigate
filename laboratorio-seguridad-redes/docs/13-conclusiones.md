# 13 · Conclusiones

- La **DMZ** (port3, rol DMZ) aísla a los tres servidores: no pueden iniciar tráfico hacia VLAN 10/20 y su salida a Internet se limita a tres FQDN de actualización (Ubuntu y Alpine); google.com y facebook.com por HTTPS no responden.
- La **segmentación por VLAN** obliga a que todo el tráfico entre usuarios y servidores pase por el FortiGate, donde cada flujo tiene política y log.
- El **SSH queda reservado a VLAN 20**: VLAN 10 fue bloqueada y VLAN 20 accedió a los tres servidores.
- El acceso de VLAN 10 al **Inventario** se bloquea con *Web Filter* (capa 7), con página de bloqueo y log como prueba; el resto de puertos hacia la DMZ (HTTPS, SSH) lo cierra `VLAN10-DMZ-DENY`.
- Los **switches** aplican seguridad de capa 2 básica (port-security sticky, BPDU Guard, troncal acotado, SSH v2) que se evidenció con violaciones registradas.
