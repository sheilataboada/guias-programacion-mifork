# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

La composición en C se utiliza cuando una estructura está formada por otras estructuras, expresando una relación del tipo “tiene-un”. Esto permite construir modelos más complejos a partir de elementos más simples. Por ejemplo, una línea puede considerarse como un objeto que tiene dos puntos, y cada punto, a su vez, tiene dos coordenadas (x e y). Este enfoque facilita la organización de los datos y mejora la claridad del programa.

En este contexto, se puede definir una estructura Punto con dos atributos (x, y) y una estructura Linea que contenga dos objetos de tipo Punto. Además, es habitual acompañar estas estructuras con funciones que operen sobre ellas, como el cálculo de la distancia entre dos puntos o la longitud de una línea, reutilizando la lógica definida previamente.

A continuación se muestra un ejemplo completo en C que implementa esta idea:

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


Este ejemplo ilustra claramente la composición: la estructura Linea no almacena datos simples directamente, sino que contiene otras estructuras (Punto). Además, se observa cómo la función de longitud reutiliza la función de distancia, lo que favorece la modularidad y evita duplicar código.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

En orientación a objetos, la composición se expresa mediante clases que contienen objetos de otras clases como atributos, manteniendo la relación “tiene-un”. En este caso, una clase Linea estará compuesta por dos objetos de tipo Punto. A diferencia de C, Java permite aplicar encapsulación para ocultar los datos internos y controlar su acceso, lo que facilita garantizar ciertas propiedades, como la inmutabilidad.

Para conseguir que los objetos sean inmutables, se declaran los atributos como private y final, y no se proporcionan métodos setters. De este modo, una vez creado un objeto Punto o Linea, sus valores no pueden modificarse. Además, la funcionalidad se incorpora dentro de las propias clases mediante métodos, como el cálculo de la distancia entre puntos o la longitud de una línea, aprovechando así las ventajas de la programación orientada a objetos frente al enfoque procedural.

A continuación se muestra una posible implementación en Java:

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

En este diseño se observa claramente la composición: la clase Linea tiene dos objetos Punto. Además, gracias a la encapsulación y al uso de atributos final, se garantiza que ni los puntos ni la línea puedan modificarse tras su creación, lo que mejora la seguridad y coherencia del programa respecto a la solución en C.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta