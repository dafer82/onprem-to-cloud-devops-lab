# Debian Installation — SVR-LNX-01

## Objetivo

Documentar la instalación **Debian GNU/Linux (Debian 13 "Trixie")** en la máquina virtual **SRV-LNX-01**, previo a la configuración de red interna. 

Esta etapa establece una base estable, limpia y actualizada del sistema operativo.

---

## Alcance
**Incluye:**
- Instalación del sistema operativo
- Configuración regional básica
- Creación del usuario administrativo
- Aplicación de actualizaciones iniciales
- Validaciones básicas post-instalación

**No incluye:**
- Configuración de red persistente
- Asignación de IP fija o DNS
- Integración con dominio
- Configuración de sssd
- Firewall o hardening
- Ajustes avanzados de privilegios (root / sudo)

----

## Detalles de la instalación 

- Sistema operativo: Debian GNU/Linux 13 (Trixie)
- Tipo de instalación: Standard system utilities (sin entorno gráfico)
- Hypervisor: VirtualBox
- Disco: 50 GB (VDI dinámico)
- Red durante la instalación: NAT

---

## Configuración inicial del sistema

### Nombre del servidor

Durante la instalación se definió el nombre del host como:

```text
SRV-LNX-01
```

### Usuario administrativo

Se creó el siguiente usuario administrativo durante la instalación:

```text
admin
```