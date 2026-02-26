---
layout: post
title: "El día que Windows intentó reescribir toda la historia del repo"
date: 2026-02-25
categories: [git, devops, aventuras]
---


Hay enemigos peligrosos en el desarrollo de software.

- Los bugs intermitentes.
- Los deploys de viernes.
- Y después… está Windows con sus saltos de línea.

Hoy vamos a hablar de uno de los clásicos combates navales del desarrollo multiplataforma:

> “¿Por qué Git dice que TODO el archivo fue modificado si nadie tocó nada?”

Spoiler: no fue magia negra. Fue `CRLF`.

---

## 🏝️ La batalla invisible: LF vs CRLF

En sistemas civilizados (Linux, macOS), el salto de línea es:

```text
LF → \n
```

Minimalista. Elegante. Como un buen script en Python.

En Windows, en cambio:

```text
CRLF → \r\n
```

Dos caracteres. Porque uno solo claramente no era suficiente.

Entonces, ¿qué pasa?

1. Vos en Linux guardás archivos con `LF`.
2. Tu compa en Windows abre el archivo.
3. Windows dice: “Esto necesita más drama” y lo convierte a `CRLF`.
4. Hace commit.
5. Git ve que TODAS las líneas cambiaron.
6. El diff parece una escena de incendio en Monkey Island.

Y ahí estás vos, mirando 400 líneas “modificadas” donde nadie cambió nada.

---

## 🧠 Por qué en otros proyectos no pasa

Porque esos proyectos ya tienen la bendición ancestral:

`.gitattributes`

Ese archivo es básicamente el gobernador colonial que le dice a Git:

> “Acá se guarda todo en LF. Orden y disciplina.”

---

## 🛡️ La solución como verdaderos piratas senior

### 1️⃣ Crear `.gitattributes` en la raíz del repo

```bash
touch .gitattributes
```

Y dentro:

```text
* text=auto
```

¿Qué hace esto?

- Normaliza todo a `LF` cuando se hace commit.
- Permite que Windows use `CRLF` localmente si quiere.
- Pero en el repo todo queda prolijo y consistente.

Es como decir:

> “En tu casa hacé lo que quieras, pero en el barco mando yo.”

---

### 2️⃣ Configuración recomendada por sistema

Tu compa en Windows:

```bash
git config --global core.autocrlf true
```

Vos en Linux:

```bash
git config --global core.autocrlf input
```

Traducción pirata:

- Windows convierte al bajar.
- Linux no toca nada al bajar.
- Ambos convierten a `LF` al subir.

Paz mundial.

---

## 🧹 ¿Y si ya explotó todo?

Si ya hay un commit donde “se modificó todo”:

1. Asegúrense de no tener cambios pendientes (`git status` limpio).
2. Ejecuten:

```bash
git add --renormalize .
```

3. Commit correctivo:

```bash
git commit -m "Normalizando saltos de linea a LF"
```

Y listo. El repo deja de parecer que fue saqueado por corsarios incompetentes.

---

## 🧱 Versión más sólida (para equipos que ya no usan rueditas)

Si querés algo todavía más explícito y profesional:

```text
* text=auto eol=lf
```

Y para evitar que Git toque binarios:

```text
*.png binary
*.jpg binary
*.jpeg binary
*.gif binary
*.pdf binary
```

Porque lo último que queremos es que Git intente “normalizar” una imagen como si fuera un poema.

---

## ⚔️ Lección final

Los conflictos entre sistemas operativos no son técnicos.

Son culturales.

Linux cree en la simplicidad.  
Windows cree en la sobreingeniería emocional.

Pero Git, como buen capitán, necesita reglas claras.

Si no tenés `.gitattributes`, no tenés civilización.

---

La próxima vez que veas un diff donde cambió todo y no cambió nada…

No culpes a tu compañero.  
Culpá al retorno de carro.

---

Y recordá:

> Un verdadero pirata no le teme a los merges…  
> le teme a los saltos de línea mal configurados.
