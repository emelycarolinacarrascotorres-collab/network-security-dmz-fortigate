# Scripts

| Archivo | Propósito | Requisitos | Cómo ejecutarlo | Resultado |
|---|---|---|---|---|
| [`configurar-ip-servidores.txt`](configurar-ip-servidores.txt) | Asigna IP y gateway a Caja (.2), Inventario (.3) y DB (.4) | Consola de cada contenedor/servidor con `iproute2`, usuario root | Pegar el bloque del servidor correspondiente | `eth0` con IP `/28` y ruta por defecto `20.25.97.1` (verificable con `ip a`) |
| [`comandos-de-prueba.md`](comandos-de-prueba.md) | Comandos usados en las pruebas | PowerShell (Windows) / shell Linux | Ver archivo | Salidas en `evidence/` |

Las configuraciones de switch están en [`configs/`](../configs/).
