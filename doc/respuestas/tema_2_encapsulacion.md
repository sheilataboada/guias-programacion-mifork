# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

En POO, la encapsulación busca agrupar en una misma unidad (la clase) los datos (atributos) y las operaciones (métodos) que trabajan con esos datos. La idea es que el objeto “se gestione a sí mismo”: en lugar de que otras partes del programa toquen directamente sus variables internas, se interactúa mediante métodos que definen cómo se consulta o modifica el estado. En Java esto se apoya con los modificadores de acceso (por ejemplo, declarando atributos como private).

La ocultación de información (information hiding) persigue esconder los detalles internos de implementación y exponer solo una interfaz pública (los métodos que se permiten usar). Así, desde fuera se conoce qué hace el objeto (sus servicios), pero no cómo lo hace internamente ni cómo almacena exactamente sus datos. Esto permite que el interior pueda cambiar sin “romper” el código que lo usa, siempre que se mantenga la interfaz.

Ventajas típicas de la ocultación de información: (1) se reduce el acoplamiento (menos dependencias entre clases), (2) se facilita el mantenimiento y la evolución (se pueden cambiar atributos o algoritmos internos sin afectar a quienes usan la clase), (3) se protege la consistencia del objeto (se pueden validar datos y evitar estados inválidos), y (4) se mejora la reutilización y el diseño (interfaces más claras y código más modu



## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La interfaz pública de una clase en POO es el conjunto de métodos y elementos declarados como public que pueden ser utilizados desde otras clases. Es decir, representa todo aquello que se permite usar desde el exterior del objeto. Desde el punto de vista de quien utiliza la clase, la interfaz pública define qué operaciones se pueden realizar sobre el objeto, sin necesidad de conocer cómo están implementadas internamente.

En Java, la interfaz pública suele estar formada por los métodos públicos (por ejemplo, constructores, getters y setters u otros métodos de servicio). Los atributos normalmente se declaran como private, de modo que no pueden modificarse directamente desde fuera. Así, el acceso a los datos se realiza siempre a través de los métodos definidos en la interfaz pública.

La relación con la ocultación de información es directa: al exponer únicamente la interfaz pública y mantener el resto como private, se esconden los detalles internos de implementación. De esta manera, el funcionamiento interno puede modificarse sin afectar al código que utiliza la clase, siempre que la interfaz pública se mantenga estable.



## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

Se debe diseñar con cuidado la interfaz pública de una clase porque constituye el punto de contacto entre esa clase y el resto del programa. Todo lo que se declare como public podrá ser utilizado por otras clases, por lo que cualquier decisión tomada en ese momento afectará directamente a cómo se usará el objeto. Si la interfaz está mal planteada (por ejemplo, exponiendo datos innecesarios o métodos poco coherentes), se generará un diseño más frágil y difícil de mantener.

La interfaz pública actúa como un “contrato”: define qué operaciones ofrece la clase y cómo deben utilizarse. Una vez que otras partes del programa dependen de ese contrato, modificarlo puede provocar errores en el código que ya lo está utilizando. Por ello, no es recomendable exponer más elementos de los necesarios ni cambiar con frecuencia la parte pública de la clase.

En general, no es fácil cambiar la interfaz pública cuando la clase ya está siendo utilizada. Aunque técnicamente se puede modificar el código, cualquier cambio en los métodos públicos (nombre, parámetros o tipo de retorno) obligará a adaptar todas las clases que los usen. Por este motivo, se recomienda diseñarla con previsión y mantener estables los elementos públicos, dejando los detalles internos como private para poder modificarlos sin afectar al exterior.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
