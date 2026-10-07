# Comandos de prueba (tomados de las capturas)

**Windows 10 (VLAN 10 / VLAN 20) — PowerShell**
```powershell
Test-NetConnection 20.25.97.2 -Port 22      # SSH
Test-NetConnection 20.25.97.2 -Port 80      # HTTP
ssh admin0697@20.25.97.2                    # también .3 y .4
```

**Servidores DMZ — shell**
```sh
ip a
ss -tln | grep -E ':22|:80'
ping -c 4 20.25.6.139                       # DMZ -> LAN (bloqueado)
ping -c 4 8.8.8.8                           # DMZ -> Internet (bloqueado)
wget --spider -T 10 http://archive.ubuntu.com
wget -S -T 10 -O /dev/null http://archive.ubuntu.com
```

**VPCS**
```text
ip dhcp
show ip
```

**Switch**
```text
show interfaces trunk
show vlan brief
show run
```
