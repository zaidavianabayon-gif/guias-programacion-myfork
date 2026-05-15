<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.
Un puntero a una función es una variable que almacena la dirección de memoria de una función. Esto permite invocar funciones de manera indirecta, igual que ocurre con los punteros a variables. Su uso es habitual cuando se desea pasar una función como parámetro, implementar callbacks o seleccionar dinámicamente qué función ejecutar durante la ejecución del programa.

En C, la declaración de un puntero a función debe indicar el tipo de retorno y los parámetros de la función a la que apuntará. En este caso, se define una función que recibe una cadena de caracteres y transforma su contenido a mayúsculas. Después, se crea un puntero local llamado `aMayusculas` que apunta a dicha función y se utiliza para realizar la llamada.

```c
#include <stdio.h>
#include <ctype.h>

void convertirMayusculas(char *cadena) {
    int i = 0;

    while (cadena[i] != '\0') {
        cadena[i] = toupper(cadena[i]);
        i++;
    }
}

int main() {
    char texto[] = "hola mundo";

    /* Puntero a función */
    void (*aMayusculas)(char *);

    /* Asignación del puntero */
    aMayusculas = convertirMayusculas;

    /* Invocación mediante el puntero */
    aMayusculas(texto);

    printf("%s\n", texto);

    return 0;
}
```

En el ejemplo anterior, `aMayusculas` almacena la dirección de la función `convertirMayusculas`. Posteriormente, la función se ejecuta usando el propio puntero, demostrando que las funciones en C pueden manipularse mediante referencias de memoria igual que otros tipos de datos.



## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima que puede definirse directamente dentro de una expresión y almacenarse en una variable o pasarse como argumento. Su principal objetivo es permitir escribir funciones pequeñas de forma más compacta y flexible, evitando la necesidad de declarar métodos o funciones completas cuando solo se necesita un comportamiento concreto de manera puntual.

Las funciones lambda son habituales en los lenguajes modernos porque facilitan la programación funcional. Gracias a ellas, las funciones pueden tratarse como datos: almacenarse en variables, devolverse desde otras funciones o enviarse como parámetros. Conceptualmente, cumplen un papel similar al de los punteros a función en C, aunque con una sintaxis más sencilla y segura.

En Javascript, una función lambda puede asignarse directamente a una variable local. El siguiente ejemplo recibe una cadena y devuelve su contenido en mayúsculas:

```javascript id="xkqfui"
function main() {
    const aMayusculas = (cadena) => {
        return cadena.toUpperCase();
    };

    let texto = "hola mundo";

    let resultado = aMayusculas(texto);

    console.log(resultado);
}

main();
```

En Java, las funciones lambda suelen utilizarse junto con interfaces funcionales. En este caso, se emplea `Function<String, String>` para representar una función que recibe un `String` y devuelve otro `String`:

```java id="jlwmjg"
import java.util.function.Function;

public class Main {

    public static void main(String[] args) {

        Function<String, String> aMayusculas =
                cadena -> cadena.toUpperCase();

        String texto = "hola mundo";

        String resultado = aMayusculas.apply(texto);

        System.out.println(resultado);
    }
}
```

En ambos ejemplos, la variable local `aMayusculas` contiene una referencia a la función lambda. Posteriormente, dicha referencia se utiliza para ejecutar la operación de conversión a mayúsculas, mostrando cómo las funciones pueden manipularse de forma similar a cualquier otro dato del programa.



## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?
El paradigma funcional es un estilo de programación basado en el uso de funciones como elemento principal para construir programas. En este paradigma, las operaciones se expresan mediante la evaluación de funciones y se intenta evitar la modificación de estados y variables globales. El objetivo es que las funciones reciban datos de entrada y produzcan resultados sin provocar efectos secundarios sobre el resto del sistema.

Una de las características más importantes del paradigma funcional es el uso de funciones puras. Una función pura siempre devuelve el mismo resultado para los mismos parámetros y no altera datos externos. Esto facilita el razonamiento sobre el código, mejora la reutilización y reduce errores relacionados con cambios de estado inesperados. Además, suelen emplearse conceptos como funciones lambda, composición de funciones y operaciones sobre colecciones de datos.

Lenguajes originalmente orientados a objetos, como Java a partir de Java 8, incorporaron características funcionales como expresiones lambda, referencias a métodos y streams. Debido a ello, se consideran lenguajes multi-paradigma, ya que permiten combinar diferentes estilos de programación dentro del mismo lenguaje. Así, un programa puede diseñarse usando clases y objetos, pero también emplear técnicas funcionales cuando resulten más adecuadas.

Cuando se afirma que las funciones son “ciudadanos de primera clase”, se quiere decir que las funciones pueden tratarse igual que cualquier otro dato del lenguaje. Por ejemplo, pueden almacenarse en variables, pasarse como parámetros, devolverse desde otras funciones o incluirse en estructuras de datos. Esto permite escribir código más flexible y expresivo, ya que el comportamiento de un programa puede manipularse dinámicamente mediante funciones.



## 4. Explica la sintaxis básica de una función lambda en Java.

En Java, una función lambda es una forma compacta de representar una implementación de una interfaz funcional, es decir, una interfaz que contiene un único método abstracto. Su sintaxis fue incorporada en Java 8 para facilitar la programación funcional y reducir la cantidad de código necesaria al trabajar con comportamientos simples.

La sintaxis general de una expresión lambda es la siguiente:

```java id="qmbwla"
(parametros) -> expresion
```

o bien:

```java id="hztrzk"
(parametros) -> {
    instrucciones
}
```

A la izquierda del operador `->` se indican los parámetros de entrada, mientras que a la derecha se define el cuerpo de la función. Si el cuerpo contiene una única expresión, el valor se devuelve automáticamente sin necesidad de usar `return`. Cuando el cuerpo incluye varias instrucciones, deben utilizarse llaves y la sentencia `return` si existe valor de retorno.

El siguiente ejemplo define una función lambda que recibe una cadena y devuelve su contenido en mayúsculas:

```java id="tzlfwm"
import java.util.function.Function;

public class Main {

    public static void main(String[] args) {

        Function<String, String> aMayusculas =
                texto -> texto.toUpperCase();

        String resultado = aMayusculas.apply("hola mundo");

        System.out.println(resultado);
    }
}
```

En este caso, `texto -> texto.toUpperCase()` es la expresión lambda. El parámetro `texto` se recibe como entrada y el resultado de `toUpperCase()` se devuelve automáticamente. La variable `aMayusculas` almacena la referencia a la función lambda y posteriormente se ejecuta mediante el método `apply`.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

En programación funcional es habitual pasar funciones como parámetros a otras funciones o métodos. Esto permite separar el comportamiento de la lógica principal, consiguiendo código más flexible y reutilizable. En este caso, el método `transformar` recibe una cadena y una función transformadora, encargándose de ejecutar dicha función sobre el texto recibido.

En Javascript, las funciones pueden pasarse directamente como argumentos porque son ciudadanos de primera clase. El método `transformar` recibe el texto y la función `aMayusculas`, invocándola desde su interior:

```javascript id="myybki"
function transformar(texto, transformadora) {
    return transformadora(texto);
}

const aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

let resultado = transformar("hola mundo", aMayusculas);

console.log(resultado);
```

En Java, este comportamiento se consigue utilizando interfaces funcionales. En el siguiente ejemplo, `transformar` recibe un `String` y un objeto de tipo `Function<String, String>`, que representa la función lambda encargada de realizar la transformación:

```java id="ghrxyl"
import java.util.function.Function;

public class Main {

    public static String transformar(
            String texto,
            Function<String, String> transformadora) {

        return transformadora.apply(texto);
    }

    public static void main(String[] args) {

        Function<String, String> aMayusculas =
                cadena -> cadena.toUpperCase();

        String resultado =
                transformar("hola mundo", aMayusculas);

        System.out.println(resultado);
    }
}
```

En ambos ejemplos, la función transformadora se pasa como argumento y se ejecuta desde dentro del método `transformar`. Este enfoque permite reutilizar el mismo método con distintas operaciones, simplemente proporcionando funciones diferentes como parámetro.



## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

Las funciones lambda también pueden definirse directamente en el momento en que se pasan como parámetro a otra función o método. Esto resulta útil cuando la operación solo va a utilizarse una vez y no es necesario almacenarla previamente en una variable. De esta manera, el código puede escribirse de forma más compacta y cercana al punto donde se utiliza el comportamiento.

En Javascript, la función lambda que invierte la cadena puede declararse directamente dentro de la llamada a `transformar`:

```javascript id="prgtxz"
function transformar(texto, transformadora) {
    return transformadora(texto);
}

let resultado = transformar(
    "hola mundo",
    (cadena) => {
        return cadena.split("").reverse().join("");
    }
);

console.log(resultado);
```

En este ejemplo, la función lambda recibe la cadena, la divide en caracteres mediante `split("")`, invierte el orden con `reverse()` y finalmente reconstruye la cadena con `join("")`. Todo ello se define directamente como argumento de la llamada.

En Java, el mismo comportamiento puede implementarse utilizando una expresión lambda pasada directamente al método `transformar`:

```java id="ubkycw"
import java.util.function.Function;

public class Main {

    public static String transformar(
            String texto,
            Function<String, String> transformadora) {

        return transformadora.apply(texto);
    }

    public static void main(String[] args) {

        String resultado = transformar(
                "hola mundo",
                cadena -> new StringBuilder(cadena)
                                .reverse()
                                .toString()
        );

        System.out.println(resultado);
    }
}
```

En ambos lenguajes, la función lambda se crea justo en el instante en que se pasa como parámetro. Esto demuestra cómo las funciones pueden utilizarse dinámicamente para modificar el comportamiento de un método sin necesidad de declarar funciones auxiliares independientes.



## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un cierre o *closure* es una función que puede acceder a variables definidas en el contexto donde fue creada, incluso aunque dichas variables no pertenezcan directamente a la propia función. Esto significa que una función lambda puede “capturar” variables externas y utilizarlas posteriormente durante su ejecución. Los cierres son una característica fundamental de la programación funcional, ya que permiten crear funciones más flexibles y adaptables al contexto.

En Java, las funciones lambda pueden acceder a variables locales del método donde se definen, siempre que dichas variables sean finales o efectivamente finales (*effectively final*). Una variable es efectivamente final cuando su valor no cambia después de inicializarse. De esta manera, Java garantiza que el valor capturado por la lambda permanezca consistente.

El siguiente ejemplo modifica el método `transformar` anterior. Ahora se define una variable local externa llamada `sufijo`, y la función lambda la utiliza para concatenarla al texto recibido:

```java id="wdblht"
import java.util.function.Function;

public class Main {

    public static String transformar(
            String texto,
            Function<String, String> transformadora) {

        return transformadora.apply(texto);
    }

    public static void main(String[] args) {

        String sufijo = "!!!";

        String resultado = transformar(
                "hola mundo",
                cadena -> cadena + sufijo
        );

        System.out.println(resultado);
    }
}
```

En este caso, la función lambda `cadena -> cadena + sufijo` accede directamente a la variable local `sufijo`, aunque dicha variable esté definida fuera de la lambda. Esto demuestra el funcionamiento de un cierre: la función conserva acceso al entorno donde fue creada y puede utilizar los datos de ese contexto durante su ejecución.



## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?
Aunque las funciones lambda y los punteros a función permiten tratar funciones como valores y almacenarlas en variables, existen diferencias importantes entre ambos mecanismos. En C, un puntero a función simplemente almacena la dirección de memoria de una función ya definida. Su comportamiento es relativamente básico: únicamente permite invocar indirectamente dicha función mediante su dirección.

Las funciones lambda, en cambio, son construcciones más avanzadas propias de lenguajes modernos como Java o Javascript. Una lambda no solo representa una referencia a código ejecutable, sino también un objeto que puede capturar variables de su entorno mediante cierres (*closures*). Esto permite que la función conserve información contextual incluso después de haber sido creada.

Otra diferencia importante es la sintaxis y el nivel de abstracción. En C, las funciones deben declararse explícitamente y los punteros a función suelen tener una sintaxis compleja y poco legible. Las funciones lambda ofrecen una sintaxis mucho más compacta y permiten definir comportamientos directamente en el lugar donde se utilizan, sin necesidad de crear funciones auxiliares con nombre.

Además, las lambdas suelen integrarse con características avanzadas de programación funcional, como operaciones sobre colecciones, composición de funciones o procesamiento declarativo de datos. Los punteros a función en C proporcionan flexibilidad procedural, pero no incorporan directamente conceptos funcionales como cierres, inferencia de tipos o interfaces funcionales.



## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

En programación funcional, una función también puede devolver otras funciones como resultado. Esto permite crear comportamientos dinámicos configurados mediante parámetros previos. En este caso, la función `crearDescuento` recibe un porcentaje y devuelve otra función encargada de aplicar dicho descuento sobre una cantidad determinada.

En Java, puede utilizarse `Function<Double, Double>` para representar una función que recibe un valor numérico y devuelve otro. La función devuelta conservará el porcentaje de descuento mediante un cierre (*closure*):

```java id="svfzji"
import java.util.function.Function;

public class Main {

    public static Function<Double, Double>
            crearDescuento(double porcentaje) {

        return cantidad ->
                cantidad - (cantidad * porcentaje / 100.0);
    }

    public static void main(String[] args) {

        Function<Double, Double> descuento10 =
                crearDescuento(10);

        Function<Double, Double> descuento25 =
                crearDescuento(25);

        double precio = 200.0;

        double resultado1 = descuento10.apply(precio);
        double resultado2 = descuento25.apply(precio);

        System.out.println("Descuento 10%: " + resultado1);
        System.out.println("Descuento 25%: " + resultado2);
    }
}
```

En el ejemplo anterior, `crearDescuento(10)` genera una función especializada en aplicar un descuento del 10%, mientras que `crearDescuento(25)` crea otra distinta para el 25%. Ambas funciones son independientes y pueden reutilizarse posteriormente sobre diferentes cantidades.

La closure aparece porque la función lambda devuelta utiliza la variable `porcentaje`, definida en el contexto externo de `crearDescuento`. Aunque el método termina su ejecución, cada lambda conserva internamente el valor capturado del porcentaje correspondiente. Gracias a ello, cada función “recuerda” el descuento con el que fue creada y puede seguir utilizándolo más adelante.



## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

En Java, una interfaz funcional es una interfaz diseñada para representar una única operación abstracta. Su objetivo principal es servir como tipo para expresiones lambda y referencias a métodos. Cuando se asigna una función lambda a una variable, Java necesita conocer el tipo exacto de dicha función, y ese tipo viene determinado por la interfaz funcional asociada.

El requisito fundamental de una interfaz funcional es que debe contener exactamente un único método abstracto. Este método define la firma de la función lambda: parámetros de entrada y tipo de retorno. Gracias a ello, el compilador puede comprobar estáticamente si la lambda es compatible con la interfaz. Aunque solo puede existir un método abstracto, sí se permiten métodos `default`, métodos `static` e incluso métodos heredados de `Object`.

Para indicar explícitamente que una interfaz está pensada como interfaz funcional, suele utilizarse la anotación `@FunctionalInterface`. Esta anotación no es obligatoria, pero ayuda al compilador a detectar errores si accidentalmente se añade más de un método abstracto. Un ejemplo sencillo sería el siguiente:

```java id="aycwzn"
@FunctionalInterface
interface Transformadora {

    String transformar(String texto);
}
```

Esta interfaz puede utilizarse como tipo de una función lambda compatible:

```java id="vzqmep"
Transformadora aMayusculas =
        texto -> texto.toUpperCase();
```

En este caso, la lambda recibe un `String` y devuelve otro `String`, coincidiendo con la firma del método abstracto `transformar`. De esta manera, Java puede tratar la función lambda como una implementación automática de la interfaz funcional.



## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

En Java, una interfaz funcional puede definirse manualmente igual que cualquier otra interfaz. La diferencia es que únicamente debe contener un método abstracto, que será el que represente la operación de la función lambda. En este caso, se desea representar una transformación de una cadena `String` en otra cadena `String`.

La interfaz funcional `Transformador` puede definirse de la siguiente manera:

```java id="gtjrsx"
@FunctionalInterface
public interface Transformador {

    String transformar(String texto);
}
```

La anotación `@FunctionalInterface` indica que la interfaz está pensada para utilizarse con funciones lambda. El método abstracto `transformar` define la firma de la operación: recibe un `String` y devuelve otro `String`.

Una vez creada la interfaz, puede utilizarse como tipo de una función lambda:

```java id="xeymsp"
public class Main {

    public static void main(String[] args) {

        Transformador aMayusculas =
                texto -> texto.toUpperCase();

        String resultado =
                aMayusculas.transformar("hola mundo");

        System.out.println(resultado);
    }
}
```

En este ejemplo, la función lambda implementa automáticamente el método `transformar` definido en la interfaz funcional. De esta forma, Java trata la lambda como un objeto cuyo comportamiento viene especificado por la interfaz `Transformador`.



## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

En Java, las interfaces funcionales también pueden definirse utilizando genericidad. Esto permite reutilizar la misma interfaz con diferentes tipos de datos, haciendo el código más flexible y reutilizable. En lugar de crear una interfaz específica para transformar `String` en `String`, puede definirse un transformador genérico que convierta un tipo cualquiera `T` en otro tipo `R`.

La interfaz funcional genérica `Transformador<T, R>` podría definirse de la siguiente manera:

```java id="pmgqoa"
@FunctionalInterface
public interface Transformador<T, R> {

    R transformar(T valor);
}
```

En este caso, `T` representa el tipo de entrada y `R` el tipo de salida. Gracias a los generics, la misma interfaz puede emplearse para transformar distintos tipos de datos sin necesidad de duplicar código.

El siguiente ejemplo crea un transformador que convierte un `Double` en un `Integer` mediante redondeo:

```java id="mwljrb"
public class Main {

    public static void main(String[] args) {

        Transformador<Double, Integer> redondeador =
                valor -> (int) Math.round(valor);

        Integer resultado =
                redondeador.transformar(3.75);

        System.out.println(resultado);
    }
}
```

En este ejemplo, la función lambda recibe un valor `Double` y devuelve un `Integer` usando `Math.round`. La interfaz funcional genérica permite expresar claramente el tipo de transformación realizada, manteniendo la seguridad de tipos proporcionada por Java.



## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

En Java, muchas interfaces funcionales habituales ya vienen definidas en el paquete `java.util.function`. Estas interfaces permiten trabajar con programación funcional sin necesidad de crear tipos personalizados como `Transformador<T, R>` en los casos más comunes. La interfaz `Function<T, R>`, por ejemplo, representa exactamente una función que transforma un objeto de tipo `T` en otro de tipo `R`.

Estas interfaces predefinidas cubren operaciones frecuentes como consumir datos, producir valores, realizar transformaciones o evaluar condiciones. Gracias a ello, las expresiones lambda pueden integrarse fácilmente con colecciones, streams y otras APIs funcionales del lenguaje.

Algunas de las interfaces funcionales más utilizadas son las siguientes:

| Interfaz funcional    | Descripción                                             |
| --------------------- | ------------------------------------------------------- |
| `Function<T, R>`      | Recibe un valor de tipo `T` y devuelve uno de tipo `R`. |
| `Consumer<T>`         | Recibe un valor de tipo `T` y no devuelve resultado.    |
| `Supplier<T>`         | No recibe parámetros y devuelve un valor de tipo `T`.   |
| `Predicate<T>`        | Recibe un valor de tipo `T` y devuelve un `boolean`.    |
| `UnaryOperator<T>`    | Recibe y devuelve el mismo tipo `T`.                    |
| `BinaryOperator<T>`   | Recibe dos valores de tipo `T` y devuelve otro `T`.     |
| `BiFunction<T, U, R>` | Recibe dos parámetros y devuelve un resultado.          |
| `BiConsumer<T, U>`    | Recibe dos parámetros y no devuelve valor.              |
| `BiPredicate<T, U>`   | Recibe dos parámetros y devuelve un `boolean`.          |

Por ejemplo, `Predicate<T>` suele emplearse para realizar comprobaciones lógicas:

```java id="jlwmql"
import java.util.function.Predicate;

public class Main {

    public static void main(String[] args) {

        Predicate<Integer> esPar =
                numero -> numero % 2 == 0;

        System.out.println(esPar.test(10));
    }
}
```

En este ejemplo, `esPar` recibe un número entero y devuelve `true` si el número es par. Este tipo de interfaces funcionales permiten reutilizar patrones comunes sin necesidad de declarar nuevas interfaces en cada situación.



## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

En Java, el método `forEach` permite recorrer colecciones utilizando programación funcional. Este método recibe una función como parámetro y la ejecuta automáticamente para cada elemento de la colección. Su comportamiento es similar al de un bucle `for`, pero con un enfoque más declarativo, ya que se describe qué hacer con cada elemento en lugar de controlar manualmente el recorrido.

El método `forEach` pertenece a la interfaz `Iterable` y suele utilizarse junto con expresiones lambda. La función que recibe corresponde normalmente a un `Consumer<T>`, es decir, una operación que acepta un valor y no devuelve resultado. Esto hace que el código sea más compacto y expresivo.

El siguiente ejemplo recorre una lista de números enteros y muestra un mensaje únicamente cuando el número es positivo:

```java id="ihqkwm"
import java.util.List;

public class Main {

    public static void main(String[] args) {

        List<Integer> numeros =
                List.of(10, -3, 7, 0, -5, 12);

        numeros.forEach(numero -> {

            if (numero > 0) {
                System.out.println(
                        numero + " es positivo");
            }

        });
    }
}
```

En este caso, la función lambda se ejecuta automáticamente sobre cada elemento de la lista. Para cada número, se comprueba si es mayor que cero y, en caso afirmativo, se muestra el mensaje correspondiente. Este estilo funcional permite separar el recorrido de la colección de la operación concreta que se desea realizar sobre cada elemento.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

En Java, el método `forEach` utiliza la firma `Consumer<? super T>` en lugar de `Consumer<T>` para hacer el código más flexible respecto a la herencia y la genericidad. El método `forEach` consume elementos de tipo `T`, es decir, entrega objetos de la colección a la función lambda. Gracias a `? super T`, también es posible utilizar consumidores definidos para tipos más generales que `T`.

Esto está relacionado con el principio conocido como **PECS** (*Producer Extends, Consumer Super*). Esta regla sirve para decidir cuándo utilizar `extends` y cuándo utilizar `super` en tipos genéricos. La idea es la siguiente:

* **Producer Extends**: si una estructura produce datos que se van a leer, se usa `? extends T`.
* **Consumer Super**: si una estructura consume datos que se le van a pasar, se usa `? super T`.

En el caso de `forEach`, la función `Consumer` recibe elementos de la colección, por lo que actúa como consumidor. Por ejemplo, si se tiene una lista de `Integer`, puede utilizarse un `Consumer<Number>` porque un `Number` también puede consumir objetos `Integer`:

```java id="gwtqyn"
import java.util.List;
import java.util.function.Consumer;

public class Main {

    public static void main(String[] args) {

        List<Integer> numeros = List.of(1, 2, 3);

        Consumer<Number> mostrar =
                n -> System.out.println(n);

        numeros.forEach(mostrar);
    }
}
```

La misma idea puede aplicarse al método `transformar`. Inicialmente, podría definirse así:

```java id="ahmsrq"
public static <T, R> R transformar(
        T valor,
        Function<T, R> transformadora) {

    return transformadora.apply(valor);
}
```

Sin embargo, puede mejorarse usando PECS:

```java id="ynfclq"
public static <T, R> R transformar(
        T valor,
        Function<? super T, ? extends R> transformadora) {

    return transformadora.apply(valor);
}
```

Aquí, `? super T` indica que la función puede aceptar objetos de un tipo más general que `T`, mientras que `? extends R` permite devolver objetos más específicos que `R`. Esta versión es más flexible y reutilizable, ya que aprovecha correctamente la relación de herencia entre tipos genéricos.


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Las referencias a métodos permiten almacenar un método existente en una variable funcional sin necesidad de escribir explícitamente una función lambda que lo invoque. Conceptualmente, una referencia a método reutiliza directamente una función ya definida. Esto resulta útil para simplificar el código cuando la lambda únicamente se limita a llamar a otro método.

En Javascript, los métodos de un objeto pueden almacenarse en variables porque las funciones son ciudadanos de primera clase. Sin embargo, debe tenerse cuidado con el contexto `this`, por lo que suele utilizarse `bind` para asociar correctamente el método al objeto:

```javascript id="wqznki"
class Persona {

    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const persona = new Persona("Ana");

/* Referencia al método */
const referenciaSaludar =
        persona.saludar.bind(persona);

/* Invocación mediante la referencia */
referenciaSaludar();
```

En este ejemplo, `referenciaSaludar` almacena una referencia al método `saludar` del objeto `persona`. Posteriormente, la función se ejecuta indirectamente mediante la variable.

En Java, las referencias a métodos utilizan el operador `::`. Puede obtenerse una referencia a un método de instancia y almacenarla en una interfaz funcional compatible:

```java id="ghxtwj"
public class Main {

    static class Persona {

        private String nombre;

        public Persona(String nombre) {
            this.nombre = nombre;
        }

        public void saludar() {
            System.out.println(
                    "Hola, soy " + nombre);
        }
    }

    public static void main(String[] args) {

        Persona persona = new Persona("Ana");

        /* Referencia al método */
        Runnable referenciaSaludar =
                persona::saludar;

        /* Invocación mediante la referencia */
        referenciaSaludar.run();
    }
}
```

En este caso, `persona::saludar` crea una referencia al método de instancia `saludar`. Dicha referencia se almacena en un `Runnable`, ya que el método no recibe parámetros ni devuelve valor. Posteriormente, el método se ejecuta indirectamente usando `run()`.



## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

En Java, las referencias a métodos permiten reutilizar métodos ya existentes mediante el operador `::`. Son una forma abreviada de escribir determinadas expresiones lambda cuando la lambda simplemente invoca un método ya definido. Java admite varios tipos de referencias a métodos dependiendo de qué elemento se quiera referenciar.

Los tipos principales de referencias a métodos son:

* Referencia a método estático.
* Referencia a método de instancia de un objeto concreto.
* Referencia a método de instancia sobre cualquier objeto de un tipo.
* Referencia a constructor.

Una referencia a método estático utiliza el nombre de la clase seguido de `::` y el nombre del método:

```java id="ibahke"
import java.util.function.Function;

public class Main {

    public static int cuadrado(int x) {
        return x * x;
    }

    public static void main(String[] args) {

        Function<Integer, Integer> f =
                Main::cuadrado;

        System.out.println(f.apply(5));
    }
}
```

Una referencia a método de instancia de un objeto concreto utiliza una instancia específica ya creada:

```java id="glhsmq"
public class Main {

    static class Persona {

        private String nombre;

        public Persona(String nombre) {
            this.nombre = nombre;
        }

        public void saludar() {
            System.out.println(
                    "Hola, soy " + nombre);
        }
    }

    public static void main(String[] args) {

        Persona persona = new Persona("Ana");

        Runnable r = persona::saludar;

        r.run();
    }
}
```

Una referencia a método de instancia sobre cualquier objeto de un tipo se utiliza cuando el objeto se recibe implícitamente como primer parámetro:

```java id="hwlbpn"
import java.util.function.Function;

public class Main {

    public static void main(String[] args) {

        Function<String, Integer> longitud =
                String::length;

        System.out.println(
                longitud.apply("hola"));
    }
}
```

En este caso, `String::length` equivale aproximadamente a la lambda `s -> s.length()`.

Finalmente, una referencia a constructor permite crear objetos utilizando una interfaz funcional:

```java id="cdxtoq"
import java.util.function.Function;

public class Main {

    static class Persona {

        private String nombre;

        public Persona(String nombre) {
            this.nombre = nombre;
        }

        @Override
        public String toString() {
            return "Persona: " + nombre;
        }
    }

    public static void main(String[] args) {

        Function<String, Persona> creador =
                Persona::new;

        Persona p = creador.apply("Ana");

        System.out.println(p);
    }
}
```

Aquí, `Persona::new` referencia el constructor de la clase y permite crear objetos de manera funcional.



## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.
En Java, el método `Collections.sort` permite ordenar listas utilizando un comparador funcional. Gracias a las expresiones lambda, es posible definir directamente el criterio de ordenación sin necesidad de crear clases comparadoras independientes. En este ejemplo, la lista debe ordenarse primero por edad y, cuando dos personas tengan la misma edad, por orden alfabético del nombre.

La siguiente versión implementa la comparación manualmente dentro de la expresión lambda:

```java id="ekrhgq"
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Main {

    static class Persona {

        private String nombre;
        private int edad;

        public Persona(String nombre, int edad) {
            this.nombre = nombre;
            this.edad = edad;
        }

        public String getNombre() {
            return nombre;
        }

        public int getEdad() {
            return edad;
        }

        @Override
        public String toString() {
            return nombre + " (" + edad + ")";
        }
    }

    public static void main(String[] args) {

        List<Persona> personas = new ArrayList<>();

        personas.add(new Persona("Luis", 30));
        personas.add(new Persona("Ana", 25));
        personas.add(new Persona("Carlos", 30));
        personas.add(new Persona("Beatriz", 25));

        Collections.sort(personas, (p1, p2) -> {

            if (p1.getEdad() != p2.getEdad()) {
                return p1.getEdad() - p2.getEdad();
            }

            return p1.getNombre()
                     .compareTo(p2.getNombre());
        });

        personas.forEach(System.out::println);
    }
}
```

En este caso, la lambda compara manualmente las edades y, si coinciden, compara los nombres mediante `compareTo`. Aunque esta solución funciona correctamente, Java proporciona utilidades más expresivas mediante la clase `Comparator`.

La siguiente versión utiliza métodos auxiliares de `Comparator`, consiguiendo un código más legible y declarativo:

```java id="uvokxm"
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Main {

    static class Persona {

        private String nombre;
        private int edad;

        public Persona(String nombre, int edad) {
            this.nombre = nombre;
            this.edad = edad;
        }

        public String getNombre() {
            return nombre;
        }

        public int getEdad() {
            return edad;
        }

        @Override
        public String toString() {
            return nombre + " (" + edad + ")";
        }
    }

    public static void main(String[] args) {

        List<Persona> personas = new ArrayList<>();

        personas.add(new Persona("Luis", 30));
        personas.add(new Persona("Ana", 25));
        personas.add(new Persona("Carlos", 30));
        personas.add(new Persona("Beatriz", 25));

        Collections.sort(
                personas,
                Comparator.comparing(Persona::getEdad)
                          .thenComparing(Persona::getNombre)
        );

        personas.forEach(System.out::println);
    }
}
```

Esta segunda versión utiliza `Comparator.comparing` para indicar el criterio principal de ordenación y `thenComparing` para el criterio secundario. El resultado es más compacto y reutilizable, aprovechando mejor las capacidades funcionales incluidas en Java.

