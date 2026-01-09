# Network configuration VM Windows

## Interfaces:
- **Ethernet** 
    - Tipo: NAT
    - Asignación: DHCP
    - Propósito: salida a Internet (updates, descargas)

- **Ethernet 2**
    - Tipo: Host-Only
    - Asignación: IP fija
    - Propósito: red interna del laboratorio

---


## IPs configuradas

### Ethernet (NAT)
- IP: asignada dinámicamente (DHCP)
- Gateway: presente
- DNS: provisto por NAT de VirtualBox

### Ethernet 2 (Host-Only)
- IP: `192.168.56.10`
- Subnet: `255.255.255.0`
- Default Gateway: **no configurado**
- DNS: **no configurado en esta etapa**

---

## Justificación
- Uso de **doble NIC** para separar tráfico:
    - NAT para conectividad externa
    - Host-Only para comunicación interna
- El **gateway se configura únicamente en la interfaz NAT** para evitar rutas ambiguas.
- La interfaz Host-Only permanece aislada y será utilizada por Active Directory y servicios internos.

---

## Validación 

Desde PowerShell:

```powershell
ipconfig
```