---
layout: post
title: "🏴‍☠️ Cómo convertir una Dell Wyse en un kiosco pirata con Porteus Kiosk"
date: 2026-07-02
categories: [linux, porteus, wyse, hardware]
tags: [porteus kiosk, wyse, dell, thin client, linux, chrome]
lang: es
ref: wyse-porteus-kiosk
---

# 🏴‍☠️ Cómo convertir una Dell Wyse en un kiosco pirata con Porteus Kiosk

> "Hay barcos que nacieron para navegar... y hay thin clients que nacieron para jubilarse en un depósito. Nosotros elegimos otro destino."

Si alguna vez compraste una **Dell Wyse** por dos mangos en Marketplace, seguramente te pasó lo mismo que a mí.

La mirás.

La enchufás.

Pensás:

> *"Esto tiene que servir para algo más que juntar polvo."*

Y tenías razón.

Con **Porteus Kiosk** podés convertir esa cajita en un navegador prácticamente indestructible para cartelería, domótica, Home Assistant, paneles de control, recepción de empresas o cualquier pantalla que sólo necesite abrir un navegador y olvidarse del resto.

---

# ☠️ Antes de empezar

Primero una aclaración.

Las Wyse vienen en varias versiones (7010, 7020, etc.) y muchas veces ni Dell parece ponerse de acuerdo.

La forma más fácil de identificar la tuya es mirando el procesador:

```bash
lscpu
```

o

```bash
cat /proc/cpuinfo | grep "model name"
```

Con eso normalmente ya sabés qué modelo tenés realmente.

---

# 🏴 Descargar Porteus Kiosk

El sitio oficial es:

https://porteus-kiosk.org/download.html

Hasta hace unos años había un botón enorme que decía **Download ISO**.

Ahora no.

Bienvenido al mundo moderno.

Para descargar la versión Community tenés que crear una cuenta gratuita en el Customer Panel.

Una vez logueado, el sistema genera la ISO asociada a tu cuenta y recién ahí aparece el enlace de descarga.

Descargá la versión **Porteus Kiosk Standard**.

---

# ⚓ Preparando el disco

En mi caso reutilicé una de esas cosas que todos tenemos guardadas "por las dudas".

¿Viste esos discos externos USB que un día murieron?

Bueno... adentro tienen una pequeña placa SATA-USB.

Le saqué la placa a un disco que había pasado a mejor vida y terminé usando ese adaptador para conectar el módulo SATA de la Wyse a la notebook.

Reciclaje nivel Monkey Island.

Obviamente, si pertenecés al mundo capitalista, también existen adaptadores SATA-USB que se compran y funcionan exactamente igual.

---

# 🦜 Identificar el disco

Antes de conectar el disco ejecutamos:

```bash
lsblk
```

Por ejemplo:

```text
NAME        SIZE
nvme0n1    512G
```

Ahora conectamos el disco de la Wyse.

Y volvemos a ejecutar exactamente el mismo comando.

```bash
lsblk
```

Ahora aparece algo así:

```text
NAME        SIZE
nvme0n1    512G
sdb          16G
```

¡Perfecto!

Comparando ambas salidas queda clarísimo cuál apareció recién.

En mi caso fue:

```text
sdb
```

**MUY IMPORTANTE**

Vamos a usar únicamente la unidad.

No la partición.

O sea:

✔️ `/dev/sdb`

NO

❌ `/dev/sdb1`

Porque sino estaríamos escribiendo dentro de una partición y no sobre el disco completo.

---

# ⚓ Desmontar el disco

Ubuntu suele montarlo automáticamente.

Antes de grabar la imagen conviene desmontarlo.

```bash
sudo umount /dev/sdb*
```

(reemplazando la letra por la que corresponda).

---

# 💣 Grabar la imagen

Ahora viene el famoso `dd`.

Ese comando tiene una habilidad especial.

Es increíblemente rápido...

...e increíblemente capaz de destruir el disco equivocado.

Revisá dos veces la letra.

Después una tercera.

Y recién ahí ejecutalo.

```bash
sudo dd if=~/Descargas/Porteus-Kiosk-6.x.x-x86_64.iso of=/dev/sdb bs=4M status=progress
```

Donde:

- `if=` → archivo ISO descargado.
- `of=` → disco destino.
- `bs=4M` → copia mucho más rápido.
- `status=progress` → muestra el progreso en tiempo real.

Durante varios minutos la terminal parece poseída mientras copia bloques.

Es completamente normal.

---

# ⚓ Esperar que termine de verdad

Cuando `dd` finaliza todavía pueden quedar datos pendientes en la caché.

Para obligar al sistema a escribir absolutamente todo ejecutamos:

```bash
sync
```

Cuando vuelve el prompt...

Ya está.

Podés retirar el disco.

---

# 🚀 Primer arranque

Volvés a instalar el módulo SATA dentro de la Wyse.

Conectás:

- teclado
- monitor
- red (preferentemente Ethernet)

y encendés la máquina.

Después de unos segundos aparece el asistente de Porteus Kiosk.

Desde ahí configurás:

- idioma
- red
- URL inicial
- resolución
- comportamiento del navegador
- actualizaciones

En menos de diez minutos tenés un kiosco funcionando.

---

# ☠️ ¿Por qué Porteus?

Porque hace exactamente una cosa.

Y la hace muy bien.

No hay escritorio.

No hay usuarios.

No hay Office.

No hay "¿qué tocó el cliente ahora?".

Hay un Linux mínimo cuyo único trabajo es abrir un navegador y seguir funcionando durante años.

Y eso, para cartelería digital, Home Assistant, dashboards de Grafana, recepciones o sistemas industriales, vale oro.

---

# 🍺 Epílogo

Mientras algunos instalan veinte gigabytes de sistema operativo para abrir una página web...

...nosotros reciclamos una thin client de oficina con un disco rescatado de un cadáver electrónico y la dejamos lista para navegar durante años.

LeChuck estaría orgulloso.

Windows... bueno... seguramente todavía esté buscando actualizaciones.

---

*"Porque todo problema tecnológico puede resolverse con una terminal, un café y la cantidad justa de piratería."*
