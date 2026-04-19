# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

La herencia en orientación a objetos permite crear una clase nueva a partir de otra ya existente. La idea importante es la relación “es-un”: una subclase debe ser realmente un tipo más concreto de la superclase. Por eso se dice, por ejemplo, que Círculo es un tipo de Figura. En Java se expresa con extends, y en los apuntes también se insiste en que no debe usarse la herencia solo por comodidad o para reutilizar código sin que exista esa clasificación real.

Las dos consecuencias principales son estas. La primera es la compatibilidad de tipos: un objeto de la subclase puede tratarse como si fuese de la superclase. En los apuntes esto aparece como conversión a la superclase o upcasting, y se indica que una referencia de la superclase puede recibir objetos de sus subclases; además, eso permite crear arrays de la superclase con elementos que en realidad son objetos de distintas subclases. La segunda es la herencia de estado y comportamiento: la subclase reutiliza atributos y métodos definidos en la superclase y, además, puede añadir los suyos propios o reescribir métodos si fuese necesario.

En el ejemplo pedido, Artillero y Zapador son tipos de Soldado, así que pueden guardarse en un array de Soldado y todos pueden usar el método saludar(). Como nombre es private, no se accede directamente desde las subclases; lo correcto es que Soldado lo inicialice en su constructor y lo utilice dentro de saludar(). Las subclases solo añaden su información específica, que en este caso son el número de cohetes y el número de minas, accesibles mediante sus getters.
```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre + ".");
    }
}

class Artillero extends Soldado {
    private int numeroCohetes;

    public Artillero(String nombre, int numeroCohetes) {
        super(nombre);
        this.numeroCohetes = numeroCohetes;
    }

    public int getNumeroCohetes() {
        return numeroCohetes;
    }
}

class Zapador extends Soldado {
    private int numeroMinas;

    public Zapador(String nombre, int numeroMinas) {
        super(nombre);
        this.numeroMinas = numeroMinas;
    }

    public int getNumeroMinas() {
        return numeroMinas;
    }
}

public class Principal {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];

        ejercito[0] = new Soldado("Luis");
        ejercito[1] = new Artillero("Ana", 12);
        ejercito[2] = new Zapador("Marta", 8);
        ejercito[3] = new Artillero("Pablo", 20);

        for (int i = 0; i < ejercito.length; i++) {
            ejercito[i].saludar();
        }

        Artillero artillero = new Artillero("Diego", 15);
        Zapador zapador = new Zapador("Sara", 6);

        System.out.println(artillero.getNombre() + " tiene "
                + artillero.getNumeroCohetes() + " cohetes.");
        System.out.println(zapador.getNombre() + " tiene "
                + zapador.getNumeroMinas() + " minas.");
    }
}
```


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al crear un objeto de una subclase, no se ejecuta solo su constructor, sino también los de las clases que están por encima en la jerarquía. En los apuntes se indica que los constructores se ejecutan en orden “descendente”: primero se crea el objeto de la subclase, pero el constructor de esa subclase se detiene y transfiere el control al constructor de la superclase; si esa superclase tiene otra superclase, el proceso se repite. Por tanto, en un caso como Artillero extends Soldado, al crear un Artillero se ejecutan dos constructores: primero el de Soldado y después el de Artillero.

Dentro de un constructor, super se usa para referirse a la superclase. En el tema de herencia aparece expresamente que super(argumentos) sirve para invocar de forma explícita el constructor de la clase base, y además debe escribirse como primera sentencia del constructor de la subclase. Esto encaja con la idea de que antes de inicializar la parte específica del subtipo, primero debe quedar correctamente inicializada la parte heredada. También se explica que super permite acceder a la versión de la superclase del objeto referenciado por this.

Si la clase base tiene un constructor sin parámetros y es accesible, Java lo invoca automáticamente, por lo que no es obligatorio escribir super() de forma explícita. En cambio, si la clase base no tiene visible ese constructor sin parámetros y solo dispone, por ejemplo, de constructores con parámetros, entonces la subclase sí debe llamar a super(argumentos) obligatoriamente. Los apuntes lo dicen de forma directa: si el constructor de la clase base no tiene parámetros, la llamada es implícita; si está parametrizado, la clase derivada debe invocarlo explícitamente con super(argumentos).


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Sí, los atributos privados de la superclase forman parte de una instancia de la subclase en memoria. Al fin y al cabo, al crear un objeto de una subclase también se ejecuta el constructor de la superclase, precisamente para dejar inicializada esa parte heredada del objeto. Además, en los apuntes se explica que los atributos definidos a nivel de clase mantienen su valor mientras el objeto perdure, es decir, forman parte de su estado. Por eso, un Artillero no solo tiene sus propios datos, sino también la parte de Soldado, aunque esa parte haya sido definida en la clase base.

Ahora bien, que esos atributos existan dentro del objeto no significa que puedan usarse directamente desde el código de la subclase. En Java, private significa que ese elemento solo puede ser accedido dentro de la clase en la que fue definido. Por tanto, si Soldado tiene private String nombre;, ese nombre está dentro del objeto Artillero, pero el código de Artillero no puede escribir simplemente nombre ni modificarlo de forma directa. Para eso haría falta usar métodos públicos o protegidos de Soldado, como un getNombre(), o bien cambiar la visibilidad si el diseño lo justificase.

Con el ejemplo pedido, al crear Artillero art = new Artillero("Ana", 10);, el objeto art contiene el nombre heredado de Soldado y también numeroCohetes propio de Artillero. Sin embargo, dentro de Artillero no se podría hacer System.out.println(nombre); si nombre es privado. Lo correcto sería que Soldado gestionase ese dato, por ejemplo con saludar() o con un getter. En otras palabras: sí forma parte del objeto en memoria, pero no por eso deja de respetarse la encapsulación.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad a nivel de tipos implica que un objeto de una subclase puede tratarse como si fuese un objeto de la superclase. En términos de extensibilidad, esto permite añadir nuevos tipos concretos sin tener que rehacer el código que ya trabaja con referencias del tipo general. Si todos los subtipos son Soldado, entonces el código que maneja un conjunto de Soldado sigue siendo válido aunque aparezcan clases nuevas como Medico, Explorador o cualquier otra.

La ventaja práctica es que el programa queda más fácil de ampliar y mantener. El código cliente no necesita saber todos los tipos concretos que existen, sino solo que todos ellos son compatibles con Soldado y que comparten el comportamiento común que interesa, en este caso saludar(). Por eso, al añadir un nuevo subtipo, lo normal es modificar únicamente la parte donde se crean objetos, pero no la parte que los recorre para pedirles que saluden. Esa es precisamente la idea de reutilizar una superclase como tipo común.


```java
class Soldado {
    private String nombre; // atributo comun a todos los soldados

    public Soldado(String nombre) {
        this.nombre = nombre; // inicializa el nombre del soldado
    }

    public String getNombre() {
        return nombre; // devuelve el nombre
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre + "."); // saludo comun
    }
}

class Artillero extends Soldado {
    private int numeroCohetes; // dato especifico del artillero

    public Artillero(String nombre, int numeroCohetes) {
        super(nombre); // inicializa la parte heredada
        this.numeroCohetes = numeroCohetes; // inicializa su dato propio
    }

    public int getNumeroCohetes() {
        return numeroCohetes; // devuelve el numero de cohetes
    }
}

class Zapador extends Soldado {
    private int numeroMinas; // dato especifico del zapador

    public Zapador(String nombre, int numeroMinas) {
        super(nombre); // inicializa la parte heredada
        this.numeroMinas = numeroMinas; // inicializa su dato propio
    }

    public int getNumeroMinas() {
        return numeroMinas; // devuelve el numero de minas
    }
}

class Medico extends Soldado {
    private int numeroBotiquines; // nuevo tipo de soldado con su propio dato

    public Medico(String nombre, int numeroBotiquines) {
        super(nombre); // inicializa la parte heredada
        this.numeroBotiquines = numeroBotiquines; // inicializa su dato propio
    }

    public int getNumeroBotiquines() {
        return numeroBotiquines; // devuelve el numero de botiquines
    }
}

public class Principal {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[5]; // array del tipo general

        ejercito[0] = new Soldado("Luis"); // soldado base
        ejercito[1] = new Artillero("Ana", 12); // subtipo compatible con Soldado
        ejercito[2] = new Zapador("Marta", 8); // subtipo compatible con Soldado
        ejercito[3] = new Artillero("Pablo", 20); // otro artillero
        ejercito[4] = new Medico("Elena", 3); // nuevo subtipo añadido

        // Este codigo no se modifica aunque se añada un nuevo tipo de Soldado
        for (int i = 0; i < ejercito.length; i++) {
            ejercito[i].saludar(); // todos pueden saludar porque todos son Soldado
        }
    }
}
```
En este ejemplo, la novedad es la clase Medico. Sin embargo, el bucle que pide el saludo no cambia en absoluto, porque sigue trabajando con referencias de tipo Soldado. Esa es la idea principal de la extensibilidad: se puede ampliar la jerarquía con nuevos subtipos sin tener que reescribir el código general que usa el tipo común.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
