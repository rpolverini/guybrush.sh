---
layout: post
title: "¡Mirá atrás tuyo! ¡Un mono de tres cabezas! (Y cómo renombrar tu rama de master a Main en menos de lo que canta un pollo de goma)"
date: 2026-01-16 12:00:00 -0300
categories: [git, devops, tutorial]
author: "Guybrush Threepwood"
---

¡Hola! Soy Guybrush Threepwood, y quiero ser un ingeniero de DevOps.

He luchado contra el pirata fantasma LeChuck, he aguantado la respiración bajo el agua por diez minutos (¡posta!) y he encontrado el tesoro del Big Whoop. Pero te juro que nada me dio tanto miedo como la primera vez que tuve que cambiar la rama `master` a `main` en un repositorio en producción. Pensé que todo iba a explotar como el Scumm Bar en una noche de pelea.

Pero resulta que, al igual que el duelo de insultos, el secreto no está en la fuerza bruta, sino en la técnica. ¡Y hoy te voy a enseñar cómo hacerlo sin que tu CI/CD te mande al infierno de los condenados!

## Paso 1: El Mapa del Tesoro (La interfaz de GitHub)

Lo primero es lo primero, che. No te hagas el héroe intentando hacerlo todo desde la consola oscura y húmeda todavía. Andá a la web de GitHub, que es más fácil que robarle el ídolo a la Gobernadora Marley mientras duerme.

1.  Entrá a tu repo y andá a la pestaña de **Settings** (Configuración).
2.  En el menú de la izquierda, buscá **General** (suele ser la primera opción, no tiene pérdida).
3.  Baja un cachito hasta donde dice **"Default branch"**.
4.  Vas a ver que está la vieja `master`. Dale clic al ícono del lápiz ✏️ y cambiale el nombre a `main`.

¡Zas! GitHub hace la magia vudú automáticamente: te redirecciona los enlaces viejos y te mueve las *Branch Protection Rules* al toque. ¡Magia, te digo! Ni la Lady Voodoo te hace un laburo tan limpio.

## Paso 2: El Duelo de Espadas (Tu terminal local)

Ahora sí, volvé a tu barco (tu laptop). Tu Git local va a estar más perdido que Stan intentando vender un ataúd usado en el medio del mar. Tenés que explicarle quién manda acá.

Abrí la terminal y tirale estos comandos, como si fueran estocadas:

```bash
# 1. Cambiale el nombre a tu rama local (Renombrá el barco)
git branch -m master main

# 2. Traete las novedades del viejo mundo (GitHub ya sabe que existe main)
git fetch origin

# 3. Conectá tu rama local con la remota (Alineá los cañones)
git branch -u origin/main main

# 4. Actualizá la referencia (¡Fuego!)
git remote set-head origin -a
```

¡Y listo el pollo (de goma con una polea en el medio)! Tu local ya está sincronizado y joya.

## Paso 3: Cuidado con las trampas de LeChuck (CI/CD)
¡Pará la mano, grumete! No cantes victoria todavía.

Si tenés algún pipeline de despliegue, un Dockerfile o scripts locos, es muy probable que todavía estén buscando a master. Y si no lo encuentran, tu deploy va a fallar más feo que mi primer intento de insultar a un pirata.

Revisá tus pergaminos (.github/workflows, bitbucket-pipelines.yml, o lo que uses) y asegurate de que digan:

```yaml
on:
  push:
    branches:
      - main  # <--- CAMBIÁ ESTO, BOLÓ
```

Si usas Vercel, Netlify o alguna de esas yerbas modernas, entrá a su panel y deciles que la rama de producción ahora es main. Si no lo hacés, vas a ser pasto de los caimanes.

¡Che, grumetes del teclado!

Espero que esto les sirva para que sus repos no queden más viejos que la campera de Stan. Ya vieron que cambiar de rama es una pavada si sabés el truco... es casi tan fácil como ganarle una pulseada al viejo de la tienda (bueno, capaz no tanto, ese tipo tenía fuerza).

Pero ojo, no se duerman en los laureles. Revisen esos pipelines, porque si llegan a romper producción un viernes a la tarde, les juro que LeChuck va a parecer un nene de pecho al lado de su Tech Lead enojado.

¡Nos vemos en el próximo post! Me voy rajando que Elaine me está esperando para tomar unos mates con Grog en el Scumm Bar... o capaz me manda a buscar el tesoro del Big Whoop otra vez, nunca se sabe con ella.

¡Ah, y nunca paguen más de 20 pesos por un videojuego de computadora! (Ni por un dominio .com).