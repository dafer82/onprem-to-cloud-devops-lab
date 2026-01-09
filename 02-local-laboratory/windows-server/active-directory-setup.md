# Active Directory Setup – Local Laboratory

## Objetivo
Documentar la configuración final de Active Directory Domain Services implementada en el laboratorio on-premise.

Este documento actúa como referencia base de identidad para las siguientes fases del laboratorio (Linux, DevOps e integración híbrida).

---

## Dominio
- Nombre del dominio: corp.local
- NetBIOS: CORP
- Tipo: Nuevo forest

---

## Controlador de Dominio
- Hostname: SRV-AD-01
- Sistema Operativo: Windows Server 2022
- Roles:
  - Active Directory Domain Services
  - DNS Server
- Global Catalog: habilitado
- RODC: no

---

## DNS
- DNS integrado con Active Directory
- Escucha en:
  - 192.168.56.10 (Host-Only)
  - Interfaz NAT (para salida externa)
- Forwarders configurados durante la instalación

---

## Red
- NAT:
  - DHCP
  - Gateway habilitado
- Host-Only:
  - IP fija: 192.168.56.10
  - Sin gateway
- Justificación: evitar rutas ambiguas y aislar tráfico interno

---

## Validaciones
```powershell
dcdiag
nslookup corp.local

