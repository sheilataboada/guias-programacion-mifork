# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

Las cuatro características básicas de la programación orientada a objetos son abstracción, encapsulamiento, herencia y polimorfismo. Estas propiedades permiten organizar el código en torno a objetos que representan entidades del mundo real, facilitando la reutilización, el mantenimiento y la claridad del programa. A diferencia de la programación estructurada tradicional, donde el foco está en funciones y procedimientos, en este paradigma el elemento central es la clase y los objetos que se crean a partir de ella.

La abstracción consiste en representar únicamente las características esenciales de un objeto, ignorando los detalles innecesarios para el problema que se desea resolver. Por ejemplo, en una clase que represente un coche, puede interesar almacenar su marca y velocidad, pero no necesariamente el funcionamiento interno del motor. La abstracción permite centrarse en qué hace un objeto, más que en cómo lo hace internamente.

El encapsulamiento implica agrupar los datos y los métodos que operan sobre ellos dentro de una misma clase, y controlar el acceso a dichos datos. En Java, esto se consigue utilizando modificadores como private para los atributos y proporcionando métodos públicos (getters y setters) para acceder o modificar su valor. De esta forma, se protege el estado interno del objeto y se evita su manipulación directa desde el exterior.

La herencia permite crear nuevas clases a partir de otras ya existentes, reutilizando su código y extendiendo su comportamiento. Por su parte, el polimorfismo posibilita que distintos objetos respondan de manera diferente ante el mismo mensaje o método. Estas dos características trabajan conjuntamente para favorecer la reutilización y la flexibilidad del software, permitiendo construir jerarquías de clases y comportamientos más generales y especializados.




## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Existen numerosos lenguajes de programación que permiten aplicar el paradigma de la programación orientada a objetos. Entre los más populares se encuentran Java, C++, C# y Python. Estos lenguajes ofrecen mecanismos para definir clases, crear objetos y utilizar conceptos como encapsulamiento, herencia y polimorfismo.

Java es uno de los lenguajes más representativos de la orientación a objetos, ya que prácticamente todo se estructura en torno a clases y objetos. C++, aunque tiene un origen más cercano a la programación estructurada en C, incorpora características orientadas a objetos como clases, herencia y sobrecarga de funciones.

C#, desarrollado por Microsoft, también está fuertemente basado en la programación orientada a objetos y se utiliza ampliamente en el desarrollo de aplicaciones de escritorio, web y videojuegos. Por su parte, Python es un lenguaje multiparadigma que permite trabajar con programación orientada a objetos de forma sencilla y flexible, siendo muy utilizado en ámbitos como la ciencia de datos y el desarrollo web.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La programación estructurada es un paradigma anterior a la programación orientada a objetos que organiza el programa en torno a instrucciones, estructuras de control y subprogramas. Se basa en el uso de secuencias, condicionales y bucles para controlar el flujo de ejecución, evitando el uso indiscriminado de saltos como goto. En este enfoque, el programa se divide en funciones o procedimientos que realizan tareas concretas, pero los datos suelen gestionarse de forma más global.

La programación modular es una evolución dentro del paradigma estructurado que consiste en dividir el programa en módulos independientes, cada uno con una responsabilidad específica. Cada módulo puede contener funciones relacionadas entre sí y trabajar con un conjunto concreto de datos. El objetivo es mejorar la organización del código, facilitar su mantenimiento y permitir la reutilización de partes del programa en otros proyectos.

Mientras que en la programación estructurada el énfasis está en las funciones y el control del flujo, en la programación modular se da un paso más hacia la organización lógica del software. Ambos enfoques preparan el camino para la programación orientada a objetos, donde ya no solo se agrupan funciones, sino también datos y comportamiento dentro de una misma entidad llamada clase.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

En programación orientada a objetos, un objeto se define mediante tres elementos fundamentales: identidad, estado y comportamiento. Estos tres componentes permiten diferenciar un objeto de otro y describir qué información contiene y qué acciones puede realizar. A diferencia de la programación estructurada, donde se trabaja principalmente con funciones y variables separadas, en este paradigma ambos aspectos se agrupan dentro de una misma entidad.

La identidad es la característica que permite distinguir un objeto de otro, aunque tengan los mismos valores en sus atributos. Cada objeto ocupa una posición distinta en memoria, lo que hace que dos objetos puedan ser similares en contenido pero diferentes en identidad. Esta propiedad es esencial para poder trabajar con múltiples instancias de una misma clase.

El estado está formado por los datos o atributos que describen al objeto en un momento determinado. Por ejemplo, en un objeto que represente una cuenta bancaria, el saldo sería parte de su estado. El comportamiento, por su parte, está definido por los métodos que el objeto puede ejecutar, es decir, las acciones que puede realizar o las operaciones que puede aplicar sobre su propio estado.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una clase es una plantilla o modelo que define la estructura y el comportamiento que tendrán los objetos creados a partir de ella. En una clase se especifican los atributos (datos) y los métodos (acciones) que caracterizan a un determinado tipo de entidad. Por ejemplo, una clase que represente un estudiante puede definir atributos como nombre y edad, y métodos como matricularse o mostrarDatos. La clase no es un objeto en sí misma, sino la definición a partir de la cual se crean.

Un objeto no es lo mismo que una clase. Un objeto es una entidad concreta creada a partir de una clase, es decir, una realización específica de esa plantilla. Cuando se crea un objeto de una clase, se dice que se está creando una instancia de dicha clase. Por tanto, instancia y objeto se refieren al mismo concepto: un ejemplar concreto basado en la definición de la clase.

No todos los lenguajes orientados a objetos manejan exactamente el mismo concepto de clase. En lenguajes como Java, C++ o C#, la clase es el elemento fundamental para crear objetos. Sin embargo, existen lenguajes que siguen un modelo basado en prototipos, donde no se utilizan clases de la misma forma tradicional. Aun así, todos los lenguajes orientados a objetos permiten definir estructuras que agrupan datos y comportamiento, aunque el mecanismo interno pueda variar.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

En lenguajes como Java, los objetos se almacenan en una zona de memoria denominada heap (montículo). Cuando se crea un objeto mediante el operador new, se reserva espacio en esa área dinámica de memoria. Las variables que se declaran en métodos no contienen directamente el objeto, sino una referencia que apunta a la dirección donde dicho objeto se encuentra almacenado en el heap. Esto diferencia claramente entre la variable que referencia y el objeto real en memoria.

No es igual en todos los lenguajes. En C++ tradicional, por ejemplo, los objetos pueden almacenarse tanto en memoria automática (pila o stack) como en memoria dinámica (heap), dependiendo de cómo se declaren. Además, en C++ la gestión de la memoria puede hacerse manualmente mediante new y delete, mientras que en Java no existe la liberación manual explícita de objetos por parte del programador.

La recolección de basura (garbage collection) es un mecanismo automático que se encarga de liberar la memoria ocupada por objetos que ya no están siendo utilizados. En Java, el sistema detecta cuándo un objeto deja de tener referencias activas y, en algún momento posterior, libera automáticamente ese espacio de memoria. Este proceso evita muchos errores típicos relacionados con la gestión manual de memoria, como pérdidas de memoria o liberaciones incorrectas.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

Un método es una función definida dentro de una clase que describe una acción o comportamiento que pueden realizar los objetos de esa clase. Mientras que en la programación estructurada se habla simplemente de funciones o procedimientos, en programación orientada a objetos los métodos forman parte de una clase y pueden acceder a los atributos del objeto. De esta forma, los datos y las operaciones que actúan sobre ellos se encuentran agrupados en una misma estructura.

En Java, un método se define indicando su modificador de acceso, el tipo de valor que devuelve (o void si no devuelve nada), el nombre y la lista de parámetros. Cuando se crea un objeto, sus métodos pueden invocarse utilizando el operador punto. Esto permite que cada objeto ejecute comportamientos que pueden modificar su propio estado o devolver información sobre él.

La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre dentro de una misma clase, pero con distinta lista de parámetros (ya sea en número, tipo o ambos). El compilador es capaz de distinguir cuál debe ejecutarse en cada caso en función de los argumentos utilizados en la llamada. La sobrecarga permite reutilizar un mismo nombre para operaciones relacionadas, mejorando la claridad y organización del código sin necesidad de crear nombres diferentes para cada variante.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

A continuación se muestra un ejemplo mínimo de una clase en Java llamada Punto, con dos atributos x e y con visibilidad por defecto (sin modificador de acceso). Se define además un método llamado calculaDistanciaAOrigen, que devuelve la distancia del punto a la posición (0,0) utilizando la fórmula matemática correspondiente. Posteriormente se incluye un ejemplo sencillo de uso mediante la creación de una instancia y la llamada al método.

class Punto {

    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

Ejemplo de uso en una clase principal:

public class Main {

    public static void main(String[] args) {

        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + distancia);
    }
}
En este ejemplo, se crea un objeto p de tipo Punto, se asignan valores a sus atributos y se invoca el método mediante el operador punto. El resultado mostrado será 5.0, ya que corresponde a la distancia del punto (3,4) al origen en el plano cartesiano.


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

El punto de entrada en un programa en Java es el método main. Es el primer método que la máquina virtual de Java (JVM) ejecuta cuando se inicia la aplicación. Su cabecera habitual es public static void main(String[] args). Sin este método con esa firma concreta, el programa no puede comenzar su ejecución, ya que la JVM necesita una referencia clara desde la cual iniciar el flujo del programa.

La palabra clave static indica que el método pertenece a la clase y no a un objeto concreto. Esto significa que puede ejecutarse sin necesidad de crear previamente una instancia de la clase. En el caso de main, esto es imprescindible, ya que cuando el programa arranca todavía no existe ningún objeto creado por el usuario. No obstante, static no se utiliza únicamente en el método main; también puede emplearse en otros métodos o atributos que deban pertenecer a la clase en general y no a cada objeto individual.

La palabra clave final se utiliza para indicar que un elemento no puede modificarse una vez asignado. Cuando se combina con variables, impide que su valor cambie después de la inicialización. En el caso de métodos, final evita que puedan ser redefinidos en clases hijas. Aunque main no suele declararse como final, esta combinación puede aparecer en otros contextos para reforzar la inmutabilidad o evitar modificaciones en la herencia.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Para compilar y ejecutar un programa en Java desde la línea de comandos se utilizan los comandos javac y java. Primero se escribe el programa en un archivo con extensión .java, por ejemplo Main.java. Para compilarlo se ejecuta javac Main.java, lo que genera un archivo llamado Main.class. Después, para ejecutar el programa, se utiliza el comando java Main, indicando únicamente el nombre de la clase que contiene el método main, sin la extensión. Ambos comandos deben ejecutarse en el directorio donde se encuentra el archivo fuente.

Java es un lenguaje compilado, pero su proceso es diferente al de lenguajes como C o C++. El compilador javac no genera directamente código máquina, sino un formato intermedio denominado byte-code. Este byte-code es independiente del sistema operativo y del procesador, lo que permite que el mismo programa pueda ejecutarse en distintas plataformas sin necesidad de recompilarlo.

La máquina virtual de Java (JVM) es el entorno que se encarga de ejecutar el byte-code. Cuando se utiliza el comando java, la JVM carga el archivo .class, que contiene el byte-code generado durante la compilación. Los ficheros .class almacenan ese código intermedio que la JVM interpreta o traduce internamente a código máquina, haciendo posible la portabilidad característica de Java.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

La palabra clave new se utiliza en Java para crear un objeto a partir de una clase. Cuando se escribe una instrucción como Punto p = new Punto();, se está reservando memoria para un nuevo objeto en el heap y se está devolviendo una referencia a ese objeto. Sin el uso de new, únicamente se tendría una variable de tipo referencia, pero no existiría todavía ningún objeto real en memoria.

Un constructor es un método especial que se ejecuta automáticamente cuando se crea un objeto mediante new. Su nombre debe coincidir exactamente con el nombre de la clase y no tiene tipo de retorno, ni siquiera void. El constructor se utiliza para inicializar los atributos del objeto en el momento de su creación, asegurando que comience en un estado válido.

A continuación se muestra un ejemplo de una clase Empleado con un constructor que inicializa los atributos dni, nombre y apellidos:

class Empleado {

    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

En este ejemplo, el constructor recibe tres parámetros y asigna sus valores a los atributos del objeto utilizando this, que permite distinguir los atributos de la clase de los parámetros con el mismo nombre. Al crear un objeto con new Empleado("12345678A", "Ana", "López"), el constructor se ejecuta automáticamente e inicializa el estado del nuevo objeto.


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

La referencia this en Java se utiliza para hacer referencia al objeto actual, es decir, al propio objeto que está ejecutando el método o el constructor en ese momento. Permite acceder explícitamente a los atributos o métodos de la clase cuando existe ambigüedad, por ejemplo, cuando los parámetros de un constructor tienen el mismo nombre que los atributos. De esta forma, se distingue claramente entre el atributo del objeto y la variable local o parámetro.

No todos los lenguajes utilizan exactamente la misma palabra clave, aunque el concepto suele existir en la mayoría de lenguajes orientados a objetos. En Java y C++ se emplea this, mientras que en otros lenguajes como Python se utiliza self. Aunque el nombre cambie, la idea es la misma: representar al objeto actual dentro de su propia definición.

A continuación se muestra un ejemplo de uso de this en la clase Punto:

class Punto {

    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}

En el constructor, this.x y this.y permiten diferenciar los atributos del objeto de los parámetros recibidos. Aunque en el método no sería estrictamente obligatorio escribir this.x, su uso hace explícito que se está accediendo al estado del propio objeto.


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

Se puede añadir un nuevo método a la clase Punto que permita calcular la distancia entre el objeto actual (this) y otro objeto de tipo Punto recibido como parámetro. Este método debe aplicar la fórmula de la distancia entre dos puntos en el plano: 
sqrt((x2 - x1)^2 + (y2 - y1)^2)
De esta forma, cada objeto Punto podrá calcular su distancia respecto a cualquier otro punto.

El método recibirá como parámetro otro objeto Punto y utilizará sus atributos junto con los del objeto actual. Para acceder al punto actual se emplea this, mientras que para el punto recibido se utiliza el nombre del parámetro. El resultado será un valor de tipo double.

A continuación se muestra la clase Punto con el nuevo método añadido:

class Punto {

    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    double distanciaA(Punto otro) {
        double diferenciaX = otro.x - this.x;
        double diferenciaY = otro.y - this.y;
        return Math.sqrt(diferenciaX * diferenciaX + diferenciaY * diferenciaY);
    }
}

Con este método, si se tienen dos objetos p1 y p2, se puede calcular la distancia entre ellos mediante p1.distanciaA(p2). El cálculo se realiza utilizando los valores almacenados en ambos objetos, demostrando cómo los métodos pueden trabajar con otros objetos como parámetros.




## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

En Java, el paso de parámetros siempre se realiza por valor, pero es importante distinguir qué significa esto cuando se trabaja con objetos. Cuando se pasa un objeto como Punto a un método, no se copia el objeto completo, sino el valor de su referencia. Es decir, el método recibe una copia de la referencia que apunta al mismo objeto en memoria. Por tanto, aunque la referencia sea una copia, ambos apuntan al mismo objeto.

Esto implica que si dentro del método se modifica algún atributo del objeto recibido, los cambios sí se reflejan fuera del método, ya que el objeto es el mismo. Sin embargo, si dentro del método se asigna la referencia a un nuevo objeto (por ejemplo, otro = new Punto(...)), ese cambio no afecta a la referencia original fuera del método, porque solo se ha modificado la copia de la referencia.

En cambio, si en lugar de un objeto se recibe un tipo primitivo como int, lo que se copia es el propio valor numérico. Si ese entero se modifica dentro del método, el cambio no afecta a la variable original fuera del mismo. Esto ocurre porque los tipos primitivos en Java se pasan por valor y no existe ningún mecanismo de paso por referencia directa como en otros lenguajes.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

El método toString() en Java es un método que permite obtener una representación en forma de texto de un objeto. Está definido en la clase base Object, de la cual heredan todas las clases en Java. Por defecto, devuelve una cadena que incluye el nombre de la clase y un código hash, pero normalmente se redefine para mostrar información más útil y legible sobre el estado del objeto.

Este concepto también existe en otros lenguajes orientados a objetos, aunque puede tener otro nombre. Por ejemplo, en C++ se suele sobrecargar el operador de salida <<, y en Python existe el método __str__(). En todos los casos, la idea es proporcionar una representación textual del objeto que facilite su impresión o depuración.

A continuación se muestra un ejemplo de redefinición de toString() en la clase Punto:

class Punto {

    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}

Con esta redefinición, si se ejecuta System.out.println(p); donde p es un objeto Punto, se mostrará algo como Punto(3.0, 4.0) en lugar del formato por defecto menos informativo.


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta

Una clase en Java puede parecer similar a un struct en C, ya que ambos permiten agrupar varios datos bajo un mismo tipo. En C, un struct define una estructura que contiene variables relacionadas entre sí, y las variables de ese tipo almacenan directamente esos datos. Desde este punto de vista, una clase también agrupa atributos, por lo que existe una cierta semejanza básica en cuanto a organización de información.

Sin embargo, a un struct en C le faltan varios elementos para ser equivalente a una clase orientada a objetos. En primer lugar, una clase no solo contiene datos, sino también métodos que definen el comportamiento asociado a esos datos. Además, una clase permite aplicar principios como encapsulamiento, herencia y polimorfismo, mientras que un struct en C tradicional solo agrupa datos y no incorpora directamente mecanismos de control de acceso ni de reutilización mediante herencia.

Para que un struct se comportara como una clase completa, debería incluir métodos asociados al tipo, mecanismos de visibilidad (como público y privado) y soporte para herencia y polimorfismo. En Java, cuando se declara una variable de tipo clase y se crea con new, se está creando una instancia con identidad propia y comportamiento asociado, algo que en C requiere técnicas adicionales y no forma parte del modelo básico del lenguaje.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

La clase Punto de Java se podría “emular” en C utilizando un struct para representar los datos y una función externa que reciba dicho struct como parámetro para realizar el cálculo. En C no existen métodos asociados directamente al tipo, por lo que el comportamiento debe implementarse mediante funciones independientes. De este modo, la agrupación de datos se mantiene, pero la organización no es completamente equivalente a la de una clase.

Un ejemplo en C podría ser el siguiente:

#include <stdio.h>
#include <math.h>

struct Punto {
    double x;
    double y;
};

double calculaDistanciaAOrigen(struct Punto p) {
    return sqrt(p.x * p.x + p.y * p.y);
}

En este caso, la función calculaDistanciaAOrigen recibe una copia del struct Punto y utiliza sus campos para realizar el cálculo. No existe ningún método dentro del struct, ya que en C los tipos de datos no contienen funciones asociadas directamente.

Respecto a this, en C no existe un mecanismo automático equivalente. En Java, this representa el objeto actual que está ejecutando el método. En C, ese papel lo cumple explícitamente el parámetro que se pasa a la función. Es decir, el objeto que en Java estaría implícito como this, en C debe pasarse manualmente como argumento a la función.
