---
title: Lab:03 DOM XSS in document.write sink using source location.search
published: true
---
En este laboratorio aprenderemos que es un DOM-based XSS.
[PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based) proporciona una explicación de cómo funciona esta vulnerabilidad.
Una vez teniendo esta base sobre su funcionamiento, pasemos a la acción.

# [](#header-1) DOM XSS
La descripción del laboratorio nos dice lo siguiente: 
**Este laboratorio contiene una vulnerabilidad de secuencias de comandos entre sitios basada en DOM en la funcionalidad de seguimiento de consultas de búsqueda.
Utiliza la document.write función JavaScript, que escribe datos en la página. La document.write función se llama con datos de location.search,
que puede controlar mediante la URL del sitio web. Para resolver este laboratorio, realice un ataque de secuencias de comandos entre sitios que llame a la función alert.**

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS03/images1.png)

A primera vista, tenemos un campo de búsqueda donde podemos interactuar y validar la sanitización de los datos ingresados.
Así que, al igual que en los laboratorios anteriores, vamos a ingresar los siguientes caracteres: `& < > ' " /`

![](/images/images_XSS03/images2.png)

Así que abrimos nuestra herramienta de desarrollo de Chrome y verifiquemos cómo procesa nuestros parámetros ingresados.

![](/images/images_XSS03/images3.png)

Notamos que nuestros parámetros no los encierra en una etiqueta `<img>` creada por un script que se encuentra arriba de nuestra etiqueta. Veamos cómo funciona este script.
El script es el siguiete:

![](/images/images_XSS03/images4.png)

* La función **trackSearch(query)** toma un parámetro llamado **query**
* Dentro de la función, se utiliza **document.write()** para insertar contenido en el documento HTML actual. En este caso, se está insertando una etiqueta de imagen **(`<img>`)**
* La etiqueta de imagen tiene un atributo **src** que apunta a una imagen llamada **"tracker.gif"** ubicada en el directorio **"/resources/images/"** del sitio web.
* Además, el valor del parámetro **query** se agrega a la URL de la imagen como un parámetro llamado **"searchTerms"**. Esto se logra concatenando el valor de **query** a la URL
después de **"?searchTerms=".**
* Cuando este script se ejecuta, primero busca el valor del parámetro **"search"** en la URL de la página utilizando **URLSearchParams**. Si encuentra un valor
para el parámetro **"search"**, invoca la función **trackSearch(query)** y pasa el valor de **"search"** como argumento.

En resumen, el script toma el valor del parámetro search presente en la URL actual y lo utiliza para construir una URL de imagen que apunta al recurso /resources/images/tracker.gif.
Luego, inserta esta etiqueta de imagen en el documento utilizando document.write().

Una vez sabiendo que el script toma el valor del parámetro **'search'**, podemos establecer un valor malicioso usando la consola de la herramienta de desarrollo de Chrome,
ejecutando el siguiente comando: `'window.location.search = "?search=\"><img src=x onerror=alert(\"DOM-XSS\");>"`

![](/images/images_XSS03/images5.png)
> window.location.search es una propiedad de JavaScript que devuelve la cadena de consulta (query string) de la URL actual.
> La cadena de consulta es la parte de la URL que sigue al signo de interrogación **?** y generalmente se utiliza para enviar parámetros o datos adicionales al servidor.

Una vez dándole a Enter, nos muestra la ventana emergente con el mensaje 'DOM-XSS', y [PortSwigger](https://portswigger.net/) nos muestra el mensaje de laboratorio resuelto.

![](/images/images_XSS03/images6.png)

Nuestro código malicioso fue este: `><img src=x onerror=alert(1);>`

El primer carácter **'>'** lo colocamos al inicio para escapar de cualquier contexto HTML en que se esté utilizando el valor del parámetro.
El objetivo es romper la estructura del código HTML existente y cerrar una etiqueta HTML previamente abierta, permitiendo que el código malicioso agregue una etiqueta,
en este caso **'`<img>`'**, que incluya un evento **'onerror'** que ejecutará código JavaScript malicioso.
Si el código vulnerable no realiza una adecuada validación o escape del valor del parámetro **'search'**, el código JavaScript malicioso que sigue después de **'><img'** se ejecutará.

;)
