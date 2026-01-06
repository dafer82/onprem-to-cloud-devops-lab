# Network setup – VirtualBox (NAT + Host-Only)

## Objetivo
Configurar la red base del laboratorio en VirtualBox para:

- Mantener conectividad a Internet desde las VMs (actualizaciones/descargas) mediante **NAT**
- Crear una red interna controlada entre VMs y host mediante **Host-Only**

Este esquema evita exponer servicios críticos (ej. Active Directory) a la red física.

---

## 1. Diseño de red del laboratorio

### Redes utilizadas
- **NAT**
  - Propósito: salida a Internet desde las VMs
  - Observación: VirtualBox crea NAT automáticamente en cada VM configurada como NAT

- **Host-Only (vboxnet0)**
  - Propósito: red interna del laboratorio (LAN simulada)
  - Segmento: `192.168.56.0/24`
  - IP del host (adaptador host-only): `192.168.56.1`
  - DHCP: **OFF** (se usarán IPs estáticas)

---

## 2. Crear la red Host-Only (vboxnet0)

### 2.1 Crear adaptador Host-Only
En VirtualBox:

1. `File` → `Tools` → `Host Network Manager`
2. Crear una red Host-Only:
   - Nombre: `vboxnet0`
   - IPv4 Address (host): `192.168.56.1`
   - IPv4 Network Mask: `255.255.255.0`
   - DHCP Server: **Disabled**

> Nota: si ya existe `vboxnet0`, no crear una nueva. Ajustar la existente.

---

## 3. Validaciones en el host (Windows 11)

### 3.1 Verificar adaptador creado
En PowerShell:

```powershell
Get-NetAdapter | Select-Object Name, Status, LinkSpeed
```
