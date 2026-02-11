<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
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

Las invariantes de clase son condiciones o reglas que deben cumplirse siempre para que un objeto se encuentre en un estado válido. Se trata de propiedades que deben mantenerse verdaderas durante toda la vida del objeto, excepto quizá en momentos internos muy controlados (por ejemplo, mientras se está ejecutando un método). Por ejemplo, si una clase representa una cuenta bancaria, una invariante podría ser que el saldo nunca sea negativo.

Estas invariantes forman parte del diseño lógico de la clase, aunque no siempre estén escritas explícitamente en el código. Definen qué significa que el objeto esté “correcto” o “coherente”. Si en algún momento se incumplen, el objeto puede comportarse de forma inesperada o producir errores en el programa.

La ocultación de información ayuda a mantener las invariantes porque impide que otras clases modifiquen directamente los atributos internos. Al declarar los atributos como private y permitir su modificación únicamente a través de métodos públicos, se puede comprobar y validar que cualquier cambio respete las condiciones establecidas. De esta manera, se protege la coherencia interna del objeto y se evita que desde el exterior se introduzcan estados inválidos.



## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

A continuación se muestra un ejemplo sencillo de una clase Punto con dos coordenadas x e y de tipo double, aplicando ocultación de información mediante atributos private:

public class Punto {

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAlOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }
}

En este ejemplo, la interfaz pública de la clase está formada por el constructor Punto(double x, double y), el método calcularDistanciaAlOrigen() y los métodos getX() y getY(). Estos son los únicos elementos accesibles desde otras clases, es decir, lo que se puede utilizar externamente para interactuar con el objeto.

La palabra clave public indica que el elemento puede ser accedido desde cualquier otra clase del programa. En cambio, private significa que el atributo solo puede utilizarse dentro de la propia clase. Gracias a esto, las coordenadas x e y no pueden modificarse directamente desde fuera, lo que permite controlar cómo se accede a los datos y mantener la coherencia interna del objeto.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores de acceso public y private se pueden aplicar principalmente a clases, atributos y métodos. Cuando se aplican a un atributo o a un método dentro de una clase, determinan desde dónde se puede acceder a ese elemento. Por ejemplo, un atributo declarado como private solo puede utilizarse dentro de la propia clase, mientras que si es public podrá ser accedido desde otras clases.

En el caso de las clases, el modificador public indica que la clase puede ser utilizada desde cualquier otro paquete del programa. Si no se especifica ningún modificador, la clase tiene visibilidad por defecto (accesible solo dentro del mismo paquete). Sin embargo, una clase de nivel superior no puede declararse como private; el modificador private se utiliza únicamente para elementos internos de una clase (atributos, métodos o clases internas).

Por tanto, public se emplea para definir la parte visible o accesible de una clase, mientras que private se usa para ocultar detalles internos. Esta distinción es fundamental para aplicar correctamente la encapsulación y controlar qué partes del programa pueden interactuar con cada elemento.




## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

En POO, además de la visibilidad pública y privada, existen otros niveles de acceso según el lenguaje. La idea general es controlar desde dónde se puede acceder a una clase, atributo o método. No todos los lenguajes ofrecen exactamente los mismos modificadores, pero la mayoría incorporan algún mecanismo intermedio entre lo totalmente público y lo totalmente privado.

En Java, además de public y private, existen otros dos niveles: acceso por defecto (package-private) y protected. El acceso por defecto se aplica cuando no se escribe ningún modificador y permite el uso del elemento solo dentro del mismo paquete. El modificador protected permite el acceso desde el mismo paquete y también desde clases que hereden de esa clase (aunque la herencia todavía no se haya estudiado en profundidad). Por tanto, en Java hay cuatro niveles de visibilidad.

En otros lenguajes, como C++, también existen varios niveles de acceso, como public, private y protected, con un funcionamiento similar. La finalidad en todos los casos es la misma: controlar la visibilidad para aplicar correctamente la encapsulación y limitar el acceso a los detalles internos de las clases.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

En Java, los miembros de instancia declarados como private están ocultos para (a) otras clases, pero no para (b) otras instancias de la misma clase. Esto significa que el acceso private se restringe a la clase, no al objeto concreto. Por tanto, cualquier método definido dentro de la misma clase puede acceder a los atributos privados de otro objeto del mismo tipo.

A continuación se muestra la clase Punto ampliada con el método calcularDistanciaAPunto(Punto otro):

public class Punto {

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAlOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double diferenciaX = this.x - otro.x;
        double diferenciaY = this.y - otro.y;
        return Math.sqrt(diferenciaX * diferenciaX + diferenciaY * diferenciaY);
    }
}

En el método calcularDistanciaAPunto, se accede directamente a otro.x y otro.y, aunque esos atributos son private. Esto es posible porque el acceso se realiza desde dentro de la misma clase Punto. Sin embargo, si se intentara acceder a x o y desde una clase distinta, el compilador produciría un error. Por tanto, la ocultación private protege frente a otras clases, pero no impide que los objetos de la misma clase accedan entre sí a sus miembros privados.



## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

En los lenguajes orientados a objetos, los métodos getter y setter son métodos públicos que permiten acceder y modificar los atributos de una clase cuando estos están declarados como private. Su finalidad es aplicar la encapsulación: en lugar de permitir el acceso directo a los datos, se controla ese acceso mediante métodos.

Un getter es un método que devuelve el valor de un atributo. Normalmente no recibe parámetros y su nombre suele comenzar por get, por ejemplo getEdad(). Un setter es un método que permite asignar un nuevo valor a un atributo; suele recibir un parámetro y su nombre comienza por set, como setEdad(int edad). De esta forma, el atributo sigue siendo privado, pero se puede consultar o modificar de manera controlada.

La ventaja de utilizar getters y setters es que permiten validar los datos antes de modificarlos o aplicar reglas internas sin que el exterior lo perciba. Por ejemplo, en un setter se podría comprobar que un valor no sea negativo antes de asignarlo. Así, se mantiene la coherencia del objeto y se respeta la ocultación de información.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

Cuando se afirma que la ocultación de información mejora la “seguridad” del programa, no se está haciendo referencia a que el programa no pueda ser hackeado en el sentido de seguridad informática externa. La encapsulación no protege frente a ataques de red, accesos no autorizados al sistema o vulnerabilidades de ese tipo.

En este contexto, la “seguridad” se refiere a la seguridad interna del diseño del software. Es decir, se evita que otras partes del programa modifiquen directamente los datos internos de una clase y los dejen en un estado inconsistente. Al declarar los atributos como private y permitir su modificación únicamente a través de métodos controlados, se reducen los errores y se protege la coherencia del objeto.

Por tanto, la mejora de la seguridad consiste en aumentar la robustez y fiabilidad del código. Se limita el acceso indebido a los detalles internos y se obliga a utilizar la interfaz pública prevista por el diseñador de la clase. Esto contribuye a que el programa sea más estable y fácil de mantener, pero no implica protección frente a ataques externos.



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
