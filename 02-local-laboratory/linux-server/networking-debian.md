# Debian Networking Configuration

## Objetivo
Establecer una configuración de red persistente, predecible y controlada para el servidor Debian del laboratorio on-premise, sirviendo como base para:
- Integración con Active Directory
- Sincronización de tiempo
- Hardening posterior
- Escenarios híbridos (on-prem / cloud)

## Contexto
El servidor Debian se ejecuta como máquina virtual con dos interfaces de red:
- NAT → salida a Internet (DHCP)
- Host-Only → red interna del laboratorio (IP fija)

Durante la instalación base no se definió una red persistente, por lo cual fue necesario identificar y declarar explícitamente el gestor de red.

## Auditoría inicial
Antes de configurar, se validó el estado del sistema mediante comandos de auditoría para identificar interfaces y gestores activos.

```bash
ip -br link
ip -br addr
systemctl is-active NetworkManager
systemctl is-active systemd-networkd
```

Resultado observado:

- Interfaces físicas detectadas (`enp0s3`, `enp0s8`)
- NetworkManager inactivo
- systemd-networkd inicialmente inactivo
- Configuración de red no persistente

La auditoría inicial confirmó que el sistema contaba con conectividad básica, pero sin un gestor de red activo ni configuración declarativa, lo que implicaba un estado no persistente.

## Decisión de diseño

Se selecciona systemd-networkd como único gestor de red porque:
- Es declarativo y reproducible
- Adecuado para servidores
- Permite control explícito de IP, rutas y DNS
- Evita comportamientos implícitos de NetworkManager

## Estado final

- Gestor de red: systemd-networkd
- Red declarativa y persistente
- DNS controlado
- Entorno listo para los siguientes pasos del laboratorio