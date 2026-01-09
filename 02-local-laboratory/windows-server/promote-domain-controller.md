# Promote to Domain Controller – SRV-AD-01

## Objetivo
Promover el servidor Windows Server 2022 a Domain Controller principal del laboratorio, creando un nuevo forest con DNS integrado.

---

## Configuración aplicada
- Dominio: `corp.local`
- NetBIOS: `CORP`
- Forest/Domain Functional Level: Windows Server 2016
- Roles:
  - Active Directory Domain Services
  - DNS Server
- Global Catalog: habilitado
- RODC: no

---

## Proceso
La promoción se realizó mediante **Server Manager**, utilizando el asistente de Active Directory Domain Services.

El sistema realizó la instalación de componentes, configuró DNS integrado y ejecutó un reinicio automático.

---

## Validaciones

### Active Directory
```powershell
dcdiag
# Promote to Domain Controller – SRV-AD-01

## Objetivo
Promover el servidor Windows Server 2022 a Domain Controller principal del laboratorio, creando un nuevo forest con DNS integrado.

---

## Configuración aplicada
- Dominio: `corp.local`
- NetBIOS: `CORP`
- Forest/Domain Functional Level: Windows Server 2016
- Roles:
  - Active Directory Domain Services
  - DNS Server
- Global Catalog: habilitado
- RODC: no

---

## Proceso
La promoción se realizó mediante **Server Manager**, utilizando el asistente de Active Directory Domain Services.

El sistema realizó la instalación de componentes, configuró DNS integrado y ejecutó un reinicio automático.

---

## Validaciones

### Active Directory
```powershell
dcdiag

