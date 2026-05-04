# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

En C no existen excepciones como mecanismo estructurado de gestión de errores, por lo que el control debe diseñarse manualmente. Si se implementa una función raiz que recibe un número float positivo, el problema aparece cuando se pasa un valor negativo. Como el usuario debe ser informado desde fuera de la función, es necesario que la propia función indique de alguna forma que ha ocurrido un error. Existen varias estrategias de diseño para conseguirlo.

Una primera opción consiste en utilizar un valor especial de retorno que indique error. Por ejemplo, se puede devolver un número imposible en condiciones normales (como -1.0 si se asume que la raíz cuadrada válida siempre será no negativa) o utilizar NAN (Not a Number) de la biblioteca matemática. El código podría ser:

``` c
#include <stdio.h>
#include <math.h>

float raiz(float numero) {
    if (numero < 0) {
        return -1.0;  // valor que indica error
    }
    return sqrt(numero);
}

int main() {
    float resultado = raiz(-4.0);

    if (resultado < 0) {
        printf("Error: no se puede calcular la raiz de un numero negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}
```
Una segunda opción consiste en devolver un indicador de estado separado del resultado, por ejemplo mediante un parámetro adicional pasado por referencia (puntero). De esta forma, la función no solo calcula el valor, sino que informa explícitamente si la operación fue correcta. Esto separa el resultado del estado del error y suele considerarse un diseño más claro:
``` c

#include <stdio.h>
#include <math.h>

int raiz(float numero, float *resultado) {
    if (numero < 0) {
        return 0;  // 0 indica error
    }
    *resultado = sqrt(numero);
    return 1;      // 1 indica éxito
}

int main() {
    float resultado;
    int estado = raiz(-4.0, &resultado);

    if (!estado) {
        printf("Error: numero negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}
```

En conclusión, en C el control de errores debe diseñarse manualmente, bien utilizando valores especiales de retorno o bien separando resultado y estado mediante parámetros adicionales. Ambas soluciones permiten informar al usuario desde fuera de la función, aunque la segunda ofrece una separación más clara entre dato calculado y señal de error.


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un mecanismo que permite gestionar situaciones anómalas o errores que se producen durante la ejecución de un programa. En lugar de devolver valores especiales como en C, en Java se crea un objeto que representa el error y que interrumpe el flujo normal del programa hasta que alguien lo gestione. De esta forma, el control del error no se mezcla directamente con el código principal, sino que se separa mediante estructuras específicas como try, catch y throw.

El objetivo de usar excepciones cuando se implementa una función es indicar que se ha producido una situación incorrecta que impide continuar normalmente, por ejemplo recibir un parámetro inválido. En lugar de devolver un valor ambiguo, se “lanza” una excepción para que el código que llamó a la función decida cómo actuar. Esto mejora la claridad del diseño, ya que el método solo se centra en su responsabilidad principal y delega el tratamiento del error.

Cuando se llama a una función que puede producir excepciones, el programador debe decidir si las captura y gestiona en ese punto, o si las vuelve a propagar hacia niveles superiores. De esta manera se consigue un control estructurado de errores, más organizado que en C, y coherente con el modelo orientado a objetos visto en Java.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java, el control de errores puede realizarse mediante excepciones, lo que permite separar claramente el cálculo del tratamiento del error. Si se reescribe el ejemplo de la raíz cuadrada, se puede crear una clase Calculadora que contenga un método raiz. Si el número recibido es negativo, el método puede lanzar una excepción para indicar que la operación no es válida. De este modo, la función no devuelve valores ambiguos, sino que comunica el error de forma explícita.

Desde el método main, que se encuentra fuera de la clase Calculadora, se puede controlar el error utilizando un bloque try-catch. Así se demuestra cómo la excepción lanzada dentro del método es capturada externamente, permitiendo mostrar un mensaje adecuado al usuario sin que el programa termine de forma abrupta. Este diseño es más estructurado que en C y encaja con el modelo orientado a objetos.
``` java

class Calculadora {

    public double raiz(double numero) {
        if (numero < 0) {
            throw new IllegalArgumentException("No se puede calcular la raiz de un numero negativo");
        }
        return Math.sqrt(numero);
    }
}

public class Main {

    public static void main(String[] args) {

        Calculadora calculadora = new Calculadora();

        try {
            double resultado = calculadora.raiz(-4.0);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```
## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

Lanzar una excepción significa crear un objeto que representa un error y enviarlo fuera del método mediante la instrucción throw. Cuando se ejecuta throw, el flujo normal del programa se interrumpe inmediatamente y el método deja de ejecutarse en ese punto. No se continúa con las instrucciones siguientes, sino que el sistema busca un bloque catch adecuado que pueda gestionar ese tipo de excepción.

Controlar o capturar una excepción significa interceptarla mediante un bloque try-catch. Si el tipo de la excepción coincide con el del catch, el control pasa a ese bloque y se ejecuta el código que gestiona el error. En cambio, si en ese método no existe un catch adecuado, se dice que la excepción se propaga hacia el método que realizó la llamada. Esta propagación continúa recorriendo la pila de llamadas hacia atrás hasta que se encuentra un manejador o, si no se encuentra ninguno, el programa termina mostrando el error.

Durante la propagación, los métodos por los que pasa la excepción finalizan su ejecución sin completar normalmente su código. Es decir, no continúan después de la llamada que produjo el error, sino que se “deshace” la pila de llamadas hasta llegar al punto donde se captura. Las funciones que no la controlan no se reanudan automáticamente en el punto donde estaban; simplemente terminan y el flujo continúa en el bloque catch que la gestiona.

Usando el ejemplo anterior de la raíz cuadrada, si el método raiz lanza una excepción al recibir un número negativo, el control pasa inmediatamente al catch del main, siempre que allí se haya incluido:
``` java

class Calculadora {

    public double raiz(double numero) {
        if (numero < 0) {
            throw new IllegalArgumentException("Numero negativo");
        }
        return Math.sqrt(numero);
    }
}

public class Main {

    public static void main(String[] args) {

        Calculadora calculadora = new Calculadora();

        try {
            double resultado = calculadora.raiz(-4.0);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error capturado: " + e.getMessage());
        }

        System.out.println("El programa continua despues del bloque try-catch");
    }
}
```
En este caso, al ejecutarse throw, el método raiz termina inmediatamente y no devuelve ningún valor. La excepción se propaga hasta main, donde es capturada en el bloque catch. Tras ejecutarse ese bloque, el programa continúa normalmente con las instrucciones posteriores al try-catch.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La propagación natural de excepciones en Java permite que un error viaje automáticamente por la pila de llamadas hasta encontrar un manejador adecuado. Esto supone una ventaja frente a C, donde cada función debe comprobar manualmente los valores de retorno y decidir qué hacer en cada nivel. En Java no es necesario escribir comprobaciones constantes después de cada llamada, ya que el propio mecanismo del lenguaje se encarga de trasladar el problema hacia arriba si no se captura localmente.

Otra ventaja importante es la separación entre lógica principal y control de errores. En C, el código suele llenarse de condiciones para verificar si ha ocurrido algún fallo, lo que puede dificultar la lectura y el mantenimiento. En Java, el flujo normal del programa permanece más limpio, y el tratamiento del error se concentra en bloques try-catch específicos. Esto mejora la claridad y reduce la probabilidad de olvidar una comprobación.

Además, la propagación automática facilita un diseño más modular. Cada método puede centrarse en su responsabilidad y, si no puede resolver el problema, simplemente deja que la excepción continúe propagándose. El nivel superior del programa, que suele tener más contexto global, puede decidir cómo reaccionar. En consecuencia, se obtiene un sistema de gestión de errores más estructurado, coherente con la orientación a objetos y más robusto que el modelo basado únicamente en códigos de retorno.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

En orientación a objetos, las excepciones son objetos. En Java, todas las excepciones son clases que heredan de la clase base Exception (directa o indirectamente). Esto significa que una excepción no es simplemente un código de error, sino una instancia que puede contener información, como un mensaje descriptivo o incluso otros datos relacionados con el problema ocurrido.

Desde el punto de vista de la encapsulación, esto aporta una ventaja importante: el error se representa como una entidad que agrupa en su interior toda la información necesaria sobre la situación anómala. En lugar de depender de valores especiales o variables externas, el propio objeto excepción encapsula el estado del error. Esto mantiene el diseño más coherente con el modelo de clases y objetos, ya que el comportamiento y los datos se organizan de forma estructurada.

Además, al ser clases, es posible crear excepciones personalizadas. Para ello se define una nueva clase que herede de Exception (o de alguna de sus subclases) y se añade el comportamiento o la información que se considere necesaria. De esta forma, se pueden representar errores específicos del dominio de la aplicación, logrando un sistema de control de errores más expresivo y adaptado al problema que se está resolviendo.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

En Java, cualquier objeto excepción lleva incorporada información esencial que resulta muy útil cuando se llega a un manejador. En primer lugar, contiene un mensaje descriptivo, que puede consultarse mediante métodos como getMessage(). Este mensaje permite conocer de forma precisa qué ha ocurrido, sin depender de códigos numéricos o convenciones externas como en C.

Además, una excepción almacena información sobre el punto exacto del programa donde se produjo el error, lo que se conoce como la traza de la pila (stack trace). Esta información indica la secuencia de llamadas que llevó hasta el fallo, mostrando clases y métodos implicados. Gracias a ello, el programador puede identificar con mayor facilidad el origen del problema, algo que en C requiere mecanismos adicionales y no viene integrado de forma automática.

Por tanto, frente al modelo basado en valores de retorno, el objeto excepción encapsula tanto el tipo de error como el contexto en el que ocurrió. Esta combinación de mensaje y traza de ejecución proporciona al manejador información rica y estructurada, lo que mejora significativamente la capacidad de diagnóstico y depuración del programa.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

En Java, en un bloque try-catch se pueden definir varios bloques catch después de un mismo try. Esto permite manejar distintos tipos de excepciones de manera diferenciada. Cada catch especifica el tipo de excepción que es capaz de capturar, lo que ofrece un control más preciso sobre cómo reaccionar ante diferentes errores.

Sin embargo, aunque pueda haber varios catch, solo se ejecuta uno cuando ocurre una excepción. El sistema compara el tipo de la excepción lanzada con los tipos declarados en los catch, en orden, y ejecuta el primero que sea compatible. Una vez encontrado y ejecutado ese bloque, los demás catch no se ejecutan.

Este diseño permite organizar el tratamiento de errores de forma jerárquica. Por ejemplo, se pueden capturar primero excepciones más específicas y después una más general. De esta manera se mantiene un control estructurado del flujo del programa y se evita ejecutar múltiples manejadores para un mismo error.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

Cuando una excepción interrumpe el flujo normal del programa, puede provocar que no se ejecuten ciertas instrucciones posteriores, como el cierre de ficheros o la liberación de recursos. Para garantizar que un bloque de código se ejecute siempre, independientemente de que haya o no excepción, Java proporciona el bloque finally. El código incluido en finally se ejecuta tanto si ocurre una excepción como si no, y también aunque la excepción continúe propagándose.

Si se utiliza junto con catch, el bloque finally se ejecuta después de que se haya gestionado la excepción. Si no se incluye un catch, el finally se ejecuta igualmente antes de que la excepción se propague hacia niveles superiores de la pila. Esto resulta fundamental para asegurar la correcta gestión de recursos, especialmente en operaciones de entrada/salida.

Ejemplo con catch:

```java

try {
    int resultado = 10 / 0;  // provoca ArithmeticException
} catch (ArithmeticException e) {
    System.out.println("Error: division por cero");
} finally {
    System.out.println("Este bloque se ejecuta siempre");
}

```
Ejemplo sin catch:
```java

try {
    int resultado = 10 / 0;  // provoca ArithmeticException
} finally {
    System.out.println("Este bloque se ejecuta siempre, antes de propagarse la excepcion");
}
```
En el segundo caso, aunque no se capture la excepción, el bloque finally se ejecuta antes de que el error continúe propagándose. De este modo, se garantiza que el código crítico de limpieza se ejecute en cualquier circunstancia.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

En Java, el bloque finally puede utilizarse sin catch, siempre que exista un bloque try. No es obligatorio capturar la excepción en ese mismo nivel; puede dejarse que se propague. En ese caso, el bloque finally se ejecutará igualmente antes de que la excepción continúe su propagación por la pila de llamadas. Esto permite asegurar la ejecución de código de limpieza aunque no se gestione el error en ese punto.

El bloque finally se ejecuta siempre, tanto si ocurre una excepción como si no ocurre ninguna. Si el código dentro del try se ejecuta con normalidad, el finally se ejecuta al final. Si se produce una excepción y se captura con un catch, el finally se ejecuta después del catch. Y si no se captura, el finally se ejecuta antes de que la excepción se propague.

Incluso si dentro del try aparece un return, el bloque finally también se ejecuta antes de que el método termine realmente. Es decir, el valor de retorno se prepara, pero antes de salir del método se ejecuta el finally. Por ejemplo:

```java
public static int ejemplo() {
    try {
        return 5;
    } finally {
        System.out.println("Se ejecuta el finally antes de salir del metodo");
    }
}
```
En este caso, el método devuelve 5, pero antes de finalizar se ejecuta el código del finally. Esto demuestra que finally está diseñado para garantizar la ejecución de código crítico independientemente de cómo termine el bloque try.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

En Java, las excepciones controladas (checked exceptions) son aquellas que el compilador obliga a declarar o capturar. Si un método puede producir una de estas excepciones, debe indicarlo con throws o gestionarla con un bloque try-catch. En cambio, las excepciones no controladas (unchecked exceptions) no están obligadas a declararse ni capturarse. Estas últimas suelen representar errores de programación o situaciones que no se consideran recuperables de forma normal.

La clase RuntimeException juega un papel central en esta clasificación. Todas las excepciones que heredan de RuntimeException son no controladas. Por ejemplo, ArithmeticException, NullPointerException o IndexOutOfBoundsException pertenecen a este grupo. En cambio, excepciones como IOException, SQLException o FileNotFoundException son controladas y obligan a tratarlas explícitamente.

Ejemplos típicos de excepciones controladas que se usan habitualmente:

IOException (errores de entrada/salida).
FileNotFoundException (fichero no encontrado).
ParseException (error al interpretar un formato).

Ejemplos típicos de excepciones no controladas:

ArithmeticException (división por cero).
NullPointerException (uso de referencia nula).
NumberFormatException (conversión incorrecta de texto a número).

Se suele preferir una excepción controlada cuando el error es previsible y el programa puede recuperarse, por ejemplo al abrir un fichero que puede no existir o al leer datos externos. En cambio, se suele preferir una excepción no controlada cuando el error indica un fallo de programación, como acceder a un índice inválido o usar un objeto no inicializado, ya que en esos casos no se espera una recuperación normal sino una corrección del código.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

En Java, throws se utiliza en la cabecera de un método para indicar que dicho método puede producir una excepción controlada y que no la gestiona internamente. De esta forma, en lugar de capturar la excepción con un bloque try-catch, el método declara que la excepción puede propagarse al código que lo llama. Esto forma parte del mecanismo de comprobación en tiempo de compilación de las excepciones controladas.

Se considera una alternativa a capturar la excepción porque permite delegar la responsabilidad de gestionarla en un nivel superior. En vez de resolver el problema en el propio método, se deja que otro método con más contexto decida cómo actuar. Esto es útil cuando el método no dispone de información suficiente para tratar el error adecuadamente o cuando se desea centralizar el tratamiento en un punto concreto del programa.

Por ejemplo, si un método abre un fichero y puede producir una IOException, puede declararse así:
```java

import java.io.IOException;

public void leerDatos() throws IOException {
    // código que puede producir IOException
}
```

En este caso, quien llame a leerDatos() estará obligado a capturar la excepción o a volver a declararla con throws. Así, throws no elimina la excepción, sino que la propaga de forma explícita, manteniendo el control estructurado que caracteriza a las excepciones controladas en Java.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

Cuando un método abre un fichero, puede producirse una excepción controlada como FileNotFoundException o IOException. Si no se desea manejarla en ese mismo método, se puede declarar con throws en la firma, permitiendo que la excepción se propague hacia el método que realiza la llamada. De este modo, se delega la responsabilidad del tratamiento del error en un nivel superior del programa.

No obstante, aunque se decida no capturar la excepción, puede ser necesario ejecutar código de limpieza. Para ello se utiliza el bloque finally, que se ejecuta siempre, tanto si ocurre la excepción como si no. Esto resulta útil, por ejemplo, para cerrar recursos abiertos o mostrar mensajes de control, antes de que la excepción continúe propagándose.

Un ejemplo simplificado podría ser el siguiente:
```java

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Lector {

    public void leerFichero(String nombre) throws IOException {
        BufferedReader lector = null;

        try {
            lector = new BufferedReader(new FileReader(nombre));
            System.out.println("Fichero abierto correctamente");
        } finally {
            System.out.println("Se ejecuta el bloque finally");
            if (lector != null) {
                lector.close();
            }
        }
    }
}
```

En este caso, el método declara throws IOException, por lo que no captura la posible excepción si el fichero no existe. Sin embargo, el bloque finally se ejecuta siempre antes de que la excepción se propague hacia el método llamador, garantizando así la ejecución del código necesario de cierre o control.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

En Java sí es posible declarar en la cláusula throws excepciones no controladas, como las que heredan de RuntimeException. El lenguaje no lo prohíbe, ya que cualquier tipo de excepción puede aparecer en la firma de un método. Sin embargo, a diferencia de las excepciones controladas, el compilador no obliga a hacerlo ni obliga al llamador a capturarlas.

Si un método declara en throws una RuntimeException, el método llamador no está obligado a escribir un bloque try-catch. Puede hacerlo si lo desea, pero no es necesario desde el punto de vista del compilador. Esto se debe a que las excepciones no controladas representan normalmente errores de programación o situaciones que no se consideran recuperables de manera habitual.

En cuanto al sentido práctico, declarar una excepción no controlada en throws puede tener valor documental, ya que informa explícitamente de que el método puede producir cierto tipo de error. No obstante, no cambia el comportamiento obligatorio del llamador. Por ello, en la práctica, no suele ser necesario declarar las excepciones no controladas en la firma, ya que forman parte del modelo implícito del lenguaje y su captura queda a criterio del programador según el diseño de la aplicación.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Se recomienda utilizar excepciones controladas (como IOException) cuando el error es previsible y forma parte del funcionamiento normal del sistema, especialmente si el programa puede recuperarse de la situación. Por ejemplo, al trabajar con ficheros, redes o bases de datos, es razonable que algo falle y que el programa deba reaccionar (volver a intentar, mostrar un mensaje, pedir otro dato, etc.). En estos casos, obligar al programador a declarar o capturar la excepción ayuda a no olvidar su tratamiento.

En cambio, se suelen utilizar excepciones no controladas (como IllegalArgumentException) cuando el error indica un uso incorrecto del método o un fallo de programación. Por ejemplo, pasar un parámetro inválido a un método que exige ciertas condiciones. En estas situaciones no se espera que el programa “se recupere” de forma normal, sino que se corrija el código que provoca el error. Por ello no se obliga a capturarlas, ya que representan errores lógicos más que situaciones externas.

No todos los lenguajes distinguen entre excepciones controladas y no controladas. Java es uno de los casos más representativos donde sí existe esta separación formal. En muchos otros lenguajes modernos (como C#, Python o C++), todas las excepciones funcionan de manera similar a las no controladas de Java, es decir, no existe una obligación en tiempo de compilación de declararlas o capturarlas. Por tanto, cuando solo existe una opción, lo habitual es el modelo equivalente a las excepciones no controladas, donde la gestión queda a criterio del programador y no del compilador.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí tiene sentido lanzar excepciones dentro de un bloque catch. Cuando se captura una excepción, se puede decidir que el nivel actual no es el más adecuado para resolver el problema y, tras realizar alguna acción (por ejemplo, registrar un mensaje), lanzar otra excepción. Esto permite transformar una excepción técnica en otra más acorde al dominio de la aplicación o añadir información adicional antes de que continúe propagándose.

También es posible relanzar la misma excepción capturada utilizando simplemente throw e;. En este caso, no se crea una nueva excepción, sino que se deja que la original continúe su propagación por la pila de llamadas. Esto tiene sentido cuando se quiere realizar alguna acción local (como registrar el error o liberar recursos) pero sin asumir la responsabilidad de gestionarlo completamente.

Ejemplo de lanzar una nueva excepción desde el catch:
```java

try {
    int resultado = Integer.parseInt("abc");
} catch (NumberFormatException e) {
    throw new IllegalArgumentException("Formato de numero invalido");
}

Ejemplo de relanzar la misma excepción:

try {
    int resultado = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Se detecta el error, pero no se gestiona aqui");
    throw e;  // se relanza la misma excepcion
}

```
En el primer caso se transforma la excepción original en otra más adecuada al contexto. En el segundo caso se permite que la excepción siga su curso después de realizar una acción intermedia, lo que resulta útil cuando la gestión definitiva corresponde a un nivel superior del programa.

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Que una excepción sea la causa de otra significa que una excepción original (normalmente de bajo nivel) ha provocado que se lance una segunda excepción (normalmente de nivel más alto), conservando la primera como información interna. Este mecanismo permite encapsular un error técnico dentro de una excepción más adecuada al dominio de la aplicación, sin perder el detalle de lo que realmente ocurrió. En Java, esto se realiza pasando la excepción original como parámetro al constructor de la nueva excepción.

Este enfoque es útil cuando no se desea exponer detalles internos (como errores de acceso a fichero o base de datos) al resto del sistema, pero sí mantener esa información para diagnóstico. Así, se captura la excepción de bajo nivel, se crea una excepción personalizada más representativa del problema general y se establece la original como su causa. De esta forma se respeta la encapsulación y se mantiene la trazabilidad del error.

Ejemplo en Java:
```java

class ErrorAccesoDatos extends Exception {

    public ErrorAccesoDatos(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class Servicio {

    public void procesar() throws ErrorAccesoDatos {
        try {
            int valor = Integer.parseInt("abc");  // provoca NumberFormatException
        } catch (NumberFormatException e) {
            throw new ErrorAccesoDatos("Error al procesar los datos", e);
        }
    }

    public static void main(String[] args) throws ErrorAccesoDatos {
        new Servicio().procesar();
    }
}
```
Cuando una excepción con causa se muestra por pantalla (por ejemplo, si no se captura y se deja que el sistema la imprima), aparece tanto la excepción principal como la causa, normalmente indicada con un mensaje del tipo “Caused by”. Por tanto, sí se ve la excepción original, lo que facilita la depuración al conservar la información completa de la cadena de errores.
