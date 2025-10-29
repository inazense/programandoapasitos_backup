---
title: Aprendiendo Python (I). Introducción e instalación
description: Guía completa para principiantes sobre Python 3: instalación paso a paso en Windows, Linux y Mac, configuración del intérprete, primer programa Hola Mundo y diferencias entre Python 2 y 3.
author: Inazio Claver
date: 2016-10-19 12:10:00 +0800
categories: [Python]
tags: [python]
pin: false
math: false
mermaid: false
#image:
#  path: /commons/devices-mockup.png
#  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#  alt: Responsive rendering of Chirpy theme on multiple devices.
---

Antes de nada más que un **tutorial sobre Python** esto va a ser un diario de aprendizaje. Iremos conociendo este lenguaje de la mano, intentaré explicar todo que vaya aprendiendo y espero que os resulte productivo.

**Python** es un lenguaje interpretado y multiparadigma, ya que soporta orientación a objetos, programación funcional y programación imperativa, además de contar con licencia abierta. Y por si fuese poco todo esto, también es multiplataforma. ¿Que más podemos pedir?

El nombre del lenguaje no tiene nada que ver con el tipo de serpientes que te vendrán a la cabeza, sino con los **Monty Python**. Su creador, **Guido van Rossum**, era un gran aficionado a los británicos y decidió homenajearlos poniéndole este nombre. Ni pintado le salió al hombre...

![Monty Python guido van rossum](/img/posts/20160919_1.jpg)

**Python** se está alzando ultimamente como el lenguaje más usado para iniciarse en la programación porque no requiere ni aprender una sintaxis compleja, ni emplear llaves, ni siquiera punto y coma. Hasta hay libros para niños que les permite aprender **Python**. Yo me inicié con **C** puro y duro, y desde luego en ese lenguaje dudo que encuentre algo "orientado para niños"...

Bueno, hasta ahora hemos hecho un breve repaso a las características de **Python** y estamos deseosos de ponernos a programar con él. Notamos un subidón por las venas, los dedos ansían teclear... Solo nos queda en que **Python** programar.

¿En qué **Python**? Sí, como lo oyes. A día de hoy contamos con dos intérpretes de **Python**, dos vertientes, la 2.7.x y la 3.5.x. Las diferencias son sustanciales, partiendo porque no todas las bibliotecas que están operativas para la versión 2.7.x estarán traducidas para la 3.5.x, pero cada vez se está trabajando más en ello y la eliminación de **Python 2** está siendo gradual. Eso es uno de los motivos que me ha llevado a aprender la tecnología de la versión 3. Otra es que aunque yo programe en **Windows** ahora mismo, me encanta emplear **distros de Linux** (tengo pendiente saber desenvolverme bien en **Kali Linux**, por cierto) y en muchas ya viene por defecto la última versión de **Python**. Y la última basicamente es porque es lo que me apetece. Si el programa que quiera desarrollar lo puedo hacer en la versión 3, genial, si no siempre podré consultar y bajarme la 2.7 para realizar una tarea específica.

Entonces, vamos a instalar nuestro intérprete. Si eres usuario de **Linux** lo más seguro es que ya lo tengas instalado (puedes comprobarlo escribiendo en la terminal ```python -V```). Si no es tu caso puedes instalarlo desde la misma consola escribiendo ```sudo apt-get install python```. Para **Windows** (mi caso) o **Mac**, deberemos ir a la web oficial y descargarnos el ejecutable.

Puedes acceder pulsando [aquí](https://www.python.org/downloads/).

Ya lo tenemos descargado, ahora simplemente lo ejecutamos y veremos una pantalla como esta

![instalación de python paso a paso 1](/img/posts/20160919_2.png)

Marca la casilla de ```Add Python 3.5 to PATH``` y elige ```Install Now```. El proceso se completará automáticamente, momento en el que veremos la siguiente ventana.

![instalación de python paso a paso 1](/img/posts/20160919_3.png)

Ahora vamos a comprobar si nos funciona correctamente. Puedes abrir una terminal, escribir ```python``` y pulsar Enter, o bien abrir la aplicación **Python 3.5**. El resultado deberá ser el mismo, ver la siguiente terminal

```bash
Python 3.5.2 (v3.5.2:4def2a2901a5), Jun 25 2016, 22:01:18 [MSC v.1900 32 vit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

Más adelante pasaré a comentar en más profundidad la consola de comandos, pero por el momento nos basta con escribir el siguiente texto

```python
print("Hola mundo!")
```

Y al pulsar Enter habremos generado nuestro primer Hola mundo. Esto nos indica que funciona correctamente.

Por cierto, si quieres cerrar la terminal basta con escribir

```python
exit()
```

Esto ha sido todo por la primera entrega. Simplemente hemos preparado nuestro entorno para ejecutar **Python**. Como estamos aprendiendo el lenguaje yo recomiendo usar cualquier editor de texto sin corrección ni autocompletado. **Gedit** es perfecto para linuxeros, en Windows puedes tirar de **Notepad++**, **Atom**, **Sublime Text**... a tu elección. Más adelante ya habrá tiempo de usar un **IDE** completo pero ahora es lo mejor para hacerte con los entresijos del lenguaje.

**¡Salud y coding!**