# AD Time & DNS — Troubleshooting Preventivo

## Objetivo
Detectar y prevenir errores de tiempo y DNS antes del domain join

---

## 🧠 Modelo mental (Regla de oro)

**En Active Directory:**
- ❌ "Tengo red" ≠ "puedo unirme al dominio"
- ❌ "Resuelve google.com" ≠ "DNS correcto" 
- ❌ "La hora parece bien" ≠ "Kerberos funciona"

AD es sensible, no tolerante.

---

## 1️⃣ Problema de TIEMPO


### Escenario Ⅰ — Hora correcta pero no sincronizada

**Síntoma:**

```bash
timedatectl

```
**Muestra:**
- Hora correcta visualmente
- ❌ `System clock synchronized: no`

**Por qué pasa:**
- La VM tomó hora del host
- El reloj quedó "bien de casualidad"
- Pero no hay NTP activo

**Impacto en AD**
- kerberos exige sincronización real
- El join puede fallar **de forma intermitente**

**Detección preventiva**

```bash
timedatectl

```
**🚨Red flag:**
- Hora correcta
- `NTP service: inactive`


### Escenario Ⅱ — Sincroniza, pero contra Internet

**Síntoma**

```bash
timedatectl

```
**Muestra:**
- `NTP service: active`  
- Hora sincronizada
- ❌ Pero no con el DC

**Por qué pasa:**
- `FallbackNTP=`activo
- DHCP/NAT inyecta NTP público
- systemd-timesyncd elige otro servidor

**Impacto**
- Drift progresivo entre DC y cliente
- Login AD falla horas después
- Error difícil de reproducir

**Detección preventiva**

```bash
timedatectl timesync-status

```
🔎 **Server** → debe ser `192.168.56.10`


### Regla preventiva de tiempo
✔️ El DC Manda el tiempo
✔️ Sin fallback
✔️ Sin Internet

---

## 2️⃣ Problemas de DNS

### Escenario Ⅲ — "Tengo DNS" pero no es AD

**Síntoma**

```bash
resolvectl status

```
**Muestra:**
- DNS `8.8.8.8`
- o DNS del NAT
- o DNS del router

**Por qué pasa:**
- DHCP de NAT
- Configuración incompleta `.network`
- `/etc/resolv.conf` sobrescrito

**Impacto**
- `nslookup corp.local` puede fallar
- SRV records no resuelven
- Join falla con errores genéricos

**Detección preventiva**

```bash
resolvectl status

```
**🚨Red flag:**
- DNS ≠ IP del DC 


### Escenario Ⅳ — DNS correcto, pero dominio no configurado

**Síntoma**

```bash
nslookup srv-ad-01

```
Falla, pero:
```bash
nslookup srv-ad-01.corp.local

```
**Funciona**

Por qué pasa
- Falta `Domains=corp.local`
- No hay search domain

**Impacto**
- Herramientas AD fallan
- realmd se comporta errático

**Detección preventiva**

```bash
resolvectl status

```
🔎 **Mirar:** 
- `Search Domains` o `Domain`

### Escenario Ⅴ — DNS correcto, pero en la NIC incorrecta

**Síntoma**
- DNS aparece asociado a `enp0s3 (NAT)`
- No a `enp0s8 (Host-Only)`

**Por qué pasa**
- DNS definido global
- Configuración mal atada a interfaz

**Impacto**
- Resolución inconsistente
- Cambia según ruta
- Difícil de debuggear

**Detección preventiva**

```bash
resolvectl status

```
🔎 **Mirar:** 
- DNS **por interfaz**
- No solo global


### Escenario Ⅵ — Todo parece bien, pero /etc/resolv.conf fue modificado 

#### /etc/resolv.conf es regenerado inesperadamente

**Síntoma:**
- A veces resuelve
- A veces no
- Después de reboot cambia

**Por qué pasa**
- DHCP sigue inyectando DNS
- `UseDNS=no` no está definido (mal escrito o ausente)
- `systemd-networkd` ignora la directiva inválida
- `/etc/resolv.conf` regenerado automáticamente

**Impacto**
- No depende de que sea symlink o archivo
- Depende de **quién lo controla**

**Detección preventiva**

```bash
ls -l /etc/resolv.conf
cat /etc/resolv.conf

```
✔️ Correcto si:
- El contenido apunta solo al DC
- No cambia tras reboot
- No contiene DNS de NAT

❌ Incorrecto si:
- Aparece DNS externo
- Cambia tras reiniciar
- DHCP sigue influyendo


#### /etc/resolv.conf sobrescrito por DHCP

**Síntoma:**
- nslookup cambia de servidor
- despues de reboot falla

**Causa:**
- resolv.conf editado a mano
- DHCP/NAT lo regenera

#### Caso real observado — Directiva DHCP mal definida

**Configuración problemática:**

```ini
[DHCP]
UserDNS=no

```
**Problema:**
- `UserDNS` no es una directiva válida 
- `systemd-networkd` la ignora silenciosamente
- El DNS del DHCP (NAT) se aplica igual

**Síntomas observados:**
- `/etc/resolv.conf` apunta a DNS de NAT  
- `nslookup corp.local` falla
- `resolvectl` no refleja DNS del DC 
- `El error reaparece tras reboot

**Corrección:**

```ini
[DHCP]
UseDNS=no


```
**Lección aprendida:**
- systemd-networkd no valida directivas desconocidas
- Un typo puede romper AD sin generar errores visibles

   > ⚠️ En system-networkd, una directiva mal escrita **no falla**, simplemente **no existe**. Lo cual lo hace difícil de detectar.

   > “Nota: tras corregir la directiva, es necesario reiniciar systemd-networkd para limpiar el estado DHCP previamente adquirido.”


### Regla preventiva de DNS
✔️ DNS = DC
✔️ Configurado por gestor
✔️ No editar resolv.conf

---

## 3️⃣ Problemas combinados

### Escenario Ⅶ — DNS OK + Tiempo MAL = Kerberos roto

**Síntoma**
- DNS resuelve perfecto
- Ping al DC OK
- Join falla igual

**Error típico**
- `Clock skew too great`
- `KDC unreachable`

**Detección preventiva**

```bash
timedatectl
resolvectl status

```
👉 Siempre validar ambos juntos

---

## ✅Checklist preventivo antes del join
- timedatectl
- resolvectl status
- nslookup corp.local

---

## ✅ 1. Conclusión técnica 


### Problema real 

> El problema **no era la red**, ni el DC, ni Active Directory.
> El problema fue un **DNS no persistente** causado por un modelo incompleto de **gestión de resolución de nombre** en Debian.


### Causa raíz confirmada

- `systemd-networkd` estaba correctamente configurado
- **No se utilizaba** `systemd-resolved` 
- `/etc/resolv.conf` quedaba:
   - regenerado por DHCP (NAT)
   - sobrescrito tras reboot
> Una configuración DHCP inválida fue ignorada silenciosamente por systemd-networkd, permitiendo que el DNS entregado por DHCP sobrescribiera la configuración esperada.

👉 Resultado:
DNS  de AD funcionaba **a veces**, pero **no era determinista ni persistente**.

---

## ✅ 2. Resolución aplicada 

**Modelo elegido (y documentado)**

### 🅰️ Modelo A — DNS manual sin systemd-resolved


**Características finales:**

- ❌ No se usa `systemd-resolved`
- ✅ `systemd-networkd` es el único gestor de red
- ✅ DHCP no inyecta DNS
- ✅ `/etc/resolv.conf` es la fuente final
- ✅ Persistencia garantizada


**Configuración final relevante**

NAT (enp0s3)

```ini
[Network]
DHCP=yes

[DHCP]
UseDNS=no

```

Host-Only (enp0s8)

```ini
[Network]
Address=192.168.56.20/24
DNS=192.168.56.10
Domains=corp.local

```

/etc/resolv.conf

```text
search corp.local
nameserver 192.168.56.10

```

(Opcional pero recomendado en lab)

```bash
sudo chattr +i /etc/resolv.conf

```
---

✅ 3. Validaciones finales 
Todos estos puntos deben cumplirse simultáneamente:


**DNS**

```bash
nslookup corp.local

```

✔️ Resuelve
✔️ Usa `192.168.56.10`

```bash
cat /etc/resolv.conf

```

✔️ Solo DNS del DC
✔️ No DNS de NAT
✔️ No cambia tras reboot


**Red**

```bash
ip route

```

✔️ Default route por NAT
✔️ Red `192.168.56.0/24` por Host-Only


**Tiempo**

```bash
timedatectl

```

✔️ `System clock synchronized: yes`
✔️ `NTP service: active`


**Identidad**

```bash
hostname -f

```

✔️ `srv-lnx-01.corp.local`

---

## ✅ Checklist de cierre del Paso 3

- DNS resuelve dominio AD

- DNS es persistente tras reboot

- DHCP no inyecta DNS

- Tiempo sincronizado

- Hostname correcto

- Modelo documentado

- Causa raíz identificada

---

> **Lección aprendida** 
>
> El error no fue técnico, fue de modelo mental.
> No alcanza con **que funcione**: en AD tiene **que ser determinista, explícito y persistente**. 

---

> Nota: Este troubleshooting debe ejecutarse siempre antes de intentar un domain join.
