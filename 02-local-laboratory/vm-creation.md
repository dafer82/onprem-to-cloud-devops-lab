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
- Sistema Operativo: Debian GNU/Linux (stable – Debian 13 "Trixie")
- Tipo: Servidor Linux

---

## Consideraciones generales
- Todas las VMs se ejecutan sobre VirtualBox (Type 2 Hypervisor)
- La red será configurada utilizando:
    - NAT (salida a Internet)
    - Host-Only (comunicación interna del laboratorio)
- La instalación de los sistemas operativos se documentará en pasos posteriores

---

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
  - Adapter 2 (future): Host-Only (internal lab network)

### Notas
- La máquina virtual se crea apagada.
- Se configurarán adaptadores de red adicionales después de la instalación del sistema operativo.
- La asignación de recursos está pensada para un entorno de laboratorio, no productivo.

---

## Linux VM Creation - SRV-LNX-01

### Descripción general
Esta máquina virtual actúa como el nodo operativo y de servicios ágiles del laboratorio. Su función principal es servir como host para el despliegue de microservicios mediante contenedores y como motor central de automatización (Infrastructure as Code) para gestionar el parque de servidores, incluido el controlador de dominio.

### Configuración de la VM
- OS: Debian GNU/Linux (Debian 13 "Trixie")
- CPU: 1 vCPU 
- Memory: 2 GB RAM 
- Disk:
  - Type: VDI
  - Max size: 50 GB
  - Allocation: Dynamic
- Network
  - Adapter 1: NAT (internet access during setup)
  - Adapter 2 (future): Host-Only (internal lab network)

### Notas
- Esta VM será integrada posteriormente al dominio Windows para utilizar Active Directory como fuente de identidad y DNS.
- Desde este nodo se ejecutarán tareas de automatización y gestión remota sobre el entorno Windows.

---

## Estado de la etapa
Con la creación de ambas máquinas virtuales finalizada, el laboratorio está listo para proceder con la instalación y configuración de los sistemas operativos.
