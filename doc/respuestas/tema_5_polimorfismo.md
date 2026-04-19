# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

El **polimorfismo** es la capacidad de tratar objetos de clases distintas de una forma común, siempre que todos sepan responder al mismo método. Dicho de manera sencilla, permite enviar el mismo mensaje a objetos diferentes y que cada uno responda según su propio tipo. Esto resulta útil porque hace posible trabajar con una referencia más general, por ejemplo de una superclase, sin necesidad de conocer exactamente la subclase concreta del objeto en cada momento.

Su utilidad principal está en que el código se vuelve más general, más flexible y más fácil de mantener. Así, se puede recorrer, por ejemplo, un conjunto de objetos de una misma jerarquía e invocar el mismo método sobre todos ellos, dejando que cada objeto ejecute la versión que le corresponde. Esto se apoya en el **enlace tardío**, es decir, la decisión de qué método concreto se ejecuta se toma en tiempo de ejecución.

La **sobreescritura** de métodos consiste en que una subclase redefine un método heredado de la superclase para darle un comportamiento propio. El método debe mantener el mismo nombre y la misma lista de parámetros, pero su implementación cambia para adaptarse al caso concreto de la subclase. De este modo, aunque se invoque el método mediante una referencia general, se ejecutará la versión adecuada al objeto real.

La sobreescritura es una pieza clave para que exista polimorfismo en herencia. Gracias a ella, una superclase puede definir una operación común y cada subclase especializarla según sus necesidades. Así, no se necesita preguntar constantemente de qué clase es cada objeto para saber qué hacer con él, sino que cada objeto responde por sí mismo de la forma correcta.



## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La ligadura dinámica o enlace tardío consiste en que la decisión sobre qué método concreto se ejecuta no se cierra en compilación, sino en tiempo de ejecución, según el tipo real del objeto sobre el que se hace la llamada. Su relación con el polimorfismo es directa: gracias a ese mecanismo se puede trabajar con una referencia más general y, aun así, hacer que se ejecute la versión redefinida en la subclase correspondiente. Sin enlace tardío, una llamada así quedaría resuelta de forma fija y el comportamiento polimórfico se perdería.

En C++ sí hay que indicarlo de forma explícita cuando se quiere polimorfismo dinámico mediante herencia: el método de la clase base debe declararse como virtual, y entonces la llamada se resuelve dinámicamente; si no es virtual, la resolución normal es estática. En Java, en cambio, los métodos de instancia redefinidos usan despacho dinámico de forma natural, por lo que no existe un equivalente a tener que escribir virtual en cada método; lo que sí puede hacerse es impedir la redefinición con final, y además los métodos static no se sobreescriben, sino que se ocultan. Por eso puede decirse que en C++ el polimorfismo dinámico debe marcarse más explícitamente, mientras que en Java forma parte del comportamiento habitual de los métodos de instancia.

En Python, el comportamiento es todavía más dinámico. Las clases derivadas pueden redefinir métodos de sus clases base y la propia documentación explica, pensando precisamente en quien viene de C++, que los métodos en Python son “efectivamente virtuales”. Además, Python favorece mucho el duck typing: importa menos la clase exacta del objeto y más que el objeto tenga el método o atributo que se necesita. Por eso, en Python tampoco se marca el enlace dinámico con una palabra como virtual; normalmente basta con definir métodos con el mismo nombre y dejar que la llamada se resuelva sobre el objeto real en ejecución.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

Para un ejemplo sencillo, puede colocarse todo en un único fichero, por ejemplo `Principal.java`. Se define una clase `Soldado` con un método `saludar()`, y dos subclases: `Zapador` y `Artillero`. En este caso, **`Zapador` sobrescribe** `saludar()` y sustituye por completo el comportamiento heredado, mientras que `Artillero` mantiene el comportamiento general de `Soldado`.

La idea del **polimorfismo** se ve al crear un array de tipo `Soldado[]`. Aunque el array está declarado con referencias de la superclase, dentro puede almacenar objetos de subclases distintas. Después, al recorrer el array y llamar a `saludar()`, no se ejecuta siempre la versión de `Soldado`, sino la que corresponda al objeto real almacenado en cada posición.

Así, cuando la referencia apunta a un objeto `Zapador`, se ejecuta la versión redefinida en `Zapador`; cuando apunta a un objeto `Artillero`, se ejecuta la versión heredada de `Soldado`. Eso muestra que una misma llamada escrita de la misma forma puede producir comportamientos distintos según el objeto real que reciba el mensaje.

```java
public class Principal {

    public static void main(String[] args) {

        // Array de referencias de tipo Soldado
        Soldado[] soldados = new Soldado[2];

        // En cada posición se guarda un objeto de una subclase distinta
        soldados[0] = new Zapador();
        soldados[1] = new Artillero();

        // Se recorre el array usando referencias de tipo Soldado
        for (int i = 0; i < soldados.length; i++) {
            soldados[i].saludar();
        }
    }
}

class Soldado {

    public void saludar() {
        System.out.println("Soy un soldado y saludo de forma general.");
    }
}

class Zapador extends Soldado {

    @Override
    public void saludar() {
        System.out.println("Soy un zapador y saludo preparando el terreno.");
    }
}

class Artillero extends Soldado {
    // No sobrescribe saludar()
    // Usa el comportamiento heredado de Soldado
}
```

**Salida esperada:**

```java
Soy un zapador y saludo preparando el terreno.
Soy un soldado y saludo de forma general.
```



## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí, al sobrescribir un método se puede **invocar primero la versión de la clase base** y, a partir de ahí, añadir o modificar el comportamiento. Esto permite no sustituir completamente lo heredado, sino **aprovechar lo que ya hace la superclase** y extenderlo con algo más específico de la subclase. En este caso, `Zapador` puede saludar igual que `Soldado` y después añadir su mensaje propio.

La palabra clave que se utiliza para invocar el método de la clase base es **`super`**. Con `super.saludar();` se llama explícitamente al método `saludar()` definido en la superclase. Así, dentro del método sobrescrito de `Zapador`, primero se ejecuta el saludo normal heredado y después se añade `"ZAPADOR A SUS ORDENES"`.

Puede hacerse en un único fichero, por ejemplo `Principal.java`:

```java
public class Principal {

    public static void main(String[] args) {

        Soldado soldado1 = new Zapador();
        Soldado soldado2 = new Artillero();

        soldado1.saludar();
        soldado2.saludar();
    }
}

class Soldado {

    public void saludar() {
        System.out.println("Soy un soldado y saludo de forma general.");
    }
}

class Zapador extends Soldado {

    @Override
    public void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}

class Artillero extends Soldado {
}
```

En este ejemplo, `Zapador` no elimina por completo el comportamiento heredado, sino que lo reutiliza. Primero ejecuta el saludo general de `Soldado` mediante `super.saludar();` y después añade su parte específica. `Artillero`, en cambio, como no sobrescribe el método, sigue usando directamente el comportamiento heredado de `Soldado`.



## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al sobreescribir un método, debe mantenerse el mismo método heredado en lo esencial: mismo nombre y misma forma de llamada, es decir, los parámetros no pueden cambiar si se quiere seguir hablando de reescritura. Además, el tipo de retorno también tiene que ajustarse al método heredado; no sirve cambiar solo el resultado devuelto y pretender que siga siendo el mismo método. Si se alteran los parámetros, ya no se está redefiniendo el método de la clase base, sino haciendo otra cosa distinta.

La diferencia entre **`sobreescritura`** (overriding) y **`sobrecarga`** (overloading) está en que la sobreescritura aparece entre superclase y subclase, mientras que la sobrecarga consiste en tener varios métodos con el mismo nombre, pero con signaturas distintas. Esa diferencia puede estar en el número de parámetros, en sus tipos o en su orden. El tipo de retorno no forma parte de la signatura, así que no se pueden tener dos métodos cuya única diferencia sea lo que devuelven. Por eso, cambiar solo los parámetros produce sobrecarga, no sobreescritura.

La anotación `@Override` sirve para que el compilador compruebe que realmente se está reescribiendo el método de la clase base de forma correcta. Esa comprobación ayuda a detectar errores en tiempo de compilación, por ejemplo si se cambia sin querer algún parámetro y se termina creando una sobrecarga en vez de reescribir el método heredado. Por eso es recomendable usarla siempre: hace el código más claro, evita fallos y deja explícito que la intención era redefinir comportamiento heredado y no crear un método nuevo parecido.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Sí, en Java se empieza a **usar** ese mecanismo bastante pronto, aunque a veces todavía no se le haya puesto formalmente el nombre de **polimorfismo**. Toda clase hereda, directa o indirectamente, de una clase común, y por eso cuando una clase redefine un método heredado ya se está trabajando con la misma idea básica: existe un comportamiento general y cada clase concreta puede especializarlo a su manera. En ese sentido, al redefinir métodos heredados se entra ya en el terreno del polimorfismo.

Si se sobrescribe `toString()` o `equals()`, se está haciendo una **reescritura de métodos**. Eso significa que la clase toma un método heredado y le da una implementación propia. En `toString()` se cambia la forma en que el objeto se representa como texto, y en `equals()` se cambia el criterio con el que se considera si dos objetos deben verse como iguales. Por tanto, sí: ahí ya se está usando el mismo mecanismo que luego se estudia de forma explícita en polimorfismo.

Ahora bien, conviene distinguir dos ideas. **Sobrescribir** un método no es exactamente lo mismo que **ver claramente el polimorfismo en acción**. La sobrescritura es la base; el polimorfismo se aprecia con más claridad cuando se trabaja con una referencia de un tipo más general y, al llamar al método, se ejecuta la versión correspondiente al objeto real en tiempo de ejecución. Dicho de forma simple: sobrescribir `toString()` o `equals()` ya forma parte del fenómeno, pero el ejemplo más típico de polimorfismo es cuando varias subclases responden de forma distinta al mismo mensaje.

Por eso puede decirse que, en Java, el polimorfismo no aparece de golpe solo en el tema donde recibe ese nombre, sino que se va utilizando antes a través de la herencia y la reescritura. Lo que ocurre en el tema de polimorfismo es que esa idea se organiza y se estudia de manera más consciente: cómo una misma llamada puede producir comportamientos distintos según el objeto concreto que la reciba.



## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una **clase abstracta** es una clase pensada para servir de base a otras clases, pero no para crear objetos directamente a partir de ella. Se utiliza cuando interesa definir una idea general común, dejando algunas partes sin concretar para que cada subclase las complete a su manera. Por eso, **no se pueden crear instancias** de una clase abstracta con `new`.

Un **método abstracto** es un método que solo se declara, pero **no tiene cuerpo** en esa clase. Su función es obligar a las subclases a dar una implementación propia. En este caso, puede hacerse que `Soldado` tenga un comportamiento común en `saludar()`, pero que el método `atacar()` quede sin definir en la clase general, para que cada tipo de soldado ataque de una forma distinta.

La palabra clave **`abstract`** debe ponerse en dos sitios. Por un lado, en la cabecera de la clase: `abstract class Soldado`. Por otro lado, en la cabecera del método abstracto: `public abstract void atacar();`. Debe observarse que ese método termina en `;` porque no lleva cuerpo. Después, cada subclase concreta, como `Zapador` o `Artillero`, está obligada a implementar `atacar()` si quiere poder crear objetos de esa clase.

```java
public class Principal {

    public static void main(String[] args) {

        Soldado[] soldados = new Soldado[2];

        soldados[0] = new Zapador();
        soldados[1] = new Artillero();

        for (int i = 0; i < soldados.length; i++) {
            soldados[i].saludar();
            soldados[i].atacar();
        }

        // Esto no se puede hacer:
        // Soldado s = new Soldado();
    }
}

abstract class Soldado {

    public void saludar() {
        System.out.println("Soy un soldado y saludo de forma general.");
    }

    public abstract void atacar();
}

class Zapador extends Soldado {

    @Override
    public void atacar() {
        System.out.println("El zapador ataca colocando explosivos.");
    }
}

class Artillero extends Soldado {

    @Override
    public void atacar() {
        System.out.println("El artillero ataca disparando la artilleria.");
    }
}
```

En este ejemplo, `Soldado` define lo común y deja `atacar()` sin concretar. Después, cada subclase completa ese método con su propia acción. Así, al recorrer un array de referencias de tipo `Soldado`, puede pedirse a todos que `ataquen`, aunque cada objeto responda de manera distinta según su tipo real.



## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

La palabra clave **`final`** limita la posibilidad de extender comportamiento. Si se aplica a una **clase**, impide que esa clase tenga subclases. Si se aplica a un **método**, impide que ese método pueda ser **sobrescrito** en una clase derivada. Por tanto, `final` se usa cuando se quiere dejar cerrada una parte del diseño y evitar que otra clase la cambie mediante herencia. Además, una clase no puede ser a la vez `abstract` y `final`, porque una obliga a derivar para completar comportamiento y la otra prohíbe precisamente esa derivación.

Su relación con el **polimorfismo** es directa: el polimorfismo por herencia necesita que una subclase pueda redefinir métodos de una superclase. Si una clase es `final`, ya no puede haber subclases; y si un método es `final`, ya no puede existir una versión redefinida en una clase hija. En consecuencia, `final` **recorta** o **bloquea** esa variación polimórfica. Dicho de forma simple, donde se pone `final`, se está diciendo que ahí no se quiere permitir una especialización posterior mediante herencia.

Sí, existe algún ejemplo muy conocido en la API estándar de Java: **`String`** es una clase `final`. Eso significa que no se puede crear una subclase de `String`. Es un ejemplo muy representativo porque se usa constantemente y muestra bien la idea de una clase cuyo comportamiento queda cerrado y no se deja extender por herencia.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Las **interfaces** se usan para definir un conjunto de métodos que una clase debe ofrecer. No representan tanto una implementación concreta como un **compromiso de comportamiento**: si una clase implementa una interfaz, queda obligada a dar cuerpo a esos métodos. Por eso resultan útiles cuando interesa decir *qué sabe hacer* una clase, sin entrar todavía en *cómo lo hace*.

Pueden entenderse como algo **muy parecido a una clase abstracta pura**. La idea es que en la interfaz se escriben las cabeceras de los métodos, pero no su implementación, y después las clases concretas se encargan de completarlos. La diferencia práctica que suele destacarse es que una clase abstracta forma parte de una jerarquía de herencia, mientras que una interfaz sirve para fijar unas operaciones que otras clases deben cumplir.

Sí, una clase puede **implementar más de una interfaz**. De hecho, esa es una de sus utilidades más importantes, ya que Java solo permite herencia simple entre clases, pero sí permite que una misma clase cumpla varios contratos a la vez. La sintaxis general es escribir la clase y, después, usar `implements` seguido de una o varias interfaces separadas por comas.

```java
interface Saludador {
    void saludar();
}

interface Atacante {
    void atacar();
}

class Soldado implements Saludador, Atacante {

    @Override
    public void saludar() {
        System.out.println("El soldado saluda.");
    }

    @Override
    public void atacar() {
        System.out.println("El soldado ataca.");
    }
}
```



## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

Se puede resolver haciendo que `Punto` sea una **clase abstracta** y que declare el método abstracto `calcularDistanciaA(Punto otroPunto)`. Después, cada subtipo concreto implementa ese cálculo según sus propias coordenadas. Así, `Punto2D` calcula con dos componentes y `Punto3D` con tres. De ese modo, la operación existe en todos los puntos, pero cada clase la realiza de forma distinta.

Como el parámetro del método es de tipo general `Punto`, dentro de cada implementación debe comprobarse si el objeto recibido pertenece al subtipo correcto. Para eso se usa `instanceof`. Si la comprobación es verdadera, se hace **downcasting** para convertir la referencia general `Punto` en `Punto2D` o `Punto3D` y poder acceder a los datos necesarios para el cálculo. Si no coincide el tipo, conviene lanzar una excepción, porque no tendría sentido mezclar un punto 2D con uno 3D en esa distancia.

La clase `Linea` muestra el polimorfismo con claridad. `Linea` recibe dos referencias de tipo `Punto` y no necesita saber si son 2D o 3D. Simplemente pide a un extremo que calcule la distancia al otro. La versión concreta del método que se ejecuta se decide en tiempo de ejecución según el tipo real del objeto. Así, la clase `Linea` puede trabajar con distintas dimensiones sin conocerlas directamente.

```java
public class Principal {

    public static void main(String[] args) {

        Punto punto2D1 = new Punto2D(1.0, 2.0);
        Punto punto2D2 = new Punto2D(4.0, 6.0);

        Punto punto3D1 = new Punto3D(1.0, 2.0, 3.0);
        Punto punto3D2 = new Punto3D(5.0, 5.0, 7.0);

        Linea linea2D = new Linea(punto2D1, punto2D2);
        Linea linea3D = new Linea(punto3D1, punto3D2);

        System.out.println("Longitud de la linea 2D: "
                + linea2D.calcularLongitud());
        System.out.println("Longitud de la linea 3D: "
                + linea3D.calcularLongitud());

        try {
            Linea lineaInvalida = new Linea(punto2D1, punto3D1);
            System.out.println("Longitud invalida: "
                    + lineaInvalida.calcularLongitud());
        }
        catch (IllegalArgumentException excepcion) {
            System.out.println(excepcion.getMessage());
        }
    }
}

abstract class Punto {

    public abstract double calcularDistanciaA(Punto otroPunto);
}

class Punto2D extends Punto {

    private double x;
    private double y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    @Override
    public double calcularDistanciaA(Punto otroPunto) {

        if (!(otroPunto instanceof Punto2D)) {
            throw new IllegalArgumentException(
                    "No se puede calcular la distancia entre un Punto2D y un punto de otro subtipo."
            );
        }

        Punto2D otroPunto2D = (Punto2D) otroPunto;

        double diferenciaX = this.x - otroPunto2D.getX();
        double diferenciaY = this.y - otroPunto2D.getY();

        return Math.sqrt(diferenciaX * diferenciaX
                + diferenciaY * diferenciaY);
    }
}

class Punto3D extends Punto {

    private double x;
    private double y;
    private double z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    public double getZ() {
        return z;
    }

    @Override
    public double calcularDistanciaA(Punto otroPunto) {

        if (!(otroPunto instanceof Punto3D)) {
            throw new IllegalArgumentException(
                    "No se puede calcular la distancia entre un Punto3D y un punto de otro subtipo."
            );
        }

        Punto3D otroPunto3D = (Punto3D) otroPunto;

        double diferenciaX = this.x - otroPunto3D.getX();
        double diferenciaY = this.y - otroPunto3D.getY();
        double diferenciaZ = this.z - otroPunto3D.getZ();

        return Math.sqrt(diferenciaX * diferenciaX
                + diferenciaY * diferenciaY
                + diferenciaZ * diferenciaZ);
    }
}

class Linea {

    private Punto inicio;
    private Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double calcularLongitud() {
        return inicio.calcularDistanciaA(fin);
    }
}
```

```text
Salida aproximada:

Longitud de la linea 2D: 5.0
Longitud de la linea 3D: 6.4031242374328485
No se puede calcular la distancia entre un Punto2D y un punto de otro subtipo.
```



## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

La **herencia de interfaces** consiste en que una interfaz puede ampliar otra para **heredar su contrato** y añadir nuevas operaciones. En este caso no se hereda código de implementación, sino **cabeceras de métodos** que después deberán cumplirse. Puede verse como una forma de especializar una interfaz más general en otra más concreta. Así, `FicheroEscribible` puede partir de `Fichero`, conservar la capacidad de leer contenido y añadir además operaciones nuevas, como escribir o borrar.

Sí, en Java existe **herencia múltiple de interfaces**. Eso significa que una interfaz puede heredar de varias interfaces al mismo tiempo, y también que una clase puede implementar varias interfaces. La idea es útil porque permite reunir varios comportamientos en una misma clase sin necesidad de recurrir a herencia múltiple entre clases. De ese modo, una clase concreta puede comprometerse a cumplir varios contratos a la vez.

En el ejemplo siguiente, `Fichero` define la operación básica de lectura y `FicheroEscribible` **extiende** esa interfaz para añadir escritura y borrado. La clase `FicheroTexto` implementa la interfaz más completa y, por tanto, queda obligada a definir todos esos métodos.

```java
public class Principal {

    public static void main(String[] args) {

        FicheroEscribible fichero = new FicheroTexto("Hola");

        System.out.println(fichero.leerContenido());
        fichero.escribirContenido("Nuevo contenido");
        System.out.println(fichero.leerContenido());
        fichero.eliminar();
        System.out.println(fichero.leerContenido());
    }
}

interface Fichero {

    String leerContenido();
}

interface FicheroEscribible extends Fichero {

    void escribirContenido(String contenido);

    void eliminar();
}

class FicheroTexto implements FicheroEscribible {

    private String contenido;

    public FicheroTexto(String contenidoInicial) {
        contenido = contenidoInicial;
    }

    @Override
    public String leerContenido() {
        return contenido;
    }

    @Override
    public void escribirContenido(String contenido) {
        this.contenido = contenido;
    }

    @Override
    public void eliminar() {
        contenido = "";
    }
}
```

Si se quisiera mostrar **herencia múltiple de interfaces** de forma más clara, podría hacerse que `FicheroEscribible` heredase de varias interfaces distintas, por ejemplo una para leer y otra para borrar. Pero la idea de base es la misma: una interfaz puede extender otra u otras, y una clase concreta implementa después ese contrato completo.
