---
layout: single 
title:  "Spring Framework: Transactional Event Listener"
date:   2016-02-27
---

<p class="intro"><span class="dropcap">D</span>esarrollo de aplicaciones Java con Spring Framework y sus listener de eventos.</p>

En el mundo del desarrollo de software en el lenguaje Java, es casi un estandar el uso del framework <a href="https://spring.io/">Spring</a>. Su eslogan es un hecho <strong><i>"Spring helps development teams everywhere build simple, portable,
fast and flexible JVM-based systems and applications."</i></strong>.

Dentro de Spring hay numerosos modulos que se pueden incorporar a nuestras aplicaciones. Todos ellos nos facilitan algunas situaciones comunes. Yo he utilizado los siguientes:
<ul>
	<li>Spring Boot</li>
	<li>Spring Framework</li>
	<li>Spring Data</li>
	<li>Spring Integration</li>
	<li>Spring Batch</li>
	<li>Spring Security</li>
	<li>Spring Web Services</li>
	<li>Spring MVC</li>
</ul>
	
<a href="https://spring.io/projects">Aquí</a> podeis encontrar un listado de todos los proyectos dentro de Spring. 

Después de esta introducción para los que no conozcan este framework, quería comentar una nueva funcionalidad que han introducido en la versión 4.2 que he tenido que utilizar esta semana. Una nueva anotación que se puede utilizar <strong>@TransactionalEventListener</strong>. 

Esta mejora de los listener de eventos que han introducido es la habilidad de linkar el listener de un evento dependiendo de la fase de la transacción que nos interese. 

<strong>@TransactionalEventListener</strong> se utiliza igual que la anterior anotación <strong>@EventListener</strong> y además nos permite controlar la transacción. Por defecto se  inicializará el listener tras realizarse el commit <strong>AFTER_COMMIT</strong>, pero también se puede utilizar en las siguientes fases (<strong>BEFORE_COMMIT</strong>, <strong>AFTER_ROLLBACK</strong> and <strong>AFTER_COMPLETION</strong>).

Nota: Si no hay transacción el evento no se lanzará por defecto, a no ser que se haya anotado el método con el atributo <strong>"fallbackExecution"</strong>.

Un ejemplo de como utilizarlo, sencillamente a continuación:

{% highlight java %}
@Component
public class MyComponent {
  
  @TransactionalEventListener
  public void handleCreatedEvent(CreationEvent<Order> creationEvent) { 
    ...
  }

}
{% endhighlight %}

Saludos.






