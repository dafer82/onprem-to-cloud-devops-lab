# Time & DNS Configuration — Debian (SRV-LNX-01)

## Objetivo

Alinear el servidor Debian con los requisitos de Active Directory, garantizando:
- Sincronización horaria confiable contra el Domain Controller
- Resolución DNS autoritativa provista por AD
- Base correcta para Kerberos, autenticación y domain join

Esta configuración es obligatoria antes de unir el servidor al dominio.

---

## Contexto

- Entorno: laboratorio on-premise
- Servidor: Debian GNU/Linux (SRV-LNX-01)
- Domain Controller:
  - IP: `192.168.56.10`
  - Dominio: `corp.local`
- Gestor de red: `systemd-networkd`

En Active Directory, **tiempo y DNS son servicios de identidad**, no configuraciones opcionales.

---

## Principios de diseño

- El Domain Controller es la única fuente de tiempo
- El Domain Controller es el único servidor DNS
- No se utilizan servidores externos (Internet, NAT, router)
- La configuración debe ser persistente y declarativa

---

## Parte A — Sincronización de Tiempo

### Auditoría inicial

```bash
timedatectl
```

**Se verifica:**

- Estado de sincronización de reloj
- Servicio NTP activo o inactivo
- Zona horaria

### Decisión técnica

Se utiliza `systemd-timesyncd` por ser:
- Simple
- Integrado en Debian
- Suficiente para entornos AD

### Comfiguración de `systemd-timesyncd`

**Archivo**

```bash
/etc/systemd/timesyncd.conf
```

**Contenido**

```ini
[Time]
NTP=192.168.56.10
FallbackNTP=
```

**Notas:**
- El DC es la única fuente
- No se define fallback para evitar desviós de tiempo

### Activación del servicio

```bash
sudo systemctl enable systemd-timesyncd
sudo systemctl restart systemd-timesyncd
```

**Validación:**

```bash
timedatectl
```

**Resultado esperado:**
- `system clock synchronized: yes`
- `NTP service: active`

## Parte B — DNS alineado con Active Directory

### Principio clave

En entorno AD:
  
  **DNS = servicio de directorio**

Por lo tanto:

- No se utilizan DNS públicos
- No se utilza DNS automático de NAT

### Auditoría del estado DNS

```bash
resolvectl status
```
**Se valida:**

- DNS efectivo por interfaz
- Dominio de búsqueda

### Configuración DNS vía `systemd-networkd`

**Archivo:**

```bash
/etc/systemd/network/20-hostonly.network
```

**Configuración:**

```ini
[Match]
Name=enp0s8

[Network]
Address=192.168.56.20/24
DNS=192.168.56.10
Domains=corp.local
```
Claves:

- El DNS queda ligado a la NIC interna
- `Domains=` habilita resolución de servicios AD

## Aplicación de la configuración

```bash
sudo systemctl restart systemd-networkd
```
---

## Validaciones finales

### DNS efectivo

```bash
resolvectl status
```

**Debe mostrar**

- DNS Server: `192.168.56.10`
- Domain: `corp.local`

### Resolución de dominio

```bash
nslookup corp.local
nslookup srv-ad-01.corp.local
```

### Chequeo combiando

```bash
timedatectl
resolvectl status
```
Si ambos son correctos, el sistema está listo para el join al dominio.

---

## Estado final

- Tiempo sincronizado contra el DC
- DNS autoritativo de Active Directory
- Configuración persistente y declarativa
- Sistema preparado para Kerberos y autenticación
