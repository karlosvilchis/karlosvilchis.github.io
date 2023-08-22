---
title: Lab:13 Reflected XSS with event handlers and href attributes blocked
published: true
---

La descripción nos dice: **Este laboratorio contiene una vulnerabilidad XSS reflejada con algunas etiquetas incluidas en la lista blanca, pero todos los eventos y atributos href
de anclaje están bloqueados.
Para resolver el laboratorio, realiza un ataque de secuencias de comandos entre sitios (XSS) que inyecte un vector que, al ser clicado, llame a la función.
Ten en cuenta que debes etiquetar tu vector con la palabra "Hacer clic" para inducir al usuario del laboratorio simulado a hacer clic en tu vector. Por ejemplo:
`<a href="#">Haz clic en mí</a>`**

Lista blanca: **Es una lista de elementos o características permitidos. En este contexto, las "etiquetas" se refieren a las etiquetas HTML que se pueden usar en la entrada del usuario
o en algún otro contexto de la aplicación web. 
Una lista blanca significa que solo se permiten ciertas etiquetas específicas y todas las demás se bloquean o filtran.**

Eventos: ** En el contexto de la vulnerabilidad XSS, los "eventos" se refieren a acciones que pueden desencadenar código JavaScript, como hacer clic en un botón o introducir datos
en un formulario.**

href: **El atributo "href" se utiliza comúnmente en las etiquetas de anclaje (`<a>`) para definir la URL de destino cuando se hace clic en el enlace.**

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS13/images1.png)

Notamos un campo de búsqueda. Inyecté una etiqueta HTML en este caso, la etiqueta `<h1>test</h1>`, para ver cómo la página web la procesa.
Una vez que presioné Enter, la página me mostró un error con el siguiente mensaje: 'La etiqueta no está permitida', y una respuesta 400

![](/images/images_XSS13/images2.png)

Sabiendo cómo se muestra el mensaje y el estado de respuesta, usaré Burp Suite para realizar un ataque de fuerza bruta con etiquetas, con el objetivo de verificar cuál de ellas provoca
una respuesta diferente.

![](/images/images_XSS13/images3.png)

en la pestaña de HTTP history de burpsuite podemos ver nuestra busqueda, con ctrl+i lo mandamos a la pestaña intruder reemplazamos el valor de la busqueda con `<>`
y entre los corchetes angulares hacemos click en agregar 2 veces ya que va ser donde ingresaremos nuestra carga

![](/images/images_XSS13/images4.png)

Hacemos clic en la opción ‘Copy Tags to Clipboard’ de la hoja de trucos de XSS para copiar las etiquetas al portapapeles. Luego nos dirigimos a la pestaña ‘Payloads’
y en la sección de ‘Payload Settings’, pegamos nuestra lista.

![](/images/images_XSS13/images5.png)

Presionamos el botón que dice ‘Iniciar Ataque’. Una vez finalizado el ataque, notamos que la mayoría de las solicitudes reciben una respuesta con código 400, a excepción de ‘svg’, que
generó una respuesta con código 200. Esto sugiere que la etiqueta ‘svg’ no está siendo bloqueada

![](/images/images_XSS13/images6.png)

Investigué un poco sobre los eventos de SVG y cómo pueden utilizarse para ejecutar JavaScript. 
El objetivo era encontrar una forma de eludir el atributo href, que estaba bloqueado en el contexto.

En SVG, `<animate>` es un elemento utilizado para animar propiedades de otros elementos
El atributo attributeName en el elemento `<animate>` de SVG sirve para especificar el nombre del atributo o propiedad del elemento SVG que se va a animar.
Este atributo determina qué aspecto del elemento SVG será modificado durante la animación.

attributeName="href" = En este caso, se utiliza para modificar el atributo href, que normalmente se utiliza para definir la dirección de destino de un enlace hipertexto.

Con esto claro, procedemos a utilizar el siguiente payload y lo ingresamos en la URL: `<svg><a><animate attributeName=href values=javascript:alert(1) /><text x=20 y=20>Click me</text></a>`
Esto nos creará un botón llamado 'Click me'. Cuando la víctima haga clic en él, ejecutará el script malicioso.

Una vez echo click [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-event-handlers-and-href-attributes-blocked)
nos muestra una ventana emergente y nos muestra el mensaje de laboratorio resuelto.

![](/images/images_XSS13/images7.png)

;)
