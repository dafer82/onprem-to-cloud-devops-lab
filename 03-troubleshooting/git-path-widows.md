# Git no reconocido en PowerShell (Windows PATH Issue)

## Contexto 
Durante la inicialización del repositorio del laboratorio DevOps 
híbrido, VS Code detectaba Git correctamente, pero PowerShell no
reconocía el comando `git`.

El problema surgió al intentar configurar la identidad de Git y realizar el primer commit del repositorio.

## Síntoma
Al ejecutar el comando:
```powershell
git config --global user.name "Denis Fernandez"
```

git : El término 'git' no se reconoce como nombre de un cmdlet, función, archivo de script o programa ejecutable.
