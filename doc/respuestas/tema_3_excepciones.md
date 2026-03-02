# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

En C no existen excepciones como mecanismo estructurado de gestión de errores, por lo que el control debe diseñarse manualmente. Si se implementa una función raiz que recibe un número float positivo, el problema aparece cuando se pasa un valor negativo. Como el usuario debe ser informado desde fuera de la función, es necesario que la propia función indique de alguna forma que ha ocurrido un error. Existen varias estrategias de diseño para conseguirlo.

Una primera opción consiste en utilizar un valor especial de retorno que indique error. Por ejemplo, se puede devolver un número imposible en condiciones normales (como -1.0 si se asume que la raíz cuadrada válida siempre será no negativa) o utilizar NAN (Not a Number) de la biblioteca matemática. El código podría ser:

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

Una segunda opción consiste en devolver un indicador de estado separado del resultado, por ejemplo mediante un parámetro adicional pasado por referencia (puntero). De esta forma, la función no solo calcula el valor, sino que informa explícitamente si la operación fue correcta. Esto separa el resultado del estado del error y suele considerarse un diseño más claro:

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

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta
