---
layout: post
title: "🏠 Cómo instalar Home Assistant OS en una Dell Wyse"
date: 2026-09-20
categories: [linux, home assistant, wyse, hardware, domotica]
tags: [home assistant, haos, wyse, dell, thin client, linux, domotica, uefi]
lang: es
ref: wyse-home-assistant
---

# 🏠 Cómo instalar Home Assistant OS en una Dell Wyse

> "Todo pirata necesita un puerto base. Un lugar chiquito, feo por fuera, que nunca se apaga y siempre sabe dónde está todo."

Hace unos meses [convertimos una Dell Wyse en un kiosco pirata con Porteus Kiosk]({% post_url 2026-07-02-como-convertir-una-dell-wyse-en-un-kiosco-pirata-con-porteus-kiosk %}).

En ese post mencioné a Home Assistant como caso de uso al menos tres veces.

Y siempre de la misma manera: como *la pantalla que muestra Home Assistant*.

Nunca como **el servidor que corre Home Assistant**.

Hoy arreglamos eso.

Porque si hay una máquina hecha para correr domótica es exactamente esta: sin ventilador, consumiendo diez o quince watts, enchufada en un rincón, encendida los 365 días del año.

Una Raspberry Pi cuesta más, tiene menos RAM y arranca desde una microSD que un día —siempre, inevitablemente, un domingo— se va a corromper.

La Wyse, en cambio, ya vino con disco de verdad.

---

# ☠️ Antes de empezar: la letra chica del UEFI

Acá viene el filtro que decide si este post te sirve o no.

Y conviene leerlo **antes** de descargar dos gigas de nada.

Home Assistant OS tiene un requisito no negociable:

> El sistema tiene que ser 64-bit y tiene que poder arrancar por **UEFI**.

BIOS legacy no está soportado. No es "anda pero con quilombo". Simplemente no arranca.

Y esto importa mucho, porque las Wyse se dividen bastante limpio en dos bandos:

**Las que sí:**

- Wyse 3040 (Atom x5)
- Wyse 5070 (Celeron / Pentium Silver)
- Cualquier Wyse con firmware UEFI en el setup

**Las que probablemente no:**

- Wyse 5010 / 7010 (AMD G-series, la clásica de Marketplace)
- Modelos anteriores con BIOS legacy únicamente

Antes de hacer cualquier cosa, entrá al setup de la Wyse (`F2`, a veces `Del`) y buscá algo tipo **Boot Mode**, **Boot List Option** o **UEFI Boot**.

Si podés elegir UEFI: seguí leyendo, este post es para vos.

Si sólo existe *Legacy*: no tires la máquina, saltá directo al [Plan B](#-plan-b-si-tu-wyse-es-legacy-bios) al final.

---

# 🦜 El otro detalle: el disco

El segundo punto donde la gente se choca la nariz.

En el post de Porteus usé un módulo SATA de **16 GB** y sobraba espacio, porque a Porteus no le importa nada: se carga en RAM y listo.

Home Assistant OS es otra historia.

HAOS es un sistema operativo completo con particiones duplicadas para actualizaciones A/B, más Docker, más los add-ons, más la base de datos que crece **todos los días** con cada sensor que registrás.

La imagen entra en 16 GB.

El problema no es instalarla. El problema es el mes número tres, cuando la base de datos ya se comió el disco y empezás a borrar historial para poder respirar.

Recomendación práctica, de la experiencia y no del manual:

- **32 GB** → mínimo razonable
- **64 GB o más** → lo que querés de verdad
- Un SSD de 2.5" barato → lo que probablemente termines poniendo

Los módulos SATA de las Wyse son reemplazables y hay 64 GB dando vueltas por dos mangos.

Un detalle más, porque es el tipo de cosa que te hace perder una tarde entera: el disco tiene que usar **sectores lógicos de 512 bytes** (512n o 512e). Los discos 4Kn nativos no bootean HAOS. Cualquier SSD normal de consumo está bien; esto aplica a discos empresariales raros.

---

# ⚓ Identificar tu Wyse

Igual que la vez pasada, la forma más honesta de saber qué compraste es mirar el procesador.

Si tenés cualquier Linux arrancado ahí:

```bash
lscpu
```

o

```bash
cat /proc/cpuinfo | grep "model name"
```

Un `AMD G-T56N` te está diciendo, con mucha educación, que vas directo al Plan B.

Un `Celeron J4105` o un `Atom x5-Z8350` significa que estás en carrera.

---

# 🏴 Descargar Home Assistant OS

Nada de crear cuentas esta vez. Después de la odisea de Porteus, esto se siente casi como una caricia.

La imagen vive en GitHub, a la vista de todos:

https://github.com/home-assistant/operating-system/releases

Buscá el release marcado como **Latest** y descargá el archivo que dice `generic-x86-64`.

Al momento de escribir esto, la versión estable es la **18.3**:

```bash
wget https://github.com/home-assistant/operating-system/releases/download/18.3/haos_generic-x86-64-18.3.img.xz
```

Son unos 500 MB comprimidos.

**No lo descomprimas todavía.** Después te explico por qué.

Antes de seguir, verificá que el archivo llegó entero. En la página del release está el `sha256`:

```bash
sha256sum haos_generic-x86-64-18.3.img.xz
```

Comparás los primeros y últimos caracteres con lo que dice GitHub y listo.

Dos segundos ahora te ahorran una hora de "por qué no bootea".

---

# 💣 Grabar la imagen

Acá el manual oficial y yo tomamos caminos distintos.

La documentación de Home Assistant recomienda usar la utilidad **Disks** de Ubuntu o **Balena Etcher**. Es la ruta segura, con interfaz gráfica y botones grandes.

Si nunca grabaste una imagen a un disco en tu vida, hacelo así. En serio. Bajá Etcher, apretá *Flash from file*, elegí el disco, listo.

Pero este es un blog pirata, y nosotros ya tenemos el módulo SATA colgando de un adaptador USB —el mismo que le robé al cadáver de un disco externo en el post anterior.

Así que vamos con la terminal.

## Identificar el disco

El truco de siempre. Antes de conectar nada:

```bash
lsblk
```

```text
NAME        SIZE
nvme0n1    512G
```

Conectás el disco de la Wyse. Mismo comando otra vez:

```bash
lsblk
```

```text
NAME        SIZE
nvme0n1    512G
sdb         64G
```

El que apareció recién es el nuestro.

**MUY IMPORTANTE**, y lo repito porque lo repetí la vez pasada y lo voy a repetir siempre:

Usamos la **unidad**, no la partición.

✔️ `/dev/sdb`

❌ `/dev/sdb1`

## Desmontar

Ubuntu monta todo lo que encuentra, con un entusiasmo que a veces molesta.

```bash
sudo umount /dev/sdb*
```

Si dice que no estaba montado, mejor todavía.

## Escribir

Y acá está la razón por la que no descomprimimos el `.xz` antes: no hace falta. Lo descomprimimos al vuelo y se lo damos de comer a `dd` por una tubería.

```bash
xzcat haos_generic-x86-64-18.3.img.xz | sudo dd of=/dev/sdb bs=4M status=progress
```

Donde:

- `xzcat` → descomprime la imagen sin crear un archivo intermedio de varios gigas.
- `of=` → el disco destino. **Miralo tres veces.**
- `bs=4M` → bloques grandes, copia mucho más rápido.
- `status=progress` → para que la terminal te cuente qué está pasando.

`dd` sigue teniendo el mismo talento de siempre: es rapidísimo y es perfectamente capaz de aniquilar el disco equivocado sin preguntar nada ni pedir perdón.

Revisá la letra. Después revisala de nuevo. Y recién entonces dale enter.

## Esperar de verdad

Cuando `dd` termina, todavía puede haber datos flotando en la caché.

```bash
sync
```

Cuando vuelve el prompt, ya está. Podés desconectar el disco.

---

# 🔧 Preparar la BIOS

Volvés a montar el módulo SATA dentro de la Wyse y entrás al setup (`F2`).

Tres cosas, ni una más:

1. **Boot Mode → UEFI**. Sin esto nada de lo anterior sirvió.
2. **Secure Boot → Disabled**. HAOS no está firmado para Secure Boot.
3. **Boot order** → el disco interno primero.

Y una cuarta, opcional pero muy recomendable si esto va a ser tu servidor de domótica: buscá en el setup algo tipo **AC Recovery**, **Restore on AC Power Loss** o **After Power Failure** y ponelo en **Power On**.

Así, cuando se corta la luz y vuelve, la Wyse arranca sola.

Un servidor de domótica que necesita que alguien vaya a apretarle el botón no es un servidor. Es una mascota.

Guardás, salís.

---

# 🚀 Primer arranque

Conectá:

- monitor (sólo para esta primera vez)
- **Ethernet con internet** — esto no es opcional
- corriente

La red cableada importa de verdad en el primer arranque: HAOS viene con el sistema operativo, pero **descarga Home Assistant Core desde internet** la primera vez que enciende.

Sin red, la pantalla se queda mirándote y no pasa nada.

Encendés.

Después de un minuto aparece un banner de bienvenida en la pantalla. Eso significa que el sistema base arrancó bien.

Y después... esperás.

La primera vez tarda. Varios minutos. Está bajando contenedores y armando todo desde cero.

Es un momento raro, con la pantalla casi inmóvil, donde uno empieza a dudar de todas las decisiones que tomó ese día.

Aguantá. Anda bien.

---

# 🧭 Entrar a Home Assistant

Desde cualquier compu de la misma red:

http://homeassistant.local:8123

Si tu router es de los que no resuelven `.local` —pasa seguido con los que da la empresa de internet— probá:

- http://homeassistant:8123
- `http://LA.IP.DE.LA.WYSE:8123`

Para averiguar la IP: entrá al router y buscá el cliente nuevo, o mirá la pantalla de la Wyse, que la muestra en el banner.

Una vez que carga, el onboarding te pide:

- usuario y contraseña de administrador
- nombre de la instalación
- ubicación y zona horaria
- si querés compartir estadísticas anónimas

Y ahí Home Assistant sale a buscar dispositivos en tu red por su cuenta.

La primera vez es un momento lindo. Aparecen cosas que ni sabías que estaban conectadas: el Chromecast, la impresora, la tele, los Shelly, algún foquito perdido.

Tu casa te acaba de mandar el inventario.

---

# 🩹 Si no bootea

Pasa. Sobre todo en firmwares medio testarudos, que no registran la entrada de arranque UEFI del disco nuevo.

La Wyse queda mirándote con cara de "no encuentro nada booteable" y uno jura que `dd` falló.

No falló. La imagen está ahí; lo que falta es que el firmware sepa que existe.

Arrancás un Linux live por USB (en modo UEFI) y le decís a mano:

```bash
sudo efibootmgr --create --disk /dev/sda --part 1 --label "HAOS" \
   --loader '\EFI\BOOT\bootx64.efi'
```

Reemplazando `/dev/sda` por el disco donde grabaste HAOS —ojo, acá el nombre es el que tiene **dentro de la Wyse**, que no es necesariamente el mismo que tenía colgado del USB en tu notebook.

Algunas BIOS también te dejan agregar la opción de arranque a mano, apuntando a:

```text
\EFI\BOOT\bootx64.efi
```

Reiniciás y ahora sí.

---

# ⚙️ Las tres cosas que hay que hacer ya

Antes de ponerte a jugar con automatizaciones y dashboards, hacé estas tres. Son quince minutos y te salvan de dolores futuros.

**1. IP fija.** Reservá la IP de la Wyse en el DHCP del router. Todo lo que integres después va a apuntar a esa dirección, y el día que el router decida cambiarla se rompe medio sistema.

**2. Backups automáticos.** En *Configuración → Sistema → Backups*. Programalos y mandalos afuera de la Wyse: un NAS, Google Drive vía add-on, lo que tengas. Un backup guardado en el mismo disco que se puede morir no es un backup, es una expresión de deseo.

**3. Sacá el monitor.** Ya no lo necesitás. La Wyse arranca sola, sin teclado ni pantalla. Enchufala en el rincón donde va a vivir y olvidate.

Y si querés aprovechar que ahora tenés un puerto USB libre: un dongle **Zigbee** (un Sonoff ZBDongle-E, por ejemplo) convierte esto en un hub de domótica completo, sin nube y sin depender de la app de nadie.

Pero eso ya es otro post.

---

# 🛠 Plan B: si tu Wyse es legacy BIOS

Si llegaste hasta acá y tu Wyse no tiene UEFI, no la tires.

Home Assistant OS está descartado, sí. Pero Home Assistant no.

El camino es: **Debian 12 mínimo + Home Assistant en Docker**.

Debian arranca sin problema en BIOS legacy, pesa poquísimo, y Home Assistant corre igual en un contenedor:

```yaml
services:
  homeassistant:
    image: ghcr.io/home-assistant/home-assistant:stable
    container_name: homeassistant
    volumes:
      - ./config:/config
      - /etc/localtime:/etc/localtime:ro
    restart: unless-stopped
    privileged: true
    network_mode: host
```

Lo que perdés es el Supervisor, y con él la tienda de add-ons con un clic.

Lo que ganás es que podés levantar Mosquitto, Zigbee2MQTT o Frigate al lado como contenedores propios, y que todo el sistema entra cómodo en 16 GB.

Cambiás comodidad por control. Que es, básicamente, la decisión que uno toma cada vez que abre una terminal.

---

# 🍺 Epílogo

Una thin client que alguna empresa jubiló por obsoleta ahora es el cerebro de una casa.

Sin ventilador. Sin nube. Sin suscripción. Sin que nadie del otro lado del mundo sepa a qué hora apagás las luces.

Quince watts haciendo el trabajo que la industria te quiere cobrar por mes.

Guybrush nunca compró un mapa completo: juntó tres pedazos sueltos y con eso encontró el tesoro.

Nosotros juntamos una Wyse de Marketplace, un SSD rescatado y una imagen de GitHub.

Windows, mientras tanto, sigue buscando actualizaciones.

---

*"Porque todo problema tecnológico puede resolverse con una terminal, un café y la cantidad justa de piratería."*
