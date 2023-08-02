---
title: Lab:02 Stored XSS into HTML context with nothing encoded
published: false
---

En este laboratorio aprenderemos que es un Stored XSS (XSS Almacenado)
también conocido como xss persistente.

Por lo consiguiente, [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/stored) nos proporciona una explicación detallada de qué consiste
esta vulnerabilidad y cómo detectarla.

## [](#header-2)Explicación de lo entendido sobre Stored XSS

¿Qué son las Stored XSS (Secuencias de Comandos entre Sitios Almacenadas)?
Las Stored XSS son una variante de XSS. Ocurren cuando el código malicioso se inyecta en la aplicación web y se almacena en el servidor o en una base de datos.
A diferencia de otros tipos de XSS, donde el código malicioso se inyecta y se ejecuta cada vez que se carga la página, en las Stored XSS, el código se almacena en el servidor
y se entrega a todos los usuarios que accedan a una página específica.

El proceso de una Stored XSS implica los siguientes pasos:

1. **El atacante encuentra un campo en la aplicación web que acepta datos de los usuarios, como un formulario de comentarios o un campo de publicación.**
2. **El atacante inserta código malicioso, generalmente JavaScript, en ese campo. Por ejemplo: `<script>alert('¡Soy un ataque XSS!');</script>`**
3. **La aplicación web, en lugar de filtrar o validar adecuadamente los datos ingresados por el atacante, almacena el código malicioso en su base de datos.**
4. **Cuando otros usuarios acceden a la página que muestra los comentarios o contenido con el código malicioso, el navegador del usuario ejecuta el script sin su conocimiento.**

Una vez entendido el funcionamiento y la diferencia que hay entre reflected xss y stored xss pasemos a la práctica.

# [](#header-1)Stored XSS
La descripción del laboratorio nos dice lo siguiente: **Este laboratorio contiene una vulnerabilidad de secuencias de comandos entre sitios almacenada en la función de comentarios.
Para resolver este laboratorio, envíe un comentario que llame a la función alert cuando se vea la publicación del blog.**

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS02/images1.png)

Así que vamos a seguir los pasos mencionados anteriormente:

**1.-** **Encontrar campos de entrada donde el usuario pueda ingresar datos.**

Al parecer, hay unos campos de entrada al abrir cualquier publicación donde el usuario puede ingresar un comentario, su nombre, correo electrónico y su sitio web.

![](/images/images_XSS02/images2.png)

Vamos a ingresar los datos y vamos a ver qué información de lo ingresado se refleja en la página.

![](/images/images_XSS02/images3.png)

Al parecer, solo dos campos se reflejan: el nombre y el comentario. Vamos a empezar a jugar con esos campos para verificar alguna anomalía al ingresar etiquetas o caracteres especiales.

**2.-** **Comprobar si los campos tienen una mala sanitización y si es así ingresar código malicioso.**

Al ingresar etiquetas HTML u otros caracteres especiales, podemos verificar si la aplicación tiene un mecanismo adecuado de sanitización y escape para evitar que los datos ingresados
se interpreten como código real.

![](/images/images_XSS02/images4.png)

La etiqueta `<h1>` se utiliza precisamente para definir el título más importante y destacado de la página, Este texto generalmente aparecerá en una fuente más grande y en negrita.
enviamos los datos.

![](/images/images_XSS02/images5.png)

En el campo de comentarios, la página nos interpreta y ejecuta la etiqueta en vez de mostrarla como texto, como lo hace el campo de nombre de usuario.
Esto podría indicar una posible vulnerabilidad de Cross-site Scripting (XSS).

Esto ocurre por qué la aplicación no realiza una correcta sanitización y validación de los datos antes de mostrarlos en la página.

**Una vez verificado, ingresamos nuestro código malicioso: `<script>alert("Stored XSS")</script>`**

![](/images/images_XSS02/images6.png)

Y lo ejecuta, mostrando la pantalla emergente. Además, [PortSwigger](https://portswigger.net/) nos muestra el mensaje de laboratorio resuelto.

![](/images/images_XSS02/images7.png)

**3.- La aplicación web, en lugar de filtrar o validar adecuadamente los datos ingresados por el atacante, almacena el código malicioso en su base de datos.**

**4.- Como es una vulnerabilidad de tipo stored XSS (XSS almacenado), Cuando otros usuarios acceden a la página que muestra los comentarios o contenido con el código malicioso,
el navegador del usuario ejecuta el script sin su conocimiento.**

;)
