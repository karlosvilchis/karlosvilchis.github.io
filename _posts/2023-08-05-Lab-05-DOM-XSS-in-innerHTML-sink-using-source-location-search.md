---
title: Lab:05 DOM XSS in innerHTML sink using source location.search
published: true
---
[PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-innerhtml-sink) Portswigger nos da otro laboratorio de DOM XSS
para reforzar el funcionamiento de esta vulnerabilidad; observemos la descripción:

**Este laboratorio contiene una vulnerabilidad de secuencias de comandos entre sitios basada en DOM en la función de blog de búsqueda.
Utiliza una asignación innerHTML, que cambia el contenido HTML de un elemento div, utilizando datos de location.search.**

**Para resolver este laboratorio, realice un ataque de secuencias de comandos entre sitios que llame a la función alert.**

# [](#header-1) DOM XSS

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS05/images1.png)

En la página web, encontramos a simple vista un campo de búsqueda donde los usuarios pueden interactuar. Ingresamos cualquier dato; en este caso, **"lmh-4"**.
Vamos a ver cómo la página web procesa los datos de entrada del usuario.

![](/images/images_XSS05/images2.png)

Usando la herramienta de desarrollo de Chrome, analizamos los datos ingresados:

![](/images/images_XSS05/images3.png)

 y observamos que hay dos etiquetas `<span>`: una que contiene el mensaje de resultados encontrados de nuestra búsqueda y la otra contiene un atributo `id=searchMessage`
 que muestra nuestro dato ingresado. Además, notamos un script debajo que hace un llamado a este id. Vamos a ver cómo funciona este código de JavaScript.

`doSearchQuery(query)`**: Esta función toma un parámetro llamado query**

`document.getElementById('searchMessage').innerHTML = query;`**: Dentro de la función `doSearchQuery`, se utiliza `document.getElementById('searchMessage')`
para obtener una referencia al elemento con el id `"searchMessage"` en el DOM. Luego, se asigna el valor de la variable `query` al contenido de ese elemento utilizando `innerHTML`.
Esto significa que el contenido del elemento `"searchMessage"` será reemplazado por el valor de la consulta de búsqueda.**

`var query = (new URLSearchParams(window.location.search)).get('search');`**: Aquí se obtiene el valor del parámetro de búsqueda desde la URL actual.
El código crea un objeto `URLSearchParams` con la cadena de consulta de la URL actual `(window.location.search)` y luego utiliza el método `get('search')` 
para obtener el valor del parámetro llamado `'search'`. El valor obtenido se almacena en la variable `query`**

`if(query) { doSearchQuery(query); }`**: En esta parte, el código verifica si la variable `query` contiene algún valor (es decir, si se proporcionó algún término de búsqueda en la URL).
Si `query` tiene un valor (es diferente de null, undefined o una cadena vacía), entonces llama a la función `doSearchQuery(query)` con el valor de query como argumento.
Esto hará que el término de búsqueda se muestre en la página web dentro del elemento con ID `"searchMessage"`**

ya que sabemos como funciona el codigo javascript, El problema radica en que el valor de búsqueda, que es obtenido directamente de la URL sin una adecuada validación o sanitización,
al ingresar un codigo malicioso `"><img src=x onerror=alert("DOM-XSS");>` se establecerá directamente en el contenido del elemento con **ID "searchMessage"** utilizando **innerHTML.**
Como resultado, el script malicioso se ejecutará en el contexto del sitio web, y en este caso, mostrará una alerta con el mensaje **"DOM-XSS",** 
y afectará a otros usuarios que visiten la página.

![](/images/images_XSS05/images4.png)

Ingresamos el código malicioso y observamos que se muestra una ventana emergente con el mensaje 'DOM-XSS',
y [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-innerhtml-sink) nos muestra el mensaje de laboratorio resuelto.

![](/images/images_XSS05/images5.png)

:)
