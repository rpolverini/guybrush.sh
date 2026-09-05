---
layout: post
title: "Mi stack de IA para programar el finde sin quemar la API de la empresa"
date: 2026-09-05
categories: [ia, desarrollo, cli, herramientas]
tags: [opencode, gemini, siliconflow, openrouter, ai-coding, cli]
lang: es
ref: ai-weekend-stack
---

Hay una situación que probablemente conozcas.

Es viernes por la noche.

Tenés una idea.

Es una idea hermosa.

Probablemente no te vaya a hacer ganar plata.

Probablemente tampoco sea especialmente necesaria.

Pero **tenés que hacerla**.

El problema aparece cuando abrís tu agente de coding favorito y recordás que la API que tenés configurada es la de la empresa.

Y ahí aparece esa pequeña voz interior:

> "Che... ¿esto lo estamos pagando nosotros?"

No.

**Este proyecto es mío.**

Y además... ya es sábado.

Así que la API de la empresa queda cerrada :D... naaa, pero no corresponde usarla para esto... así que empieza la búsqueda de una herramienta para jugar con IA sin tener que hipotecar el sueldo.

# El problema: quiero un Claude Code, pero para mis boludeces

Durante bastante tiempo tuve una solución que me gustaba mucho:

**trybons.ai.**

La idea era fantástica: una CLI que hacía de intermediario y permitía probar distintos modelos de IA para programación.

Era exactamente el tipo de herramienta que uno quiere para un proyecto personal:

```text
terminal
   │
   ▼
   Bonsai
   │
   ├── modelo A
   ├── modelo B
   ├── modelo C
   └── modelo D
```

El problema de estas cosas es que hay una diferencia importante entre:

**"esto funciona gratis"**

y
**"esto funciona gratis mientras alguien siga pagando la fiesta".**

Y aparentemente la fiesta de Bonsai se terminó... o alguien se olvidó de regar el arbolito.

¯\*(ツ)\*/¯

Así que tocaba buscar reemplazo.

# Lo primero que descubrí: el truco no es encontrar "la IA gratis"

Mi primera tentación fue buscar:

> "¿Cuál es la mejor API gratis para programar?"

Pero creo que esa es la pregunta equivocada.

La pregunta interesante es:

> **¿Cuál es la mejor CLI que me permita cambiar de proveedor sin cambiar mi forma de trabajar?**

Y ahí aparece **OpenCode**.

OpenCode es una herramienta de coding agent para terminal que no intenta obligarte a utilizar un único proveedor.

Actualmente soporta una enorme cantidad de providers y también permite utilizar modelos locales.

La arquitectura empieza a ponerse bastante linda:

```text
                    ┌── OpenRouter
                    │
                    ├── SiliconFlow
                    │
OpenCode ───────────┼── Groq
                    │
                    ├── Gemini
                    │
                    ├── OpenAI
                    │
                    ├── Anthropic
                    │
                    └── Ollama
```

Y eso cambia completamente el problema.

Ya no estoy buscando **"mi nueva IA"**.

Estoy buscando **un buen router para mi terminal**.

# OpenCode: la pieza que me faltaba

La gracia de OpenCode es que la herramienta y el modelo dejan de ser la misma cosa.

Yo puedo tener:

```text
opencode
```

como interfaz de trabajo y después decidir dónde quiero mandar las consultas.

Por ejemplo:

```text
OpenCode
   │
   └── OpenRouter
          │
          ├── modelo gratis
          ├── otro modelo gratis
          └── modelo pago
```

O:

```text
OpenCode
   │
   └── SiliconFlow
          │
          ├── Qwen
          ├── GLM
          ├── Kimi
          └── otros modelos
```

O incluso:

```text
OpenCode
   │
   └── Ollama
          │
          └── modelo local
```

Y esto último tiene una propiedad bastante interesante:

**costo marginal de API = $0.**

Porque el servidor sos vos.

Bueno...

También tenés que pagar la electricidad.

Pero si la IA empieza a cobrarte por los watts, ya tenemos otro problema.

# ¿Y SiliconFlow?

Acá viene una pequeña aclaración respecto de varias listas que circulan por internet.

SiliconFlow aparece frecuentemente recomendado como proveedor gratuito de modelos chinos, especialmente DeepSeek, Qwen, GLM, etc.

Pero **no asumiría que SiliconFlow es actualmente una API gratis ilimitada**.

Su catálogo actual es enorme, con más de 200 modelos, y ofrece una API compatible con proveedores habituales, pero su esquema comercial actual es principalmente pay-as-you-go.

Por ejemplo, hoy aparecen modelos como:

- GLM
- Kimi
- Gemma
- GPT-OSS
- distintos modelos de razonamiento
- modelos especializados en coding

Y algunos son realmente interesantes para programación.

Incluso Kimi K3 está disponible en SiliconFlow, con una ventana de contexto de alrededor de 1 millón de tokens.

Pero si mi objetivo es:

> **"No quiero gastar un peso este sábado"**

entonces no voy a construir toda mi estrategia alrededor de un proveedor que cobra por token.

Lo voy a considerar **una pieza intercambiable**.

Y esa diferencia es importante.

# Entonces... ¿qué usaría gratis?

Acá aparece otra opción bastante interesante:

## Gemini CLI

Google tiene su propia CLI open source para utilizar Gemini desde la terminal.

Y su free tier actual es bastante generoso:

```text
60 requests / minuto
1000 requests / día
```

con una cuenta personal de Google.

Eso para un proyecto personal de fin de semana es una barbaridad.

Especialmente porque muchas veces no necesitás:

```text
THE ULTIMATE FRONTIER MODEL
```

para hacer:

```text
"Che, haceme un endpoint para esto"
```

A veces un modelo razonablemente bueno alcanza perfectamente.

# Mi stack del sábado

Entonces, si tuviera que armar hoy mi setup para proyectos personales, lo pensaría así:

```text
                  MI PROYECTO
                       │
                       ▼
                    OpenCode
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
      Gemini CLI   OpenRouter   SiliconFlow
          │            │            │
          ▼            ▼            ▼
       Gemini       modelos       modelos
                    gratuitos     abiertos
```

Y si el proyecto crece:

```text
                  OpenCode
                     │
       ┌─────────────┼─────────────┐
       │             │             │
     Gratis        Barato        Local
       │             │             │
    Gemini       SiliconFlow    Ollama
    OpenRouter    OpenRouter
```

La herramienta permanece.

El backend cambia.

**Ese es el verdadero superpoder.**

# ¿Y OpenRouter?

OpenRouter es otra pieza muy interesante para este esquema porque funciona como una capa unificada delante de muchos modelos.

OpenCode lo soporta directamente y permite seleccionar modelos desde la propia interfaz.

El concepto es exactamente el que buscaba después de Bonsai:

```text
OpenCode
    │
    ▼
OpenRouter
    │
    ├── modelo gratis
    ├── modelo barato
    ├── modelo experimental
    └── "hoy quiero probar este"
```

Eso me permite cambiar de modelo sin convertir mi `~/.bashrc` en un cementerio arqueológico de API keys.

# ¿Y Claude gratis?

Acá también hay que tener cuidado con las listas que encontramos dando vueltas por internet.

Por ejemplo, algunas recomendaciones todavía hablan de DuckDuckGo como si ofreciera:

```text
Claude 3.5 Haiku
Llama 3.3
GPT-4o mini
o3-mini
```

Pero eso ya quedó viejo.

La oferta gratuita actual de Duck.ai incluye, entre otros:

```text
Claude 4.5 Haiku
Mistral Small 4
GPT-5.4 nano
GPT-5.4 mini
gpt-oss-120b
Gemma 4 31B
```

según la propia documentación de DuckDuckGo.

Moraleja:

**no copies una lista de modelos de un artículo de seis meses atrás y asumas que sigue vigente.**

En IA eso es aproximadamente equivalente a usar documentación de Django 1.8 para configurar Django 5.

# La estrategia que me gusta

Después de mirar todas estas opciones, creo que la solución no es:

> "Encontré EL proveedor gratuito."

Porque probablemente en tres meses ese proveedor cambie las condiciones.

La estrategia más resistente es:

```text
             ┌─────────────────┐
             │     OpenCode     │
             └────────┬────────┘
                      │
          ┌───────────┼───────────┐
          │           │           │
          ▼           ▼           ▼
       Gemini     OpenRouter   SiliconFlow
          │           │           │
          └───────────┼───────────┘
                      │
                      ▼
                    Ollama
                  (si pinta)
```

Es decir:

**la CLI es estable; los modelos son descartables.**

Y eso me parece mucho más sano.

# Pero hay una cosa que no hay que olvidar

Que algo sea gratuito no significa que sea privado.

Si estás trabajando con:

```text
API keys
credenciales
datos de clientes
código propietario
secretos
variables de entorno
```

no deberías asumir que una API gratuita es equivalente a ejecutar un modelo local.

Para mi proyecto personal del sábado:

```text
proyecto propio
código público
experimentos
ideas
pruebas
```

perfecto.

Para el repositorio de la empresa:

```text
NO.
```

Y especialmente:

```bash
export OPENAI_API_KEY=...
```

no es una invitación para pegar el `.env` completo en un prompt.

# ¿Entonces qué instalaría?

Para arrancar rápido:

```bash
npm install -g opencode-ai
```

Después:

```bash
opencode
```

Y desde ahí configuraría el proveedor que quiera utilizar.

OpenCode permite conectarse a los providers mediante `/connect` y seleccionar modelos con `/models`.

Por ejemplo, conceptualmente:

```text
/connect
```

elegís:

```text
OpenRouter
```

ponés la API key y después:

```text
/models
```

Y elegís el modelo.

La belleza está en que mañana puedo hacer:

```text
/connect
```

y cambiar de proveedor.

No tengo que cambiar de herramienta.

# Mi setup ideal para este finde

Si lo tuviera que resumir en una receta:

### 🥇 Para arrancar gratis

**Gemini CLI**

Porque tiene un free tier muy generoso y está pensado directamente para trabajar desde terminal.

### 🥈 Para experimentar con muchos modelos

**OpenCode + OpenRouter**

Porque desacopla la herramienta del proveedor y permite saltar entre modelos.

### 🥉 Para tener acceso a muchos modelos asiáticos/open source

**OpenCode + SiliconFlow**

Especialmente interesante si querés experimentar con Kimi, GLM, Gemma, GPT-OSS y compañía.

### 🏠 Para independencia total

**OpenCode + Ollama**

Porque ahí directamente dejás de depender de una API externa.

# Y ahora viene lo divertido

Hay algo que me gusta especialmente de este enfoque.

Supongamos que hoy estoy trabajando en:

```text
santaclara.ar
```

y quiero hacer:

```bash
$ opencode
```

y empezar a programar.

No me interesa demasiado si detrás está:

```text
Gemini
Qwen
Kimi
GLM
DeepSeek
GPT
Claude
```

Mi workflow sigue siendo:

```text
terminal
   ↓
agente
   ↓
repositorio
   ↓
git
   ↓
commit
```

El modelo se convirtió en **infraestructura intercambiable**.

Y eso, para mí, es bastante más interesante que encontrar "el mejor modelo".

# La conclusión de Guybrush

Creo que el error que cometí inicialmente fue buscar un reemplazo para Bonsai.

No necesito otro Bonsai.

Necesito algo mejor:

**una herramienta que haga que Bonsai sea reemplazable.**

Y ahí OpenCode tiene muchísimo sentido.

No porque sea mágicamente el mejor agente del planeta.

Sino porque me permite decir:

> "Hoy tengo ganas de usar este modelo."

Y mañana:

> "Este modelo se quedó sin cuota. Probemos otro."

Y pasado:

> "Ya fue. Instalemos Ollama."

Sin cambiar todo mi workflow.

```text
                ┌─────────────┐
                │   OpenCode  │
                └──────┬──────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
     Gemini        OpenRouter    SiliconFlow
        │              │              │
        └──────────────┼──────────────┘
                       │
                    Ollama
                       │
                       ▼
                 "Es mi máquina."
```

Y así es como termino un sábado programando un proyecto que probablemente no necesitaba hacer...

**sin gastar la API de la empresa.**

Porque una cosa es hacer software innecesario.

Y otra muy distinta es hacerlo **pagando con la tarjeta de otro**.

---

_Guybrush Threepwood, programador, músico y ocasionalmente culpable de abrir otro proyecto un sábado a las 11 de la mañana._

> "Si funciona, no lo toques.
>
> Si es gratis, tampoco preguntes demasiado.
>
> Y si es sábado... hacé el proyecto."
