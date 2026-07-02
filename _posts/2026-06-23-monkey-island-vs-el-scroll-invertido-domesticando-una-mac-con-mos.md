---
layout: post
title: "Monkey Island vs el Scroll Invertido: domesticando una Mac con Mos"
date: 2026-06-23
categories: [macos, productividad, herramientas]
tags: [mac, mos, smooth-scrolling, mouse, productividad, cambio-laboral]
lang: es
ref: monkey-island-mos-scroll
---

# Monkey Island vs el Scroll Invertido: domesticando una Mac con Mos

Hay momentos en la vida donde uno debe tomar decisiones difíciles.

Abandonar una startup.
Cambiar de trabajo.
Dejar Windows.

Bueno, tampoco exageremos.

La cuestión es que en esta nueva misión laboral apareció una condición innegociable:

> "Te damos una Mac."
Y ahí estaba yo.

Una nave espacial de aluminio cepillado.
Procesador que parece alimentarse de plutonio.
Batería que dura más que algunos gobiernos latinoamericanos.

Pero con un pequeño problema.

**El scroll estaba poseído.**

---

## El primer encuentro con la maldición

Los piratas veteranos de Monkey Island saben reconocer una maldición cuando la ven.

Abrí Chrome.
Moví la ruedita del mouse hacia abajo.

La página subió.

Volví a moverla.

La página siguió subiendo.

Pensé que estaba cansado.

Probé otra vez.

No.

La Mac estaba convencida de que mi cerebro funcionaba al revés.

---

## La explicación oficial de Apple

Apple llama a esto:

> Natural Scrolling
Que es una forma elegante de decir:

> "Vamos a asumir que todo el mundo usa trackpad."
Y ahí está el problema.

Cuando hacés scroll con los dedos sobre un trackpad, el movimiento tiene cierta lógica:

- Empujás el contenido hacia arriba.
- El contenido sube.
- Ves más abajo.
Perfecto.

Pero cuando conectás un mouse como una persona civilizada que pasó décadas usando computadoras...

...esa lógica se convierte en una especie de ritual vudú.

Movés la rueda hacia abajo.

La pantalla sube.

Tu cerebro entra en conflicto existencial.

---

## El problema verdadero

La locura no termina ahí.

macOS tiene una única configuración para ambos dispositivos:

- Trackpad
- Mouse
Si desactivás el scroll invertido:

- El mouse queda bien.
- El trackpad queda raro.
Si lo activás:

- El trackpad queda bien.
- El mouse queda raro.
Es como si en Monkey Island hubieran decidido que la misma llave abre todas las puertas del Caribe.

---

## La solución: Mos

Después de recorrer tabernas, cuevas secretas y repositorios de GitHub apareció el objeto mágico.

### Mos

Mos es una utilidad gratuita para macOS que hace dos cosas extremadamente importantes:

1. Permite configurar el scroll de un mouse por separado.
2. Agrega smooth scrolling real.
En otras palabras:

- Trackpad feliz.
- Mouse feliz.
- Usuario feliz.
Algo que Apple aparentemente consideró opcional.

---

## Instalación

Entramos al sitio oficial:

### Sitio oficial
👉 [https://mos.caldis.me](https://mos.caldis.me/)

O directamente al repositorio:

👉 [https://github.com/Caldis/Mos](https://github.com/Caldis/Mos)

Descargamos la última versión y la instalamos normalmente.

La primera vez macOS probablemente intente protegerte de vos mismo.

Aparecerá algún mensaje dramático tipo:

> "Esta aplicación fue descargada de Internet."
Aceptamos.

Respiramos.

Seguimos adelante.

---

## Permisos necesarios

Una vez abierto Mos, probablemente solicite permisos de accesibilidad.

Ir a:

```
System Settings
→ Privacy & Security
→ Accessibility
```

Y habilitar:

```
Mos
```

Sin eso no podrá interceptar correctamente los eventos del mouse.

---

## Configuración recomendada

Una vez instalado aparece un ícono en la barra superior.

Abrimos Preferences.

Como en la captura:

[![Preferencias de Mos](https://github.com/assets/img/posts/mos-preferences.png)](https://github.com/assets/img/posts/mos-preferences.png)

### General
Activar:

```
☑ Launch on Login
```

Así no tenemos que acordarnos nunca más.

---

### Scrolling
Acá está la verdadera magia.

Configurar:

```
☑ Smooth Scrolling
☑ Reverse Scrolling (solo para mouse si así lo preferís)
```

La idea es recuperar décadas de memoria muscular sin arruinar la experiencia del trackpad.

---

### Ajustando la sensación

Cada mouse es distinto.

Yo recomiendo comenzar con:

```
Step: Medio
Duration: Medio
Acceleration: Baja
```

Y después ajustar según gusto.

El objetivo no es convertir el scroll en una montaña rusa.

El objetivo es que deje de sentirse como una carretilla sobre adoquines.

---

## La diferencia después de instalarlo

Antes:

- Scroll raro.
- Saltos bruscos.
- Sensación extraña en Chrome.
- Sensación extraña en VSCode.
- Sensación extraña en todo.
Después:

- Scroll consistente.
- Movimiento suave.
- Mouse funcionando como cualquier persona esperaría.
- Cerebro dejando de insultar a Apple cada cinco minutos.

---

## ¿Vale la pena?

Absolutamente.

Si venís de Linux o Windows y usás mouse físico todos los días, Mos probablemente sea una de las primeras aplicaciones que deberías instalar.

La Mac seguirá teniendo algunas costumbres peculiares.

Pero al menos dejará de discutir con tu memoria muscular cada vez que intentás bajar una página.

Y en el Caribe digital, como aprendimos hace años, evitar peleas innecesarias siempre deja más tiempo para buscar tesoros.

---

## Kit de supervivencia del pirata recién llegado a macOS

Mi lista inicial después de desembarcar en la isla:

- Mos → arreglar el scroll.
- Rectangle → manejar ventanas como una persona normal.
- Raycast → reemplazar Spotlight.
- iTerm2 → terminal seria.
- Stats → monitoreo liviano del sistema.
Pero esa es una historia para otro viaje.

---

> "Nunca confíes en un sistema operativo que te diga que mover una rueda hacia abajo significa subir."
> 
> — Guybrush Threepwood, probablemente.
