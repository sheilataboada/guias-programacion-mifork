# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

En POO, la encapsulación busca agrupar en una misma unidad (la clase) los datos (atributos) y las operaciones (métodos) que trabajan con esos datos. La idea es que el objeto “se gestione a sí mismo”: en lugar de que otras partes del programa toquen directamente sus variables internas, se interactúa mediante métodos que definen cómo se consulta o modifica el estado. En Java esto se apoya con los modificadores de acceso (por ejemplo, declarando atributos como private).

La ocultación de información (information hiding) persigue esconder los detalles internos de implementación y exponer solo una interfaz pública (los métodos que se permiten usar). Así, desde fuera se conoce qué hace el objeto (sus servicios), pero no cómo lo hace internamente ni cómo almacena exactamente sus datos. Esto permite que el interior pueda cambiar sin “romper” el código que lo usa, siempre que se mantenga la interfaz.

Ventajas típicas de la ocultación de información: (1) se reduce el acoplamiento (menos dependencias entre clases), (2) se facilita el mantenimiento y la evolución (se pueden cambiar atributos o algoritmos internos sin afectar a quienes usan la clase), (3) se protege la consistencia del objeto (se pueden validar datos y evitar estados inválidos), y (4) se mejora la reutilización y el diseño (interfaces más claras y código más modular).


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

``` java

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
```

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

``` java

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
```

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

Un miembro de instancia es aquel que pertenece a cada objeto concreto creado a partir de una clase. Cada vez que se crea un objeto, se generan sus propios atributos y métodos de instancia, que pueden almacenar valores distintos en cada objeto. En cambio, un miembro de clase (declarado con la palabra clave static en Java) pertenece a la clase en sí misma y no a un objeto concreto. Esto implica que existe una única copia compartida por todas las instancias.

Por ejemplo, un atributo de instancia como double x; tendrá un valor diferente en cada objeto Punto. Sin embargo, un atributo declarado como static int contador; será común para todos los objetos de esa clase. Los métodos static también se invocan sobre la clase y no requieren crear un objeto previamente.

Los miembros de clase también se pueden ocultar utilizando modificadores de acceso como private. Un atributo o método static private solo podrá utilizarse dentro de la propia clase. Por tanto, tanto los miembros de instancia como los de clase pueden formar parte de la interfaz pública (public) o permanecer ocultos (private), aplicándose igualmente el principio de encapsulación.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí, tiene sentido que los constructores sean private en determinados casos. Si un constructor es privado, no se pueden crear objetos de esa clase desde fuera de ella utilizando new. Esto permite controlar o restringir la creación de instancias, algo que puede ser útil en ciertos diseños.

Por ejemplo, puede interesar impedir que se creen objetos libremente y forzar que la clase proporcione un método público específico para obtener instancias. También puede utilizarse cuando la clase solo contiene métodos static y no tiene sentido crear objetos de ella. En ese caso, declarar el constructor como private evita instanciaciones accidentales.

Por tanto, un constructor privado es una herramienta de control dentro del diseño de la clase. No es lo habitual en clases sencillas, pero puede resultar útil cuando se desea limitar cómo y cuándo se crean los objetos, reforzando así la encapsulación y el control sobre la clase.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

En Java, los miembros de clase se indican utilizando la palabra clave static. Esto significa que el atributo o método pertenece a la clase en sí misma y no a cada objeto individual. Por tanto, existe una única copia compartida por todas las instancias. Se accede a ellos normalmente mediante el nombre de la clase (por ejemplo, Punto.maxX).

A continuación se muestra una versión ampliada de la clase Punto, donde se incluyen miembros static que almacenan los valores máximos de x e y creados hasta el momento:

``` java
public class Punto {

    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;

        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}
```
En este ejemplo, maxX y maxY son atributos de clase (static) que guardan el mayor valor de x e y observado en todos los objetos creados. Cada vez que se construye un nuevo Punto, el constructor actualiza esos máximos si corresponde. Los métodos getMaxX() y getMaxY() también son static, ya que consultan información común a toda la clase y no a un objeto concreto. De esta forma, se distingue claramente entre miembros de instancia (propios de cada objeto) y miembros de clase (compartidos por todos).


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
``` java
public static Punto crearPuntoRedondeado(double x, double y) {
    double xRedondeado = Math.round(x);
    double yRedondeado = Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}
```
Sí, se ha utilizado static. Un método factoría suele declararse como miembro de clase porque su función es crear objetos, y para ello no es necesario disponer previamente de una instancia. Al ser static, puede invocarse directamente sobre la clase, por ejemplo: Punto.crearPuntoRedondeado(2.7, 3.4);.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

Para cambiar la implementación interna sin modificar la interfaz pública, se pueden sustituir los dos atributos double x y double y por un array interno de dos posiciones. Desde el exterior, la clase seguirá ofreciendo los mismos constructores y métodos públicos, por lo que el código que la utilice no tendrá que cambiar.

 ``` java
public class Punto {

    private double[] coordenadas = new double[2];

    public Punto(double x, double y) {
        coordenadas[0] = x;
        coordenadas[1] = y;
    }

    public double calcularDistanciaAlOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] +
                         coordenadas[1] * coordenadas[1]);
    }

    public double getX() {
        return coordenadas[0];
    }

    public double getY() {
        return coordenadas[1];
    }
}
```
En este diseño, la interfaz pública no se ha modificado: el constructor y los métodos getX(), getY() y calcularDistanciaAlOrigen() siguen existiendo y funcionan igual desde el punto de vista del usuario. Lo único que cambia es la representación interna de los datos.

Este ejemplo muestra claramente la utilidad de la ocultación de información: al mantener los atributos como private, es posible cambiar la implementación interna (de dos variables a un array) sin afectar al código externo que utiliza la clase.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Aunque un atributo tenga métodos getter y setter públicos, no es recomendable declararlo public. Si el atributo fuese público, podría modificarse directamente desde cualquier clase, sin pasar por ningún control. En cambio, si se mantiene private y se accede a él mediante métodos, se conserva la posibilidad de validar los datos o aplicar reglas antes de asignarlos.

La convención más habitual en Java es declarar los atributos como private y proporcionar métodos públicos solo cuando sea necesario. De hecho, no siempre es obligatorio ofrecer un setter; en algunos casos solo se permite la lectura (getter) para evitar modificaciones indebidas. Esto forma parte del diseño de la interfaz pública y del control sobre el estado interno del objeto.

Esta decisión está directamente relacionada con las invariantes de clase. Si los atributos son públicos, cualquier parte del programa podría asignar valores que rompan las condiciones que deben cumplirse siempre. En cambio, al utilizar setters, se pueden comprobar esas condiciones antes de aceptar un nuevo valor. Por tanto, mantener los atributos privados ayuda a preservar las invariantes y la coherencia interna del objeto.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase es inmutable cuando, una vez creado el objeto, su estado no puede cambiar. Es decir, los valores de sus atributos se establecen en el constructor y no existen métodos que los modifiquen posteriormente. En Java, esto suele implicar declarar los atributos como private y no proporcionar métodos que alteren su valor. De esta manera, el objeto mantiene siempre el mismo estado durante toda su vida.

Un método modificador es aquel que cambia el estado interno del objeto, es decir, altera el valor de uno o varios atributos. Un setter es un ejemplo típico de método modificador, ya que asigna un nuevo valor a un atributo. Sin embargo, no todo método modificador es necesariamente un setter; cualquier método que cambie el estado interno, aunque tenga otro nombre o realice operaciones más complejas, también se considera modificador.

Una clase inmutable presenta varias ventajas. Al no poder cambiar su estado, se reduce la posibilidad de errores y se simplifica el razonamiento sobre el comportamiento del objeto. Además, se facilita el mantenimiento de las invariantes de clase, ya que el estado solo se establece en el momento de la construcción. Esto contribuye a un diseño más seguro y predecible.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No es recomendable incluir métodos setter siempre y de forma automática como si fuese una obligación. Aunque es habitual que muchos atributos tengan getters y setters, su inclusión debe depender del diseño de la clase y de si realmente se necesita permitir la modificación del estado. Añadir setters sin una razón clara puede debilitar la encapsulación.

Si todos los atributos tienen setters públicos, cualquier parte del programa podrá cambiar el estado del objeto en cualquier momento. Esto puede dificultar el mantenimiento de las invariantes de clase y aumentar el riesgo de estados inconsistentes. En algunos casos, puede ser preferible permitir solo lectura (getter) o incluso no exponer ningún método de acceso si no es necesario.

Por tanto, la convención más adecuada no es “siempre incluir setters”, sino diseñar cuidadosamente la interfaz pública. Solo deben ofrecerse métodos modificadores cuando tenga sentido que el objeto pueda cambiar de estado. De esta manera, se mantiene un mayor control sobre la coherencia interna y se refuerza la encapsulación.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase String en Java es inmutable. Esto significa que, una vez creado un objeto de tipo String, su contenido no puede modificarse. Si aparentemente se cambia una cadena, en realidad se crea un nuevo objeto con el nuevo contenido, mientras que el anterior permanece sin cambios.

Al concatenar dos cadenas, por ejemplo con el operador +, no se modifica la cadena original, sino que se genera una nueva String que contiene el resultado de la concatenación. Por ejemplo, si se hace texto = texto + "abc";, se crea un nuevo objeto con el contenido combinado y la variable pasa a referenciar ese nuevo objeto.

Si se va a realizar una operación que implique concatenar muchas veces para construir una cadena muy larga paso a paso, no es eficiente usar repetidamente String con +, ya que se crearán muchos objetos intermedios. En estos casos, se recomienda utilizar clases como StringBuilder, que permiten modificar el contenido de forma más eficiente antes de obtener la cadena final.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

En POO, dos objetos de una misma clase se pueden comparar de dos maneras: por identidad o por contenido. Comparar por identidad significa comprobar si ambas variables hacen referencia exactamente al mismo objeto en memoria. Comparar por contenido significa comprobar si los valores internos de los objetos son equivalentes, aunque se trate de instancias distintas.

En Java, el método equals se utiliza para comparar objetos por contenido. Sin embargo, el método equals definido por defecto en la clase base Object compara por identidad, es decir, devuelve true solo si ambas referencias apuntan al mismo objeto. Para comparar por contenido, es necesario sobrescribir (override) el método equals en la clase correspondiente.

En el caso de las cadenas (String), se deben comparar utilizando el método equals, no el operador ==. El operador == compara referencias (identidad), mientras que equals compara el contenido de las cadenas. Por tanto, para saber si dos cadenas tienen el mismo texto, se debe utilizar cadena1.equals(cadena2).


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Las clases wrapper son clases que “envuelven” un tipo primitivo para tratarlo como un objeto. En Java, los tipos primitivos como int, double o char no son objetos, por lo que no pueden utilizarse directamente en ciertos contextos donde se requieren objetos. Para ello existen clases como Integer, Double o Character, que encapsulan el valor primitivo dentro de un objeto.

Este proceso puede hacerse de forma explícita, por ejemplo utilizando métodos como Integer.valueOf(5), o de forma automática mediante el mecanismo llamado autoboxing. El autoboxing permite que el compilador convierta automáticamente un valor primitivo en su clase wrapper correspondiente, y el proceso inverso (unboxing) también se realiza automáticamente cuando es necesario. Por tanto, en muchos casos el programador no tiene que hacer la conversión manualmente.

Las clases wrapper ofrecen ventajas como la posibilidad de usar valores primitivos en estructuras que trabajan con objetos y disponer de métodos adicionales para operar con esos valores. No todos los lenguajes orientados a objetos necesitan wrappers, ya que algunos no distinguen entre tipos primitivos y objetos. Java sí distingue ambos conceptos, y por ello necesita estas clases envoltorio para integrar los tipos primitivos en su modelo orientado a objetos.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

En POO, un tipo de dato enumerado es un tipo que define un conjunto fijo y limitado de valores posibles. En lugar de permitir cualquier valor, se restringe a una lista concreta de constantes simbólicas. Por ejemplo, los días de la semana o los estados de un pedido pueden representarse mediante un enumerado para evitar valores inválidos.

En Java, un tipo enumerado se declara con la palabra clave enum y, técnicamente, es una clase especial. Cada valor del enumerado es una instancia de esa clase. Por tanto, un enum en Java no es simplemente un conjunto de constantes como en otros lenguajes más antiguos, sino que puede incluir atributos, métodos e incluso constructores, aunque su uso básico suele limitarse a definir valores constantes.

En términos de encapsulación, los enumerados aportan seguridad y claridad. Al limitar los valores posibles, se evita que se asignen datos incorrectos y se refuerzan las invariantes del programa. Además, al estar definidos como un tipo propio, mejoran la legibilidad y reducen errores frente al uso de números o cadenas para representar estados.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta
``` java
public enum Mes {

    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private int dias;
    private int ordinal;

    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }
}
```
En este ejemplo, Mes es un tipo enumerado con doce instancias posibles. Cada constante (por ejemplo, ENERO) es una instancia del propio tipo Mes. El enumerado incluye atributos private (dias y ordinal) y un constructor, que también es implícitamente privado, para inicializar esos valores.

Se proporcionan métodos públicos (getDias() y getOrdinal()) para acceder a la información, manteniendo la encapsulación. De esta manera, los datos internos del enumerado están protegidos y solo se pueden consultar mediante la interfaz pública definida.


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
``` java

public boolean esDeInvierno(boolean esHemiferioNorte) {
    if (esHemiferioNorte) {
        return this == DICIEMBRE || this == ENERO || this == FEBRERO;
    } else {
        return this == JUNIO || this == JULIO || this == AGOSTO;
    }
}

public boolean esDePrimavera(boolean esHemiferioNorte) {
    if (esHemiferioNorte) {
        return this == MARZO || this == ABRIL || this == MAYO;
    } else {
        return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
    }
}

public boolean esDeVerano(boolean esHemiferioNorte) {
    if (esHemiferioNorte) {
        return this == JUNIO || this == JULIO || this == AGOSTO;
    } else {
        return this == DICIEMBRE || this == ENERO || this == FEBRERO;
    }
}

public boolean esDeOtono(boolean esHemiferioNorte) {
    if (esHemiferioNorte) {
        return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
    } else {
        return this == MARZO || this == ABRIL || this == MAYO;
    }
}
```