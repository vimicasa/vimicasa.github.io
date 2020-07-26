---
layout: single 
title:  "Fortificación de cabeceras de seguridad en NGINX"
date:   2016-05-06
---

<p class="intro"><span class="dropcap">E</span>n la seguridad de una web además de servir el contenido cifrado, las cabeceras que se envían en las peticiones tienen un papel muy importante, es por ello que hay que configurar correctamente el servidor para mitigar ataques por vulnerabilidades XSS, CSRF, Clickjacking..etc   </p>

A continuación detallo algunas de las cabeceras de seguridad más importantes así como las líneas de configuración necesarias para el servidor NGINX.

<h4>Content-Security-Policy</h4>
Es una cabecera que permite proteger tu sitio de ataques XSS. Se indica una lista blanca de sitios origen desde los que se permite cargar el contenido.
{% highlight nginx %}
	add_header Content-Security-Policy "default-src 'self'";
{% endhighlight %}


<h4>X-Frame-Options</h4>
Es una cabecera que permite proteger tu sitio de ataques Clickjacking. Indica al navegador si las páginas de tu sitio pueden estar dentro de iframe o no.
{% highlight nginx %}
	add_header X-Frame-Options "SAMEORIGIN" always; <br/>
	add_header X-Frame-Options "DENY"; 
{% endhighlight %}

<h4>X-Content-Type-Options</h4>
Esta cabecera fuerza al navegador a utilizar el tipo de contenido de los ficheros que tu sitio le indica.
{% highlight nginx %}
	add_header X-Content-Type-Options "nosniff" always;
{% endhighlight %}

<h4>Strict-Transport-Security</h4>
Esta cabecera fuerza al navegador que tiene que usar el protocolo https.
{% highlight nginx %}
	add_header Strict-Transport-Security "max-age=31557600; includeSubDomains";
{% endhighlight %}

<h4>X-Xss-Protection</h4>
Esta cabecera habilita el filtro anti-XSS de los navegadores
{% highlight nginx %}
	add_header X-Xss-Protection "1";
{% endhighlight %}

El siguiente enlace a <a href="https://securityheaders.io">SecurityHeaders.io</a> permite evaluar y ver el nivel de seguridad de las cabeceras configuradas en tu web. Con lo comentado anteriormente es sencillo protegerse de las vulnerabilidades más genéricas, y sacar "buena nota" como podeis ver en mi dominio.

<figure>
     <img src="{{ '/assets/img/posts/resultado_security_headers_io.png' | prepend: site.baseurl }}" alt="Resultado A de la configuración de las cabeceras.">
</figure>

Saludos.
