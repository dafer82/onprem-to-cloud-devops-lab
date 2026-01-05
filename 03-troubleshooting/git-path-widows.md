# Git no reconocido en PowerShell (Windows PATH Issue)

## Contexto 
Durante la inicialización del repositorio del laboratorio DevOps 
híbrido, Visual Studio Code detectaba Git correctamente, pero PowerShell no reconocía el comando `git`.

El problema apareció al intentar configurar la identidad de Git y realizar el primer commit del repositorio.

Este escenario es común en entornos Windows donde Git fue instalado sin integrarse correctamente al PATH del sistema.

---

## Síntoma
Al ejecutar el comando:
```powershell
git config --global user.name "Denis Fernandez"
```

git: El término 'git' no se reconoce como nombre de un cmdlet, función, archivo de script o programa ejecutable.
Mientras tanto, VS Code sí detectaba Git, generando confusión inicial.

---

## Causa raíz

```text
Git estaba correctamente instalado en el sistema, pero su directorio de binarios (`git.exe`) no estaba incluido en la variable de entorno PATH de Windows.
```

Esto provocó que:

    - VS Code pudiera usar Git (porque lo detecta internamente).
    - PowerShell no pudiera ejecutar comandos Git directamente.

---

## Resolución
#### 1. Verificar la ruta de instalación de Git:
`C:\Program Files\Git\cmd`
#### 2. Agregar manualmente esa ruta a la variable de entorno PATH del sistema:
    - Panel de control ➡️ Sistema ➡️ Configuración avanzada
    - Variable de entorno ➡️ PATH ➡️ Editar ➡️ Agregar la ruta de Git
#### 3. Cerrar y volver a abrir PowerShell para aplicar los cambios.

---

## Verificación
Desde una nueva sesión de PowerShell
``` powershell
git --version
```

**Resultado esperado:**
        git version 2.x.x.windows.x
A partir de este punto, los comandos Git funcionaron correctamente y se pudo realizar el primer commit del repositorio.

---

## Lecciones aprendidas
```md
- VS Code puede detectar Git aunque el PATH esté mal configurado.
- En entornos DevOps sobre Windows, verificar PATH es un paso inicial clave.
- Documentar este tipo de problemas evita bloqueos futuros y acelera troubleshooting.
```
---

## Relación con el laboratorio
Este incidente ocurrió durante la fase inicial del laboratorio y forma parte de la experiencia real de operación y configuración del entorno local DevOps.

Documentarlo permite:
    - Reproducibilidad
    - Transferencia de conocimiento
    - Mejora continua del setup base

---

## Siguiente paso
Continuar con la configuración del laboratorio local, una vez validado el entorno de desarrollo y versionado.

