---
layout: post
title: "OpenRouter: una API para gobernarlos a todos"
date: 2026-09-05
tags: [openrouter, ai, api, llm, experimentation]
lang: es
ref: openrouter-api
---

Hay fines de semana en los que uno quiere programar algo tranquilo.

Sin tocar el entorno de producción.
Sin gastar los tokens de la empresa.
Y, preferentemente, sin tener que sacar una tarjeta de crédito para cada experimento que se nos ocurre a las 2 de la mañana.

Ahí aparece **OpenRouter**.

## ¿Qué es OpenRouter?

OpenRouter es, básicamente, una API que nos permite acceder a modelos de inteligencia artificial de distintos proveedores desde un mismo lugar.

En vez de tener:

- una API para OpenAI,
- otra para Anthropic,
- otra para Google,
- otra para DeepSeek,
- otra para vaya uno a saber qué modelo apareció esta semana...

tenemos un único punto de entrada.

Y podemos elegir qué modelo queremos utilizar.

Es particularmente interesante para experimentar porque cambiar de modelo no implica reescribir toda nuestra aplicación.

## ¿Y qué tiene de especial?

Que podemos probar modelos muy diferentes utilizando una interfaz bastante parecida.

Por ejemplo, podemos tener un proyecto que hoy usa un modelo de Google y mañana queremos probar uno de DeepSeek, Qwen o algún otro modelo.

En lugar de cambiar toda nuestra integración, cambiamos el modelo.

Es una pequeña diferencia que, cuando empezamos a experimentar con varios LLM, se vuelve bastante grande.

## ¿Es gratis?

Acá viene la parte que nos interesa a los pobres.

**Sí, hay modelos disponibles gratuitamente.**

**Pero gratis no significa barra libre de inteligencia artificial hasta que LeChuck resucite por quinta vez.**

Los modelos gratuitos tienen límites de uso y disponibilidad bastante más acotados que los modelos pagos.

Para probar cosas, hacer pequeños experimentos, jugar con un CLI o desarrollar durante un fin de semana, pueden ser más que suficientes.

Cuando necesitamos más volumen, mejores límites o queremos utilizar modelos que no tienen una variante gratuita, entra en juego el saldo pago.

La gracia es que podemos cargar crédito y utilizar los modelos que cobran por uso, sin tener que mantener una cuenta y una integración independiente para cada proveedor.

## ¿Cómo arrancamos?

Primero vamos a [OpenRouter](https://openrouter.ai/) y creamos una cuenta.

Una vez dentro, vamos a la sección de **API Keys** y generamos una nueva clave.

Algo importante: tratemos esa API key como lo que es.

**Una contraseña.**

No la subimos a GitHub, no la pegamos en un README y mucho menos la ponemos en un `print()` para después olvidarnos de borrarla.

La idea es guardarla como variable de entorno:

```bash
export OPENROUTER_API_KEY="..."
```

Y listo.

Ya tenemos una credencial que podemos utilizar desde nuestras herramientas.

## ¿Y ahora qué?

Acá es donde se pone interesante.

La API de OpenRouter se puede utilizar desde nuestras propias aplicaciones, por ejemplo desde **Python**, pero también podemos integrarla con herramientas que soporten proveedores compatibles.

Por ejemplo, si estamos jugando con un agente de código como **OpenCode**, podemos configurar el acceso a través de OpenRouter y empezar a probar diferentes modelos.

Eso nos permite hacer algo bastante divertido:

**probar modelos sin casarnos con uno.**

Y para un proyecto experimental esto es oro.

Hoy probamos un modelo gratuito.

Mañana encontramos uno que razona mejor.

Pasado mañana aparece otro que cuesta la mitad.

Cambiamos el modelo y seguimos trabajando.

## Entonces, ¿para qué me sirve?

Para mí, la principal ventaja de OpenRouter no es simplemente "tener muchos modelos".

Es tener **una puerta de entrada común para experimentar con ellos**.

Si estás aprendiendo sobre IA, desarrollando un proyecto personal o simplemente querés probar qué modelo se lleva mejor con tu código, te ahorra bastante fricción.

Y además tiene una ventaja fundamental para un sábado a la tarde:

**no necesitás convertir tu tarjeta de crédito en infraestructura de desarrollo.**

Porque bastante caro sale ya el café.

---

_Guybrush Threepwood, futuro pirata del Caribe y actual usuario de APIs de inteligencia artificial._

> "Nunca pagues por tres APIs cuando podés pelearte con una sola."
