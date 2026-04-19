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

Sí, se puede tener una referencia del supertipo que apunte a un objeto real de un subtipo. Eso ocurre porque la herencia también establece una relación de tipos: un Artillero puede tratarse como un Soldado. A esta conversión hacia arriba se le llama upcasting, y es segura. Gracias a ello, en un array de Soldado pueden almacenarse objetos Soldado, Artillero o cualquier otro subtipo compatible.

Ahora bien, con una referencia de tipo Soldado no se pueden invocar directamente métodos que solo existan en Artillero, porque el compilador solo deja usar lo que pertenece al tipo declarado de la referencia. Sí puede ocurrir que se invoque un método heredado o reescrito, como saludar(), pero si se quiere acceder a algo específico del subtipo, como getNumeroCohetes(), hace falta convertir la referencia al subtipo real. Esa conversión hacia abajo se llama downcasting, y no siempre es segura, porque no toda referencia Soldado apunta realmente a un Artillero.

Para comprobar en tiempo de ejecución si el objeto real pertenece a un subtipo concreto se utiliza instanceof. Si el resultado es verdadero, entonces puede hacerse el cast con seguridad razonable y acceder al comportamiento específico de ese subtipo. Esta técnica puede ser útil en casos concretos, aunque no conviene abusar de ella, porque si se necesitase continuamente distinguir tipos concretos, el diseño podría estar perdiendo generalidad.

``` java
class Soldado {
    private String nombre; // nombre comun a todos los soldados

    public Soldado(String nombre) {
        this.nombre = nombre; // inicializa el nombre
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
        return numeroCohetes; // devuelve los cohetes
    }
}

class Zapador extends Soldado {
    private int numeroMinas; // dato especifico del zapador

    public Zapador(String nombre, int numeroMinas) {
        super(nombre); // inicializa la parte heredada
        this.numeroMinas = numeroMinas; // inicializa su dato propio
    }

    public int getNumeroMinas() {
        return numeroMinas; // devuelve las minas
    }
}

public class Principal {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4]; // array del supertipo

        ejercito[0] = new Soldado("Luis"); // objeto del tipo base
        ejercito[1] = new Artillero("Ana", 12); // upcasting implicito
        ejercito[2] = new Zapador("Marta", 8); // upcasting implicito
        ejercito[3] = new Artillero("Pablo", 20); // upcasting implicito

        for (int i = 0; i < ejercito.length; i++) {
            ejercito[i].saludar(); // todos pueden saludar porque todos son Soldado

            if (ejercito[i] instanceof Artillero) { // comprueba el tipo real del objeto
                Artillero artillero = (Artillero) ejercito[i]; // downcasting
                System.out.println(
                    artillero.getNombre() + " tiene "
                    + artillero.getNumeroCohetes() + " cohetes."
                ); // acceso a un metodo especifico de Artillero
            }
        }
    }
}
```


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El acceso protected es un nivel de visibilidad intermedio entre private y public. Un miembro protected no queda accesible para cualquiera, pero sí puede utilizarse desde las subclases. Esto resulta útil cuando se quiere mantener cierta ocultación de información respecto al exterior y, al mismo tiempo, permitir que las clases derivadas reutilicen directamente un atributo o método heredado sin necesidad de hacerlo completamente público.

En Java se implementa escribiendo la palabra reservada protected delante del atributo o del método. Si, por ejemplo, en Soldado se define protected String nombre;, ese atributo seguirá perteneciendo a Soldado, pero podrá ser usado dentro de Zapador o Artillero. La idea es que el dato no quede abierto para cualquier clase, pero sí para aquellas que forman parte de la jerarquía de herencia. Así, Zapador podría usar nombre directamente en su método ponerBombas().

En el ejemplo pedido, nombre deja de ser privado y pasa a ser protegido para que la subclase pueda acceder a él sin necesidad de un getter. De este modo, Zapador puede utilizar ese atributo heredado para mostrar qué soldado está poniendo minas o bombas. Es una solución válida cuando se desea que el acceso exista solo para las subclases, aunque en otros diseños también podría preferirse mantener private y acceder mediante métodos públicos o protegidos.
 
``` java
class Soldado {
    protected String nombre; // atributo accesible desde las subclases

    public Soldado(String nombre) {
        this.nombre = nombre; // inicializa el nombre del soldado
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre + "."); // metodo comun
    }
}

class Zapador extends Soldado {
    private int numeroMinas; // dato propio del zapador

    public Zapador(String nombre, int numeroMinas) {
        super(nombre); // inicializa la parte heredada
        this.numeroMinas = numeroMinas; // inicializa el dato propio
    }

    public int getNumeroMinas() {
        return numeroMinas; // devuelve el numero de minas
    }

    public void ponerBombas() {
        System.out.println(nombre + " esta poniendo "
                + numeroMinas + " minas."); // usa directamente el atributo protected
    }
}

public class Principal {
    public static void main(String[] args) {
        Zapador zapador = new Zapador("Marta", 8); // crea un zapador
        zapador.saludar(); // metodo heredado de Soldado
        zapador.ponerBombas(); // metodo propio de Zapador
    }
}
``` 


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

No en todos los lenguajes orientados a objetos tiene por qué existir una única clase base común para todos los objetos. Eso depende del diseño del lenguaje y de cómo organice su jerarquía de tipos. En algunos casos sí existe una raíz común para todos los objetos, mientras que en otros el modelo no obliga a que todo pase por una única clase base universal. Por tanto, no conviene asumir esa idea como una regla general de toda la orientación a objetos.

En Java sí ocurre. Existe una clase común llamada Object, que actúa como superclase de todas las clases. Esto significa que cualquier clase definida en Java es, directa o indirectamente, una extensión de Object, incluso aunque no se escriba explícitamente extends Object. Por eso se dice que en Java hay una herencia implícita desde esa clase base común.

La consecuencia práctica es que todos los objetos de Java comparten ciertos comportamientos generales heredados de Object, como por ejemplo la posibilidad de disponer de una representación en forma de cadena mediante toString(). Después, cada clase concreta puede mantener ese comportamiento tal cual o redefinirlo si se necesita una versión más específica. Así, Soldado, Artillero o Zapador serían todos, al mismo tiempo, objetos de sus propias clases y también objetos de Object.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple consiste en que una misma clase pueda heredar directamente de más de una superclase al mismo tiempo. Es decir, una clase derivada recibiría estado y comportamiento de varias clases base distintas. La idea puede parecer útil porque permitiría reutilizar elementos procedentes de varias jerarquías, pero también complica mucho el diseño, ya que pueden aparecer conflictos si dos superclases definen miembros con el mismo nombre o comportamientos incompatibles.

En Java no existe herencia múltiple de clases. En Java solo se permite la herencia simple, es decir, una clase puede extender directamente de una sola clase mediante extends. Por eso, una clase como Artillero puede ser subclase de Soldado, pero no podría extender a la vez de Soldado y de otra clase distinta. Así, la jerarquía de clases queda más clara y se evitan ambigüedades.

Por tanto, la respuesta es que la herencia múltiple sí existe como concepto general en orientación a objetos, pero Java no la admite entre clases. En Java, cada clase tiene una única superclase directa, aunque después pueda seguir existiendo una cadena de herencia hacia arriba.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

Las excepciones en Java son objetos y, por eso, también pueden especializarse mediante herencia. Cuando interesa distinguir distintos errores, puede definirse una clase propia para representar un caso concreto. Si además se quiere que la excepción sea no controlada, lo habitual es hacer que herede de RuntimeException, de modo que no exista obligación de capturarla o declararla con throws. La idea es la misma que en otras jerarquías: se parte de una clase más general y se crea un subtipo más específico para clasificar mejor el error.

En el ejemplo pedido, la excepción UsuarioNoEncontradoException no solo hereda comportamiento de la jerarquía de excepciones, sino que además está compuesta con un objeto Usuario. Eso permite guardar dentro de la excepción el usuario concreto que provocó el problema. Así, el error no solo transmite un mensaje, sino también información adicional útil para consultar después mediante un getter. Esa relación es de composición porque la excepción contiene como atributo una referencia a otro objeto.

Para incluir la causa subyacente, basta con sobrecargar el constructor y ofrecer una segunda versión que reciba también un Throwable. En esa versión se llama a super(mensaje, causa), de forma que la excepción personalizada conserve tanto su propio contexto como el error original que la provocó. Con ello se consigue una excepción más completa: clasifica el error, guarda el Usuario implicado y, si hace falta, encadena la causa interna.

``` java
class Usuario {
    private String nombre; // nombre del usuario
    private String identificador; // identificador del usuario

    public Usuario(String nombre, String identificador) {
        this.nombre = nombre; // inicializa el nombre
        this.identificador = identificador; // inicializa el identificador
    }

    public String getNombre() {
        return nombre; // devuelve el nombre
    }

    public String getIdentificador() {
        return identificador; // devuelve el identificador
    }

    public String toString() {
        return nombre + " (" + identificador + ")"; // representacion del usuario
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario; // composicion: la excepcion contiene un Usuario

    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje); // envia el mensaje a la superclase
        this.usuario = usuario; // guarda el usuario que dio el problema
    }

    public UsuarioNoEncontradoException(String mensaje, Usuario usuario,
            Throwable causa) {
        super(mensaje, causa); // guarda mensaje y causa subyacente
        this.usuario = usuario; // guarda el usuario que dio el problema
    }

    public Usuario getUsuario() {
        return usuario; // devuelve el usuario asociado al error
    }
}

public class Principal {
    public static void main(String[] args) {
        Usuario usuario = new Usuario("Ana", "u123"); // usuario de ejemplo

        try {
            // ejemplo de lanzamiento sin causa
            throw new UsuarioNoEncontradoException(
                    "No se ha encontrado el usuario.", usuario);
        } catch (UsuarioNoEncontradoException excepcion) {
            System.out.println(excepcion.getMessage()); // muestra el mensaje
            System.out.println(excepcion.getUsuario()); // muestra el usuario
        }

        try {
            // ejemplo de lanzamiento con causa subyacente
            NullPointerException causa = new NullPointerException(
                    "Fallo interno al buscar en memoria.");

            throw new UsuarioNoEncontradoException(
                    "No se ha encontrado el usuario.", usuario, causa);
        } catch (UsuarioNoEncontradoException excepcion) {
            System.out.println(excepcion.getMessage()); // muestra el mensaje
            System.out.println(excepcion.getUsuario()); // muestra el usuario
            System.out.println(excepcion.getCause()); // muestra la causa
        }
    }
}
```


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
