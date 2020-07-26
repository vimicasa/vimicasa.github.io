---
layout: single 
title:  "Bash Script para publicar un post en mi blog automáticamente"
date:   2016-12-30
---
<p class="intro"><span class="dropcap">B</span>ash script desarrollado para automatizar el despliegue de mi blog desarrollado en jekyll ante cualquier cambio que se produzca.</p>

<p>En primer lugar hay que recordar que para mi blog uso Jekyll, es una herramienta que permite transformar texto plano en 
web estáticas y blogs. Para mi es muy sencillo, no se necesita ninguna base de datos, moderación de comentarios entre otros. Si queréis conocer un poco más ir a su <a href="https://jekyllrb.com/">web</a> que está muy bien explicado como iniciarte. </p>

<p>Lo que es relevante para nosotros, es que el código estático del blog se genera en una carpeta con el nombre <strong>_site</strong>. Esta carpeta contiene todos los elementos necesarios (assets, css, js, images, html, ... etc). </p>

<p><strong>Objetivo:</strong> Desplegar esta carpeta en el servidor donde se aloja el blog.</p>

<p><strong>Tareas:</strong>
<ul>
    <li>Realizar un backup del contenido actual</li>
    <li>Comprimir la carpeta _site</li>
    <li>Enviar / Copiar dicha carpeta al servdidor</li>
    <li>Sustituir el contenido</li>
</ul></p>

<p>Se ha desarrollado el siguiente script en bash que nos permite realizar todas las tareas. Está alojado en mi <a href="https://github.com/vimicasa/codes/blob/master/Scripts/publish.sh">GitHub</a>.</p>

<figure class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="c">#!/bin/bash</span>

<span class="c"># Run a backup</span>
ssh username@domain.com <span class="s1">&#39;./backup.sh&#39;</span>

<span class="c">#Compress the folder and copy it to the server</span>
tar -cvzf site.tgz _site
scp site.tgz username@domain.com:/home/username

<span class="c">#Deploy new version</span>
ssh -t username@domain.com bash -c <span class="s2">&quot;&#39;</span>
<span class="s2">sudo mv site.tgz /var/www/</span>
<span class="s2">cd /var/www/</span>
<span class="s2">sudo tar -xvzf /var/www/site.tgz</span>
<span class="s2">sudo rm site.tgz</span>
<span class="s2">sudo chown -R root:root _site</span>
<span class="s2">sudo chmod 0744 _site</span>
<span class="s2">sudo cp -Rf _site/* html/</span>
<span class="s2">&#39;&quot;</span></code></pre></figure>

<p><strong>Notas: </strong>  (Preparación para este ejemplo)
<ul>
    <li><strong><i>backup.sh</i></strong> es un script que simplemente realiza una copia y compresion del contenido renombrandolo con la fecha actual.</li>
    <li>el blog está alojado en la ruta <strong><i>/var/www/html</i></strong></li>
    <li>el usuario que lo ejecuta es <strong><i>root</i></strong></li>
</ul></p>

<p>Saludos.</p>
