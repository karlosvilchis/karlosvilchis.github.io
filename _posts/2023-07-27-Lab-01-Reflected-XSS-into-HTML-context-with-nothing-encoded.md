---
layout: post
title: Lab:01 Reflected XSS into HTML context with nothing encoded
published: true
permalink: /lab-01-xss/
---
**¡Saludos, apasionados del hacking ético! Hoy me complace darles la bienvenida a mi ruta de aprendizaje.
Comenzaré con una de las más populares vulnerabilidades web, que es Cross-Site Scripting (XSS),
utilizando los laboratorios de [portswigger](https://portswigger.net/).
Así que, a prepararse.**

El primer laboratorio que enfrentaremos es una vulnerabilidad reflected xss
En este escenario, [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/reflected) nos proporciona una explicación detallada de qué consiste esta
vulnerabilidad y cómo detectarla.
Es el punto de partida perfecto para sumergirnos y comprender su funcionamiento.
Una vez que hayamos asimilado en que consiste esta vulnerabilidad, es hora de pasarnos a la práctica, que es en donde en verdad se aprende.

# [](#header-1)Reflected XSS
La descripción del laboratorio nos dice lo siguiente: **Para resolver el laboratorio, realice un ataque de secuencias de comandos entre sitios que llame a la función alert.**

Iniciando el laboratorio, nos encontramos con la siguiente página web

![](/images/inicio.png)

En la página web, encontramos a simple vista un campo de búsqueda, un lugar donde los usuarios pueden ingresar
palabras clave para buscar información específica. Ahora, nuestro siguiente paso es poner a prueba este campo de búsqueda
para verificar cómo la página maneja y procesa la entrada del usuario, especialmente cuando se ingresan caracteres especiales.

**Ingresamos los siguientes caracteres en el campo de búsqueda:** `& < > ' " /`

El Ingresar caracteres especiales nos permite verificar si los datos se están escapando correctamente antes de ser incluidos en el contenido HTML.
El escapado adecuado es fundamental para evitar problemas como inyecciones de código malicioso o interpretación incorrecta de los datos.

![](/images/images2.png)

Ahora analizamos los parámetros ingresados usando la herramienta de desarrollo de Chrome, observamos que la aplicación web no escapa correctamente los caracteres especiales.
Esto significa que los caracteres ingresados se muestran tal cual en la página, sin ser modificados o escapados.

![](/images/images3.png)

Ya que en cambio, si la aplicación escapa adecuadamente los caracteres especiales, los veríamos en su forma escapada, es decir, como entidades HTML. Por ejemplo, deberíamos
ver algo como esto: `&amp; &lt; &gt; &#039; &quot; &#x2F;`.

Como los datos ingresados por el usuario no son escapados antes de ser mostrados en la página, podríamos
empezar a inyectar código malicioso para comprobar la vulnerabilidad de secuencias de comandos entre sitios reflejada (Reflected XSS) inyectando lo siguiente
en el campo de búsqueda: `<script>alert("Reflected XSS")</script>`
El objetivo de este código malicioso es mostrar una alerta en el navegador con el mensaje "Reflected XSS"

![](/images/images4.png)

Con esto comprobamos la vulnerabilidad  de secuencias de comandos entre sitios reflejada (reflected xss) y obtenemos el mensaje de laboratorio resuelto.

![](/images/images5.png)

:)
