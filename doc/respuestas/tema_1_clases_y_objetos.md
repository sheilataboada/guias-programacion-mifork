# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

Las cuatro características básicas de la programación orientada a objetos son abstracción, encapsulamiento, herencia y polimorfismo. Estas propiedades permiten organizar el código en torno a objetos que representan entidades del mundo real, facilitando la reutilización, el mantenimiento y la claridad del programa. A diferencia de la programación estructurada tradicional, donde el foco está en funciones y procedimientos, en este paradigma el elemento central es la clase y los objetos que se crean a partir de ella.

La abstracción consiste en representar únicamente las características esenciales de un objeto, ignorando los detalles innecesarios para el problema que se desea resolver. Por ejemplo, en una clase que represente un coche, puede interesar almacenar su marca y velocidad, pero no necesariamente el funcionamiento interno del motor. La abstracción permite centrarse en qué hace un objeto, más que en cómo lo hace internamente.

El encapsulamiento implica agrupar los datos y los métodos que operan sobre ellos dentro de una misma clase, y controlar el acceso a dichos datos. En Java, esto se consigue utilizando modificadores como private para los atributos y proporcionando métodos públicos (getters y setters) para acceder o modificar su valor. De esta forma, se protege el estado interno del objeto y se evita su manipulación directa desde el exterior.

La herencia permite crear nuevas clases a partir de otras ya existentes, reutilizando su código y extendiendo su comportamiento. Por su parte, el polimorfismo posibilita que distintos objetos respondan de manera diferente ante el mismo mensaje o método. Estas dos características trabajan conjuntamente para favorecer la reutilización y la flexibilidad del software, permitiendo construir jerarquías de clases y comportamientos más generales y especializados.




## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta