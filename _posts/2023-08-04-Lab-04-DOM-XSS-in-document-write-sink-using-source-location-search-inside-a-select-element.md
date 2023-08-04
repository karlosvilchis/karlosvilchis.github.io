---
title: Lab:04 DOM XSS in document.write sink using source location.search inside a select element
published: true
---
[PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink-inside-select-element) Nos proporciona otro laboratorio de DOM-based XSS.

# [](#header-1) DOM XSS
La descripción del laboratorio nos dice lo siguiente:
**Este laboratorio contiene una vulnerabilidad de secuencias de comandos entre sitios basada en DOM en la función de verificación de existencias.
Utiliza la document.write función JavaScript, que escribe datos en la página. La document.write función se llama con datos desde location.search los que puede controlar utilizando
la URL del sitio web.
Los datos se incluyen dentro de un elemento de selección.
Para resolver este laboratorio, realice un ataque de secuencias de comandos entre sitios que salga del elemento seleccionado y llame a la alertfunción.**

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS04/images1.png)

Con lo visto anteriormente, nuestro primer paso es encontrar un campo en la aplicación web que acepte datos de los usuarios, como un formulario de comentarios o un campo de publicación,
o cualquier otro campo donde el usuario pueda interactuar.

_Encontré este campo que contiene un menú desplegable con opciones predefinidas de ciudades ("London", "Paris", "Milan"), con el cual el usuario puede interactuar._

![](/images/images_XSS04/images2.png)

Con la herramienta de desarrollo de Chrome, al analizar ese campo de menús, observamos el siguiente script:

![](/images/images_XSS04/images3.png)

Vamos a analizar cuál es su función.

1. _var stores = ["London","Paris","Milan"];: Se define una variable stores que es un array que contiene tres ciudades: "London", "Paris" y "Milan"._

2. _var store = (new URLSearchParams(window.location.search)).get('storeId');: Se utiliza URLSearchParams para obtener el valor del parámetro "storeId" de la URL actual.
Si el parámetro "storeId" está presente en la URL, su valor se almacenará en la variable store._

3. _`document.write('<select name="storeId">');`: Se utiliza **document.write()** para comenzar a escribir el código HTML en el documento actual.
Se inicia la creación de un menú desplegable **(select)** con el atributo **"name"** establecido como **"storeId".**_

4. _**if(store) { ... }**: Se verifica si la variable store tiene un valor (es decir, si el parámetro **"storeId"** está presente en la URL).
Si es así, se ejecuta el bloque de código dentro de este **if.**_

5. _`document.write('<option selected>'+store+'</option>');`: Dentro del bloque **if**, se escribe una opción (option) dentro del menú desplegable. Esta opción contendrá
el valor de store (es decir, el valor del parámetro **"storeId"**) y estará seleccionada por defecto, ya que tiene el atributo **"selected".**_

6. _`for(var i=0;i<stores.length;i++) { ... }`: Después del **if**, se inicia un bucle for que recorre el array stores que contiene las ciudades._

7. _`if(stores[i] === store) { continue; }`: En cada iteración del bucle, se verifica si el valor de **stores[i]** es igual al valor de store (el valor del parámetro **"storeId"**).
Si son iguales, se omite esta opción utilizando **continue**, ya que ya se ha agregado previamente al menú desplegable._

8. _`document.write('<option>'+stores[i]+'</option>');:` Si la ciudad actual (**stores[i]**) no coincide con el valor de **store**, se escribe una opción **(option)** dentro del
menú desplegable con el valor de la ciudad actual._

9. _`document.write('</select>');`: Finalmente, después del bucle **for**, se cierra el menú desplegable utilizando **document.write().**_

**En resumen, el script crea un menú desplegable en una página web con opciones predefinidas de ciudades ("London", "Paris", "Milan"). Si se proporciona un parámetro "storeId" en la URL,
selecciona la opción correspondiente a la ciudad con ese valor. El menú desplegable permite al usuario seleccionar una ciudad y, en función de la opción seleccionada, realizar
alguna acción o envío de formulario en el sitio web.**

**Entonces, Si podemos controlar el valor del parámetro _'storeId'_ y este valor no se escapa o valida adecuadamente, podríamos inyectar código JavaScript malicioso en el sitio web.**

_Usando la herramienta de Chrome en la pestaña de consola, ingresamos lo siguiente:_
```
window.location.search = "?productId=1&storeId=\"></select><img%20src=1%20onerror=alert(\"DOM-XSS-VULN\")>"
```
![](/images/images_XSS04/images4.png)

Una vez dándole a Enter, nos muestra la ventana emergente con el mensaje 'DOM-XSS-VULN', y [PortSwigger](https://portswigger.net/) nos muestra el mensaje de laboratorio resuelto.

![](/images/images_XSS04/images5.png)

;)
