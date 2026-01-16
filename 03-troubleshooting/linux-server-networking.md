# Troubleshooting — Linux Server (Networking)

## Objetivo

Documentar los problemas reales encontrados durante la configuración del servidor Debian y las lecciones aprendidas, con el objetivo de:

- Acelerar futuros diagnósticos
- Evitar repetir errores comunes
- Servir como referencia operativa (runbook)

Este documento no describe pasos ideales, sino escenarios reales de fallo.

---

## Alcance

Este troubleshooting cubre únicamente:
- Gestión de red en Debian Server
- systemd-networkd/systemd-resolved
- DNS y conectividad básica

**No incluye:**
- Problemas de Active Directory
- Hardening
- Azure / cloud

---

## Escenario 1 - Red funciona pero no es persistente

### Sintomas
- La red funciona tras el arranque
- La IP desaparece luego reboot o restart de servicios

### Diagnóstico

```bash
ip -br addr
systemctl is-active systemd-networkd
```

**Causa raíz:**
- La ip fue asignada de forma implícita (DHCP temporal o configuración manual), sin declaración persistente.

**Resolución:**
- Elegir un gestor de red explícito
- Declarar la configuración en archivos `.network`

### Lección aprendida
**Si no está declarada, la configuración no existe**

---

## Escenario 2 – Ningún gestor de red activo

### Síntomas

- La red funciona parcialmente

- No hay claridad sobre quién gestiona las interfaces

### Diagnóstico

```bash
systemctl is-active NetworkManager systemd-networkd dhcpcd
```
**Causa raíz**

- Sistema instalado sin red persistente definida.

**Resolución**

- Seleccionar un gestor (systemd-networkd)
- Activarlo explícitamente

---

## Escenario 3 – DNS cambia o se pierde

### Síntomas

- Resolución DNS intermitente
- Cambios inesperados en `/etc/resolv.conf`

### Diagnóstico

```bash
ls -l /etc/resolv.conf
resolvectl status
```
**Causa raíz**
- Edición manual de `resolv.conf` o DNS no declarado en el gestor de red.

**Resolución**
- No editar `resolv.conf`
- Declarar DNS en el archivo `.network`

---

## Escenario 4 – Múltiples gateways

### Síntomas
- Conectividad errática
- DNS o AD falla de forma intermitente

### Diagnóstico

```bash
ip route
```

**Causa raíz**
- Más de una interfaz configurada con gateway por defecto.

**Resolución**
- Definir gateway solo en la interfaz de salida
- Eliminar gateway de redes internas

---

## Escenario 5 – Archivos `.network` duplicados o ambiguos

### Síntomas

- Interfaces UP/DOWN sin lógica
- Configuración aparentemente correcta pero inestable

### Diagnóstico

```bash
ls -l /etc/systemd/network/ | cat -A
ls -li /etc/systemd/network/
```
**Causa raíz**

- Archivos duplicados
- Espacios invisibles en nombres
- Configuraciones heredadas

**Resolución**
- Eliminar archivos sobrantes
- Mantener un archivo `.network` por interfaz

---

## Conclusiones

**Los problemas de red en Linux suelen estar asociados a:**

- Ambigüedad en la gestión
- Configuración implícita
- Múltiples fuentes de verdad

**La solución consistente es:**

- Declarar intención
- Usar un único gestor
- Validar persistencia

---

## Nota

Este documento complementa:
- `install-debian.md`
- `networking-debian.md`

Y refleja problemas reales encontrados durante el laboratorio.