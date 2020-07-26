---
layout: single 
title:  "Ahora sólo mediante HTTPS gracias al certificado de Let's Encrypt"
date:   2016-04-30
---

<p class="intro"><span class="dropcap">F</span>inalmente he configurado el certificado SSL, gracias a <a href="https://letsencrypt.org/">Let's Encrypt</a> para acceder al blog únicamente mediante HTTPS. La verdad que despues de haberlo hecho, ha sido un proceso muy rápido. En primer lugar hay que agradecer a Let's Encrypt que gracias a ellos es posible que todo el mundo pueda acceder a un certificado SSL de manera gratuita y adaptarse a la actualidad .. seguridad, http2 (solo con SSL), posicionamiento en buscadores .. </p>

Hasta ahora si querías un certificado SSL (sin contar los certificados autofirmados) todas las autoridades certificadores requerían un pago [impuesto],<i>en mi opinión abusivo</i>, siendo algo que los particulares no podemos afrontar para estos proyectos personales, y ahora gracias a Let's Encrypt es gratis, automatizado y open.

En mi caso utilizo el servidor HTTP: Nginx, así que si buscais en google Nginx y Lets Encrypt encontrareís infinitas entradas donde explican el proceso. Yo os dejo el enlace a la siguiente <a href="http://www.tecmint.com/secure-nginx-with-lets-encrypt-ssl-certificate-on-ubuntu-and-debian/">guía</a> de Tecmint que me ha parecido bastante completa y didáctica. 

Una vez instalado podeís ir a la web de <a href="https://www.ssllabs.com/ssltest/analyze.html">SSL Labs</a> y evaluar el nivel del certificado SSL instalado en tu dominio. Os dejo la imagen del resultado de mi dominio con A+, aunque cualquiera puede consultarlo.


<figure>
     <img src="{{ '/assets/img/posts/certificado_ssl_a_vcatalan.com.png' | prepend: site.baseurl }}" alt="Resultado A+ de la configuración del certificado de ssllabs.">
</figure>

En un futuro (si no es ya en la actualidad) el certificado SSL y el acceso seguro por HTTPS serán de obligado complimiento, así que cuanto antes te unas mucho mejor!!

Saludos.

