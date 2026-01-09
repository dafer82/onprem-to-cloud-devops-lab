# Windows Server Installation - SRV-AD-01

## Objetivo
Documentar la instalación base de Windows Server 2022 en la máquina virtual **SRV-AD-01**, previo a la configuración de red interna y a la instalación de roles como Active Directory Domain Services. 

Esta etapa establece una base estable y actualizada del sistema operativo.

---

## Alcance
**Incluye:**
- Instalación del sistema operativo
- Configuración regional básica
- Aplicación de actualizaciones del sistema
- Validaciones iniciales

**No incluye:**
- Configuración de red Host-Only
- IP estática
- Instalación de roles (AD DS/ DNS)

---

## Detalles de la instalación 

- Sistema Operativo: Windows Server 2022 Standard
- Tipo de instalación: Desktop Experiencie
- Hypervisor: VirtualBox
- Disco: 80Gb (VDI dinámico)
- Red durante instalación: NAT

---

## Configuración inicial del sistema

### Nombre del servidor
El servidor fue nombrado a:

```text
SRV-AD-01
```

### Configuración regional
- Teclado: Latinoamericano
- zona horaria: UTC-3
- Hora del sistema validada antes de instalar roles

Esta configuración se realizó previo a la promoción del servidor como Domain Controller para evitar inconsistencias de tiempo en Active Directory.

### Servicio de tiempo
Previo a la promoción a Domain Controller, el servicio Windows Time (w32time) se encontraba detenido, lo cual es un comportamiento esperado en un servidor standalone recién instalado.

La configuración de sincronización de tiempo se realizará una vez instalado Active Directory.