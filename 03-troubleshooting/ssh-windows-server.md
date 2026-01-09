# Troubleshooting – OpenSSH en Windows Server

## Contexto
Durante un intento previo de implementación del laboratorio, se intentó administrar el servidor Windows Server mediante OpenSSH desde el host Windows 11.

Debido a múltiples problemas de configuración y para evitar arrastrar inconsistencias, se decidió reiniciar el laboratorio desde cero.
Este documento conserva las lecciones aprendidas de ese intento.

---

## Síntomas
- Error `Connection reset` al intentar conexión SSH
- El servicio `sshd` no iniciaba correctamente
- Configuración aparentemente válida en `sshd_config`

---

## Acciones de troubleshooting realizadas

### 1. Configuración de autenticación
Se habilitaron explícitamente:
- `PasswordAuthentication yes`
- `KbdInteractiveAuthentication yes`

---

### 2. Gestión de llaves SSH
Se eliminaron llaves corruptas y se regeneraron:
```powershell
Remove-Item -Recurse -Force C:\ProgramData\ssh\ssh_host_*
ssh-keygen -A
```

---

### 3. Permisos de archivos (ACLs)
Se identificó como el punto más crítico del problema.

OpenSSH en Windows requiere permisos estrictos sobre los archivos y carpetas ubicados en `C:\ProgramData\ssh`.

Se utilizó `icacls` para:
- Eliminar permisos heredados
- Restringir el acceso a usuarios comunes
- Permitir acceso únicamente a:
  - SYSTEM
  - El servicio `sshd`

---

### 4. Limpieza de herencia
Se deshabilitó la herencia de permisos para evitar que Windows reasignara permisos inseguros automáticamente:

```powershell
icacls C:\ProgramData\ssh /inheritance:r
```
---

## Estado actual
Este procedimiento no se aplica al laboratorio actual.

El entorno fue reconstruido desde cero con una configuración limpia,y OpenSSH no forma parte del flujo operativo actual del laboratorio.

Este documento se conserva como referencia técnica y lección aprendida.