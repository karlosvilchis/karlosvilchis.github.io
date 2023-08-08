---
title: Lab:07 DOM XSS in jQuery selector sink using a hashchange event
published: true
---
En el material de apoyo que nos brinda [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based), se nos dice que hay otro punto en el código
donde se pueden introducir datos o valores externos. En este caso, es la función de selección de **jQuery (la parte dentro de $())** que toma una cadena como entrada
y selecciona elementos del DOM basados en esa cadena.

El material explica cómo esta vulnerabilidad se podía explotar en el pasado, cuando jQuery era muy popular.
Si un sitio web usaba la función de selección de jQuery junto con el evento (hashchange), existía un riesgo de que un atacante pudiera insertar código malicioso en el DOM
utilizando un vector de ataque.

El código muestra cómo un controlador de eventos hashchange podría ser manipulado por un atacante para inyectar un vector XSS en la función de selección de jQuery.
El atacante podría utilizar un valor controlado por el usuario en el hash de la URL para insertar código malicioso en el DOM.

También se menciona que las versiones más recientes de jQuery han corregido esta vulnerabilidad al evitar que se inyecte HTML
en la función de selección cuando la entrada comienza con un carácter de almohadilla (#). Sin embargo, aún podría haber código vulnerable en uso.

**El texto también advierte que incluso las versiones más nuevas de jQuery podrían seguir siendo vulnerables si se tiene un control total sobre punto en el código
donde se pueden introducir datos o valores externos del selector ($()); siempre y cuando la entrada no requiera un prefijo de almohadilla (#).**

**Teniendo claros estos puntos, vamos a la práctica.**

# [](#header-1) DOM XSS in jQuery selector sink using a hashchange event

observemos la descripción: **Este laboratorio contiene una vulnerabilidad de secuencias de comandos entre sitios basada en DOM en la página de inicio.
Utiliza $()la función de selección de jQuery para desplazarse automáticamente a una publicación determinada, cuyo título se pasa a través de la location.hashpropiedad.
Para resolver el laboratorio, entregue un exploit a la víctima que llame a la print()función en su navegador.**

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS07/images1.png)

usamos la herramienta de desarrollo para ver donde se ejecuta la funcion de jQuey la parte de "$()" que nos comenta la descripcion.

![](/images/images_XSS07/images2.png)

Una vez verificado que existe el código JavaScript vulnerable, pasamos a la explotación. Nos dirigimos al servidor de exploits e ingresamos el siguiente código malicioso
en el body:
`<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>`

![](/images/images_XSS07/images3.png)

Hacemos clic en 'store' para que no lo guarde y se lo enviamos a la víctima.
**Una vez enviado, recargamos la página de inicio y [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based) nos muestra el mensaje de laboratorio resuelto."**

![](/images/images_XSS07/images4.png)

:)
