# Prerequisites - Local Laboratory

## Objetivo
Este documento valida que el host local esté correctamente preparado para ejecutar el laboratorio on-premise basado en VirtualBox.

La verificación de estos prerrequisitos evita conflictos posteriores, especialmente relacionados con virtualización, red y rendimiento.

---

## 1. Requisitos de Hardware

- CPU con soporte de virtualización (Intel VT-x / AMD-V)
- Virtualización habilitada en BIOS/UEFI
- Mínimo recomendado:
    - 4 cores lógicos
    - 16 Gb de RAM
    - 80 Gb de espacio libre en disco

> En este laboratorio se prioriza estabilidad sobre performance extrema.

---

## 2. Sistema Operativo Host

- Sistema operativo: **Windows 11 Pro**
- Rol:
    - Host de virtualización
    - Administración del laboratorio
    - Punto de acceso a Internet

---

## 3. Virtualización en Firmware (BIOS/UEFI)

Debe estar habilitado:

- **Intel VT-x** / **AMD-V**
- **Second Level Address Translation (SLAT)**

Verificación en Windows (PowerShell):

```powershell
systeminfo | Select-String "Virtual"
```
