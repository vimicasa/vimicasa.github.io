---
layout: single 
title:  "Installing VirtualBox 5.1"
date:   2016-08-07
---

 <p class="intro"><span class="dropcap">I</span>nstalando Virtualbox 5.1 en Linux Mint 18, y como resolver la dependencia de libvpx1.</p>

<p>Estaba preparando un laboratorio para adentrarme en el mundo de <a href=https://www.docker.com/>Docker</a> [Si alguno no lo conoce debería meterse en la página y empezar a conocerlo!!]. Lo primero que se suele hacer en estos casos es instalarte una máquina virtual para trastear en ella. Debido a una reorganización formateé mi equipo e instalé Linux Mint 18 que lo liberaron el mes pasado (Estoy muy contento con él desde que me lo enseñaron) y aún no había sentido la necesidad de utilizar Virtualbox.</p>

<p>Resumiendo.. que me enrollo!!! En el proceso de instalación he obtenido el siguiente error <strong><i>virtualbox-5.1 : Depends: libvpx1 (&gt;= 1.0.0) </i></strong>. </p>

<p>Aggrrr!!! Menos mal que simplemente es que para ejecutar Virtualbox se necesita añadir una dependencia.</p>

<p>Después de esta introducción simplemente esto son los comandos que he ejecutado para poder instalarlo, por si a alguien más le pasa pueda resolverlo rápidamente.</p>

<figure class="highlight"><pre><code class="language-csh" data-lang="csh"><span class="c"># Descargar la librería necesaria</span>
<span class="nv">$ </span>wget http://security.ubuntu.com/ubuntu/pool/main/libv/libvpx/libvpx1_1.3.0-3ubuntu1_amd64.deb

<span class="c"># Instalar el paquete deb</span>
<span class="nv">$ </span>sudo gdebi libvpx1_1.3.0-3ubuntu1_amd64.deb</code></pre></figure>

<p>Y ahora que tenemos la dependencía instalada seguimos los pasos que podemos encontrar en la página oficial de descargas <a href="https://www.virtualbox.org/wiki/Linux_Downloads">Virtualbox</a>.</p>

<figure class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="nv">$ </span>wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- <span class="p">|</span> sudo apt-key add -
<span class="nv">$ </span>wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- <span class="p">|</span> sudo apt-key add -

<span class="nv">$ </span>sudo apt-get update
<span class="nv">$ </span>sudo apt-get install virtualbox-5.1</code></pre></figure>

<p>Saludos.</p>

