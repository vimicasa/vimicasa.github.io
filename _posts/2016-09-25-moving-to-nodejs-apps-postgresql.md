---
layout: single 
title:  "Moving to AngularJS - NodeJS - PostgreSQL Applications (I)"
date:   2016-09-25
---

<p class="intro"><span class="dropcap">I</span>nstalación  de PostgreSQL 9.5 en distribuciones basadas en Debian</p>

<p>Últimamente he tenido la oportunidad de adentrarme más en el mundo de Javascript, <a href="https://nodejs.org">NodeJS</a> y <a href="https://angularjs.org/">AngularJS</a> y como base de datos <a href="https://www.postgresql.org/">PostgreSQL</a>, he de decir que al principio era muy reticente a todo ello, sin embargo al final hay que aceptar que es un mundo que está muy chulo. Javascript tiene una potencia inabarcable y te da una gran versatilidad para poder realizar todos los requisitos, orientacion a objetos, prototipado..etc Aquí os dejo una <a href="https://www.youtube.com/playlist?list=PLeHi8rVLGcYZMVCwqN2-XN8qrhWZhM1l7">lista de videos</a> que encontré publicados por <strong>Javier Moreno</strong> en youtube que como punto de partida desde el nivel básico da una gran visión de todo lo que se puede llegar a hacer.</p>

<p>Hoy me quiero centrar en la base de datos PostgreSQL 9.5 (Actualmente acaba de publicarse la versión 9.6 <a href="https://www.postgresql.org/docs/9.6/static/release-9-6.html"><i>Relesase Notes</i></a>), es una base de datos <i>open-source</i> altamente escalable y que nos permite utilizarla como base de datos relacional pero con soporte para JSON binario (JSONB).</p>

<h3>Instalación</h3>

<p>Hay que añadir el repositorio al listado de apt. </p>

<figure class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="c"># Añadir el repositorio</span>
<span class="nv">$ </span>sudo sh -c <span class="s1">&#39;echo &quot;deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main 9.5&quot; &gt;&gt; /etc/apt/sources.list.d/pgdg.list&#39;</span>

<span class="c"># Importar la clave del repositorio</span>
<span class="nv">$ </span>sudo apt-get install wget ca-certificates
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc <span class="p">|</span> sudo apt-key add -</code></pre></figure>

<p>Después de añadirlo el repositorio actualizamos la lista de paquetes.</p>

<figure class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="nv">$ </span>sudo apt-get update</code></pre></figure>

<p>Ejecutamos la instalación</p>

<figure class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="nv">$ </span>sudo apt-get install postgresql-9.5</code></pre></figure>

<p>El proceso de instalacion creará un usuario y un grupo con el nombre <i><strong>postgres</strong></i> dentro de la base de datos y en el sistema Linux. Este usuario tendrá permisos de administrador en la base de datos. Por lo que en primer lugar deberemos conectarnos con este usuario para testear la instalación y acceder a la consola de psql</p>

<figure class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="c"># Log in &quot;postgres&quot;</span>
<span class="nv">$ </span>sudo su - postgres

<span class="c"># Iniciar la consola psql</span>
<span class="nv">$ </span>psql

<span class="c"># Comprobar la conexion</span>
postgres-# <span class="se">\c</span>onninfo

<span class="c"># Desconectar o &lt;Ctrl&gt;+D</span>
postgres-# <span class="se">\q</span> </code></pre></figure>

<h3>Configuración</h3>

<p>La base de datos se ha instalado en la ubicacion <strong>/etc/postgresql/9.5</strong>. 
Y dentro de este directorio los archivos principales de configuracion son:</br>
- /etc/postgresql/9.5/main/postgresql.conf</br>
- /etc/postgresql/9.5/main/pg_hba.conf   =&gt;  <a href="https://www.postgresql.org/docs/9.5/static/auth-pg-hba-conf.html">Documentación</a></br></p>

<p>Dentro de <strong>postgresql.conf</strong> podemos adecuar los siguientes parámetros para optimizar según nuestro sistema.</p>

<figure class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="c"># Direccion desde la que se puede acceder</span>
<span class="nv">listen_addresses</span> <span class="o">=</span> ‘127.0.0.1’ <span class="c"># or ‘*’</span>

<span class="c">#Por defecto 100</span>
<span class="nv">max_connections</span> <span class="o">=</span> 100

<span class="c">#25% total RAM, más de 4GB no suele ayudar mucho</span>
<span class="nv">shared_buffers</span> <span class="o">=</span> 1GB

<span class="c">#75% total RAM</span>
<span class="nv">effective_cache_size</span> <span class="o">=</span> 24GB

<span class="c">#24MB para 16GB RAM</span>
<span class="nv">work_mem</span> <span class="o">=</span> 24MB</code></pre></figure>

<p>Más adelante volveré con el resto de partes de NodeJS y AngularJS.</p>

<p>Saludos.</p>
