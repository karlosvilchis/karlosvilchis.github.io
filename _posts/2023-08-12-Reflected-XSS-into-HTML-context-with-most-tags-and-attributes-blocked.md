---
title: Lab:11 Reflected XSS into HTML context with most tags and attributes blocked
published: true
---

La descripción nos dice: **Este laboratorio contiene una vulnerabilidad XSS reflejada en la funcionalidad de búsqueda, pero utiliza un firewall de aplicaciones web (WAF)
para proteger contra los vectores XSS comunes.
Para resolver el laboratorio, realice un ataque de secuencias de comandos entre sitios que omita el WAF y llame a la función print().**

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS11/images1.png)

En la página web, encontramos a simple vista un campo de búsqueda. 
Introduciendo un Código Malicioso Estándar, como en este Ejemplo: `<img src=1 onerror=print();>`

![](/images/images_XSS11/images2.png)

Se nos presenta un error acompañado del mensaje: **'Tag is not allowed'** (La etiqueta no está permitida)'. 
Este mensaje emerge cuando se intenta ingresar una etiqueta o código HTML en el campo de entrada, y su aparición indica que la aplicación web bloquea la etiqueta que se intentó introducir.
En la mayoría de los casos, esta restricción forma parte de la funcionalidad de un Cortafuegos de Aplicaciones Web (WAF), u otras medidas de seguridad
implantadas en el servidor, que buscan prevenir que los usuarios incluyan contenido potencialmente riesgoso.

![](/images/images_XSS11/images3.png)

Ahora, nos disponemos a intentar eludir esta defensa de aplicaciones web (WAF) con la asistencia de Burp Suite, con el objetivo de confirmar qué etiquetas se encuentran permitidas.

![](/images/images_XSS11/images4.png)

en la pestaña de HTTP history de burpsuite podemos ver nuestra busqueda, con ctrl+i lo mandamos a la pestaña intruder
reemplazamos el valor de la busqueda con <> y entre los corchetes angulares hacemos click en agregar 2 veces
ya que va ser donde ingresaremos nuestra carga 

![](/images/images_XSS11/images5.png)

Hacemos clic en la opción 'Copy Tags to Clipboard' de la hoja de trucos de XSS para copiar las etiquetas al portapapeles.
Luego nos dirigimos a la pestaña 'Payloads' y en la sección de 'Payload Settings', pegamos nuestra lista.

![](/images/images_XSS11/images6.png)

Presionamos el botón que dice 'Iniciar Ataque'.
Una vez finalizado el ataque, notamos que la mayoría de las solicitudes reciben una respuesta con código 400, a excepción de 'body', que generó una respuesta con código 200.
Esto sugiere que la etiqueta 'body' no está siendo bloqueada por el WAF.

![](/images/images_XSS11/images7.png)

Ahora, en la pestaña 'Positions' de Burp Intruder, reemplazamos el término de búsqueda con <body%20=1> y antes del carácter '=' hacemos clic en 'Add'.

![](/images/images_XSS11/images8.png)

Una vez hecho esto, pasamos a la hoja de trucos de XSS y presionamos 'Copy events to clipboard'.

![](/images/images_XSS11/images9.png)

Después, dirigimos nuestra atención a la pestaña 'Payloads' y en la sección de 'Payload Settings', hacemos clic en 'Clear' para eliminar la lista anterior
y luego pegamos nuestra nueva lista y hacemos click en 'Iniciar ataque'.

![](/images/images_XSS11/images10.png)

Igualmente, observamos que en cuanto a las etiquetas, la mayoría de las respuestas son 400. Sin embargo, notamos que la carga útil 'onresize' generó una respuesta con código 200

Con estos elementos obtenidos, estamos ahora listos para elaborar nuestro código malicioso con el fin de eludir el WAF..

![](/images/images_XSS11/images11.png)

Ahora, nos dirigimos a la página de explotación y pegamos el siguiente código, reemplazando 'YOUR-LAB-ID' con su ID de laboratorio:
`<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()>`
Luego, haga clic en 'Almacenar' y 'Entregar exploit a la víctima'."

![](/images/images_XSS11/images12.png)

una vez echo esto [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked) nos muestra el mensaje de laboratorio resuelto

![](/images/images_XSS11/images13.png)

:)
