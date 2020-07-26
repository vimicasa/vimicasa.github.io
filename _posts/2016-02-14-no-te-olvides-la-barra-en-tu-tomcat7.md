---
layout: single 
title:  "No te olvides de poner la barra en tu Tomcat 7"
date:   2016-02-14
---
<p class="intro"><span class="dropcap">N</span>o te olvides la barra, e investiga hasta encontrar el porqué de todo y comprender el funcionamiento. El conocimiento te llevará a desarrollar tus cualidades y subir a un siguiente nivel. </p>

Buenas a todos,

Esta vez quería contaros una anécdota que me ha ocurrido esta semana y me hizo "perder" un rato investigando y lo más importante en la búsqueda del conocimiento, el POR QUÉ de las cosas. Actualmente como servidor de desarrollo en local en mi equipo trabajamos con <a href="http://tomcat.apache.org/">Apache Tomcat</a> en su versión 7.

Hasta ahora estaba trabajando con la version 7.0.41 y tras iniciar el servidor y acceder al contexto de la aplicación de la siguiente forma :
<i><strong>http://localhost:8080/example</strong></i> no tenía ningún problema ya que el servidor internamenta le añadía la raíz <strong>/</strong> al final, te presentaba la pantalla de inicio y tras introducir las credenciales de usuario te llevaba a la home. Todo Correcto!!!

Esta semana por un cambio de Sistema Operativo, (por fin me he instalado en el entorno de trabajo una distribucion de Linux, más concretamente <a href='http://www.linuxmint.com/'>Linux Mint (17.3)</a>, han hecho un gran trabajo y estoy muy contento con este cambio!! Os lo recomiendo a todos!) he reinstalado todas las aplicaciones y me descargué la última version de Tomcat 7 versión 7.0.67 para desarrollar. Y en mi trabajo diario volví a introducir la misma URL <strong>http://localhost:8080/example</strong>, sin embargo está vez no me redirigió a la raiz sino que me redirigió a la pantalla de index.html ( que parecía ser la misma que en el caso anterior). Al introducir las credenciales de usuario para sorpresa mia no me llevó a la home y me volvió a aparecer la página de inicio otra vez sin ningún tipo de error. (WTF!!) Abré introducido mal las credenciales pensé (Estoy todavía dormido) y volví a introducirlas y mme ocurrió lo mismo. Mmm...algo está pasando.

Cambié el nivel de LOG de la aplicación para que mostrará toda la información posible, y mediante "greps" encontré que el servidor estaba destruyendo la sesión actual y estaba creando una nueva al introducir las credenciales. Usamos Spring Security para el proceso de autenticación, por lo que debugando el proceso llegué donde controlaba el flujo y ví el porqué. En la primera "redirección" se guarda en la sesión que el destino es index.html, entonces como internamente el servidor crea una nueva sesión, tras introducir las credenciales se produce un error de sesión y te redirige a la URL almacenada "index.html" entrando en bucle.

Esto antes no pasaba, ya que el módulo de autenticación está probado y reprobado..entonces.. que ha podido cambiar?? Y me acordé de la versión del Tomcat y fui directo a revisar el <a href="https://tomcat.apache.org/tomcat-7.0-doc/changelog.html">ChangeLog</a> de las modificaciones introducidas en las versiones, ya que por experiencia, los cambios de tipo minor pueden hacer fallar una aplicación. Busqué la palabra clave "redirects" y ahi estaba:

<i>Move the functionality that provides redirects for context roots and directories where a trailing / is added from the Mapper to the DefaultServlet. This enables such requests to be processed by any configured Valves and Filters before the redirect is made. This behaviour is configurable via the mapperContextRootRedirectEnabled and mapperDirectoryRedirectEnabled attributes of the Context which may be used to restore the previous behaviour. (markt)</i>

El servidor había cambiado la forma interna de manejar las redirecciones para directorios, y en conjunción con los filtros de Spring Security me habían llevado a un bucle del que no consegía salir. Esto me permitió estar indagando en las tripas del Tomcat y la manera de gestionar las redirecciones. 

<i><strong>En definitiva en la informática todo tiene un por qué, y lo que ocurre es que nosotros aun no lo sabemos y solo hace falta darle explicación.</strong><i>

Así que con esta anécdota terminamos este post.. "No te olvides la barra!!". 

Un saludo