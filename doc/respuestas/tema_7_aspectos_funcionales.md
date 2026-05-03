# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un puntero a una función en C es una variable que guarda la dirección de memoria de una función. Esto permite invocar esa función de forma indirecta, usando el puntero en lugar del nombre original de la función. Para que sea correcto, el puntero debe tener el mismo tipo de retorno y los mismos tipos de parámetros que la función a la que apunta.

En este caso, la función recibe una cadena de caracteres, la modifica carácter a carácter convirtiéndola a mayúsculas y devuelve la misma cadena ya transformada. Después, dentro de `main`, se crea una variable local llamada `aMayusculas`, que es un puntero a esa función, y se invoca usando dicho puntero.

```c
#include <stdio.h>
#include <ctype.h>

/**
 * @brief Convierte una cadena de caracteres a mayúsculas.
 *
 * @param cadena Cadena que se quiere transformar.
 * @return La misma cadena recibida, pero convertida a mayúsculas.
 */
char *convertirAMayusculas(char *cadena) {
    int posicion = 0;

    while (cadena[posicion] != '\0') {
        cadena[posicion] = (char) toupper((unsigned char) cadena[posicion]);
        posicion++;
    }

    return cadena;
}

int main(void) {
    char texto[] = "Hola mundo";

    // Puntero local a una función que recibe char * y devuelve char *.
    char *(*aMayusculas)(char *) = convertirAMayusculas;

    printf("Texto en mayusculas: %s\n", aMayusculas(texto));

    return 0;
}
```



## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una **función lambda** es una función escrita de forma más compacta, normalmente sin darle un nombre propio. Se usa mucho cuando se quiere guardar una operación en una variable, pasarla como parámetro o ejecutarla más adelante. En JavaScript se suele escribir como una **función flecha** con `=>`, que es una forma abreviada de escribir una función. 

En Java, una lambda no queda “suelta” por sí sola, sino que se guarda en una referencia de una **interfaz funcional**. En este caso, `Function<String, String>` representa una función que recibe un `String` y devuelve otro `String`; por eso encaja bien con una operación que recibe una cadena y devuelve esa cadena convertida a mayúsculas. Para ejecutarla se usa el método `apply`. 

**Ejemplo en JavaScript:**

```javascript
function main() {
    let texto = "Hola mundo";

    // Variable local que guarda una función lambda.
    let aMayusculas = (cadena) => cadena.toUpperCase();

    console.log(aMayusculas(texto));
}

main();
```

**Ejemplo en Java:**

```java
import java.util.function.Function;

public class Programa {
    public static void main(String[] args) {
        String texto = "Hola mundo";

        // Variable local que referencia una función lambda.
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        System.out.println(aMayusculas.apply(texto));
    }
}
```

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es una forma de programar en la que el programa se construye principalmente a partir de funciones. En lugar de centrar todo en objetos que tienen estado y métodos, se da importancia a aplicar funciones sobre datos, combinar funciones entre sí y evitar, cuando sea posible, modificar datos externos. La idea principal es que una función reciba unos valores de entrada y produzca un resultado, de forma parecida al concepto matemático de función.

Se dice que lenguajes como Java 8 son multi-paradigma porque permiten programar combinando varias formas de pensar: orientación a objetos, programación estructurada, genericidad y también aspectos funcionales. Java sigue siendo un lenguaje basado en clases y objetos, pero con las lambdas puede representar comportamientos como valores, por ejemplo guardando una operación en una variable de tipo Function<String, String> y ejecutándola más tarde.

Que las funciones sean “ciudadanos de primera clase” significa que pueden tratarse como cualquier otro valor del lenguaje: se pueden guardar en variables, pasar como argumentos a otros métodos y devolver como resultado. En Java esto no se hace con funciones independientes como en JavaScript, sino normalmente mediante lambdas asociadas a interfaces funcionales; por ejemplo, una variable puede representar una operación que recibe una cadena y devuelve otra cadena.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
Una función lambda en Java se escribe con la forma básica **parámetros → cuerpo**. A la izquierda de `->` se colocan los parámetros que recibe la función, y a la derecha se coloca lo que hace. Por ejemplo, `cadena -> cadena.toUpperCase()` representa una función que recibe una cadena y devuelve esa misma cadena convertida a mayúsculas.

Cuando solo hay un parámetro y el tipo se puede deducir, se pueden omitir los paréntesis. Si no hay parámetros o hay varios, sí deben ponerse: `() -> ...` o `(a, b) -> ...`. Si el cuerpo tiene una sola expresión, no hace falta escribir `return`; pero si el cuerpo tiene varias instrucciones, se usan llaves `{ }` y, si devuelve un valor, se escribe `return`.

```java
import java.util.function.Function;

public class Programa {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        System.out.println(aMayusculas.apply("Hola mundo"));
    }
}
```

En este ejemplo, `Function<String, String>` indica que la lambda recibe un `String` y devuelve otro `String`. La variable local `aMayusculas` guarda esa función lambda, y para ejecutarla se usa `apply`, pasando como argumento la cadena que se quiere transformar.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

En este caso, la función `transformar` recibe dos cosas: primero, una cadena de texto; segundo, una función que indica qué transformación se quiere aplicar. Así, `transformar` no necesita saber exactamente si la cadena se va a convertir a mayúsculas, minúsculas o cualquier otra operación: simplemente recibe la función transformadora y la ejecuta desde dentro. 

Esto muestra una idea importante de los aspectos funcionales: una función puede usarse como dato, es decir, puede guardarse en una variable y pasarse como argumento a otra función o método. En JavaScript esto se hace de forma directa, mientras que en Java se usa una interfaz funcional como `Function<String, String>`, que representa una función que recibe un `String` y devuelve un `String`. 

**Ejemplo en JavaScript:**

```javascript
function transformar(texto, transformadora) {
    return transformadora(texto);
}

function main() {
    let texto = "Hola mundo";

    // Variable local que guarda una función lambda.
    let aMayusculas = (cadena) => cadena.toUpperCase();

    console.log(transformar(texto, aMayusculas));
}

main();
```

**Ejemplo en Java:**

```java
import java.util.function.Function;

public class Programa {
    public static void main(String[] args) {
        String texto = "Hola mundo";

        // Variable local que referencia una función lambda.
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        System.out.println(transformar(texto, aMayusculas));
    }

    public static String transformar(String texto, Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }
}
```

En el ejemplo de Java, el método `transformar` tiene como primer parámetro el texto original y como segundo parámetro la función transformadora. La llamada `transformadora.apply(texto)` es la que realmente ejecuta la lambda guardada en `aMayusculas`, devolviendo como resultado `"HOLA MUNDO"`.



## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

En esta variante no se crea primero una variable como `aMayusculas`, sino que la función lambda se escribe directamente en la llamada a `transformar`. Esa lambda pasa a ser el segundo parámetro de la llamada, y el método o función `transformar` la ejecuta desde dentro con el texto recibido. 

La ventaja es que la transformación queda definida justo en el punto donde se usa. Esto resulta útil cuando esa operación solo se necesita una vez, por ejemplo para invertir una cadena sin guardarla antes en una variable independiente.

**Ejemplo en JavaScript:**

```javascript
function transformar(texto, transformadora) {
    return transformadora(texto);
}

function main() {
    let texto = "Hola mundo";

    console.log(transformar(texto, (cadena) => {
        let invertida = "";

        for (let posicion = cadena.length - 1; posicion >= 0; posicion--) {
            invertida = invertida + cadena[posicion];
        }

        return invertida;
    }));
}

main();
```

**Ejemplo en Java:**

```java
import java.util.function.Function;

public class Programa {
    public static void main(String[] args) {
        String texto = "Hola mundo";

        System.out.println(transformar(texto, cadena -> {
            String invertida = "";

            for (int posicion = cadena.length() - 1; posicion >= 0; posicion--) {
                invertida = invertida + cadena.charAt(posicion);
            }

            return invertida;
        }));
    }

    public static String transformar(String texto, Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }
}
```

En ambos casos, la lambda recibe la cadena original, la recorre desde el último carácter hasta el primero y construye una nueva cadena invertida. Por eso, si el texto es `"Hola mundo"`, el resultado mostrado sería `"odnum aloH"`.



## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un **cierre** o **closure** aparece cuando una función lambda utiliza una variable que no ha sido creada dentro de la propia lambda, sino en el contexto exterior donde esa lambda fue definida. Es decir, la lambda “recuerda” o puede acceder a parte del entorno que la rodeaba en el momento de su creación.

En Java, una lambda puede acceder a variables locales externas, pero esas variables deben ser `final` o **efectivamente finales**, es decir, no pueden cambiar de valor después de haber sido inicializadas. Esto evita que la lambda trabaje con una variable local cuyo valor pueda cambiar de forma ambigua. 

```java id="owh5t0"
import java.util.function.Function;

public class Programa {
    public static void main(String[] args) {
        String texto = "Hola mundo";

        // Variable local definida fuera de la lambda.
        String textoAConcatenar = "!!!";

        // La lambda accede a textoAConcatenar aunque no se haya creado dentro de ella.
        Function<String, String> concatenarTexto = cadena -> cadena + textoAConcatenar;

        System.out.println(transformar(texto, concatenarTexto));

        // textoAConcatenar = "???"; // Esto daría error si se intenta cambiar después.
    }

    public static String transformar(String texto, Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }
}
```

En este ejemplo, la función lambda `cadena -> cadena + textoAConcatenar` recibe una cadena como parámetro, pero también usa `textoAConcatenar`, que está declarada fuera de la lambda. Por eso se puede decir que la lambda forma un cierre sobre esa variable externa. Si el texto inicial es `"Hola mundo"`, el resultado mostrado sería `"Hola mundo!!!"`."



## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

La diferencia principal es que un **puntero a función en C** es una variable que guarda la dirección de memoria de una función ya existente, mientras que una **lambda** representa una función u operación que puede escribirse directamente donde se necesita. En C se trabaja de forma más cercana a la memoria: el puntero apunta al código de una función con una firma concreta. En cambio, en Java no se manipulan direcciones de memoria, sino referencias seguras asociadas a un tipo funcional, como `Function<String, String>`. 

Otra diferencia importante es que una lambda puede estar más ligada al contexto donde se crea. Por ejemplo, puede usar variables locales externas si son finales o efectivamente finales, formando un cierre o *closure*. Un puntero a función en C, por sí solo, no guarda ese contexto externo: normalmente solo sabe a qué función llamar. Para imitar algo parecido a un cierre en C habría que pasar información adicional mediante parámetros, por ejemplo con estructuras o punteros extra.

También cambia la forma de uso dentro del diseño del programa. Con punteros a función se consigue invocar funciones de manera indirecta, pero sigue siendo una herramienta más técnica y manual. Con lambdas se expresa directamente “qué operación se quiere hacer” y se puede pasar esa operación como argumento a otro método, como ocurría con `transformar`. Por eso las lambdas encajan mejor con el estilo funcional, mientras que los punteros a función son un mecanismo más básico de llamada indirecta. 


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
Devolver funciones significa que un método no devuelve directamente un resultado numérico, sino una función preparada para usarse más tarde. En este caso, `crearDescuento(porcentaje)` recibe el porcentaje de descuento y devuelve una función de tipo `Function<Double, Double>`, que después recibirá una cantidad y calculará el precio ya descontado. 

```java id="66t0rq"
import java.util.function.Function;

public class Programa {
    public static void main(String[] args) {
        double cantidad = 200.0;

        Function<Double, Double> descuentoDiez = crearDescuento(10.0);
        Function<Double, Double> descuentoVeinticinco = crearDescuento(25.0);

        System.out.println("Cantidad original: " + cantidad);
        System.out.println("Con descuento del 10%: " + descuentoDiez.apply(cantidad));
        System.out.println("Con descuento del 25%: " + descuentoVeinticinco.apply(cantidad));
    }

    /**
     * @brief Crea una función que aplica un descuento a una cantidad.
     *
     * @param porcentaje Porcentaje de descuento que se quiere aplicar.
     * @return Función que recibe una cantidad y devuelve esa cantidad con descuento.
     */
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad - (cantidad * porcentaje / 100);
    }
}
```

La salida sería que, para una cantidad inicial de `200.0`, el descuento del `10%` devuelve `180.0`, y el descuento del `25%` devuelve `150.0`. Cada llamada a `crearDescuento` crea una función distinta porque cada una guarda un porcentaje diferente: una función recuerda el `10.0` y la otra recuerda el `25.0`.

La **closure** está en la lambda `cantidad -> cantidad - (cantidad * porcentaje / 100)`, porque la variable `porcentaje` no se declara dentro de la lambda, sino que pertenece al método `crearDescuento`. Aunque el método termina, la función devuelta sigue pudiendo usar ese valor de `porcentaje`, porque queda capturado dentro de la lambda. En Java, ese valor debe mantenerse sin modificaciones dentro de ese contexto, es decir, debe ser final o efectivamente final.



## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

Una **interfaz funcional** es una interfaz pensada para representar una única operación. En Java, una lambda no tiene un tipo aislado por sí misma, sino que necesita encajar en un tipo ya declarado. Ese tipo suele ser una interfaz funcional, porque la lambda proporciona la implementación del único método abstracto de esa interfaz.

El requisito principal es que tenga **un solo método abstracto**. Ese método abstracto es el que define la forma de la lambda: qué parámetros recibe y qué valor devuelve. Por ejemplo, `Function<String, String>` representa una operación que recibe un `String` y devuelve un `String`; por eso una lambda como `cadena -> cadena.toUpperCase()` puede guardarse en una variable de ese tipo.

Una interfaz funcional puede tener otros elementos, como constantes o métodos ya implementados, siempre que siga existiendo un único método abstracto que la lambda tenga que completar. También puede marcarse con la anotación `@FunctionalInterface`, que no es obligatoria, pero ayuda a que el compilador compruebe que realmente se cumple esa condición. 



## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Una interfaz funcional propia permite crear un tipo específico para una operación concreta. En este caso, `Transformador` representa cualquier función que reciba una cadena de texto y devuelva otra cadena de texto. Al tener un único método abstracto, una expresión lambda puede usarse como implementación de ese método. 

La anotación `@FunctionalInterface` no es obligatoria, pero es recomendable porque obliga al compilador a comprobar que la interfaz realmente tiene un solo método abstracto. Así, si por error se añade otro método abstracto, el compilador avisará. 

```java
@FunctionalInterface
interface Transformador {
    String transformar(String cadena);
}

public class Programa {
    public static void main(String[] args) {
        String texto = "Hola mundo";

        // La lambda implementa el método transformar de la interfaz Transformador.
        Transformador aMayusculas = cadena -> cadena.toUpperCase();

        System.out.println(aplicarTransformacion(texto, aMayusculas));
    }

    public static String aplicarTransformacion(String texto, Transformador transformador) {
        return transformador.transformar(texto);
    }
}
```

En este ejemplo, `Transformador aMayusculas = cadena -> cadena.toUpperCase();` significa que la variable `aMayusculas` guarda una función que recibe un `String` y devuelve otro `String`. Cuando se llama a `transformador.transformar(texto)`, se ejecuta realmente la lambda asociada, por lo que `"Hola mundo"` se transforma en `"HOLA MUNDO"`.



## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Una interfaz funcional genérica permite que la misma interfaz sirva para transformar un dato de un tipo en otro tipo distinto. En vez de fijar siempre `String` como entrada y `String` como salida, se usan parámetros genéricos, por ejemplo `T` para el tipo de entrada y `R` para el tipo de salida.

En este caso, `Transformador<T, R>` representa una operación que recibe un valor de tipo `T` y devuelve un valor de tipo `R`. Así se puede crear, por ejemplo, un `Transformador<Double, Integer>` que recibe un número decimal y devuelve un entero redondeado.

```java
@FunctionalInterface
interface Transformador<T, R> {
    R transformar(T valor);
}

public class Programa {
    public static void main(String[] args) {
        Double numero = 7.8;

        // Recibe un Double y devuelve un Integer redondeado.
        Transformador<Double, Integer> redondear = valor -> (int) Math.round(valor);

        Integer resultado = redondear.transformar(numero);

        System.out.println("Número original: " + numero);
        System.out.println("Número redondeado: " + resultado);
    }
}
```

En la línea `Transformador<Double, Integer> redondear`, el primer tipo indica el dato que entra en la lambda y el segundo tipo indica el dato que devuelve. Por eso `valor` se interpreta como un `Double`, mientras que el resultado final debe ser un `Integer`. La lambda `valor -> (int) Math.round(valor)` realiza el redondeo y adapta el resultado al tipo esperado.



## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Transformador<T, R> es útil para entender cómo funciona una interfaz funcional genérica, pero en Java ya existe una equivalente: Function<T, R>. La idea es la misma: recibe un valor de tipo T y devuelve un valor de tipo R. Por eso, en vez de crear siempre una interfaz propia, muchas veces se puede reutilizar una ya existente del paquete java.util.function, que agrupa interfaces funcionales generales preparadas para lambdas.

Por ejemplo, el transformador que redondea un Double a un Integer puede escribirse directamente con Function<Double, Integer>. La lambda se guarda en la variable redondear, y se ejecuta con apply, que es el método abstracto de Function<T, R>.

import java.util.function.Function;

public class Programa {
    public static void main(String[] args) {
        Double numero = 7.8;

        // Recibe un Double y devuelve un Integer.
        Function<Double, Integer> redondear = valor -> (int) Math.round(valor);

        Integer resultado = redondear.apply(numero);

        System.out.println("Número original: " + numero);
        System.out.println("Número redondeado: " + resultado);
    }
}

Las interfaces funcionales predefinidas más habituales son estas:
| Interfaz funcional | Método principal | Significado |
|-------------------|------------------|-------------|
| `Function<T, R>` | `R apply(T valor)` | Recibe un valor de tipo `T` y devuelve un valor de tipo `R`. |
| `BiFunction<T, U, R>` | `R apply(T valor1, U valor2)` | Recibe dos valores y devuelve un resultado. |
| `Consumer<T>` | `void accept(T valor)` | Recibe un valor y no devuelve nada. |
| `BiConsumer<T, U>` | `void accept(T valor1, U valor2)` | Recibe dos valores y no devuelve nada. |
| `Supplier<T>` | `T get()` | No recibe parámetros y devuelve un valor. |
| `Predicate<T>` | `boolean test(T valor)` | Recibe un valor y devuelve `true` o `false`. |
| `BiPredicate<T, U>` | `boolean test(T valor1, U valor2)` | Recibe dos valores y devuelve `true` o `false`. |
| `UnaryOperator<T>` | `T apply(T valor)` | Recibe un valor de tipo `T` y devuelve otro valor del mismo tipo `T`. |
| `BinaryOperator<T>` | `T apply(T valor1, T valor2)` | Recibe dos valores de tipo `T` y devuelve un valor del mismo tipo `T`. |

Además, existen versiones especializadas para tipos primitivos, como IntFunction, DoubleFunction, IntConsumer, DoubleConsumer, IntPredicate, DoublePredicate, IntSupplier, DoubleSupplier, IntUnaryOperator, DoubleUnaryOperator, IntBinaryOperator, DoubleBinaryOperator, ToIntFunction, ToDoubleFunction, etc. Estas evitan tener que trabajar siempre con clases envolventes como Integer o Double cuando se opera con tipos primitivos.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

`List.forEach` permite recorrer una lista aplicando una operación a cada elemento. Se parece al `for-each`, porque no se trabaja directamente con un índice ni se accede manualmente a cada posición, sino que cada elemento se va recibiendo uno a uno. La diferencia es que con `forEach` la acción que se quiere realizar se expresa mediante una función lambda. 

En este ejemplo, la lista contiene objetos `Integer`. El método `forEach` recibe una lambda que se ejecuta para cada número de la lista. Dentro de esa lambda se comprueba si el número es mayor que cero y, si lo es, se muestra un mensaje por pantalla.

```java
import java.util.Arrays;
import java.util.List;

public class Programa {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-3, 0, 5, 8, -1, 12);

        numeros.forEach(numero -> {
            if (numero > 0) {
                System.out.println(numero + " es positivo");
            }
        });
    }
}
```

La lambda `numero -> { ... }` representa la acción que se aplicará a cada elemento de la lista. Primero `numero` vale `-3`, después `0`, después `5`, y así sucesivamente hasta terminar la lista. Solo se imprime mensaje cuando se cumple la condición `numero > 0`, por lo que se mostrarían como positivos `5`, `8` y `12`.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

`forEach` usa `Consumer<? super T>` porque la función que se le pasa **consume** elementos de la lista: recibe cada elemento y hace algo con él, pero no devuelve un resultado. Por eso se aplica la regla **PECS**, que significa **Producer Extends, Consumer Super**: si un tipo genérico produce valores, suele usarse `extends`; si consume valores, suele usarse `super`.

En una `List<Integer>`, cada elemento que se entrega al `forEach` es un `Integer`. Pero una función que sabe consumir `Number` también puede consumir un `Integer`, porque `Integer` es un tipo de número. Lo mismo pasaría con una función que consuma `Object`, porque cualquier `Integer` también es un `Object`. Por eso `Consumer<? super Integer>` permite usar `Consumer<Integer>`, `Consumer<Number>` o `Consumer<Object>`. Si se usara solo `Consumer<T>`, sería más rígido y aceptaría menos casos.

Aplicando la misma idea al método `transformar`, la función transformadora **consume** el valor de entrada y **produce** el valor de salida. Por tanto, una versión más flexible sería esta:

```java id="4a2hnm"
import java.util.function.Function;

public class Programa {
    public static void main(String[] args) {
        Integer numero = 7;

        Function<Number, String> transformarNumero =
                valor -> "El número es " + valor;

        String resultado = transformar(numero, transformarNumero);

        System.out.println(resultado);
    }

    public static <T, R> R transformar(
            T valor,
            Function<? super T, ? extends R> transformadora
    ) {
        return transformadora.apply(valor);
    }
}
```

En `Function<? super T, ? extends R>`, la parte `? super T` indica que la función puede recibir valores de tipo `T` o de algún tipo superior, porque está consumiendo el dato. La parte `? extends R` indica que la función puede devolver un valor de tipo `R` o de algún subtipo de `R`, porque está produciendo el resultado. Así el método `transformar` queda más reutilizable sin perder seguridad de tipos.


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Una **referencia a método** permite guardar en una variable una referencia a un método ya existente, sin escribir de nuevo su código. En JavaScript esto puede hacerse guardando el método en una variable local; si el método usa `this`, conviene enlazarlo con `bind` para que siga apuntando al objeto correcto. En Java, una referencia a método se escribe con `::`, por ejemplo `persona::saludar`, y se guarda en una variable de una interfaz funcional compatible. 

**Ejemplo en JavaScript:**

```javascript id="bdy0zp"
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        return "Hola, soy " + this.nombre;
    }
}

function main() {
    const persona = new Persona("Ana");

    // Referencia local al método saludar del objeto persona.
    const saludar = persona.saludar.bind(persona);

    console.log(saludar());
}

main();
```

**Ejemplo en Java:**

```java id="6et9a7"
import java.util.function.Supplier;

class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public String saludar() {
        return "Hola, soy " + nombre;
    }
}

public class Programa {
    public static void main(String[] args) {
        Persona persona = new Persona("Ana");

        // Referencia local al método saludar del objeto persona.
        Supplier<String> saludar = persona::saludar;

        System.out.println(saludar.get());
    }
}
```

En el ejemplo de Java, `persona::saludar` no ejecuta el método inmediatamente, sino que crea una referencia a él. Como `saludar` no recibe parámetros y devuelve un `String`, encaja con `Supplier<String>`, cuyo método principal es `get()`. Al llamar a `saludar.get()`, se ejecuta realmente el método `saludar` del objeto `persona`.



## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

En Java se pueden hacer referencias a métodos cuando ya existe un método o constructor y se quiere usar como si fuera una función. No se ejecuta en el momento de escribir la referencia, sino cuando se invoca el método de la interfaz funcional correspondiente, como `apply()` o `get()`. La referencia debe coincidir con la firma esperada: parámetros de entrada y tipo de salida.

Los tipos principales son: referencia a método estático (`Clase::metodoEstatico`), referencia a constructor (`Clase::new`), referencia a método de instancia de un objeto concreto (`objeto::metodo`) y referencia a método de instancia sobre cualquier objeto de una clase (`Clase::metodoInstancia`). En este último caso, el objeto sobre el que se llama al método se pasa como parámetro de la función.

```java
import java.util.function.Function;
import java.util.function.Supplier;

class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public String saludar() {
        return "Hola, soy " + nombre;
    }

    public static String saludarEstatico(String nombre) {
        return "Hola, soy " + nombre;
    }
}

public class Programa {
    public static void main(String[] args) {
        // 1. Referencia a método estático: Clase::metodoEstatico
        Function<String, String> saludoEstatico = Persona::saludarEstatico;
        System.out.println(saludoEstatico.apply("Ana"));

        // 2. Referencia a constructor: Clase::new
        Function<String, Persona> crearPersona = Persona::new;
        Persona persona = crearPersona.apply("Laura");

        // 3. Referencia a método de instancia de una instancia concreta: objeto::metodo
        Supplier<String> saludarPersonaConcreta = persona::saludar;
        System.out.println(saludarPersonaConcreta.get());

        // 4. Referencia a método de instancia sobre cualquier instancia: Clase::metodoInstancia
        Function<Persona, String> saludarCualquierPersona = Persona::saludar;
        System.out.println(saludarCualquierPersona.apply(new Persona("Carlos")));
    }
}
```

En `Persona::saludarEstatico`, no hace falta crear un objeto porque el método pertenece a la clase. En `Persona::new`, la referencia apunta al constructor y permite crear objetos. En `persona::saludar`, la referencia queda asociada a un objeto concreto ya creado. En cambio, en `Persona::saludar`, todavía no hay un objeto fijo: la persona concreta se pasa después como argumento a `apply()`.



## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

Para ordenar una lista de `Persona`, se puede pasar a `Collections.sort` una función de comparación. Esa función recibe dos personas y devuelve un entero: negativo si la primera debe ir antes, positivo si debe ir después, y cero si se consideran equivalentes en ese criterio. En este caso, primero se compara la edad y, si ambas personas tienen la misma edad, se compara el nombre alfabéticamente.

La primera versión hace la comparación manualmente dentro de la lambda. La segunda versión usa los métodos de `Comparator`, que permiten expresar el mismo criterio de forma más compacta y legible: primero `comparingInt` para ordenar por edad y después `thenComparing` para ordenar por nombre.

```java id="dg2072"
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}

public class Programa {
    public static void main(String[] args) {
        List<Persona> personasVersionManual = new ArrayList<>(Arrays.asList(
                new Persona("Lucia", 20),
                new Persona("Ana", 18),
                new Persona("Carlos", 20),
                new Persona("Bea", 18)
        ));

        List<Persona> personasVersionComparator = new ArrayList<>(Arrays.asList(
                new Persona("Lucia", 20),
                new Persona("Ana", 18),
                new Persona("Carlos", 20),
                new Persona("Bea", 18)
        ));

        // Versión 1: comparación hecha manualmente con una expresión lambda.
        Collections.sort(personasVersionManual, (persona1, persona2) -> {
            int comparacionEdad = Integer.compare(persona1.getEdad(), persona2.getEdad());

            if (comparacionEdad != 0) {
                return comparacionEdad;
            }

            return persona1.getNombre().compareTo(persona2.getNombre());
        });

        System.out.println("Orden manual:");
        personasVersionManual.forEach(persona -> System.out.println(persona));

        // Versión 2: comparación usando métodos de Comparator.
        Collections.sort(
                personasVersionComparator,
                Comparator.comparingInt(Persona::getEdad)
                          .thenComparing(Persona::getNombre)
        );

        System.out.println("\nOrden con Comparator:");
        personasVersionComparator.forEach(persona -> System.out.println(persona));
    }
}
```

En la versión manual, `Integer.compare(persona1.getEdad(), persona2.getEdad())` decide el orden por edad. Si las edades son distintas, ya se devuelve ese resultado. Si las edades son iguales, se usa `compareTo` sobre los nombres para colocarlos por orden alfabético.

En la versión con `Comparator`, `Comparator.comparingInt(Persona::getEdad)` indica que el primer criterio es la edad, y `.thenComparing(Persona::getNombre)` añade el segundo criterio. Por tanto, ambas versiones producen el mismo orden: primero las personas más jóvenes y, dentro de la misma edad, los nombres ordenados alfabéticamente.
