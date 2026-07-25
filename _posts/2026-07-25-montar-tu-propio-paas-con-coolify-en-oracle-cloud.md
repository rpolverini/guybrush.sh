---
layout: post
title: "Montar tu propio PaaS con Coolify en Oracle Cloud (Always Free)"
date: 2026-07-25
tags: [oracle-cloud, coolify, paas, docker, devops, infraestructura, monkey-island-vibes]
lang: es
ref: coolify-oracle-paas
---

# 🏴‍☠️ Prólogo — Antes de zarpar

> *"No construimos este barco para ganar una guerra naval. Lo construimos para aprender a navegar."*
> — Guybrush Threepwood (probablemente)

Si alguna vez intentaste desplegar una aplicación en la nube, seguramente terminaste pasando por alguno de estos servicios:

- Render
- Railway
- Fly.io
- DigitalOcean
- Hetzner
- AWS
- Azure

Todos funcionan muy bien... hasta que llega la primera factura.

Oracle Cloud tiene una particularidad bastante interesante: su programa **Always Free** ofrece recursos que, bien aprovechados, alcanzan para montar un laboratorio personal sorprendentemente potente.

En este tutorial vamos a utilizar especialmente la instancia **Ampere A1 ARM**, que para desarrollo y autoalojamiento suele ofrecer una relación rendimiento/consumo excelente. Muchos proyectos modernos (Docker, PostgreSQL, Redis, Nginx, Traefik, Python, Node.js, Go, Java, etc.) funcionan perfectamente sobre ARM y aprovechan muy bien esos recursos.

Para los casos en que todavía existan imágenes Docker exclusivas para x86, agregaremos una pequeña máquina AMD como *worker* secundario. Así obtenemos lo mejor de ambos mundos.

La idea no es solamente instalar Coolify.

La idea es entender cómo Oracle organiza su infraestructura:

- Compartimentos.
- Redes virtuales.
- Reglas de firewall.
- Instancias.
- Almacenamiento.
- Claves SSH.
- Seguridad.
- Arquitecturas ARM y x86.

Una vez que comprendés esas piezas, desplegar cualquier otro servicio resulta muchísimo más sencillo.

---

# ☠️ Aviso de Seguridad (muy importante)

Este artículo está pensado como un **laboratorio de aprendizaje**.

El objetivo es tener un despliegue funcionando en pocos minutos para conocer la plataforma y entender cómo interactúan todos los componentes.

Por ese motivo vamos a dejar expuestos temporalmente algunos servicios hacia Internet, por ejemplo:

- Puerto **22** (SSH).
- Puerto **8000** (panel de administración inicial de Coolify).

Eso **no representa una configuración recomendada para producción**.

Desde una perspectiva de ciberseguridad, cualquier servicio publicado innecesariamente aumenta la superficie de ataque. En otras palabras...

> Estamos dejando la puerta del barco abierta mientras aprendemos dónde están los cañones.

En un entorno real sería recomendable, entre otras medidas:

- Limitar el acceso SSH a direcciones IP conocidas.
- Deshabilitar el acceso por contraseña y utilizar únicamente claves SSH.
- Cerrar el puerto 8000 una vez finalizada la instalación inicial de Coolify.
- Publicar únicamente HTTP/HTTPS detrás de Traefik.
- Aplicar el principio de **mínimo privilegio**, exponiendo solamente los servicios estrictamente necesarios.
- Mantener el sistema operativo actualizado.
- Implementar monitoreo, copias de seguridad y registros de auditoría.
- Considerar el uso de listas de seguridad, Network Security Groups (NSG), VPN o un *bastion host* para el acceso administrativo.

A lo largo del blog iremos endureciendo ("hardening") esta instalación paso a paso.

Este no es el destino final del barco.

Es apenas el primer puerto donde vamos a aprender a navegar.

---

# 🍌 Filosofía Guybrush

En esta serie no buscamos la infraestructura perfecta desde el primer día.

Buscamos entender qué hace cada pieza.

Primero hacemos que el barco flote.

Después aprendemos a navegar.

Y recién al final... nos preocupamos por los piratas.

---

# 🏴‍☠️ Misión: Montar tu propio PaaS con Coolify en Oracle Cloud (Always Free)

> *"Mientras otros pagan 20 dólares por mes por un VPS... nosotros saqueamos la nube de Oracle usando el Always Free. Bienvenido a Monkey Island."*

Después de pelearme varias veces con Oracle Cloud, encontré una configuración que quedó realmente sólida para correr **Coolify** sin gastar un solo doblón.

La idea es aprovechar al máximo el **Always Free Tier**, usando la poderosa máquina ARM de Ampere como servidor principal y, si algún proyecto necesita arquitectura x86, sumar una segunda VM AMD como worker.

Al finalizar este tutorial vas a tener un PaaS propio, con HTTPS automático, despliegues desde Git y espacio suficiente para alojar varios proyectos personales.

---

# ⚓ Distribución de recursos

**Región**

📍 Chile Central (Santiago) — `sa-santiago-1`

## Servidor Principal

| Recurso | Valor | Arquitectura |
|---------|-------|--------------|
| ARM Ampere A1 | — | — |
| CPU | 2 OCPU | ARM |
| RAM | 12 GB | ARM |
| Disco | 100 GB | ARM |

Será el servidor donde vive Coolify.

---

## Worker opcional

| Recurso | Valor | Arquitectura |
|---------|-------|--------------|
| AMD x86 | — | — |
| CPU | 1/8 OCPU | x86 |
| RAM | 1 GB | x86 |
| Disco | 50 GB | x86 |

Ideal para contenedores que todavía no poseen imágenes ARM.

---

## Espacio restante

Todavía quedan aproximadamente **50 GB** libres de los **200 GB** que Oracle regala.

Nada mal para ser gratis.

---

# 🏝️ Paso 1 — Crear el Compartimento

Entramos a Oracle Cloud y verificamos que la **Home Region** sea:

> **Chile (Santiago)**

Luego vamos a:

```
Identity & Security
    └── Compartments
```

Creamos uno nuevo.

Nombre:

```
prod-coolify-compartment
```

Parent:

```
Root Tenancy
```

Este compartimento nos permitirá tener todo el laboratorio ordenado.

---

# 🌐 Paso 2 — Crear la red

Entramos en:

```
Networking
    └── Virtual Cloud Networks
```

Seleccionamos nuestro compartimento.

Elegimos:

```
Start VCN Wizard
```

Luego:

```
VCN with Internet Connectivity
```

Nombre:

```
coolify-vcn
```

Cuando termine el asistente, entramos a:

```
Security Lists
```

y abrimos la **Default Security List**.

---

## Abrir los puertos

Agregar estas reglas **Ingress**.

| Puerto | Uso |
|--------|-----|
| 22 | SSH |
| 80 | HTTP + Let's Encrypt |
| 443 | HTTPS |
| 8000 | Panel inicial de Coolify |

**Importante**

Oracle tiene un pequeño detalle que hace caer a mucha gente.

En las reglas:

**NO completar "Source Port Range".**

Debe quedar completamente vacío.

El puerto va únicamente en:

```
Destination Port Range
```

---

# 💻 Paso 3 — Crear la VM ARM

Vamos a:

```
Compute
    └── Instances
        └── Create Instance
```

Nombre:

```
coolify-master-arm
```

Compartimento:

```
prod-coolify-compartment
```

---

## Elegir Ubuntu

Entramos en:

```
Change Image
```

Seleccionamos:

```
Canonical Ubuntu
```

Y elegimos explícitamente una versión LTS.

Recomendadas:

- Ubuntu 24.04 LTS
- Ubuntu 22.04 LTS

Arquitectura:

```
aarch64
```

---

## Elegir la máquina correcta

Entramos en:

```
Change Shape
```

Seleccionamos:

```
VM.Standard.A1.Flex
```

Acá Oracle esconde un pequeño secreto.

Hay que desplegar el panel de configuración avanzada (la flechita casi invisible).

Allí configuramos manualmente:

```
2 OCPU
12 GB RAM
```

Verificar que siga apareciendo:

```
Always Free Eligible
```

---

## Agregar varias claves SSH

Elegimos:

```
Paste Public Keys
```

Pegamos una debajo de la otra.

Ejemplo:

```
MacBook
Ubuntu Desktop
Notebook
```

Mientras sean claves públicas (`*.pub`) Oracle las acepta todas.

---

## Ampliar el disco

En la sección **Boot Volume** expandimos las opciones avanzadas.

Marcamos:

```
Specify custom boot volume size
```

y escribimos:

```
100 GB
```

Si no hacemos esto Oracle crea un disco bastante más chico y después ampliarlo es mucho más incómodo.

Finalmente:

```
Create
```

---

# 🔥 Paso 4 — El firewall escondido de Ubuntu

Este fue el bug que más tiempo me hizo perder.

Aunque abramos los puertos en Oracle...

...Ubuntu todavía puede bloquearlos.

Conectate por SSH.

```bash
ssh ubuntu@IP_PUBLICA
```

Ejecutá:

```bash
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT

sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 443 -j ACCEPT

sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 8000 -j ACCEPT
```

Y luego guardamos las reglas.

```bash
sudo netfilter-persistent save
```

> **Nota del capitán:** en algunas imágenes modernas de Ubuntu puede ser necesario instalar primero `iptables-persistent` (`sudo apt install -y iptables-persistent netfilter-persistent`) si el comando de guardado no existe.

---

# ⚙️ Paso 5 — Instalar Coolify

Conectados por SSH simplemente ejecutamos:

```bash
curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash
```

Tomate un café.

El instalador hace prácticamente todo solo.

Cuando termine, abrimos:

```
http://IP_PUBLICA:8000
```

Creamos el usuario administrador...

...y listo.

Ya tenemos nuestro propio Heroku, Render o Railway privado.

Y gratis.

---

# 🖥️ Paso 6 — Agregar un Worker AMD (opcional)

Si necesitás ejecutar imágenes Docker que sólo existen para x86, creamos otra VM.

Configuración:

```
VM.Standard.E2.1.Micro
Ubuntu 24.04
50 GB
```

Abrimos los mismos puertos usando el procedimiento anterior.

Luego desde Coolify:

```
Servers

+ Add Server
```

Ingresamos la IP pública.

Coolify genera una clave SSH.

Copiamos esa clave dentro de:

```
~/.ssh/authorized_keys
```

del servidor AMD.

Finalmente presionamos:

```
Validate & Install
```

En pocos minutos Coolify instala automáticamente Docker y deja el nodo listo para recibir despliegues.

---

# 🏴‍☠️ Conclusión

Con esta arquitectura obtenemos:

- ✅ Un servidor ARM de **2 OCPU y 12 GB RAM**.
- ✅ Un worker x86 para proyectos incompatibles con ARM.
- ✅ HTTPS automático mediante Traefik y Let's Encrypt.
- ✅ Despliegues desde GitHub.
- ✅ Docker administrado desde una interfaz web.
- ✅ Todo dentro del plan **Always Free** de Oracle Cloud.

No está nada mal para una infraestructura que cuesta exactamente **0 doblones por mes**.

En el próximo episodio veremos cómo conectar un dominio propio, configurar DNS y dejar Coolify publicando aplicaciones como si tuviéramos un pequeño datacenter escondido en Monkey Island.
