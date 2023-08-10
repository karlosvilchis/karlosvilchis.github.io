---
title: Lab:09 Reflected DOM XSS
published: true
---

# [](#header-1) Reflected DOM XSS

La descripción nos dice: **Este laboratorio demuestra una vulnerabilidad DOM reflejada. Las vulnerabilidades DOM reflejadas ocurren cuando la aplicación del lado del servidor procesa
los datos de una solicitud y los replica en la respuesta. Luego, un script en la página procesa los datos reflejados de manera insegura y, en última instancia, los escribe en un receptor
peligroso.**

**Para resolver este laboratorio, cree una inyección que llame a la función alert().**

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS09/images1.png)

A primera vista, notamos un campo de búsqueda, uno de nuestros lugares favoritos para realizar pruebas.
Ingresamos cualquier valor en el campo para verificar cómo lo procesa la página web.

![](/images/images_XSS09/images2.png)

Utilizando la herramienta de desarrollo en la pestaña **'Network'**, observamos la presencia de **'search-results?search=test'**, que contiene el texto que ingresamos. Vamos a analizarlo.

![](/images/images_XSS09/images3.png)

Parece que nos proporciona la respuesta de nuestra búsqueda en formato JSON.
Al probar con otros caracteres especiales, notamos que nos arroja un error en la respuesta, ya que la barra invertida no se escapa.

![](/images/images_XSS09/images4.png)

Para intentar escapar del JSON, ingresamos el siguiente payload: `test\"-alert(1)}//`

![](/images/images_XSS09/images5.png)

![](/images/images_XSS09/images6.png)

observamos que se muestra una ventana emergente, y [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based)
nos muestra el mensaje de laboratorio resuelto. 

Esto sucede porque el servidor procesa los datos de la solicitud y los replica en la respuesta. Al analizar el archivo searchResults.js, observamos que utiliza la función eval(),
la cual es un punto de riesgo, ya que procesa el argumento que se le pasa como JavaScript y, en última instancia, lo escribe en un receptor peligroso.

![](/images/images_XSS09/images7.png)

;)
