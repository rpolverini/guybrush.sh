---
layout: post
title: "LibreWind - Etapa 0: El laboratorio y el nacimiento del LibreWind S"
description: "Comienza LibreWind, un instrumento de viento libre, económico y resistente pensado para que los chicos puedan acercarse a la música. En esta etapa medimos materiales, elegimos la boquilla y construimos el primer prototipo experimental."
date: 2026-08-20
categories: [librewind, musica, lutheria]
tags: [librewind, caña-libre, musica, lutheria, diy, open-source, instrumentos, ppr, saxofon, viento]
lang: es
ref: librewind-s-stage-0
---

# LibreWind

## Codename: Caña Libre

> **La música también merece ser libre.**
>
> Porque no solo de ingeniería vive el hombre.

A veces hay que dejar un rato el teclado, agarrar un calibre, un pedazo de caño de termofusión y una boquilla de saxofón, y preguntarse qué pasaría si intentáramos fabricar un instrumento musical que un chico pudiera usar sin que el precio del instrumento fuera la primera barrera.

Y si, de paso, conseguimos que ese instrumento sobreviva a un chico de seis años...

Bueno.

Eso ya sería prácticamente tecnología militar.

# ¿Qué estamos construyendo?

LibreWind es un proyecto abierto para diseñar y documentar un instrumento de viento económico, resistente y reproducible.

La idea inicial es bastante concreta:

- caño de termofusión PP-R;
- herramientas comunes;
- una boquilla real;
- cañas reales;
- afinación en Do;
- digitación sencilla;
- dimensiones apropiadas para chicos de aproximadamente 5 a 7 años;
- y toda la documentación disponible para que cualquiera pueda construirlo, estudiarlo y mejorarlo.

No queremos copiar un saxofón.

Tampoco queremos fabricar simplemente una flauta de PVC que "más o menos suena".

Queremos **diseñar un instrumento**.

Y vamos a documentar el proceso como corresponde: incluyendo las mediciones, los cálculos, los errores, las pruebas y las decisiones que no funcionaron.

# ¿Por qué termofusión?

Porque el PP-R tiene una característica que para este proyecto nos viene bárbaro:

**es duro de matar.**

Está diseñado para instalaciones de agua, no para que un desarrollador con delirios de luthier lo use para fabricar instrumentos musicales.

Y justamente por eso nos gusta.

Un instrumento para chicos tiene que soportar:

- golpes;
- caídas;
- transporte;
- humedad;
- manos poco cuidadosas;
- y, eventualmente, ser usado como espada.

Además, el material se consigue fácilmente y se puede trabajar con herramientas comunes.

La filosofía de LibreWind es:

> **Si para construirlo necesitás un laboratorio de la NASA, no es LibreWind.**

# Etapa 0 - antes de hacer agujeros

La primera regla del proyecto fue sencilla:

## No agujerear.

Primero medir.

El nombre comercial del caño no nos dice suficiente para diseñar acústicamente.

"Caño de media" puede servir para comprarlo en una ferretería.

Para nosotros necesitamos saber:

- diámetro exterior;
- diámetro interior;
- espesor;
- longitud;
- geometría real;
- y cómo interactúa con la boquilla.

## Nuestro pequeño laboratorio

![Mesa de trabajo del proyecto LibreWind](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-taller-inicio-proyecto.jpg)

*El laboratorio de Caña Libre. Herramientas, caños, boquillas y una cantidad saludable de optimismo.*

Acá tenemos nuestro instrumental básico:

- calibre;
- taladro;
- piedra de banco;
- mechas;
- regla;
- herramientas manuales;
- caño PP-R;
- boquillas;
- y un afinador en el celular.

Nada demasiado exótico.

# El caño

El material que elegimos para los primeros experimentos es un caño de termofusión PP-R.

La primera medición nos dio aproximadamente:

| Parámetro | Primera medición | Medición corregida |
| --- | ---: | ---: |
| Diámetro exterior | 20,5 mm | **20,5 mm** |
| Diámetro interior | 13,3 mm | **14,0 mm** |
| Espesor calculado | 3,6 mm | **3,25 mm** |

Y acá aparece una de las primeras lecciones del proyecto.

### Medir dos veces no es una frase hecha.

La primera medición del diámetro interior nos dio 13,3 mm. Al repetirla con más cuidado descubrimos que había pequeñas partículas o residuos en el borde del caño que interferían con el calibre.

Además, durante las pruebas de encastre observamos que una espiga de aproximadamente 14 mm podía entrar en el caño **sin una resistencia exagerada, aunque había que empujarla**.

Eso nos hizo sospechar del primer dato.

Volvimos a medir correctamente y obtuvimos aproximadamente:

**diámetro interior = 14,0 mm**

El espesor estimado pasa entonces a ser:

```text
e = (diámetro exterior - diámetro interior) / 2

e = (20,5 - 14,0) / 2

e = 3,25 mm
```

No vamos a esconder la primera medición.

La dejamos documentada porque forma parte del experimento.

> **En LibreWind los errores también se publican.**
>
> Si un dato estaba mal medido, se corrige y se explica por qué.

# Las herramientas también se miden

Otra de las primeras cosas que aprendimos es que decir "tengo una mecha de 6,5 mm" no necesariamente significa que tengamos exactamente 6,500 mm.

Así que también vamos a medir las herramientas que utilizamos.

![Medición de una mecha con calibre](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-medicion-mecha-6-5-mm.jpg)

*La mecha que dice 6,5 mm también tiene que rendir examen.*

![Juego de mechas y herramientas](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-mechas-taladro-herramientas.jpg)

*Nuestro arsenal inicial de perforación.*

Para los futuros agujeros vamos a priorizar medidas de mecha que realmente puedan conseguirse en Argentina.

La estrategia será comenzar con diámetros pequeños y **agrandar durante la afinación**, nunca al revés.

Porque:

> ### Una mecha entra una sola vez.
>
> ### El agujero no vuelve.

# La primera gran decisión: ¿alto o soprano?

Inicialmente teníamos dos posibilidades.

Una boquilla genérica de saxofón alto y una Yamaha de soprano.

Después de medirlas y observar cómo se relacionaban con nuestro caño, decidimos que el **primer LibreWind experimental será el modelo S, basado en la boquilla de soprano**.

La Yamaha está en buenas condiciones y utiliza una caña negra Plasticover.

![Boquilla Yamaha de soprano](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-boquilla-yamaha-soprano.jpg)

*La protagonista del primer prototipo.*

La boquilla de alto queda reservada para una segunda línea experimental.

# ¿Por qué empezamos por soprano?

Hay varias razones.

La primera es ergonómica.

Un instrumento basado en una boquilla de soprano permite investigar una geometría potencialmente más compacta, algo especialmente interesante si el objetivo final son chicos de 5 a 7 años.

La segunda es que el diámetro de entrada de la boquilla soprano se aproxima mucho al diámetro interior que tenemos en nuestro caño.

La tercera es experimental:

**queremos saber primero qué puede hacer este sistema acústico concreto.**

No queremos diseñar sobre supuestos.

Queremos medir.

# El adaptador

Y acá apareció nuestra primera pequeña pieza de ingeniería.

En lugar de inventar un receptor desde cero, utilizamos una unión tipo espiga para manguera de media.

La espiga entra aproximadamente:

- **2 cm en la boquilla**;
- **2 cm en el caño**.

Para ajustarla al interior de la boquilla tuvimos que reducir aproximadamente 1 mm de diámetro.

Como no tenemos torno, la primera adaptación se hizo manualmente con la piedra de banco.

![Adaptación de la espiga para la boquilla](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-adaptador-espiga-boquilla-soprano.jpg)

*Primer adaptador experimental. No es todavía la solución definitiva: es una herramienta para aprender.*

Y acá aparece una pequeña aclaración semántica que, para Guybrush, es técnicamente importante.

La espiga **no "se usa"**.

La espiga **se empuja**.

Hay que hacer cierta fuerza para introducirla en el caño y en la boquilla. No entra suelta ni queda bailando, pero tampoco queremos considerar este ajuste como definitivo hasta comprobar:

- estanqueidad;
- resistencia mecánica;
- posibilidad de desmontaje;
- y, sobre todo, que no dañe la boquilla.

Por ahora es un **prototipo experimental**.

La solución definitiva probablemente sea un adaptador específicamente diseñado para LibreWind.

# El primer experimento acústico

Y acá empieza realmente LibreWind.

Todavía:

- no hay agujeros;
- no hay digitación;
- no hay campana;
- no hay sistema de octavación.

Tenemos solamente:

```text
boquilla Yamaha soprano
        +
adaptador
        +
caño PP-R
```

El objetivo es encontrar la longitud que haga que el tubo produzca un **Do de concierto** con todos los futuros orificios todavía inexistentes.

Nuestro objetivo es:

**Do4 ≈ 261,63 Hz**

# Primera prueba: 45 cm

Cortamos un primer tramo largo, aproximadamente de **45 cm**.

La idea era deliberadamente conservadora:

> un caño largo se puede seguir cortando; uno corto no se puede volver a alargar.

No hicimos ningún agujero.

Con la boquilla soprano, el adaptador y el cuerpo de prueba obtuvimos:

### **165 Hz**

Eso corresponde prácticamente a:

### **Mi3**

![Primer prototipo LibreWind S de 45 cm](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-s-primer-prototipo-45cm.jpg)

*Primer LibreWind S experimental, todavía sin agujeros.*

Este es nuestro primer dato acústico real.

# Segunda prueba: 30 cm

Después cortamos el cuerpo a aproximadamente **30 cm**.

Volvimos a colocar exactamente el mismo sistema de boquilla y adaptador.

Resultado:

### **233 Hz**

El afinador indicó aproximadamente:

### **La♯3 / Si♭3**

Tenemos entonces dos puntos experimentales:

| Largo del cuerpo | Frecuencia medida | Nota aproximada |
| ---: | ---: | --- |
| 45 cm | **165 Hz** | Mi3 |
| 30 cm | **233 Hz** | La♯3 / Si♭3 |
| - | **261,63 Hz** | **Do4 - objetivo** |

Y esto es muchísimo más interesante que una tabla teórica encontrada en Internet.

Ahora tenemos **nuestro propio instrumento como laboratorio**.

# ¿Cómo podemos calcular el próximo corte?

Acá aparece la física.

Para un tubo cilíndrico ideal cerrado en un extremo y abierto en el otro, la frecuencia fundamental se relaciona aproximadamente con la longitud acústica mediante:

$$
f \propto \frac{1}{L}
$$

Es decir: **cuanto más corto el tubo, más aguda la nota.**

Pero nuestro instrumento no es un tubo ideal.

Tenemos:

- boquilla;
- caña;
- cámara de la boquilla;
- espiga;
- transición entre diámetros;
- correcciones acústicas;
- y un tubo de dimensiones reales.

Por eso, en lugar de imponer una fórmula teórica perfecta, vamos a construir una aproximación a partir de **nuestras propias mediciones**.

## Dos puntos ya permiten construir una primera curva

Tenemos:

```text
45 cm → 165 Hz
30 cm → 233 Hz
```

Observemos primero algo interesante.

El largo disminuyó:

$$
\frac{45-30}{45}=33,3\%
$$

Pero la frecuencia aumentó:

$$
\frac{233-165}{165}=41,2\%
$$

Por lo tanto:

> **la frecuencia no cambia linealmente con el largo.**

No podemos decir simplemente:

> "cada centímetro suma 4,5 Hz".

Eso sería solamente el promedio de este intervalo y nos llevaría a errores cuando nos acerquemos al Do.

# Una aproximación matemática sencilla

Podemos representar nuestros datos experimentales mediante una función de potencia:

$$
f=aL^{-b}
$$

donde:

- $f$ es la frecuencia;
- $L$ es el largo;
- $a$ y $b$ son constantes obtenidas a partir de nuestras mediciones.

Con los dos puntos que tenemos:

```text
L = 45 cm → f = 165 Hz
L = 30 cm → f = 233 Hz
```

obtenemos aproximadamente:

$$
b \approx 0,851
$$

y la curva experimental queda aproximadamente:

$$
f \approx 4212,5L^{-0,851}
$$

con $L$ expresado en centímetros.

No hace falta memorizar esta fórmula.

La idea importante es otra:

> **tenemos un modelo matemático construido a partir del comportamiento real de nuestro instrumento.**

# ¿Y dónde estaría nuestro Do?

Buscamos:

$$
f=261,63\,\text{Hz}
$$

y resolvemos la ecuación anterior.

El resultado es aproximadamente:

# **L ≈ 26,2 cm**

Ese es nuestro **primer candidato matemático**.

Pero atención:

> **26,2 cm no es todavía la medida definitiva del LibreWind.**
>
> Es una predicción.

La vamos a comprobar con el instrumento real.

# ¿Cuánto cambia la nota cuando cortamos?

Acá aparece una regla práctica muy útil.

Entre nuestros dos primeros experimentos:

**45 → 30 cm**

produjo:

**165 → 233 Hz**

Pero cerca del Do la sensibilidad aumenta.

Según nuestro modelo, alrededor de los 26 cm, sacar aproximadamente **1 cm** puede representar varios Hz de diferencia.

Por eso:

### Lejos de la nota objetivo

podemos cortar centímetros.

### Cerca del objetivo

cortamos milímetros.

Y cuando estemos realmente cerca:

> **medimos, cortamos muy poquito, volvemos a medir.**

Nunca:

> "creo que le falta un cachito".

Porque "un cachito" es una unidad de medida que todavía no figura en el Sistema Internacional.

# Una observación importante

La física nos da una predicción.

El afinador nos dará la realidad.

Eso significa que el próximo episodio no empieza diciendo:

> "Hay que cortar exactamente 26,2 cm."

Empieza diciendo:

> **"Nuestro modelo predice aproximadamente 26,2 cm. Ahora vamos a comprobar cuánto se equivocó."**

Y eso es justamente lo que queremos aprender durante todo LibreWind.

# Estado del proyecto

## Etapa 0 - El laboratorio

- Nombre: **LibreWind**
- Codename: **Caña Libre**
- Material seleccionado: PP-R de termofusión
- Diámetro exterior medido: **≈20,5 mm**
- Diámetro interior corregido: **≈14,0 mm**
- Error de medición documentado
- Herramientas identificadas
- Mechas disponibles
- Boquilla Yamaha soprano seleccionada
- Caña Plasticover negra
- Adaptador experimental construido
- Primer cuerpo de prueba
- 45 cm → **165 Hz**
- 30 cm → **233 Hz**
- Primer modelo matemático
- Predicción inicial para Do4 → **≈26,2 cm**
- Comprobar experimentalmente el Do4
- Diseñar agujeros
- Diseñar digitación infantil
- Conseguir una octava
- Investigar segundo registro

# Próximo episodio

## LibreWind S - Encontrando el Do

Partimos de:

**30 cm → 233 Hz**

Nuestro modelo predice:

**≈26,2 cm → Do4**

Pero no vamos a cortar directamente hasta ahí.

Vamos a acercarnos progresivamente y dejar que el afinador nos diga dónde está realmente nuestro Do.

Todavía:

**cero agujeros.**

Primero necesitamos encontrar el corazón acústico del instrumento.

Después vendrá la parte verdaderamente peligrosa:

**hacerle agujeros.**

Y ahí sí vamos a necesitar toda la matemática, la acústica y la ergonomía que podamos juntar.

Porque:

> **una mecha entra una sola vez.**
>
> **El agujero no vuelve.**

Y si Guybrush se encuentra frente a una espiga que se resiste...

> **EMPUJAR.**
>
> Nunca "usar".
>
> Nunca.

🏴‍☠️🎷
