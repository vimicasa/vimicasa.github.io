---
layout: single 
title:  "Primer commit en github"
date:   2016-04-03
---

<p class="intro"><span class="dropcap">M</span>i primer commit en github en mi cuenta <i>vimicasa</i>.</p>

La verdad que como el título indica, he realizado mi primer commit en mi cuenta de <a href="https://github.com">github</a>, llevo bastante tiempo registrado pero no me había decidido a utilizarla. Podéis encontrar en el pie de página un enlace a mi cuenta pulsando sobre el icono de github. Mi cuenta de github es: <a href="https://github.com/vimicasa">https://github.com/vimicasa</a>.


De momento he creado dos repositorios públicos: uno llamado <strong><i>codes</i></strong> y otro <strong><i>servers</i></strong>.  En los que iré organizando trocitos de código y configuraciones que me sirvan para el futuro en nuevos desarrollos como acceso rápido desde cualquier sitio.

Y por comentar mi primer commit, ha sido en la parte de <strong><i>codes/Java</i></strong> en el que se invoca a un procedimiento almacenado en la base de datos desde un repositorio de JPA definido en una entidad que representa una tabla mediante anotaciones.


{% highlight java %}


@NamedStoredProcedureQuery(name = "procedureNameJava", procedureName = "PROCEDURE_NAME_BD", parameters = {
    @StoredProcedureParameter(mode = ParameterMode.IN, name = "param1", type = Long.class),
    @StoredProcedureParameter(mode = ParameterMode.IN, name = "param2", type = String.class),
    @StoredProcedureParameter(mode = ParameterMode.IN, name = "param3", type = String.class)
})

@Entity
@Table (name="TABLE_1"
public class Entity implements Serializable {
  
    private static final long serialVersionUID = 1L;

        ...
    
}

/*----*/

public interface EntityRepository extends JpaRepository<Entity, Long> {

  @Procedure(name = "procedureNameJava")
      void executeAOprocedure(@Param("param1") Long param1, @Param("param2") String param2); 
}


{% endhighlight %}

Saludos.

