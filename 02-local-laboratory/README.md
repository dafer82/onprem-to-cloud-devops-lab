# Local Laboratory Setup (On-Premise)

## Objetivo
Esta sección documenta la construcción del laboratorio on-premise local, siguiendo la arquitectura definida en `01-architecture`.

El propósito es disponer de un entorno funcional, reproducible y controlado antes de introducir automatización, DevOps y cloud.

---

## Alcance 
**En esta etapa se cubre:**

- Validación del host de virtualización
- Configuración de redes del laboratorio
- Creación de máquinas virtuales
- Instalación base de sistemas operativos

**No se incluyen aún:**
- Automatización
- Contenedores
- Integración con Azure

---

## Componentes del laboratorio
- Host: Windows 11 Pro
- Hypervisor: VirtualBox
- Red: 
    - NAT (acceso a Internet)
    - Host-Only (red interna del laboratorio)
- Máquinas virtuales:
    - **SRV-AD-01** (Windows Server 2022)
    - **SRV-LNX-01** (Debian Linux)

---

## Flujo de trabajo
1. Validar prerrequisitos del host
2. Configurar red interna del laboratorio
3. Crear máquinas virtuales
4. Instalar sistemas operativos
5. Validar conectividad básica

---

## Resultado esperado
Al finalizar esta sección, se dispone de:

- Dos máquinas virtuales operativas
- Conectividad a Internet vía NAT
- Red interna Host-Only funcional
- Base estable para:
  - Active Directory
  - Automatización
  - Prácticas DevOps
  - Integración híbrida con Azure


Cada paso está documentado en archivos separados dentro de esta sección.

