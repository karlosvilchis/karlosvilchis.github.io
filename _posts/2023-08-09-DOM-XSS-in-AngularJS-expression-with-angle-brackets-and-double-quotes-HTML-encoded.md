---
title: Lab:08 DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded
published: true
---
[PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based) nos explica que 'ng-app' es un atributo especial de AngularJS que se utiliza
para inicializar una aplicación de AngularJS en un elemento HTML específico.
El hecho de que "AngularJS lo procesará" implica que cuando se utiliza el atributo 'ng-app' en un elemento HTML, AngularJS toma el control del procesamiento de la página
y puede ejecutar JavaScript dentro de las expresiones en llaves dobles.

# [](#header-1) DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

obeservemos la descripción: **Este laboratorio contiene una vulnerabilidad de secuencias de comandos entre sitios basada en DOM en una expresión de AngularJS
dentro de la función de búsqueda.
AngularJS es una biblioteca de JavaScript popular, que escanea el contenido de los nodos HTML que contienen el ng-appatributo (también conocido como directiva AngularJS).
Cuando se agrega una directiva al código HTML, puede ejecutar expresiones de JavaScript entre llaves dobles. Esta técnica es útil cuando se codifican corchetes angulares.
Para resolver este laboratorio, realice un ataque de secuencias de comandos entre sitios alert que ejecute una expresión de AngularJS**

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS08/images1.png)

Introducimos una cadena alfanumérica para observar cómo la página web la procesa.
Y observamos la presencia de 'ng-app'.

![](/images/images_XSS08/images2.png)

Utilizando la hoja de trucos (cheat sheet) proporcionada por [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet), 

![](/images/images_XSS08/images3.png)

procedemos a ejecutar el primer fragmento de código malicioso en el campo de busqueda.

![](/images/images_XSS08/images4.png)

Ingresamos el código malicioso y observamos que se muestra una ventana emergente, y [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/dom-based)
nos muestra el mensaje de laboratorio resuelto. 

![](/images/images_XSS08/images5.png)

![](/images/images_XSS08/images6.png)
:)
