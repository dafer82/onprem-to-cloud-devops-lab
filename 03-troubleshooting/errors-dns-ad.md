# Troubleshooting – DNS en Active Directory

## Contexto
Durante la implementación de Active Directory en el laboratorio on-premise, no se presentaron errores críticos de DNS.

Este documento registra comportamientos observados, advertencias comunes y validaciones realizadas, con el objetivo de evitar confusiones durante las siguientes fases del laboratorio.

---

## Comportamientos observados

### Resolución por múltiples interfaces
El servidor Domain Controller cuenta con dos interfaces de red:

- NAT (IP asignada dinámicamente)
- Host-Only (IP fija: 192.168.56.10)

Durante pruebas de resolución DNS (`nslookup`), el dominio puede resolver a más de una dirección IP asociada al mismo host.

Esto es un comportamiento esperado en servidores multihomed y **no representa un error** mientras:

- La red interna (Host-Only) sea la utilizada por los clientes del dominio - No existan gateways múltiples configurados
- No existan gateways múltiples configurados

---

## Advertencias comunes (esperables)

### Advertencia de delegación DNS durante la promoción
Durante la promoción a Domain Controller, se presentó una advertencia relacionada con la delegación DNS.

Esta advertencia es normal en entornos de laboratorio donde:
- No existe un DNS padre
- Se crea un nuevo forest desde cero

No requiere acción correctiva.

---

## Validaciones realizadas

### Active Directory
```powershell
dcdiag
```

### Resolución de nombres
```powershell
nslookup corp.local
```
---

## Estado actual

El servicio DNS integrado con Active Directory se encuentra operativo y estable.

No se identificaron errores que requieran acciones de troubleshooting activas.
Este documento se conserva como referencia preventiva para futuras integraciones (Linux, automatización y escenarios híbridos).

---

## Estado del archivo

Con o sin esos micro-ajustes, el archivo está:

- Técnicamente correcto  
- Honesto (no inventa errores)
- Útil para Linux y fases futuras
- Bien ubicado en `03-troubleshooting



