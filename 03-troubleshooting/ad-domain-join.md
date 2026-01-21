# AD Domain Join — Debian (SRV-LNX-01)


## Objetivo
Integrar el servidor Debian **SRV-LNX-01** como miembro del dominio `corp.local`, permitiendo:
- Autenticación de usuarios de Active Directory en Linux
- Resolución de identidades (usuarios y grupos)
- Base para hardening, control de accesos y automatización
- Escenarios híbridos on-prem / cloud

Este paso establece **identidad**, no solo conectividad.

---

## Qué significa "unirse a un dominio" en Linux
A diferencia de Windows, en Linux **no existe un único servicio** que "haga todo".

Un domain join en Linux implica:

- Confiar criptográficamente en el dominio (Kerberos)
- Resolver usuarios y grupos desde AD (LDAP)
- Integrar esa identidad al sistema operativo (PAM / NSS)

👉 El join **no es un evento**, es un **estado** que debe mantenerse estable en el tiempo.

---

## Componentes involucrados


### DNS (Active Directory)
- Descubre el dominio
- Resuelve controladores de dominio
- Localiza servicios (SRV records)

👉 Sin DNS correcto, **nada más funciona**.


### Time / Kerberos
- Kerberos es sensible al tiempo
- Diferencias > 5 minutos rompen autenticación
- El DC es la única fuente de tiempo válida

👉 DNS puede "funcionar", pero Kerberos puede fallar silenciosamente.


### realmd
- Descubre el dominio
- Orquesta el proceso de join
- Crea la cuenta de equipo en AD
- Configura servicios locales

👉 Es el "coordinador", no el autenticador.


### sssd
- Servicio de identidad
- Consulta usuarios y grupos de AD
- Maneja cache y disponibilidad offline
- Integra con PAM y NSS

👉 Si sssd falla, el join "existe" pero **no sirve**.


### PAM / NSS
- Permiten login real en el sistema
- Vinculan credenciales AD con sesiones Linux
- Controlan acceso y creación de home directories

---

## Precondiciones obligatorias

Antes de ejecutar **cualquier** join, deben cumplirse **todas:**


### Red
- Conectividad estable con el DC
- NIC correcta (Host-Only)


### DNS
- Resolver solo contra el DC
- Dominio `corp.local` resuelve
- SRV record accesibles (SRV records validados implícitamente vía realmd)

```bash
nslookup corp.local
nslookup srv-ad-01.corp.local

```

### Tiempo
- Sincronizado contra el DC

```bash
timedatectl

```
Debe mostrar:
- `System clock synchronized: yes`
- `NTP service: active`


### Hostname / FQDN

```bash
hostname -f

```
Debe reflejar:

```lua
srv-lnx-01.corp.local

```

---

## Flujo del Domain Join
1. DNS descubre el dominio  
2. Kerberos valida identidad  
3. realmd ejecuta el join  
4. AD crea/actualiza la cuenta del equipo  
5. sssd habilita identidades  
6. PAM permite login  

👉 Si un paso falla, **todo lo que sigue se degrada**

---

## Errores típicos del join
- DNS correcto "a veces"
- Tiempo sincronizado visualmente, pero no por NTP
- Credenciales válidas, pero permisos insuficientes
- Dominio descubierto, pero join falla sin error claro

> Estos se documentan en detalle en troubleshooting

---

## Fallos silenciosos post-join
- `realm list` muestra dominio, pero:
    - usuarios no existen
    - login falla
    - `id usuario@corp.local` no responde
- sssd activo, pero mal configurado
- Cache inconsistente tras reboot

👉 El join **no termina** cuando el comando devuelve "OK".

---

## Estado esperado tras un join exitoso
- Dominio visible con `realm list`
- Usuario AD visible con `id`/`getent`
- Login interactivo funcional
- Identidad persistente tras reboot

---

## Referencias internas
- Paso 2 — Networking Debian
- Paso 3 — Time & DNS
- `ad-time-dns-troubleshooting.md`

---

## Diagramas  — Modelo mental y flujo


### Modelo mental
Este diagrama responde a la pregunta:

> **"Quién habla con quién y para qué"** 

```text
+------------------------+
|      Debian Linux      |
|      (SRV-LNX-01)      |
+------------------------+         
            |
            | 1. DNS (A / SRV)  
            v
+------------------------+
|     AD DNS Server      |
| (srv-ad-01.corp.local) |
+------------------------+         
            |
            | 2. Kerberos (time-sensitive)  
            v
+------------------------+
|       KDC / DC         |
|      corp.local        |
+------------------------+         
            |
            | 3. realmd join  
            v
+------------------------+
|    Active Directory    |
|   Domain Membership    |
+------------------------+         
            |
            | 4. LDAP queries  
            v
+------------------------+
|         sssd           |
|     Identity Cache     |
+------------------------+         
            |
            | 5. PAM / NSS  
            v
+------------------------+
|     Linux Login /      |
|     Authorization      |
+------------------------+         

```
**Como entender el grafico**
- DNS descubre
- Kerberos confía
- realmd une
- sssd mantiene identidad
- PAM/NSS permiten login

👉 Si uno falla, **todo lo que está debajo se rompe.**


#### Flujo del Domain Join

Este diagrama responde a:

> **"En qué orden lógico ocurre el join"**

```text
[ Precondiciones OK ]
         |
         v
[ DNS resuelve dominio ]
         |
         v
[ Tiempo sincronizado ]
         |
         v
[ Kerberos valida credenciales ]
         |
         v
[ realmd ejecuta join ]
         |
         v
[ AD crea cuenta de equipo ]
         |
         v
[ sssd obtiene identidades ]
         |
         v
[ PAM habilita login ]
  
```
---

## Regla de oro

> El domain join no es un comando, es una cadena de dependencias.
> Un fallo temprano degrada silenciosamente todo lo que sigue.


