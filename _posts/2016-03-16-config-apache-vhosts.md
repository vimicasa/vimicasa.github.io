---
layout: single 
title:  "Configuracion Apache - Virtual Host"
date:   2016-03-16
---

<p class="intro"><span class="dropcap">C</span>ómo configurar un virtual host en el apache para atacar la misma instancia de la aplicación para dos clientes distintos.</p>

Esta semana se nos ha presentado una situación un poco inusual, os cuento la situación;tenemos una aplicación web a la cual se le ataca desde dos contextos distintos, y cada contexto tiene sus peculiaridades, pero principalmente se diferencian visualmente. 


	dominio.es/contexto1  	|
							|=> Aplicación (running Tomcat 8)
	dominio.es/contexto2 	|

El primer problema era redireccionar las peticiones a la aplicación segun contexto y devolver las respuestas al contexto correcto. 

Otro problema era diferenciar en la aplicación las peticiones de que contexto provienen y para ello pensamos que lo mejor era introducir un parametro en la cabecera de las peticiones.

Y además también nos encontrabamos con el problema de gestionar correctamente las cookies para los dos contextos.

Como lo hemos resuelto.. gracias a <strong><a href="https://httpd.apache.org/">Apache</a></strong> y para ello hemos creado un Virtual Host y lo hemos configurado como se ve a continuación. 

{% highlight ApacheConf %}

<VirtualHost *:80>

   ServerName dominio.es

  <Location "/contexto1/">
      ProxyPass http://localhost:8080/app/ retry=1 acquire=3000 timeout=600 Keepalive=On
      ProxyPassReverse /app/
      ProxyPassReverseCookiePath /app/ /contexto1/

      Header add cs "contexto1"
      RequestHeader set cs "contexto1"    
   </Location>

  <Location "/contexto2/">
      ProxyPass http://localhost:8080/app/ retry=1 acquire=3000 timeout=600 Keepalive=On
      ProxyPassReverse /app/
      ProxyPassReverseCookiePath /app/ /contexto2/

      Header add cs "contexto2"
      RequestHeader set cs "contexto2"
   </Location>

</VirtualHost>


{% endhighlight %}

Saludos.


