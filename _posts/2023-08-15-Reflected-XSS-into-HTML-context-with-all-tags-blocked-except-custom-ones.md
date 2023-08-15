---
title: Lab:12 Reflected XSS into HTML context with all tags blocked except custom ones
published: true
---

La descripción nos dice: **Este laboratorio bloquea todas las etiquetas HTML excepto las personalizadas.
Para resolver el laboratorio, realice un ataque de secuencias de comandos entre sitios que inyecte una etiqueta personalizada.**

Iniciando el laboratorio, nos encontramos con la siguiente página web:

![](/images/images_XSS12/images1.png)

En la página web, encontramos a simple vista un campo de búsqueda. 
Introduciendo una etiqueta HTML Estándar, como en este Ejemplo: `<h1>test</h1>`

![](/images/images_XSS12/images2.png)

Se nos presenta un error acompañado del mensaje: **'Tag is not allowed'** (La etiqueta no está permitida)'. 
Este mensaje emerge cuando se intenta ingresar una etiqueta o código HTML en el campo de entrada, y su aparición indica que la aplicación web bloquea la etiqueta que se intentó introducir.
En la mayoría de los casos, esta restricción forma parte de la funcionalidad de un Cortafuegos de Aplicaciones Web (WAF), u otras medidas de seguridad
implantadas en el servidor, que buscan prevenir que los usuarios incluyan contenido potencialmente riesgoso.

Vamos a tratar de eludir esta protección creando una etiqueta personalizada.
Nos dirigimos a la página de explotación y pegamos el siguiente código, reemplazando 'YOUR-LAB-ID' con su ID de laboratorio:
`<script>location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';</script>`

![](/images/images_XSS12/images3.png)

Luego, haga clic en 'Almacenar' y 'Entregar exploit a la víctima'."

![](/images/images_XSS12/images4.png)

una vez echo esto [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-all-standard-tags-blocked) 
nos muestra el mensaje de laboratorio resuelto.

;)
