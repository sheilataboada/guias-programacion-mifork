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

La **programación genérica** consiste en escribir algoritmos o estructuras de datos sin depender de un tipo concreto. Es decir, se busca que una misma clase o método pueda trabajar con distintos tipos de datos sin tener que repetir el mismo código para `String`, `Integer`, `Persona`, etc. Por ejemplo, una pila, una cola o una lista pueden tener la misma lógica interna aunque almacenen datos de tipos diferentes.

El ejemplo anterior sí puede considerarse un ejemplo **muy básico** de programación genérica, porque la estructura de datos no está limitada a un único tipo concreto. Al usar `Object`, se permite guardar valores de distintos tipos dentro de la misma estructura, de forma parecida a como en C se puede usar `void*` para apuntar a datos de diferentes tipos.

Sin embargo, no es la forma más segura ni la más propia de la genericidad moderna en Java. Al recuperar un dato, es necesario hacer *casting*, por ejemplo `(String)` o `(Integer)`, y si se hace mal, el error aparece en tiempo de ejecución. La genericidad con `<T>` mejora esto porque permite indicar el tipo de dato desde el principio y detectar errores en tiempo de compilación.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

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

Una clase con **parámetros de tipo** puede recibir más de un tipo genérico. En este caso, una clase `Par<T, U>` permite guardar dos valores, donde el primero puede ser de un tipo y el segundo de otro distinto. Por ejemplo, podría usarse como `Par<String, Integer>`, `Par<Double, Double>` o `Par<String, Boolean>`.

Esta clase resulta útil cuando un método necesita devolver dos resultados. Como en Java un método solo puede devolver un valor directamente con `return`, se puede empaquetar más de un dato dentro de un objeto. Para calcular la media y la desviación típica de un array de `double`, el método puede devolver un `Par<Double, Double>`, donde el primer valor sea la media y el segundo valor sea la desviación típica.

```java
/**
 * @brief Clase genérica que almacena dos valores, posiblemente de tipos distintos.
 */
public class Par<T, U> {
    private T primerValor;
    private U segundoValor;

    public Par(T primerValor, U segundoValor) {
        this.primerValor = primerValor;
        this.segundoValor = segundoValor;
    }

    public T getPrimerValor() {
        return primerValor;
    }

    public U getSegundoValor() {
        return segundoValor;
    }
}
```

Un ejemplo de uso sería el siguiente:

```java
public class Estadistica {

    /**
     * @brief Calcula la media y la desviación típica de un array de números reales.
     * @param valores Array de números de tipo double.
     * @return Un Par<Double, Double>, donde el primer valor es la media
     *         y el segundo valor es la desviación típica.
     */
    public static Par<Double, Double> calculaMediaYDesviacionTipica(double[] valores) {
        double suma = 0.0;
        double media;
        double sumaDiferencias = 0.0;
        double desviacionTipica;

        if (valores == null || valores.length == 0) {
            throw new IllegalArgumentException("El array no puede ser null ni estar vacío.");
        }

        for (double valor : valores) {
            suma = suma + valor;
        }

        media = suma / valores.length;

        for (double valor : valores) {
            sumaDiferencias = sumaDiferencias + Math.pow(valor - media, 2);
        }

        desviacionTipica = Math.sqrt(sumaDiferencias / valores.length);

        return new Par<Double, Double>(media, desviacionTipica);
    }

    public static void main(String[] args) {
        double[] valores = { 2.0, 4.0, 6.0, 8.0 };

        Par<Double, Double> resultado = calculaMediaYDesviacionTipica(valores);

        System.out.println("Media: " + resultado.getPrimerValor());
        System.out.println("Desviación típica: " + resultado.getSegundoValor());
    }
}
```

En este ejemplo, `Par<T, U>` es genérica porque no está ligada a tipos concretos. Al usar `Par<Double, Double>`, se fija que ambos valores almacenados serán de tipo `Double`. De esta forma, el compilador puede comprobar que los valores usados son correctos y no hace falta recuperar datos como `Object` ni hacer conversiones manuales.



## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta


En Java no solo se pueden declarar parámetros de tipo en clases, sino también directamente en métodos. En ese caso, el método define su propio tipo genérico, aunque la clase donde esté escrito no sea genérica. Para hacerlo, se coloca el parámetro de tipo antes del tipo de retorno, por ejemplo: `public static <T> T seleccionaUno(...)`.

Si el método se define usando `Object`, acepta cualquier combinación de objetos, pero pierde información del tipo concreto. Por eso, al recuperar el resultado, normalmente hay que hacer *downcasting*. Además, nada impide llamar al método mezclando tipos distintos, como un `String` y un `Integer`, porque ambos son `Object`.

```java
public class UtilidadesObject {

    public static Object seleccionaUno(Object primerObjeto, Object segundoObjeto) {
        Object seleccionado;

        if (Math.random() < 0.5) {
            seleccionado = primerObjeto;
        } else {
            seleccionado = segundoObjeto;
        }

        return seleccionado;
    }

    public static void main(String[] args) {
        Object resultadoObject = seleccionaUno("Ana", "Luis");

        String resultado = (String) resultadoObject; // Hace falta downcasting.
        System.out.println(resultado);

        Object mezcla = seleccionaUno("Ana", 25); // Compila, aunque mezcla tipos.
        System.out.println(mezcla);
    }
}
```

En cambio, usando un método genérico, el tipo del primer parámetro, del segundo parámetro y del valor devuelto quedan relacionados mediante el mismo parámetro de tipo `T`. Así, si se trabaja con `String`, el método devuelve directamente un `String`; si se trabaja con `Integer`, devuelve directamente un `Integer`. Esto evita el *downcasting* y permite que el compilador controle mejor que los dos valores usados sean compatibles con el mismo tipo.

```java
public class UtilidadesGenericas {

    public static <T> T seleccionaUno(T primerObjeto, T segundoObjeto) {
        T seleccionado;

        if (Math.random() < 0.5) {
            seleccionado = primerObjeto;
        } else {
            seleccionado = segundoObjeto;
        }

        return seleccionado;
    }

    public static void main(String[] args) {
        String nombre = seleccionaUno("Ana", "Luis");
        System.out.println(nombre);

        Integer numero = seleccionaUno(10, 20);
        System.out.println(numero);

        // String error = seleccionaUno("Ana", 25);
        // Error: no se puede tratar el resultado como String si se mezcla con Integer.
    }
}
```

La diferencia principal es que la versión con `Object` es más general, pero menos segura, porque obliga a convertir manualmente el resultado y permite mezclar tipos sin control claro. La versión genérica mantiene la flexibilidad, pero conserva mejor la seguridad de tipos: el método sigue sirviendo para muchos tipos distintos, pero en cada llamada trabaja de forma coherente con un tipo concreto.



## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

Sí, en Java se pueden establecer **restricciones en los parámetros de tipo**. Para ello se usa `extends`, por ejemplo `T extends Number`. Esto significa que `T` no puede ser cualquier clase, sino que debe ser `Number` o una clase derivada de `Number`, como `Integer`, `Double`, `Float`, `Long`, etc. Así se evita crear, por ejemplo, un `Punto<String>`, porque `String` no representa un número.

Una primera solución sencilla consiste en guardar directamente las coordenadas como `Number`. Esto permite que las coordenadas sean números de cualquier tipo, pero la clase no recuerda exactamente si trabaja con `Integer`, `Double`, etc. Para calcular la distancia se puede convertir cada coordenada a `double` usando `doubleValue()`.

```java
public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otroPunto) {
        double diferenciaX = x.doubleValue() - otroPunto.getX().doubleValue();
        double diferenciaY = y.doubleValue() - otroPunto.getY().doubleValue();

        return Math.sqrt(diferenciaX * diferenciaX + diferenciaY * diferenciaY);
    }
}
```

Un ejemplo de uso sería:

```java
public class Main {
    public static void main(String[] args) {
        Punto puntoA = new Punto(2, 3);
        Punto puntoB = new Punto(5.5, 7.2);

        System.out.println(puntoA.calcularDistanciaA(puntoB));
    }
}
```

Una segunda solución más segura consiste en hacer la clase genérica con `T extends Number`. Así, cada objeto `Punto<T>` sabe con qué tipo numérico está trabajando. Por ejemplo, se puede crear un `Punto<Integer>` o un `Punto<Double>`, pero no un `Punto<String>`. Además, si el método `calcularDistanciaA` recibe un `Punto<T>`, se fuerza que el otro punto use el mismo tipo numérico.

```java
public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otroPunto) {
        double diferenciaX = x.doubleValue() - otroPunto.getX().doubleValue();
        double diferenciaY = y.doubleValue() - otroPunto.getY().doubleValue();

        return Math.sqrt(diferenciaX * diferenciaX + diferenciaY * diferenciaY);
    }
}
```

Ejemplo de uso:

```java
public class Main {
    public static void main(String[] args) {
        Punto<Double> puntoA = new Punto<Double>(2.0, 3.0);
        Punto<Double> puntoB = new Punto<Double>(5.5, 7.2);

        System.out.println(puntoA.calcularDistanciaA(puntoB));

        // Punto<String> puntoIncorrecto = new Punto<String>("2", "3");
        // Error de compilación: String no hereda de Number.
    }
}
```

Respecto al **type erasure**, tras la compilación el tipo genérico `T` se borra y se sustituye por su límite superior. En este caso, como se ha escrito `T extends Number`, el tipo final que se conserva internamente es `Number`. Por eso, aunque en el código se escriba `Punto<Double>` o `Punto<Integer>`, tras compilar no existen clases separadas para cada tipo, sino una estructura general basada en `Number`, con comprobaciones realizadas por el compilador.



## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta


Ambas soluciones permiten trabajar con distintos tipos de números sin duplicar la clase `Punto`, pero no ofrecen el mismo nivel de seguridad de tipos. En la solución sin genéricos, usando directamente `Number`, sí se pueden mezclar coordenadas de tipos distintos dentro del mismo punto, por ejemplo una coordenada `Integer` y otra coordenada `Double`, porque ambas son subclases de `Number`.

```java
Punto punto = new Punto(3, 4.5); // x es Integer, y es Double
```

En cambio, en la solución con genéricos, si la clase se define como `Punto<T extends Number>`, se fuerza que las dos coordenadas sean del mismo tipo `T`. Por ejemplo, si se crea un `Punto<Integer>`, tanto `x` como `y` deben ser `Integer`; si se crea un `Punto<Double>`, ambas deben ser `Double`. Así, el compilador puede detectar mejor los errores antes de ejecutar el programa.

```java
Punto<Integer> puntoEntero = new Punto<Integer>(3, 4);      // Correcto
Punto<Double> puntoReal = new Punto<Double>(3.0, 4.5);      // Correcto

// Punto<Integer> puntoIncorrecto = new Punto<Integer>(3, 4.5);
// Error: 4.5 no es Integer.
```

La diferencia también se ve en el método `getX`. En la solución sin genéricos, `getX` devuelve siempre `Number`, por lo que se pierde información concreta sobre si la coordenada era `Integer`, `Double`, `Float`, etc. En la solución con genéricos, `getX` devuelve `T`, es decir, el tipo concreto con el que se haya creado el punto: en un `Punto<Integer>` devuelve `Integer`, y en un `Punto<Double>` devuelve `Double`.

Por tanto, la versión con genéricos refuerza el chequeo de tipos porque conserva esa relación entre las coordenadas y el tipo elegido para el punto. Aunque internamente Java aplique borrado de tipos al compilar, durante la escritura y compilación del programa se consigue una comprobación más estricta que usando solo `Number`.



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

Sí, se puede añadir genericidad a la interfaz para que el método `distanciaA` solo acepte puntos del mismo tipo concreto. La idea es que la interfaz `Punto` tenga un parámetro de tipo que representa el tipo exacto de punto con el que se puede calcular la distancia. Así, `Punto2D` solo podrá calcular distancia con otro `Punto2D`, y `Punto3D` solo con otro `Punto3D`.

```java
public interface Punto<T extends Punto<T>> {
    public double distanciaA(T punto);
}
```

Con esta definición, ya no hace falta recibir un `Punto` general y comprobar después con `instanceof` si realmente es un `Punto2D` o un `Punto3D`. El compilador se encarga de controlar que el parámetro recibido por `distanciaA` sea del tipo correcto.

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x;
    private final double y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D punto) {
        return Math.sqrt(Math.pow(x - punto.x, 2)
                + Math.pow(y - punto.y, 2));
    }
}
```

```java
public class Punto3D implements Punto<Punto3D> {
    private final double x;
    private final double y;
    private final double z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D punto) {
        return Math.sqrt(Math.pow(x - punto.x, 2)
                + Math.pow(y - punto.y, 2)
                + Math.pow(z - punto.z, 2));
    }
}
```

Un ejemplo de uso sería este:

```java
public class Main {
    public static void main(String[] args) {
        Punto2D punto2DA = new Punto2D(1.0, 2.0);
        Punto2D punto2DB = new Punto2D(4.0, 6.0);

        Punto3D punto3DA = new Punto3D(1.0, 2.0, 3.0);
        Punto3D punto3DB = new Punto3D(4.0, 6.0, 8.0);

        System.out.println(punto2DA.distanciaA(punto2DB));
        System.out.println(punto3DA.distanciaA(punto3DB));

        // punto2DA.distanciaA(punto3DA);
        // Error de compilación: Punto3D no es Punto2D.
    }
}
```

Con esta solución, la seguridad mejora porque el error se detecta antes de ejecutar el programa. En la versión inicial, el método recibía un `Punto` demasiado general y después había que comprobar manualmente el tipo real del objeto. En esta versión, cada clase concreta indica en `implements Punto<...>` cuál es el único tipo de punto aceptado por su método `distanciaA`, evitando tanto `instanceof` como el *downcasting*.



## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

Aunque `String` sea subtipo de `Object`, **no significa** que `List<String>` sea subtipo de `List<Object>`. En Java, los tipos genéricos normales son **invariantes** respecto a su parámetro de tipo. Esto quiere decir que `List<String>` y `List<Object>` se consideran tipos distintos y no sustituibles entre sí, aunque entre `String` y `Object` sí exista una relación de herencia.

```java
List<String> textos = new ArrayList<String>();

// List<Object> objetos = textos;
// Error de compilación: List<String> no es subtipo de List<Object>.
```

La razón es que, si Java permitiese tratar una `List<String>` como una `List<Object>`, entonces se podría añadir a esa lista cualquier objeto que no fuese `String`, por ejemplo un `Integer`. Eso rompería la seguridad de tipos, porque una lista que debería contener solo cadenas acabaría conteniendo otro tipo de dato. Por eso, en las colecciones genéricas, el compilador evita directamente esa situación.

```java
List<String> textos = new ArrayList<String>();
// List<Object> objetos = textos; // Si esto se permitiese...
// objetos.add(25);              // ...se podría meter un Integer en una lista de String.
```

Con los arrays ocurre algo diferente: `String[]` **sí es subtipo** de `Object[]`. Por eso se dice que los arrays en Java son **covariantes**. Esto permite hacer una asignación como `Object[] objetos = textos;`, pero puede provocar un problema en tiempo de ejecución. El compilador lo acepta, pero el array real sigue siendo un array de `String`, así que si se intenta guardar dentro un objeto que no sea `String`, se produce un error durante la ejecución.

```java
String[] textos = new String[2];
Object[] objetos = textos;

objetos[0] = "Hola";       // Correcto.
// objetos[1] = 25;        // Compila, pero produce error en ejecución.
```

Un tipo genérico es **covariante** si mantiene la relación de herencia de sus parámetros, como ocurre conceptualmente con `? extends`: si `String` deriva de `Object`, se acepta trabajar con “algún tipo que derive de `Object`”. Es **contravariante** si invierte esa relación, como ocurre con `? super`, permitiendo trabajar con algún supertipo de una clase. Es **invariante** cuando no mantiene ninguna relación automática entre `Tipo<A>` y `Tipo<B>`, aunque `A` y `B` estén relacionados por herencia; este es el caso normal de `List<String>` y `List<Object>`.



## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un **wildcard** se representa con `?` y significa “un tipo desconocido”. Se usa cuando no interesa fijar un tipo exacto, pero sí se quiere controlar qué relación tiene ese tipo con otro. Por ejemplo, `List<?>` significa “lista de algún tipo”, pero no se sabe cuál; por eso se puede leer de ella como `Object`, pero no se pueden añadir elementos concretos con seguridad.

`List<? extends T>` se usa cuando se quiere aceptar una lista cuyos elementos sean de tipo `T` o de alguna subclase de `T`. Es útil cuando la lista se va a **consultar**, porque se sabe que todo lo que salga de ella puede tratarse como `T`. En cambio, normalmente no se pueden añadir elementos, porque no se conoce el subtipo exacto de la lista. Por ejemplo, una `List<Integer>` y una `List<Double>` pueden recibirse como `List<? extends Number>`.

```java
import java.util.List;

public class UtilidadesNumeros {

    public static double sumar(List<? extends Number> numeros) {
        double suma = 0.0;

        for (Number numero : numeros) {
            suma = suma + numero.doubleValue();
        }

        return suma;
    }
}
```

`List<? super T>` se usa cuando se quiere aceptar una lista de `T` o de algún supertipo de `T`. Es útil cuando la lista se va a **rellenar** con valores de tipo `T`, porque se sabe que una lista de `Integer`, `Number` u `Object` puede guardar enteros. En este caso, se pueden añadir `Integer`, pero al leer no se puede asegurar más que `Object`, porque la lista podría ser de un tipo más general.

```java
import java.util.List;

public class UtilidadesListas {

    public static void anadirEnteros(List<? super Integer> numeros) {
        numeros.add(10);
        numeros.add(20);
        numeros.add(30);
    }
}
```

En resumen, `? extends T` sirve para recuperar una covarianza controlada: aceptar listas de subtipos cuando principalmente se van a leer datos. `? super T` sirve para recuperar una contravarianza controlada: aceptar listas de supertipos cuando principalmente se van a insertar datos. Así se evita el problema de tratar directamente una `List<Integer>` como si fuese una `List<Number>`, pero se conserva flexibilidad de forma segura.
