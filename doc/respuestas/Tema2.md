<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

#### 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación y la ocultación de información buscan establecer una frontera clara entre el comportamiento externo de un objeto y su implementación interna. Mientras que en la programación estructurada en C los datos suelen estar expuestos y son manipulables por cualquier función que tenga acceso a la estructura, en la POO se persigue que el objeto sea el único responsable de gestionar su propio estado. Esto garantiza que la estructura interna de los datos sea invisible para el resto del programa, permitiendo que la interacción se realice exclusivamente a través de una interfaz controlada.

El objetivo fundamental es asegurar la integridad de los datos y reducir la complejidad del sistema. Al agrupar los datos y los métodos que los manipulan en una sola unidad (clase), se evita que agentes externos modifiquen variables de forma arbitraria o incorrecta. En términos técnicos, se busca que el "qué hace" un objeto sea público, mientras que el "cómo lo hace" permanezca privado, logrando así un desacoplamiento que facilita el mantenimiento del software.

En cuanto a las ventajas de la ocultación de información, se pueden enumerar las siguientes:

* **Robustez e integridad:** Se impide que el estado interno del objeto alcance valores inválidos o incoherentes, ya que todo cambio debe pasar por filtros de validación (métodos de acceso).
* **Facilidad de mantenimiento:** Es posible modificar la estructura interna de una clase (por ejemplo, cambiar un tipo de dato de `int` a `float`) sin necesidad de reescribir el código de las otras clases que la utilizan.
* **Reducción de la complejidad:** El programador que utiliza la clase no necesita comprender los detalles técnicos de su funcionamiento interno, sino únicamente conocer los métodos que conforman su interfaz pública.
* **Seguridad:** Se protege la lógica de negocio y los datos sensibles frente a accesos accidentales o malintencionados desde otras partes del sistema.



#### 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La **interfaz pública** de una clase se define como el conjunto de métodos, constantes y comportamientos que se ponen a disposición de otros objetos para interactuar con ella. En términos técnicos, representa el "contrato" o la lista de servicios que la clase promete cumplir. A diferencia de las estructuras en C, donde se accede directamente a los miembros de la `struct`, en la POO se espera que la interacción ocurra únicamente a través de estos puntos de entrada autorizados, los cuales suelen marcarse con el modificador `public` en Java.

Este concepto actúa como un punto de comunicación donde se define qué acciones puede realizar el objeto sin revelar cómo se llevan a cabo. Al diseñar una interfaz pública, se busca que sea lo más simple y estable posible, ocultando la complejidad subyacente. Un ejemplo cotidiano sería el panel de control de una lavadora: el usuario interactúa con botones (la interfaz), pero no necesita manipular los cables o el motor directamente para completar el ciclo de lavado.

La relación con la **ocultación de información** es de interdependencia absoluta. Mientras la ocultación se encarga de proteger los detalles técnicos y los datos sensibles (marcando atributos como `private`), la interfaz pública determina qué parte de esa funcionalidad es segura de exponer. Sin ocultación, la interfaz pública sería irrelevante porque todo el interior del objeto estaría expuesto; sin interfaz pública, el objeto sería una caja negra totalmente aislada y, por tanto, inútil para el resto del sistema.

Por tanto, se establece una división clara: la interfaz es la fachada visible y el mecanismo de control, mientras que la ocultación es la barrera que resguarda la implementación. Esta separación permite que el desarrollador de la clase pueda optimizar o reescribir el código interno de un método sin que los "clientes" de esa clase perciban cambio alguno, siempre y cuando la firma del método en la interfaz pública se mantenga constante.



#### 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

 Diseñar con cuidado la **interfaz pública** es fundamental porque representa el compromiso formal que una clase adquiere con el resto del sistema. Una vez que otros programadores o módulos comienzan a utilizar los métodos públicos de una clase, cualquier cambio en ellos genera un "efecto dominó" que puede romper la funcionalidad en múltiples puntos del software. Por ello, se debe exponer únicamente lo estrictamente necesario, siguiendo el principio de mínima exposición, para mantener el sistema lo más sencillo y robusto posible.

La interfaz debe ser vista como una frontera estable. Si se exponen detalles de implementación por error, se pierde la libertad de mejorar el código interno en el futuro sin afectar a terceros. En el desarrollo profesional, una interfaz pública mal diseñada se convierte en una "deuda técnica", obligando a mantener métodos obsoletos o ineficientes simplemente porque hay demasiado código externo que depende de ellos para funcionar.

En cuanto a la facilidad de cambio, la respuesta es que **no es fácil cambiarla** una vez que la clase está en uso. A diferencia de la lógica interna (el cuerpo de los métodos), que puede modificarse libremente siempre que el resultado final sea el mismo, alterar la firma de un método público (su nombre, sus parámetros o su tipo de retorno) requiere actualizar todas y cada una de las llamadas a ese método en todo el proyecto.

En proyectos grandes o librerías distribuidas, cambiar la interfaz pública es una tarea crítica que a menudo se evita. Si el cambio es inevitable, se suele recurrir a procesos lentos como la "deprecación" (marcar el método antiguo como obsoleto mientras se introduce el nuevo), permitiendo una transición gradual. Por el contrario, cambiar la implementación privada es sumamente sencillo y seguro, lo que refuerza la importancia de mantener la interfaz pública lo más pequeña y meditada posible.



#### 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las **invariantes de clase** son condiciones o reglas lógicas que deben cumplirse siempre para que el estado de un objeto se considere válido. Se definen durante el diseño de la clase y representan la "verdad" del objeto a lo largo de toda su existencia. Por ejemplo, en una clase que represente una fecha, una invariante sería que el valor del mes siempre debe estar en el rango de 1 a 12, o en una clase `CuentaBancaria`, que el saldo nunca sea inferior al límite de crédito permitido.

A diferencia de lo que ocurre en C, donde una `struct` puede ser manipulada externamente y quedar en un estado inconsistente (como una variable `edad` con valor -5), en Java se busca que el objeto nazca válido a través de su constructor y se mantenga así hasta su destrucción. Las invariantes actúan como las leyes internas que gobiernan la coherencia de los datos, asegurando que el objeto siempre sea una representación fiel y lógica de la entidad que pretende modelar.

La **ocultación de información** es la herramienta que permite garantizar el cumplimiento de estas invariantes. Al marcar los atributos como `private`, se impide que código externo modifique los valores de manera arbitraria. Si los datos estuvieran expuestos, cualquier otra parte del programa podría romper las reglas de la clase, ya sea por error o por desconocimiento, dejando al objeto en un estado corrupto que provocaría fallos difíciles de depurar en otras secciones del sistema.

Al centralizar el acceso a través de métodos públicos (setters), se introduce un punto de control donde se valida que el nuevo valor respete la invariante antes de ser asignado. Si el valor es incorrecto, el método puede rechazar el cambio o lanzar una excepción. En definitiva, la ocultación transforma la clase en una "fortaleza" que protege su propia integridad, permitiendo que el resto del programa confíe plenamente en que los objetos con los que interactúa siempre están en un estado correcto y previsible.



#### 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

Para ilustrar los conceptos de encapsulación y ocultación de información, se presenta a continuación la implementación de la clase `Punto`. En este diseño, los datos internos están protegidos y la interacción se realiza exclusivamente a través de métodos definidos.

```java
public class Punto {
    // Atributos privados: no accesibles desde fuera de la clase
    private double x;
    private double y;

    // Constructor: Interfaz pública para crear el objeto
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Métodos de acceso (Getters y Setters): Interfaz pública para leer/modificar
    public double getX() {
        return x;
    }

    public void setX(double x) {
        this.x = x;
    }

    public double getY() {
        return y;
    }

    public void setY(double y) {
        this.y = y;
    }

    // Método de comportamiento: Interfaz pública para realizar cálculos
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(Math.pow(x, 2) + Math.pow(y, 2));
    }
}

```

La **interfaz pública** de esta clase está compuesta por todos los elementos marcados con el modificador `public`. En este caso, incluye el constructor `Punto(double x, double y)`, los métodos para obtener y modificar las coordenadas (`getX`, `setX`, `getY`, `setY`) y el método de cálculo `calcularDistanciaAOrigen`. Cualquier programa externo que utilice esta clase solo "ve" y puede llamar a estos métodos, ignorando por completo cómo se almacenan las variables internamente.

El modificador **`private`** es la base de la ocultación de información. Al declarar los atributos `x` e `y` como privados, se garantiza que ninguna otra clase pueda modificar sus valores directamente (por ejemplo, haciendo `punto.x = 10;`). Esto es una diferencia clave respecto a las `struct` de C, donde el acceso suele ser libre. Si se quisiera añadir una regla (invariante) como que las coordenadas no pueden ser negativas, el modificador `private` permitiría centralizar esa validación en los métodos `set` sin que el resto del código se vea afectado.

Por otro lado, el modificador **`public`** define la visibilidad total. Se utiliza para exponer los servicios que la clase ofrece al mundo exterior. Mientras que `private` se usa para proteger la "cocina" o los detalles internos del objeto, `public` define el "menú" disponible para los usuarios de la clase. Una buena práctica en Java, a diferencia de la programación en C/C++ no orientada a objetos, es mantener siempre los atributos en `private` y solo hacer públicos los métodos necesarios para operar con ellos.


#### 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores de acceso `public` y `private` tienen un alcance que va más allá de los simples atributos, aplicándose a diferentes niveles de la estructura del código. Su uso principal se da sobre los **miembros de una clase**, que incluyen tanto las variables de instancia (atributos) como los métodos. Aplicar `private` a un método, por ejemplo, es útil para crear "métodos auxiliares" que realizan tareas internas y no deben ser invocados desde fuera, algo similar a las funciones estáticas en un archivo fuente de C que no se declaran en el `.h`.

Además de los miembros, estos modificadores pueden aplicarse a las **clases mismas** (clases de nivel superior), aunque con ciertas restricciones. Una clase pública (`public`) es accesible desde cualquier otra clase en cualquier paquete, lo que constituye la unidad básica de una librería. Sin embargo, en Java, una clase de nivel superior no puede ser declarada como `private`, ya que no tendría sentido una clase que nadie puede usar; el nivel de restricción equivalente para clases sería el acceso por defecto (package-private), donde solo es visible dentro de su propio paquete.

También es posible aplicar estos modificadores a los **constructores**. Un constructor `public` permite crear instancias de la clase desde cualquier lugar del programa. Por el contrario, un constructor `private` impide que otras clases creen objetos de ese tipo usando el operador `new`. Esta técnica se utiliza en patrones de diseño específicos para controlar estrictamente cuántas instancias de una clase existen o para agrupar métodos de utilidad (como la clase `Math` de Java), donde no tiene sentido crear un objeto.

Es importante notar que estos modificadores **no pueden aplicarse a las variables locales** (aquellas definidas dentro de un método). En C, una variable dentro de una función tiene un ámbito limitado a esa función, y en Java ocurre lo mismo: su visibilidad está intrínsecamente restringida al bloque de código donde fue declarada, por lo que el uso de `public` o `private` en ese contexto resultaría en un error de compilación.

¿Te gustaría que exploráramos qué sucede cuando no se pone ningún modificador, el llamado acceso por defecto o "package-private"?


#### 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Además de los niveles público y privado, la mayoría de los lenguajes de programación orientados a objetos introducen matices adicionales para gestionar la visibilidad en contextos de colaboración y jerarquía. En la programación estructurada en C, la visibilidad es binaria: o una función es global o es local al archivo (`static`). En la POO, la necesidad de organizar el código en paquetes o de permitir que las clases hijas hereden comportamientos obliga a expandir este abanico de opciones.

En el caso específico de **Java**, existen cuatro niveles de visibilidad, aunque solo tres utilizan una palabra clave explícita. Además de `public` y `private`, Java ofrece el modificador **`protected`**, que permite que un miembro sea accesible dentro del mismo paquete y también por las subclases, incluso si estas se encuentran en paquetes diferentes. Esto es fundamental para la reutilización de código mediante la herencia, permitiendo que las clases derivadas manipulen ciertos aspectos internos que el resto del mundo no puede ver.

Por otro lado, Java tiene el nivel de acceso **por defecto** (conocido como *package-private*), que se aplica cuando no se escribe ningún modificador. Este nivel restringe la visibilidad únicamente a las clases que se encuentran dentro del mismo paquete. Es una herramienta muy útil para el diseño de componentes o librerías, ya que permite que varias clases colaboren estrechamente entre sí sin exponer esa lógica interna al usuario final que importa la librería desde fuera.

En **otros lenguajes**, los conceptos son similares pero la terminología y el comportamiento varían. En **C++**, los niveles son idénticos en nombre (`public`, `private`, `protected`), pero existe el concepto de "clases amigas" (`friend`), que pueden saltarse las restricciones de acceso. Otros lenguajes modernos como **Swift** introducen niveles aún más granulares, como `fileprivate` (visible solo dentro del mismo archivo físico) o `internal` (visible en todo el módulo). Esta diversidad demuestra que la gestión de la visibilidad es una pieza clave de la arquitectura de software, permitiendo un equilibrio entre seguridad y flexibilidad.

¿Desea que profundicemos en algún escenario donde el acceso por defecto de Java sea preferible al uso de `public` o `private`?


#### 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

La respuesta correcta es la **(a): los miembros privados están ocultos para otras clases**. Es un error común pensar que la privacidad se aplica a nivel de objeto (instancia), cuando en realidad en Java se aplica a nivel de clase. Esto significa que un objeto de la clase `Punto` puede acceder directamente a los atributos privados de *otro* objeto de la clase `Punto`, ya que ambos comparten la misma definición y lógica interna.

En C, si se tuviera una estructura, cualquier función con el puntero adecuado podría acceder a los datos. En Java, el modificador `private` levanta una muralla contra el exterior, pero permite "confianza total" entre miembros de la misma familia (clase). A continuación, se muestra cómo el método `calcularDistanciaAPunto` utiliza esta característica para acceder a las coordenadas del segundo punto sin necesidad de usar métodos públicos.

```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método que recibe otra instancia de la misma clase
    public double calcularDistanciaAPunto(Punto otro) {
        // Se accede directamente a 'otro.x' y 'otro.y' a pesar de ser private
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

```

Este comportamiento es fundamental para la eficiencia y la legibilidad en POO. Como se observa en el código, el objeto actual (`this`) manipula los datos de `otro` directamente. Si la privacidad fuera por instancia (opción b), el programador se vería obligado a usar `otro.getX()`, lo cual añadiría una capa de indirección innecesaria dentro de la propia lógica de la clase donde ya se conocen los detalles de implementación.

En resumen, la ocultación protege la integridad del objeto frente a manipulaciones externas de clases desconocidas, pero no restringe la visibilidad entre objetos hermanos. Esta distinción permite que operaciones como la comparación de igualdad o cálculos matemáticos entre dos entidades del mismo tipo se realicen de manera directa y sencilla, manteniendo la seguridad frente al resto del programa.



## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
