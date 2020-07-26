---
layout: single 
title:  "Misión 002 - Una al mes de Hispasec (Solución)"
date:   2017-12-26
categories: 
  - ctf
---
<p class="intro"><span class="dropcap">R</span>eto Una al mes de Hispasec: Misión 002 (Solución)</p>

Desde Hispasec, es la segunda vez que lo hacen pero tienen el propósito de lanzar un reto al mes. Este segundo reto lo lanzaron el pasado Viernes, y solo estará disponible durante 7 días.
<a href="http://unaaldia.hispasec.com/2017/12/segunda-entrega-una-al-mes-mision-002.html" target="_blank">Aquí</a> teneis el enlace a dicha publicación.

(Spoiler Alert)
------
* En primer lugar visitamnos el enlace de la Misión #002<br/><a href="http://34.253.233.243/mission2.php" target="_blank">http://34.253.233.243/mission2.php</a>


* Como se puede ver nos describen el reto y la parte a destacar es: <br/>**Debemos de encontrar la manera de entrar y sacar la información oculta de la imagen que nos aparece.**. <br/>Además nos indican una un enlace de información adicional: <br/>
    **http://34.253.233.243/navidad/index.php**

* Accedemos a dicho enlace, y nos encontramos con un enlace "Postal" a la imagen que en principio tenemos que analizar (**http://34.253.233.243/navidad/img/renitos.jpg**) pero con el resultado **403 Forbidden**.Así que volvemos a la página e inspeccionamos el código fuente donde encontramos el siguiente trozo comentado.
<img src="{{ '/assets/img/posts/postal-source.png' | prepend: site.baseurl }}" alt="postal source code">
* Ya hemos encontrado que para entrar tenemos que estar en Canada, por lo que vamos a navegar a la web otra vez con un proxy de Canada. Por ello vamos a un listado gratuito de proxies que podemos encontrar en las siguientes webs:
    - <a href="https://hidemy.name/es/proxy-list/" target="_blank">https://hidemy.name/es/proxy-list/</a>
    - <a href="https://free-proxy-list.net/" target="_blank">https://free-proxy-list.net/</a>
* Una vez seleccionado uno invocamos con la herramienta wget y descargamos la imagen.  
**$ wget http://34.253.233.243/navidad/img/renitos.jpg -e use_proxy=yes -e http_proxy=158.69.31.45:3128**
<img src="{{ '/assets/img/posts/renitos.jpg' | prepend: site.baseurl }}" alt="renitos">
* Una vez tenemos la imagen utilizamos la herramienta **steghide** ver que información contiene la imagen. Y encontramos que contiene un fichero llamado **renitos.txt**. Así que extraemos dicho fichero.
<img src="{{ '/assets/img/posts/renitos_steghide.png' | prepend: site.baseurl }}" alt="renitos steghide">
* Al ver el contenido del fichero encontramos un texto que parece codificado en base64, pero al comprobarlo no lo es. Pensando y pensando .. probamos con su *hermano pequeño* **base32** y ahí si que encontramos la clave que estábamos buscando.
<img src="{{ '/assets/img/posts/renitos_flag.png' | prepend: site.baseurl }}" alt="renitos flag">

<p>Saludos</p>
