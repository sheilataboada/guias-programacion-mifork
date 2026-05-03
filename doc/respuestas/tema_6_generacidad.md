# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

En Java, una forma sencilla de construir una estructura de datos capaz de almacenar valores de distintos tipos consiste en usar un array de Object. Como todas las clases heredan de Object, se pueden guardar objetos de cualquier clase dentro de ese array. Esto se parece a la idea de usar void* en C, donde una dirección puede apuntar a datos de distintos tipos, aunque en Java se trabaja con referencias a objetos.

Por ejemplo, se puede crear una pila básica usando un array primitivo de Object. La pila permite añadir elementos con push y recuperar el último elemento insertado con pop. Como el array guarda Object, se pueden introducir cadenas, números, objetos propios, etc. En el caso de tipos primitivos como int o double, Java los convierte automáticamente a sus clases envoltorio, como Integer o Double.

```java

public class PilaObject {
    private Object[] elementos;
    private int numeroElementos;

    public PilaObject(int capacidad) {
        elementos = new Object[capacidad];
        numeroElementos = 0;
    }

    public void push(Object elemento) {
        if (numeroElementos < elementos.length) {
            elementos[numeroElementos] = elemento;
            numeroElementos++;
        }
    }

    public Object pop() {
        Object elemento = null;

        if (numeroElementos > 0) {
            numeroElementos--;
            elemento = elementos[numeroElementos];
            elementos[numeroElementos] = null;
        }

        return elemento;
    }
}
```

Un posible uso sería el siguiente:
```java
public class Main {
    public static void main(String[] args) {
        PilaObject pila = new PilaObject(5);

        pila.push("Hola");
        pila.push(25);
        pila.push(3.5);

        Double decimal = (Double) pila.pop();
        Integer entero = (Integer) pila.pop();
        String texto = (String) pila.pop();

        System.out.println(decimal);
        System.out.println(entero);
        System.out.println(texto);
    }
}
```

El inconveniente de esta solución es que se pierde seguridad de tipos en compilación. Al recuperar un elemento, el método pop devuelve Object, por lo que es necesario hacer una conversión explícita al tipo esperado. Si se realiza una conversión incorrecta, el error aparecerá en tiempo de ejecución. Por eso, esta solución permite almacenar cualquier tipo de dato, pero no es tan segura como usar genericidad con clases parametrizadas.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta

### Respuesta

La **programación genérica** consiste en escribir algoritmos o estructuras de datos sin depender de un tipo concreto. Es decir, se busca que una misma clase o método pueda trabajar con distintos tipos de datos sin tener que repetir el mismo código para `String`, `Integer`, `Persona`, etc. Por ejemplo, una pila, una cola o una lista pueden tener la misma lógica interna aunque almacenen datos de tipos diferentes.

El ejemplo anterior sí puede considerarse un ejemplo **muy básico** de programación genérica, porque la estructura de datos no está limitada a un único tipo concreto. Al usar `Object`, se permite guardar valores de distintos tipos dentro de la misma estructura, de forma parecida a como en C se puede usar `void*` para apuntar a datos de diferentes tipos.

Sin embargo, no es la forma más segura ni la más propia de la genericidad moderna en Java. Al recuperar un dato, es necesario hacer *casting*, por ejemplo `(String)` o `(Integer)`, y si se hace mal, el error aparece en tiempo de ejecución. La genericidad con `<T>` mejora esto porque permite indicar el tipo de dato desde el principio y detectar errores en tiempo de compilación.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta

### Respuesta

El principal problema de usar `void*` en C o `Object` en Java para crear estructuras de datos genéricas es que se pierde parte del **chequeo de tipos**. La estructura puede aceptar valores de muchos tipos diferentes, pero no controla de forma precisa qué tipo concreto se está guardando en cada momento. Esto permite crear una estructura flexible, pero también hace que sea más fácil introducir datos incorrectos.

En Java, si una estructura almacena elementos como `Object`, al recuperar un dato se obtiene también como `Object`. Por tanto, para usarlo como `String`, `Integer`, `Persona`, etc., hay que hacer una conversión explícita, es decir, un *casting*. El problema es que el compilador no siempre puede garantizar que esa conversión sea correcta, así que el error puede aparecer durante la ejecución del programa.

Por ejemplo, se podría guardar por error un `Integer` en una estructura que se pretendía usar solo para `String`. El programa podría compilar, pero al intentar recuperar ese valor como `String`, se produciría un error en tiempo de ejecución. Esto es peor que un error de compilación, porque puede no detectarse hasta que el programa ya se está ejecutando.

Por eso, aunque `Object` permite construir estructuras que aceptan cualquier tipo de objeto, no ofrece una seguridad de tipos tan buena como la genericidad con `<T>`. Con una clase genérica, como `Pila<String>` o `Pila<Integer>`, el tipo queda fijado desde el principio y el compilador puede impedir que se mezclen datos incompatibles.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta

Los **parámetros de tipo** son identificadores que se usan para representar un tipo de dato que todavía no se concreta al escribir la clase o el método. Se escriben entre los símbolos `< >`, por ejemplo `<T>`, `<E>` o `<K, V>`. Funcionan como una especie de “hueco” para el tipo real que se indicará después al usar esa clase o método.

Por ejemplo, en una clase `Caja<T>`, la `T` representa el tipo de dato que podrá guardar la caja. Si se crea una `Caja<String>`, entonces `T` pasa a ser `String`; si se crea una `Caja<Integer>`, entonces `T` pasa a ser `Integer`. Así, la misma clase sirve para distintos tipos sin tener que repetir el código.

```java
public class Caja<T> {
    private T dato;

    public void pon(T dato) {
        this.dato = dato;
    }

    public T quita() {
        return dato;
    }
}
```

Con este mecanismo, Java mantiene el chequeo de tipos en compilación. Por ejemplo, si se crea una `Caja<Integer>`, solo se podrán introducir valores compatibles con `Integer`. Esto evita tener que usar `Object` y hacer conversiones manuales al recuperar los datos, reduciendo errores en tiempo de ejecución.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En Java, la programación genérica permite crear una colección indicando el tipo concreto que va a almacenar. Por ejemplo, si se declara un `ArrayList<String>`, esa lista queda preparada para guardar únicamente objetos de tipo `String`. Si se intenta añadir un `Integer`, un `Double` u otro tipo incompatible, el error se detecta en compilación, antes de ejecutar el programa.

```java
import java.util.ArrayList;

public class EjemploGenericsJava {
    public static void main(String[] args) {
        ArrayList<String> nombres = new ArrayList<>();

        nombres.add("Ana");
        nombres.add("Luis");
        nombres.add("Marta");

        // nombres.add(25); // Error de compilación: solo admite String.

        for (String nombre : nombres) {
            System.out.println(nombre + " es de tipo "
                    + nombre.getClass().getSimpleName());
        }
    }
}
```

En C++ se puede hacer algo parecido usando `templates`, por ejemplo con un `vector<string>`. En este caso, el vector dinámico también queda asociado al tipo `string`, de modo que solo se pueden introducir cadenas de caracteres. Igual que en Java, el compilador controla que no se mezclen tipos incorrectos dentro de esa estructura.

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <typeinfo>

using namespace std;

int main() {
    vector<string> nombres;

    nombres.push_back("Ana");
    nombres.push_back("Luis");
    nombres.push_back("Marta");

    // nombres.push_back(25); // Error de compilación: solo admite string.

    for (string nombre : nombres) {
        cout << nombre << " es de tipo string" << endl;
    }

    return 0;
}
```

En ambos casos, la estructura se instancia indicando el tipo concreto entre símbolos especiales: en Java con `ArrayList<String>` y en C++ con `vector<string>`. La ventaja principal es que no hace falta usar `Object`, `void*` ni conversiones manuales, porque el propio compilador comprueba que todos los elementos son del tipo correcto.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Cuando se instancia una clase con parámetros de tipo, el compilador comprueba que los tipos usados sean correctos. Por ejemplo, si se crea una `Caja<String>`, se controla que dentro de esa caja se introduzcan valores compatibles con `String`. Así, si se intenta guardar un `Integer`, el error aparece antes de ejecutar el programa. Esto permite mantener seguridad de tipos sin tener que hacer conversiones manuales constantemente.

Java y C++ no hacen exactamente lo mismo. En **Java**, los genéricos se usan sobre todo para comprobar tipos en tiempo de compilación. Después, al compilar, se aplica lo que se conoce como **type erasure**: la información concreta del tipo genérico, como `String` o `Integer`, se elimina en gran parte del código generado. Es decir, no se crea una clase distinta para `Caja<String>` y otra distinta para `Caja<Integer>`, sino que se reutiliza una misma estructura compilada, apoyándose en comprobaciones y conversiones gestionadas por el compilador.

En **C++**, con los **templates**, el funcionamiento es diferente. Cuando se usa una plantilla con un tipo concreto, el compilador genera código específico para ese tipo. Por ejemplo, si se usa una plantilla `Caja<string>` y también `Caja<int>`, el compilador puede crear versiones distintas del código para `string` y para `int`. Esto se llama **instanciación de plantillas**.

Por tanto, la diferencia principal es que Java usa **borrado de tipos** para mantener una única versión más general del código compilado, mientras que C++ genera código especializado para cada tipo usado en la plantilla. En ambos casos se consigue programación genérica, pero el mecanismo interno del compilador no es el mismo.



## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
