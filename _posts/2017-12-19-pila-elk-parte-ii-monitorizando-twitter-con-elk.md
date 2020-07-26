---
layout: single 
title:  "Pila ELK (II): Monitorizando Twiter con ELK"
date:   2017-12-19
---
<p class="intro"><span class="dropcap">P</span>rofundizando un poco más en la pila ELK, vamos a ver algunos plugins de Logstash y como configurarlos para monitorizar una serie de keywords de Twitter.</p>

Vamos a continuar con la parte dos de la entrada anterior <a href="{{2017-12-17-pila-elk.md| prepend: site.baseurl }}">Pila ELK (I): Introducción</a>, y veremos como configurar una serie de plugins de logstash para monitorizar Twitter, almacenar la información en ElasticSearch y mostrar un dashboard en Kibana.

### Requisitos
* ElasticSearch <a href="https://www.elastic.co/products/elasticsearch" target="_blank">+Info</a>
* Logstash <a href="https://www.elastic.co/products/logstash" target="_blank">+Info</a>
* Kibana <a href="https://www.elastic.co/products/kibana" target="_blank">+Info</a>
* Twitter - Apps <a href="https://apps.twitter.com" target="_blank">+Info</a>


### Logstash - Plugins

Los plugins de logstash permiten aumentar la funcionalidad del sistema, así como la integración con otras plataformas, están divididos en los de entrada y los de salida. 

* En el siguiente <a href="https://www.elastic.co/guide/en/logstash/current/input-plugins.html" target="_blank">enlace</a> se puede encontrar un listado de los plugins de entrada. Nosotros vamos a utilizar el plugin de entrada de Twitter, <a href="https://www.elastic.co/guide/en/logstash/current/plugins-inputs-twitter.html" target="_blank">aquí</a> se pueden ver todos los parámetros que se pueden configurar.
* En el siguiente <a href="https://www.elastic.co/guide/en/logstash/current/output-plugins.html" target="_blank">enlace</a> se puede encontrar un listado de los plugins de salida. Nosotros vamos a utilizar el plugin de salida de ElasticSearch, <a href="https://www.elastic.co/guide/en/logstash/current/plugins-outputs-elasticsearch.html" target="_blank">aquí</a> se pueden ver todos los parámetros que se pueden configurar.

### Hands on

Para utilizar el plugin de Twitter se necesita que el usuario se registre en la plataforma y dé de alta una aplicación para poder atacar a la API de Twitter. Una vez tenemos los tokens para poder acceder a la API configuramos el plugin de entrada de logstash de la siguiente forma: 
{% highlight linux-config %}
input {
  twitter {
  	consumer_key => "XXX"
  	consumer_secret => "XXX"
  	oauth_token => "XXX"
  	oauth_token_secret => "XXX"
  	keywords => [ "Keyword1", "Keyword2", "Keyword3" ]
  	full_tweet => true
  	id => "id_plugin"
  }
}
{% endhighlight %}

De esta forma conseguiremos recoger toda la información publicada en Twitter con las Keywords seleccionadas.

A continuación, se necesita configurar el plugin de salida de ElasticSearch para almacenar la información de Twitter. Para ello le definimos los campos que queremos sobreescribir y para ello utilizamos una plantilla, así como la información de los índices que se van a crear. Dicho plugin de salida de logstash lo configuramos de la siguiente forma:
{% highlight linux-config %}
output {
  elasticsearch {
  	hosts => "localhost:9200"
  	index     	=> "twitter_index"
  	document_type => "tweets"
  	template  	=> "./twitter_template.json"
  	template_name => "twitter_template"
  	template_overwrite => true
  }
}
{% endhighlight %}
{% highlight linux-config %}
{
	"index_patterns": "twitter_task3",
	"mappings": {
		"tweets": {
			"properties": {
				"@timestamp": {
					"type": "date",
					"format": "dateOptionalTime"
				},
				"text": {
					"type": "text"
				},
				"user": {
					"type": "object",
					"properties": {
						"description": {
							"type": "text"
						}
					}
				},
				"coordinates": {
					"type": "object",
					"properties": {
						"coordinates": {
							"type": "geo_point"
						}
					}
				}
			}
		}
	}
}
{% endhighlight %}

***Nota***: *Para esta versión no se han utilizado filtros, pero se podrían añadir para mejorar la calidad de la información almacenada en ElasticSearch. <a href="https://www.elastic.co/guide/en/logstash/current/input-plugins.html" target="_blank">Aquí</a> se puede encontrar un listado de los filtros.*

Esta configuración completa se puede ver en el <a href="https://github.com/vimicasa/twitter-elk" target="_blank">repositorio</a> de mi github.

Y finalmente, por sencillez lanzamos el siguiente comando en un terminal y se comenzará a guardar la información publicada en Twitter. Si queremos lanzar la aplicación como servicio se puede ver más información <a href="https://www.elastic.co/guide/en/logstash/current/running-logstash.html" target="_blank">aquí</a>.
{% highlight shell %}
 ../kibana-5.0.1-linux-x86_64/bin$ ./logstash -f ~/twitter_run.config
{% endhighlight %}

## Visualización

Ya se está recogiendo la información de Twitter, por lo que vamos a visualizarla mediante Kibana. Lanzamos la aplicación y accedemos a ella mediante el puerto por defecto 5601.
Vamos a la pestaña de Discover, y seleccionando el filtro "twitter*" y ya podemos ver todos los Tweets que hemos ido almacenando. A continuación se puede observar la estructura en formato JSON de cada Tweet almacenado.

Además del formato plano, Kibana permite analizar los datos de una manera mucho más visual, mediante gráficos como : gráfico de Tarta, gráfico de barras, gráfico de líneas, métricas, tablas entre otros..

Y adicionalmente permite agrupar todos los gráficos anteriores en Dashboards. Así por ejemplo se puede analizar la información recibida según se ve a continuación:

<img src="{{ '/assets/img/posts/dashboard-kibana.png' | prepend: site.baseurl }}" alt="Dashboard Kibana">

<p>Saludos</p>
