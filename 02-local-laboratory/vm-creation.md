# VM Creation - Local Laboratory

## Objetivo
Documentar la creación y configuración inicial de las máquinas virtuales del laboratorio local, previo a la instalación de los sistemas operativos.

**Esta etapa se enfoca únicamente en:**
- Definir roles
- Configurar recursos
- Configurar red en VirtualBox

---

## Máquinas virtuales del laboratorio

### SRV-AD-01
- Rol: Controlador de Dominio (Active Directory + DNS)
- Sistema Operativo: Windows Server 2022
- Tipo: Servidor Windows

### SRV-LNX-01
- Rol: Servidor Linux para contenedores y automatización
- Sistema Operativo: Debian (stable)
- Tipo: Servidor Linux

---

## Consideraciones generales
- Todas las VMs se ejecutan sobre VirtualBox (Type 2 Hypervisor)
- La red será configurada utilizando:
    - NAT (salida a Internet)
    - Host-Only (comunicación interna del laboratorio)
- La instalación de los sistemas operativos se documentará en pasos posteriores

## Windows VM Creation – SRV-AD-01

### Descripción general
Esta máquina virtual está diseñada para actuar como el controlador de dominio principal para el entorno de laboratorio local.

### Configuración de la VM
- OS: Windows Server 2022 (64-bit)
- CPU: 2 vCPU
- Memory: 4 GB RAM
- Disk:
  - Type: VDI
  - Max size: 80 GB
  - Allocation: Dynamic (default VirtualBox behavior)
- Network:
  - Adapter 1: NAT (internet access during setup)

### Notas
- La máquina virtual se crea apagada.
- Se configurarán adaptadores de red adicionales después de la instalación del sistema operativo.
- La asignación de recursos está pensada para un entorno de laboratorio, no productivo.
