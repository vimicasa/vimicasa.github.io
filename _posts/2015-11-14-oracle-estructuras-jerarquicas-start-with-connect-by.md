---
layout: single 
title:  "SQL en estructuras jerárquicas con Oracle"
date:   2015-11-14
---
<p class="intro"><span class="dropcap">S</span>QL para estructuras jerárquicas con bases de datos Oracle. START WITH .. CONNECT BY ..</p>

Últimamente he estado trabajando con estructuras jerárquicas de datos, en conjunción con la base de datos Oracle. Para trabajar más cómodamente existe una clausula "<strong>START WITH </strong> .. <strong>CONNECT BY </strong> .." en el lenguaje SQL.

La sintaxis para su uso es: 
{% highlight sql %}
SELECT .. 
[START WITH condicion_inicial] 
CONNECT BY [NOCYCLE] condicion_recursiva;
{% endhighlight %}	

<strong>START WITH </strong> : Especifica la raíz o fila / condición inicial desde la que comienza la estructura. <br>
<strong>CONNECT BY </strong>: Especifica la relación de jerarquía entre los elementos Padre - Hijo.<br>
<strong>NOCYCLE</strong>: Parámetro opcional que permite el correcto funcionamiento en caso de que exista un bucle en los datos jerárquicos.<br>
	
Para generar la condición recursiva es necesario el uso del operador <strong><i>PRIOR</i></strong> que internamente hace referencia al elemento padre. Si se quiere recorrer la estructura jerárquica de Padre a Hijos hay que usarlo <em> <strong>PRIOR</strong> `expresion_padre` = `expresion_hijo` </em>, y si por lo contrario se quiere recorrer la estructura de Hijos a Padres hay que usarlo <i> <strong>PRIOR</strong> `expresion_hijo` = `expresion_padre` </i>.

Además de la función anterior, desde la versión de Oracle 9i, se introdujo un nuevo operador <strong>`SYS_CONNECT_BY_PATH`</strong> y de esta forma permite mostrar el camino entero desde el Padre al Hijo.<br>
La sintaxis es: 
{% highlight sql %}
SYS_CONNECT_BY_PATH( campo, 'caracter_separador' )
{% endhighlight %}	

Y desde la versión de Oracle 10g otro nuevo operador <strong>`CONNECT_BY_ROOT`</strong> que permite mostrar la raiz de la estructura jerárquica.
La sintaxis es: 
{% highlight sql %}
CONNECT_BY_ROOT ( campo )
{% endhighlight %}	


<span class="dropcap">Ejemplos</span>
<br> <br>
Dada la siguiente estructura del sistema de ficheros UNIX:

`(id = 1)__ /raiz						`<br>
`(id = 2)__|__folder A 				`<br>
`(id = 4)__|  |__folder A.1			`<br>
`(id = 7)__|	 |  |__file 1			`<br>
`(id = 8)__|	 |  |__file 2			`<br>
`(id = 3)__|__folder B 				`<br>
`(id = 5)_____|__folder B.1		`<br>
`(id = 6)________|__folder B.1.1		`<br>
`(id = 9)___________|__file 3			`<br>

 
Almacenada en una tabla <strong>FILESYSTEM</strong> (ID,PARENT_ID,NAME);
{% highlight sql %}
	SELECT ID, SYS_CONNECT_BY_PATH(NAME, '|')
	FROM FILESYSTEM fs
	START WITH fs.PARENT_ID = 0
	CONNECT BY PRIOR fs.ID = fs.PARENT_ID;
{% endhighlight %}	
`	1	|/raiz`<br>
`	2	|/raiz|folder A`<br>
`	4	|/raiz|folder A|folder A.1`<br>
`	7	|/raiz|folder A|file 1`<br>
`	8	|/raiz|folder A|file 2`<br>
`	3	|/raiz|folder B`<br>
`	5	|/raiz|folder B|folder B.1`<br>
`	6	|/raiz|folder B|folder B.1|folder B.1.1`<br>
`	9	|/raiz|folder B|folder B.1|folder B.1.1|file 3`<br>

{% highlight sql %}	
	SELECT ID, SYS_CONNECT_BY_PATH(NAME, '|')
	FROM FILESYSTEM fs
	START WITH fs.PARENT_ID = 2
	CONNECT BY PRIOR fs.ID = fs.PARENT_ID;
{% endhighlight %}	
`	4	|folder A.1`<br>
`	2	|folder A.1|folder A`<br>
`	1	|folder A.1|folder A|/raiz`<br>
`	7	|file 1`<br>
`	2	|file 1|folder A`<br>
`	1	|file 1|folder A|/raiz`<br>
`	8	|file 2`<br>
`	2	|file 2|folder A`<br>
`	1	|file 2|folder A|/raiz`<br>
	
{% highlight sql %}		
	SELECT ID, PARENT_ID, CONNECT_BY_ROOT(NAME)
	FROM FILESYSTEM fs
	START WITH fs.PARENT_ID = 0
	CONNECT BY PRIOR fs.ID = fs.PARENT_ID;
{% endhighlight %}	
`	1	/raiz`<br>
`	2	/raiz`<br>
`	4	/raiz`<br>
`	7	/raiz`<br>
`	8	/raiz`<br>
`	3	/raiz`<br>
`	5	/raiz`<br>
`	6	/raiz`<br>
`	9	/raiz`<br>

{% highlight sql %}		
	SELECT ID, PARENT_ID, CONNECT_BY_ROOT(NAME)
	FROM FILESYSTEM fs
	START WITH fs.PARENT_ID = 1
	CONNECT BY PRIOR fs.ID = fs.PARENT_ID;
{% endhighlight %}	
`	2	folder A`<br>
`	4	folder A`<br>
`	7	folder A`<br>
`	8	folder A`<br>
`	3	folder B`<br>
`	5	folder B`<br>
`	6	folder B`<br>
`	9	folder B`<br>
	

Espero que os sirva. Nos vemos pronto!
