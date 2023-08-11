---
title: Lab:10 Stored DOM XSS
published: true
---

# [](#header-1) Reflected DOM XSS

La descripción nos dice: **Esta práctica de laboratorio demuestra una vulnerabilidad DOM almacenada en la funcionalidad de comentarios del blog.
Para resolver este laboratorio, aproveche esta vulnerabilidad para llamar a la función alert().**

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS10/images1.png)

Al parecer, hay unos campos de entrada al abrir cualquier publicación donde el usuario puede ingresar un comentario, su nombre, correo electrónico y su sitio web.

![](/images/images_XSS010/images2.png)

Rellenemos los campos de entrada para ver cómo se procesan los datos una vez enviados.

![](/images/images_XSS010/images3.png)

Utilizando la herramienta de desarrollo en la pestaña **'Network'**, observamos la presencia de un archivo js **loadCommentsWithVulnerableEscapeHtml.js**, 

![](/images/images_XSS10/images4.png)

que contiene una funcion llamada **escapeHTML().** La función utiliza el método **replace()** de JavaScript para buscar y reemplazar todos los caracteres `<` en la cadena html con **&lt;**, 
que es la entidad HTML para `<.`
Luego, se encadena otro **replace()** para buscar y reemplazar todos los caracteres `>` en la cadena html con **&gt;**, que es la entidad HTML para `>`.

La función retorna la cadena de texto resultante después de que se han realizado los reemplazos.
El código en sí mismo intenta escapar los caracteres `<` y `>` en una cadena HTML para evitar que se interpreten como etiquetas HTML reales. Sin embargo, esta implementación específica
tiene una limitación importante que podría permitir ataques **XSS.**

La función escapeHTML tal como está definida solo reemplaza la primera aparición de `<` y `>`. Esto significa que si se introduce una cadena como esta: `<><img src=x onerror=alert(1);>`
La función solo reemplazaría el primer `<`, convirtiéndolo en **&lt;**, pero dejaría el segundo `<` sin cambios.
Como resultado, el navegador aún interpretaría la etiqueta y ejecutaría el código malicioso.

Una vez sabiendo cómo se procesan y filtran los datos, ingresamos el código malicioso y lo enviamos.

![](/images/images_XSS10/images5.png)

Hacemos clic hacia atrás, y observamos que se muestra una ventana emergente, y [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based)
nos muestra el mensaje de laboratorio resuelto.

![](/images/images_XSS10/images6.png)

![](/images/images_XSS10/images7.png)

:)
