---
title: Lab:06 DOM XSS in jQuery anchor href attribute sink using location.search source
published: true
---
[PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based) En este laboratorio, explotaremos otra vulnerabilidad de DOM-based XSS,
pero esta vez estaremos abusando de una dependencia de terceros que la página web utiliza. En este caso, se trata de la biblioteca de JavaScript jQuery.

La descripción del laboratorio nos dice lo siguiente: **Este laboratorio contiene una vulnerabilidad de secuencias de comandos entre sitios basada en DOM
en la página de envío de comentarios. Utiliza la $función de selección de la biblioteca jQuery para encontrar un elemento de anclaje y cambia su href atributo usando datos
de location.search. Para resolver esta práctica de laboratorio, active la alerta del enlace "atrás" document.cookie.**

# [](#header-1) DOM XSS in jQuery anchor href attribute sink using location.search source

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS06/images1.png)

Siguiendo la descripción proporcionada, nos dirigimos a la sección de envío de comentarios en la página web, donde se nos redirige hacia el siguiente enlace.
En esta nueva página, nos encontramos con diversos campos de entrada y una URL que incluye el parámetro returnPath.

![](/images/images_XSS06/images2.png)

En esta ocasión, exploraré la dinámica de la URL, que además de su función principal, también opera como un punto de acceso que permite a los usuarios interactuar con la pagina web.
No obstante, aunque también se abre la posibilidad de desencadenar una vulnerabilidad XSS si no cuenta con un filtrado sólido de los datos. 
En virtud de ello, procederé a insertar la siguiente cadena en la URL, haciendo uso del parámetro returnPath: **returPath=lmh-4**.

![](/images/images_XSS06/images3.png)

Usando la herramienta de desarrollo, analizamos los datos ingresados:

![](/images/images_XSS06/images4.png)

Observamos que la cadena aleatoria que he ingresado, en este caso **'lmh-4'**, ha sido ubicada dentro de un atributo **href**. Justo debajo, encontramos un script
que hace uso de este atributo. Analicemos el propósito de este script.

`$(function() {...});`: Esto define una función que se ejecutará una vez que la página web haya terminado de cargarse. En otras palabras, cuando el DOM esté listo.

`$('#backLink')`: Utiliza el selector de jQuery para buscar un elemento en el DOM con el ID "backLink". El símbolo "$" es una función que se usa para acceder a las funcionalidades de jQuery.

`.attr("href", ...)`: Esta función se usa para cambiar el valor de un atributo de un elemento HTML. En este caso, el atributo **"href"** de un enlace.

`(new URLSearchParams(window.location.search)).get('returnPath')`: Aquí se crea una instancia de URLSearchParams usando el valor de la cadena de consulta de la URL actual
(lo que sigue después del signo de interrogación "?"). Luego, se utiliza el método .get('returnPath') para obtener el valor del parámetro "returnPath" de la cadena de consulta.

**En resumen, este código toma el valor del parámetro "returnPath" de la URL y lo establece como el nuevo valor del atributo "href".**

**Ahora que comprendemos su funcionamiento, procederemos a intentar manipular el valor del parámetro 'returnPath', con el objetivo de inyectar nuestro propio código malicioso**
returnPath=`javascript:alert(document.cookie)`

![](/images/images_XSS06/images5.png)

Después de presionar 'Enter', hacemos clic en 'Back'. Esto provoca la aparición de una ventana emergente y
[PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based) nos muestra el mensaje del laboratorio resuelto.

![](/images/images_XSS06/images6.png)

![](/images/images_XSS06/images7.png)

;)
