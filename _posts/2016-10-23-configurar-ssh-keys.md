---
layout: single 
title:  "Como configurar claves SSH para acceder a tus servidores Linux"
date:   2016-10-23
---
<p class="intro"><span class="dropcap">V</span>amos a ver como configurar las claves públicas en nuestro servidor para poder acceder sin contraseñas</p>

<p>Para poder autenticarte en tu servidor hay dos posibilidades:</p>

<ul>
<li>Utilizar una contraseña</br></li>
<li>Utilizar una clave pública.</br></li>
</ul>

<p>En este artículo nos vamos a centrar en la segunda. Este método es el recomendado en VPS, servidores Cloud y creo ciertamente que es un método que añade un poco capa más de seguridad. Así que manos a la obra</p>

<h3>Creación de las claves</h3>

<p>Generaremos el par de claves pública (que se pondrá en nuestro servidor) y privada (sólo conoceremos nosotros) mediante rsa y un tamaño de 4096 bits. En el proceso nos pedirá donde queremos almacenar las claves generadas, si no tenemos ninguna otra clave el seleccionamos el PATH por defecto. Después nos pedirá que ingresemos un passphrase, introduciremos un passphrase lo más complejo que podamos que sea fácil de recordar.</p>

<figure class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="c"># Generación de las claves</span>
<span class="nv">$ </span>ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key <span class="o">(</span>/home/vcatalan/.ssh/id_rsa<span class="o">)</span>: 
Enter passphrase <span class="o">(</span>empty <span class="k">for</span> no passphrase<span class="o">)</span>: 
Enter same passphrase again: 
Your identification has been saved in /home/vcatalan/.ssh/id_rsa.
Your public key has been saved in /home/vcatalan/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:cHbsqJcymEUn81H2rxhvLvfxnse3ygAIrJ34QQHNx78 vcatalan@lap
The key<span class="err">&#39;</span>s randomart image is:
+---<span class="o">[</span>RSA 4096<span class="o">]</span>----+
<span class="p">|</span>   .+..   o      <span class="p">|</span>
<span class="p">|</span>    .o.o + .     <span class="p">|</span>
<span class="p">|</span>     +* * o .    <span class="p">|</span>
<span class="p">|</span>    <span class="o">=</span>.oO.*   .   <span class="p">|</span>
<span class="p">|</span>   o +..S.+   .  <span class="p">|</span>
<span class="p">|</span>    .+.. E.+ .   <span class="p">|</span>
<span class="p">|</span>    o.+ o ..+ .. <span class="p">|</span>
<span class="p">|</span>       +  .o+  <span class="nv">o</span><span class="o">=</span><span class="p">|</span>
<span class="p">|</span>           o.+o+*<span class="p">|</span>
+----<span class="o">[</span>SHA256<span class="o">]</span>-----+

<span class="c"># Añadir a la lista de ssh-agent</span>
<span class="nv">$ </span>ssh-add</code></pre></figure>

<p>Tras rellenar los pasos obtenemos los dos ficheros:</p>

<ul>
<li>Your identification has been saved in /home/vcatalan/.ssh/id_rsa.</br></li>
<li>Your public key has been saved in /home/vcatalan/.ssh/id_rsa.pub.</br></li>
</ul>

<p>Nota: Para evitar que nos pida el passphrase en la sesión ejecutamos el último comando ssh-add.</p>

<h3>Instalación en el servidor</h3>

<p>Para instalarlo se puede hacer por varias herramientas, pero puede que no esten instaladas en el equipo por lo que utilizaremos únicamente herramientas que vienen por defecto.</p>

<figure class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="c"># Creamos el directorio .ssh en el servidor remoto</span>
<span class="nv">$ </span>ssh user@remote.server mkdir .ssh

<span class="c"># Desde el directorio la clave pública</span>
<span class="c"># Copiamos al servidor</span>
~/.ssh <span class="nv">$ </span>cat id_rsa.pub <span class="p">|</span> ssh user@remote.server <span class="s1">&#39;cat &gt;&gt; .ssh/authorized_keys&#39;</span>
user@remote.server<span class="s1">&#39;s password: </span>

<span class="s1"># Seguridad: Actualizar los permisos de las carpetas en el servidor</span>
<span class="s1">~/.ssh $ ssh user@remote.server &#39;</span>chmod <span class="m">700</span> .ssh<span class="p">;</span> chmod <span class="m">640</span> .ssh/authorized_keys<span class="err">&#39;</span></code></pre></figure>

<p>Si se ha instalado correctamente, habrás podido comprobar que el último comando ya lo has podido ejecutar sin haber introducido la contraseña, y ya lo tienes configurado.</p>

<p>A continuación deshabilitaremos el acceso por contraseña al servidor, que está configurado en <strong>/etc/ssh/sshd_config</strong></p>

<figure class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="c"># Editamos el archivo</span>
<span class="nv">$ </span>sudo vi /etc/ssh/sshd_config

<span class="c"># Evita que se pueda hacer login con root</span>
<span class="c"># NOTA: ASEGURATE DE TENER UN USARIO ADMINISTRADOR</span>
<span class="c"># O PERDERAS EL ACCESO ROOT A TU SERVIDOR</span>
PermitRootLogin no

<span class="c"># Evita que se pueda hacer login con los usuarios</span>
PasswordAuthentication no 

<span class="c"># Reinicio del demonio sshd</span>
<span class="nv">$ </span>sudo service ssh restart</code></pre></figure>

<p><strong>¡Enhorabuena!</strong> Ya lo tienes configurado. Sólo tienes que proteger esa clave privada en algún sitio seguro.</p>

<p>Saludos.</p>
