# Arquitectura del Laboratorio DevOps Híbrido

## 1. Visión General
Este laboratorio simula una infraestructura corporativa on-premise
de pequeña escala, diseñada desde su origen con proyección hacia un entorno híbrido con Azure.

La arquitectura prioriza claridad, separación de responsabilidades
y reproducibilidad, sirviendo como base para prácticas DevOps y 
modernización hacia la nube.

---

## 2. Objetivo Arquitectónico
El objetivo principal de la arquitectura es:

- Separar responsabilidades entre servicios y capas.
- Mantener un entorno reproducible y fácilmente entendible.
- Preparar el laboratorio para una futura integración híbrida con Azure.
- Alinear el diseño con prácticas reales de entornos corporativos.

Este laboratorio no busca ser una infraestructura productiva, sino un entorno de aprendizaje controlado, coherente y evolutivo.

---

## 3. Componentes del Entorno

### Host físico
- Sistema operativo: Windows 11 Pro
- Rol: host de virtualización y punto de administración

### Máquinas virtuales
- **SRV-AD-01**
    - Sistema: Windows Server 2022
    - Rol: Active Directory Domain Services y DNS
- **SRV-LNX-01**
    - Sistema: Debian (stable)
    - Rol: base para contenedores, automatización y futuras pipelines DevOps

---

## 4. Topología de Red

### Redes utilizadas
- **NAT**
    - Provee acceso a Internet para actualizaciones y descargas.
- **Host-Only**
    - Red interna del laboratorio.
    - Permite comunicación segura entre las VMs y el host sin exposición externa.

### Justificación 
La separación de redes permite:
- Aislar servicios críticos del exterior.
- Simular una red corporativa interna realista.
- Facilitar la futura integración con entornos cloud (conceptos equivalentes a VNETs).

> El uso de redes Bridge se evita para mantener control, evitar dependencia de la red física y reducir riesgos en servicios sensibles como Active Directory.

Un diagrama lógico de esta topología se encuentra en:
`diagrams/network-topology.png`

---

## 5. Decisiones Técnicas Clave

### Administración de Domain Controllers
No se habilita SSH en controladores de dominio.
La administración se realiza mediante herramientas nativas de Windows:
- RSAT
- PowerShell

Esta decisión se alinea con prácticas habituales en entornos corporativos Windows, priorizando coherencia operativa y seguridad.

### Separación lógica de servicios
Cada máquina virtual cumple un rol bien definido:
- Identidad y DNS en Windows Server.
- Automatización, contenedores y DevOps en Linux.

Esto facilita mantenimiento, troubleshooting y escalabilidad conceptual del laboratorio.

---

## 6. Virtualización 

### Hypervisor elegido
Se utiliza VirtualBox como único hypervisor del laboratorio.

**Motivos:**
- Compatibilidad con Windows 11.
- Facilidad de uso en entornos educativos.
- Menor complejidad operativa frente a alternativas enterprise.

### Limitaciones aceptadas
- Rendimiento inferior a hypervisores bare-metal.
- No orientado a producción.
- Uso exclusivo para laboratorio de aprendizaje.

Un diagrama general de virtualización se referencia en:
`diagrams/virtualization-overview.png`

---

## 7. Preparación para Entorno Híbrido
La arquitectura está pensada para evolucionar progresivamente hacia un modelo híbrido:

- Uso de Active Directory como base de identidad.
- Separación clara de redes (conceptualmente equivalente a on-prem + cloud).
- Introducción futura de sincronización de identidades con Azure.
- Extensión de prácticas DevOps desde Linux hacia servicios cloud.

Este diseño permite avanzar hacia Azure sin rediseñar completamente el laboratorio.

---

## 8. Alcance y Evolución 
La arquitectura documentada en esta sección constituye la base del laboratorio.
Sobre ella se construirán las siguientes fases:

- Configuración on-premise
- Automatización y DevOpsSV
- Integración híbrida con Azure

Cada fase se documentará manteniendo coherencia con esta arquitectura base.
