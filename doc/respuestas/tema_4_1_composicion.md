# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

La composición en C se utiliza cuando una estructura está formada por otras estructuras, expresando una relación del tipo “tiene-un”. Esto permite construir modelos más complejos a partir de elementos más simples. Por ejemplo, una línea puede considerarse como un objeto que tiene dos puntos, y cada punto, a su vez, tiene dos coordenadas (x e y). Este enfoque facilita la organización de los datos y mejora la claridad del programa.

En este contexto, se puede definir una estructura Punto con dos atributos (x, y) y una estructura Linea que contenga dos objetos de tipo Punto. Además, es habitual acompañar estas estructuras con funciones que operen sobre ellas, como el cálculo de la distancia entre dos puntos o la longitud de una línea, reutilizando la lógica definida previamente.

A continuación se muestra un ejemplo completo en C que implementa esta idea:
```c

#include <stdio.h>
#include <math.h>

/**
 * @brief Estructura que representa un punto en el plano
 */
struct Punto {
    float x;
    float y;
};

/**
 * @brief Estructura que representa una línea formada por dos puntos
 */
struct Linea {
    struct Punto p1;
    struct Punto p2;
};

/**
 * @brief Calcula la distancia entre dos puntos
 * @param a Primer punto
 * @param b Segundo punto
 * @return Distancia entre los puntos
 */
float distancia(struct Punto a, struct Punto b) {
    return sqrt((a.x - b.x)*(a.x - b.x) + (a.y - b.y)*(a.y - b.y));
}

/**
 * @brief Calcula la longitud de una línea
 * @param l Línea formada por dos puntos
 * @return Longitud de la línea
 */
float longitudLinea(struct Linea l) {
    return distancia(l.p1, l.p2);
}

int main() {
    struct Punto p1 = {0, 0};
    struct Punto p2 = {3, 4};

    struct Linea linea = {p1, p2};

    printf("Distancia entre puntos: %.2f\n", distancia(p1, p2));
    printf("Longitud de la linea: %.2f\n", longitudLinea(linea));

    return 0;
}
```

Este ejemplo ilustra claramente la composición: la estructura Linea no almacena datos simples directamente, sino que contiene otras estructuras (Punto). Además, se observa cómo la función de longitud reutiliza la función de distancia, lo que favorece la modularidad y evita duplicar código.

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

En orientación a objetos, la composición se expresa mediante clases que contienen objetos de otras clases como atributos, manteniendo la relación “tiene-un”. En este caso, una clase Linea estará compuesta por dos objetos de tipo Punto. A diferencia de C, Java permite aplicar encapsulación para ocultar los datos internos y controlar su acceso, lo que facilita garantizar ciertas propiedades, como la inmutabilidad.

Para conseguir que los objetos sean inmutables, se declaran los atributos como private y final, y no se proporcionan métodos setters. De este modo, una vez creado un objeto Punto o Linea, sus valores no pueden modificarse. Además, la funcionalidad se incorpora dentro de las propias clases mediante métodos, como el cálculo de la distancia entre puntos o la longitud de una línea, aprovechando así las ventajas de la programación orientada a objetos frente al enfoque procedural.

A continuación se muestra una posible implementación en Java:
```java

/**
 * @brief Clase que representa un punto en el plano (inmutable)
 */
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    /**
     * @brief Calcula la distancia a otro punto
     * @param otro Punto con el que se calcula la distancia
     * @return Distancia entre los dos puntos
     */
    public double distancia(Punto otro) {
        return Math.sqrt(Math.pow(this.x - otro.x, 2) + Math.pow(this.y - otro.y, 2));
    }
}

/**
 * @brief Clase que representa una línea formada por dos puntos (composición e inmutabilidad)
 */
public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    /**
     * @brief Calcula la longitud de la línea
     * @return Longitud de la línea
     */
    public double longitud() {
        return p1.distancia(p2);
    }
}
```

En este diseño se observa claramente la composición: la clase Linea tiene dos objetos Punto. Además, gracias a la encapsulación y al uso de atributos final, se garantiza que ni los puntos ni la línea puedan modificarse tras su creación, lo que mejora la seguridad y coherencia del programa respecto a la solución en C.

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad en composición indica cuántas instancias de una clase están relacionadas con una instancia de otra clase. Es decir, permite especificar relaciones como uno a uno, uno a varios o muchos a uno. En el contexto de la programación orientada a objetos, la multiplicidad ayuda a entender cómo se estructuran los objetos dentro de otros y cuántos elementos participan en esa relación.

En el ejemplo anterior, una Linea está compuesta exactamente por dos objetos Punto, lo que implica que la multiplicidad desde Linea hacia Punto es 1 → 2 (una línea tiene exactamente dos puntos). No puede haber una línea con más o menos puntos en este modelo, ya que su definición fija esa cantidad. Esto refleja una composición fuerte donde los elementos que la forman están claramente definidos.

Por otro lado, desde el punto de vista inverso, un Punto podría pertenecer a ninguna, una o varias líneas, dependiendo del uso del programa. Por tanto, la multiplicidad desde Punto hacia Linea sería 0 → n (un punto puede estar en cero o muchas líneas). Esto indica que un mismo punto puede reutilizarse en diferentes líneas sin restricciones, lo cual es habitual en este tipo de modelos geométricos.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La composición fuerte y la composición débil (también llamada agregación) se diferencian principalmente en la relación que existe entre los objetos y, sobre todo, en su ciclo de vida. En ambos casos se mantiene una relación “tiene-un”, pero no todas las composiciones implican el mismo nivel de dependencia entre las partes.

En la composición fuerte, los objetos contenidos dependen completamente del objeto que los contiene. Esto significa que su ciclo de vida está ligado: si el objeto principal se destruye, también lo hacen los objetos que lo componen. Además, normalmente esos objetos no tienen sentido por sí solos fuera de esa relación. Este tipo de relación es lo que se denomina propiamente composición. Un ejemplo sería una Linea cuyos puntos no se comparten con otras estructuras y existen únicamente dentro de ella.

En cambio, en la composición débil, los objetos pueden existir de forma independiente del objeto que los contiene. Es decir, su ciclo de vida no depende del otro: si el objeto principal desaparece, los objetos contenidos pueden seguir existiendo. Este tipo de relación se conoce como agregación o asociación. Un ejemplo sería el caso en el que varios objetos Linea comparten los mismos Punto, de forma que los puntos existen independientemente de las líneas.

En resumen, la diferencia clave está en la dependencia: la composición fuerte implica dependencia total y ciclo de vida conjunto (se llama simplemente composición), mientras que la composición débil implica independencia y se denomina agregación o asociación.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

Cuando una clase utiliza otra únicamente como parámetro de un método, como valor de retorno, mediante una creación puntual con new dentro de un método o como variable local, no se está estableciendo una relación de composición. En estos casos, la relación es más débil y temporal, ya que no existe un vínculo estructural permanente entre ambas clases.

Este tipo de relación se denomina dependencia. La dependencia indica que una clase necesita a otra para realizar alguna operación concreta, pero no la mantiene como parte de su estado interno. Es decir, la clase no tiene a la otra, sino que simplemente la usa en un momento determinado. Por ello, la relación no implica control sobre el ciclo de vida del objeto utilizado.

En cambio, la composición implica que una clase contiene a otra como atributo (normalmente private), formando parte de su estructura interna y estableciendo una relación más fuerte y duradera. Por tanto, en los casos descritos en el enunciado (parámetros, variables locales, uso puntual), se habla de dependencia, no de composición.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

En este caso se pide implementar dos versiones de la relación entre Linea y Punto, diferenciando claramente el tipo de composición. La diferencia clave está en el ciclo de vida de los objetos Punto: en la composición fuerte, los puntos se crean dentro de la propia Linea y no existen fuera de ella; mientras que en la composición débil, los puntos se reciben desde fuera y pueden compartirse con otras líneas.

En la composición fuerte, la clase Linea es responsable de crear sus propios puntos. Esto implica que los puntos no pueden existir sin la línea y su ciclo de vida está completamente ligado. No se reciben objetos Punto desde fuera, sino que se construyen internamente a partir de los valores necesarios.
```java

/**
 * @brief Clase Punto (inmutable)
 */
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        return Math.sqrt(Math.pow(this.x - otro.x, 2) + Math.pow(this.y - otro.y, 2));
    }
}
```
Por otro lado, en la composición débil (agregación), la clase Linea recibe los objetos Punto desde el exterior. Esto implica que los puntos pueden existir independientemente de la línea y pueden ser compartidos entre varias líneas. La línea no controla su creación ni su ciclo de vida.
```java

/**
 * @brief Composición débil: Linea usa puntos externos
 */
public class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}

/**
 * @brief Composición débil: Linea usa puntos externos
 */
public class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```
De este modo, se observa claramente la diferencia: en la composición fuerte, los objetos Punto dependen totalmente de Linea, mientras que en la composición débil pueden existir de forma independiente. Esta distinción es fundamental en diseño orientado a objetos, ya que afecta directamente a la reutilización y al control de los objetos.

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java, en una composición fuerte, los objetos contenidos (como Punto dentro de Linea) se destruyen cuando deja de existir el objeto contenedor. Sin embargo, esta destrucción no ocurre de forma explícita en el código, sino que está gestionada automáticamente por el sistema de memoria del lenguaje.

Esto se debe a que Java utiliza un mecanismo llamado recolector de basura (garbage collector). Cuando un objeto deja de ser accesible (por ejemplo, cuando ya no existe ninguna referencia a una Linea), el recolector de basura detecta que tanto la Linea como los objetos Punto que contiene ya no se usan, y los elimina de memoria automáticamente. Por tanto, no es necesario (ni posible) destruirlos manualmente como en C o C++.

La razón por la que no se observa una destrucción explícita de los Punto es precisamente porque Java abstrae la gestión de memoria. En una composición fuerte, el programador garantiza la relación lógica (los puntos pertenecen a la línea), pero es el entorno de ejecución quien se encarga del ciclo de vida real de los objetos. Esto simplifica el desarrollo y evita errores relacionados con la liberación manual de memoria.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

En este ejemplo se modela una composición débil o asociación, porque los objetos Profesor pueden existir independientemente del Departamento; simplemente colaboran entre sí. Además, se usan dos relaciones a la vez: el departamento tiene varios profesores y también tiene un director, que debe ser uno de esos profesores. La parte delicada del diseño está en mantener la invariante: siempre debe haber director y ese director siempre debe pertenecer a la lista de profesores. Esto encaja con la idea de asociación del tema, donde los objetos colaboran sin que su existencia dependa unos de otros.

Como se pide usar Profesor[] con tamaño máximo 50 y sin romper la encapsulación, no se devuelve nunca el array completo, sino solo el número de profesores y un profesor por posición. Además, como los arrays en Java tienen tamaño fijo una vez creados, se reserva desde el principio el espacio máximo y se lleva un contador con el número real de elementos almacenados. También conviene comprobar bien los índices, porque acceder fuera de rango provoca errores.

Una implementación posible sería la siguiente:
```java

public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre del profesor no puede estar vacío.");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return this.nombre;
    }

    @Override
    public String toString() {
        return "Profesor[nombre=" + this.nombre + "]";
    }
}

public class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final String nombre;
    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(String nombre, Profesor directorInicial) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre del departamento no puede estar vacío.");
        }
        if (directorInicial == null) {
            throw new IllegalArgumentException("El departamento debe crearse con un director.");
        }

        this.nombre = nombre;
        this.profesores = new Profesor[MAX_PROFESORES];
        this.numProfesores = 0;

        this.profesores[this.numProfesores] = directorInicial;
        this.numProfesores++;
        this.director = directorInicial;
    }

    public String getNombre() {
        return this.nombre;
    }

    public Profesor getDirector() {
        return this.director;
    }

    public int getNumProfesores() {
        return this.numProfesores;
    }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= this.numProfesores) {
            throw new IndexOutOfBoundsException("Posición de profesor no válida.");
        }
        return this.profesores[posicion];
    }

    public void anhadirProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor nulo.");
        }
        if (this.numProfesores == MAX_PROFESORES) {
            throw new IllegalStateException("No caben más profesores en el departamento.");
        }

        this.profesores[this.numProfesores] = profesor;
        this.numProfesores++;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El nuevo director no puede ser nulo.");
        }
        if (!perteneceAlDepartamento(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe pertenecer al departamento.");
        }

        this.director = nuevoDirector;
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= this.numProfesores) {
            throw new IndexOutOfBoundsException("Posición de profesor no válida.");
        }

        if (this.profesores[posicion] == this.director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento.");
        }

        for (int i = posicion; i < this.numProfesores - 1; i++) {
            this.profesores[i] = this.profesores[i + 1];
        }

        this.profesores[this.numProfesores - 1] = null;
        this.numProfesores--;
    }

    private boolean perteneceAlDepartamento(Profesor profesor) {
        boolean encontrado = false;
        int i = 0;

        while (i < this.numProfesores && !encontrado) {
            if (this.profesores[i] == profesor) {
                encontrado = true;
            }
            i++;
        }

        return encontrado;
    }
}
```
En esta solución, la invariante se mantiene así: el constructor obliga a crear el departamento con director, ese director se inserta ya en la lista de profesores, el cambio de director solo permite elegir un profesor que ya pertenezca al departamento, y la eliminación impide borrar al director actual. De ese modo, nunca se llega a un estado inválido en el que el departamento carezca de director o en el que el director no forme parte de la lista de profesores.

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

Al emplear List en lugar de arrays, la idea de asociación o composición débil no cambia: el Departamento sigue teniendo varios Profesor y un director, que además debe pertenecer a esa colección. Lo que cambia es la implementación interna. Frente al array, donde el tamaño queda fijado al crearlo, una lista permite crecer y decrecer dinámicamente, por lo que desaparece la necesidad de llevar manualmente parte de la gestión del almacenamiento. En tus apuntes, precisamente se destaca que en los arrays, una vez dimensionados, no se puede variar su tamaño, y que además hay que controlar bien los índices para no salirnos de rango.

Con List, el código se simplifica porque ya no hace falta reservar 50 posiciones, ni mantener un contador numProfesores, ni desplazar manualmente los elementos al eliminar uno. Esa es la parte principal que se ahorra del código original. Además, sigue siendo importante mantener la encapsulación, ya que los apuntes insisten en que los atributos deben ser privados y en que el acceso debe controlarse mediante métodos.

```java
import java.util.ArrayList;
import java.util.List;

public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre del profesor no puede estar vacío.");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return this.nombre;
    }

    @Override
    public String toString() {
        return "Profesor[nombre=" + this.nombre + "]";
    }
}
```
```java
import java.util.ArrayList;
import java.util.List;

public class Departamento {
    private final String nombre;
    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(String nombre, Profesor directorInicial) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre del departamento no puede estar vacío.");
        }
        if (directorInicial == null) {
            throw new IllegalArgumentException("El departamento debe crearse con un director.");
        }

        this.nombre = nombre;
        this.profesores = new ArrayList<>();
        this.profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public String getNombre() {
        return this.nombre;
    }

    public Profesor getDirector() {
        return this.director;
    }

    public int getNumProfesores() {
        return this.profesores.size();
    }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= this.profesores.size()) {
            throw new IndexOutOfBoundsException("Posición de profesor no válida.");
        }
        return this.profesores.get(posicion);
    }

    public void anhadirProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor nulo.");
        }
        if (this.profesores.size() == 50) {
            throw new IllegalStateException("No caben más profesores en el departamento.");
        }

        this.profesores.add(profesor);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El nuevo director no puede ser nulo.");
        }
        if (!this.profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe pertenecer al departamento.");
        }

        this.director = nuevoDirector;
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= this.profesores.size()) {
            throw new IndexOutOfBoundsException("Posición de profesor no válida.");
        }
        if (this.profesores.get(posicion) == this.director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento.");
        }

        this.profesores.remove(posicion);
    }

    public List<Profesor> getProfesores() {
        return List.copyOf(this.profesores);
    }
}
```

Si existiese un método que devolviese todos los profesores a la vez, el problema de devolver directamente la lista interna sería que desde fuera podría modificarse su contenido, rompiendo la encapsulación y también la invariante de la clase. Por ejemplo, podría eliminarse al director o añadirse un elemento sin pasar por las comprobaciones del departamento. Para evitarlo, no debe devolverse la lista interna tal cual, sino una copia defensiva, por ejemplo con List.copyOf(...), como aparece en el código. Así, desde fuera se puede consultar la colección, pero no alterarla directamente.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

Las composiciones recursivas se producen cuando una clase contiene como atributo otra instancia de su misma clase. Es decir, se establece una relación “tiene-un” consigo misma. Este tipo de diseño es útil para modelar estructuras jerárquicas o encadenadas, como ocurre en el caso de una persona que tiene una madre, que a su vez es otra persona. Al igual que en otros casos de composición, puede aplicarse encapsulación para garantizar propiedades como la inmutabilidad.

En este ejemplo, se define una clase Persona inmutable, donde cada objeto puede tener una referencia a su madre. Al ser inmutable, los atributos se declaran private final y no existen métodos que permitan modificarlos tras la construcción. Además, se permite que la madre sea null para representar el final de la cadena (por ejemplo, la abuela si no se conoce más información).
```java

/**
 * @brief Clase Persona inmutable con composición recursiva
 */
public class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede estar vacío.");
        }
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return this.nombre;
    }

    public Persona getMadre() {
        return this.madre;
    }
}
```

A continuación, se muestra un ejemplo en main donde se construye una pequeña cadena familiar desde la abuela hasta el nieto:

```java
public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Carmen", null);
        Persona madre = new Persona("Laura", abuela);
        Persona hija = new Persona("Ana", madre);
        Persona nieto = new Persona("Lucas", hija);

        System.out.println("Nieto: " + nieto.getNombre());
        System.out.println("Madre del nieto: " + nieto.getMadre().getNombre());
        System.out.println("Abuela del nieto: " + nieto.getMadre().getMadre().getNombre());
    }
}
```

Un ejemplo clásico adicional de composición recursiva es una lista enlazada, donde cada nodo contiene un valor y una referencia al siguiente nodo, que es del mismo tipo. También ocurre en estructuras como árboles (cada nodo tiene hijos que son nodos) o en excepciones encadenadas en Java, donde una excepción puede contener otra como causa. Estos casos reflejan cómo una clase puede componerse de sí misma para modelar estructuras complejas.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Las relaciones de composición o asociación bidireccionales son aquellas en las que la relación puede recorrerse en ambos sentidos: no solo una clase conoce a la otra, sino que ambas mantienen una referencia a la otra. En el tema se explica que en composición una clase contenida aparece como atributo de la contenedora, y en asociación dos objetos colaboran entre sí sin depender necesariamente uno del otro. Si esa relación se hace bidireccional, entonces ambas clases pasan a reflejarla explícitamente en sus atributos.

En el ejemplo de Profesor y Departamento, para implementar esa bidireccionalidad habría que hacer que Departamento siga teniendo su colección de Profesor, pero además que cada Profesor tenga un atributo departamento. Como en Java los atributos deben mantenerse encapsulados y el acceso se controla mediante métodos, no bastaría con añadir el atributo: habría que asegurar que los dos lados de la relación se actualicen siempre a la vez. Por ejemplo, al añadir un profesor al departamento, también habría que asignar en ese profesor la referencia a ese departamento; y al eliminarlo, habría que poner su departamento a null.

Eso obliga a cuidar mucho la consistencia. Si se permitiese modificar cada lado por separado, podría aparecer un estado incorrecto, como un Profesor que dice pertenecer a un departamento pero no está en su lista, o al revés. Por eso, lo habitual sería centralizar los cambios en Departamento, dejando en Profesor solo un método restringido para actualizar su departamento, de manera que añadirProfesor, eliminarProfesor y cambiarDirector mantengan siempre sincronizadas ambas clases. En resumen, una relación bidireccional entre Profesor y Departamento requeriría una referencia en cada sentido y métodos que garanticen la actualización coherente de ambas partes.