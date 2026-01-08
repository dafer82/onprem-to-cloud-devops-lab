# Conflicto Hyper-V / VBS vs VirtualBox (Windows 11)

## Síntomas típicos
- Error al iniciar VM (ej. 0x8007007E)
- Rendimiento muy lento en VirtualBox
- VMs que no arrancan o quedan colgadas

## Causa raíz (resumen)
Windows puede estar usando virtualización para:
- Hyper-V
- VBS / Core Isolation (Memory Integrity)
- Windows Hypervisor Platform (WHP)
Esto puede impedir que VirtualBox use VT-x/AMD-V de forma nativa.

## Diagnóstico rápido
### 1) Estado del hypervisor (BCDEdit)
```powershell
bcdedit /enum | Select-String -Pattern "hypervisorlaunchtype"
```
