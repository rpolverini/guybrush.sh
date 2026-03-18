---
layout: post
title: "OpenClaw y la maldición del perfil messaging"
date: 2026-03-18
tags: [openclaw, devops, virtualbox, sandbox, monkey-island-vibes]
---

Hay días en los que uno se levanta con ganas de conquistar el Caribe digital… y otros en los que decide instalar OpenClaw v2026.3.2 en una máquina shieldiada dentro de VirtualBox. Yo, claramente, elegí el segundo. Error táctico digno de un pirata que intenta abrir una puerta empujando donde dice “tirar”.

Todo empezó justo cuando la gobernadora Marley —una especie de Elaine pero con RFCs bajo el brazo— decidió poner reglas claras en la isla. “Orden, seguridad, sandboxing”, decía. Y yo, ingenuo como Guybrush entrando al Scumm Bar por primera vez, pensé: _“qué bueno, esto va a andar bárbaro”_.

Spoiler: no.

## 🏴‍☠️ El perfil maldito: “messaging”

Instalo OpenClaw, lo levanto… y el bicho se comporta como un loro amaestrado:

- habla ✔️  
- responde ✔️  
- ejecuta algo útil ❌  

Ahí es donde descubrís la trampa más elegante desde el insulto con espadas: el perfil `messaging`.

Este modo es básicamente como contratar a un mercenario que solo te da consejos motivacionales. Está diseñado para “seguridad por defecto”, lo cual en criollo significa:

> “No te dejo romper nada… pero tampoco te dejo hacer nada.”

El agente no puede:
- ejecutar comandos  
- tocar archivos  
- ni siquiera mirar de reojo tu sistema  

Es como tener a LeChuck encerrado en una botella… pero sin magia. Solo inútil.

## 🧱 El sandbox: la isla dentro de la isla

Cuando pensás que ya entendiste el problema, aparece el segundo jefe del nivel: el sandbox.

OpenClaw decide que cada sesión viva dentro de su propio mini-mundo:
- Docker  
- OpenShell  
- filesystem fantasma  

Vos le decís:
> “che, andá a `/home/ramiro/proyecto`”

Y el agente te mira como Guybrush cuando le hablás de física cuántica:
> “No conozco ese lugar, capo.”

Porque claro, está encerrado en su islita virtual. Sin mounts, sin binds, sin nada. Un Robinson Crusoe pero sin Friday.

## ⚙️ El ritual para devolverle el alma

Después de putear un rato (fase obligatoria), encontrás los conjuros correctos:

```bash
openclaw config set tools.profile full
openclaw gateway restart
```

Ahí, recién ahí, el bicho empieza a comportarse como un verdadero pirata del código.

Pero no termina ahí. Porque OpenClaw es como un barco viejo: cada vela hay que desplegarla a mano:

```bash
openclaw config set tools.allow_shell true
openclaw config set tools.allow_filesystem true
openclaw config set tools.allow_network true
openclaw config set tools.allow_exec true
```

Y finalmente, le marcás el territorio:

```bash
openclaw config set agents.defaults.workspace "~/.openclaw/workspace"
```

Ahora sí. Ahora el agente deja de ser un NPC decorativo y pasa a ser tripulación.

## 🪟 Bonus track: el fantasma de Windows

En todo este proceso, hay algo que me quedó claro:  
si esto lo intentás en Windows… directamente estás jugando en modo pesadilla.

No lo digo yo, lo dice la experiencia:
es como querer navegar el Caribe con un bote inflable pinchado.

## 🧠 Conclusión: el verdadero enemigo no era LeChuck

Uno piensa que el problema es la virtualización, o Docker, o permisos…  
pero no.

El verdadero villano era ese pequeño detalle escondido en la config:  
`tools.profile = messaging`

Una línea. Un switch. Una maldición.

Y así fue como, intentando instalar OpenClaw en una VM más blindada que tesoro de pirata, terminé aprendiendo que a veces el problema no es la falta de poder…

…sino que alguien decidió, muy amablemente, que no lo tengas.

---

Y recordá: en el mundo del software, como en Monkey Island, nunca confíes en algo que funciona “por defecto”… salvo que quieras terminar peleando con una gallina de goma con polea en el medio.