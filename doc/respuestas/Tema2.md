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



#### 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Además de los niveles público y privado, la mayoría de los lenguajes de programación orientados a objetos introducen matices adicionales para gestionar la visibilidad en contextos de colaboración y jerarquía. En la programación estructurada en C, la visibilidad es binaria: o una función es global o es local al archivo (`static`). En la POO, la necesidad de organizar el código en paquetes o de permitir que las clases hijas hereden comportamientos obliga a expandir este abanico de opciones.

En el caso específico de **Java**, existen cuatro niveles de visibilidad, aunque solo tres utilizan una palabra clave explícita. Además de `public` y `private`, Java ofrece el modificador **`protected`**, que permite que un miembro sea accesible dentro del mismo paquete y también por las subclases, incluso si estas se encuentran en paquetes diferentes. Esto es fundamental para la reutilización de código mediante la herencia, permitiendo que las clases derivadas manipulen ciertos aspectos internos que el resto del mundo no puede ver.

Por otro lado, Java tiene el nivel de acceso **por defecto** (conocido como *package-private*), que se aplica cuando no se escribe ningún modificador. Este nivel restringe la visibilidad únicamente a las clases que se encuentran dentro del mismo paquete. Es una herramienta muy útil para el diseño de componentes o librerías, ya que permite que varias clases colaboren estrechamente entre sí sin exponer esa lógica interna al usuario final que importa la librería desde fuera.

En **otros lenguajes**, los conceptos son similares pero la terminología y el comportamiento varían. En **C++**, los niveles son idénticos en nombre (`public`, `private`, `protected`), pero existe el concepto de "clases amigas" (`friend`), que pueden saltarse las restricciones de acceso. Otros lenguajes modernos como **Swift** introducen niveles aún más granulares, como `fileprivate` (visible solo dentro del mismo archivo físico) o `internal` (visible en todo el módulo). Esta diversidad demuestra que la gestión de la visibilidad es una pieza clave de la arquitectura de software, permitiendo un equilibrio entre seguridad y flexibilidad.



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



#### 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los métodos *getter* y *setter* son funciones que permiten acceder y modificar los atributos privados de una clase respetando el principio de encapsulación. En lenguajes orientados a objetos como Java, se acostumbra a declarar los atributos como `private` para impedir que el código externo los manipule directamente. De este modo, se obliga a que cualquier lectura o cambio del valor pase por métodos controlados, lo que permite validar datos, mantener coherencia interna y evitar estados incorrectos del objeto.

Un *getter* es un método cuya única responsabilidad es **devolver el valor** de un atributo privado. Suele seguir la convención `getNombreAtributo()`. Por ejemplo, si una clase tiene un atributo `private int edad;`, el getter sería:

```java
public int getEdad() {
    return edad;
}
```

Por su parte, un *setter* es un método que **asigna un nuevo valor** a un atributo privado. Su nombre habitual es `setNombreAtributo(tipo valor)`. Este método puede incluir comprobaciones antes de aceptar el cambio, lo que añade una capa de seguridad y control. Siguiendo el ejemplo anterior:

```java
public void setEdad(int edad) {
    if (edad >= 0) {
        this.edad = edad;
    }
}
```

En conjunto, getters y setters permiten que el objeto mantenga su integridad interna, ya que ningún código externo puede alterar directamente sus datos sin pasar por estas funciones. Esto contrasta con C/C++ sin orientación a objetos, donde normalmente se accede a las variables de forma directa. En Java, este mecanismo es fundamental para aplicar correctamente la encapsulación y construir clases más robustas y fáciles de mantener.



#### 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No, en este contexto la palabra “seguridad” no se refiere a evitar que un programa sea hackeado. Cuando en programación orientada a objetos se habla de que la ocultación de información mejora la seguridad, se está describiendo una **seguridad interna del diseño**, no una seguridad informática frente a ataques externos. El objetivo es impedir que otras partes del código modifiquen directamente los datos internos de un objeto, lo que reduce errores, inconsistencias y comportamientos inesperados.

La encapsulación actúa como una barrera que protege el estado interno del objeto frente a usos incorrectos. Al obligar a acceder a los atributos mediante métodos controlados (getters y setters), se puede validar la información, restringir valores inválidos y mantener la coherencia del objeto. Esto evita que el programa entre en estados imposibles o dañinos debido a un mal uso del código, algo especialmente importante en sistemas grandes donde muchas partes interactúan entre sí.

Por tanto, la “seguridad” mencionada es más bien una **seguridad estructural o lógica**, que ayuda a que el programa sea más robusto, mantenible y menos propenso a fallos. No tiene relación directa con la protección frente a ataques externos, cifrado, vulnerabilidades o técnicas de hacking, que pertenecen a un ámbito completamente distinto dentro de la informática.



#### 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un **miembro de instancia** es aquel que pertenece a cada objeto creado a partir de una clase. Esto significa que cada instancia mantiene su propia copia independiente de ese atributo o método, y su valor puede variar entre objetos distintos. En Java, los miembros de instancia se crean cuando se construye un objeto con `new`, y desaparecen cuando ese objeto deja de existir. Este comportamiento se asemeja a tener varias estructuras en C, cada una con sus propios campos, pero con la diferencia de que en Java se trabaja dentro del paradigma orientado a objetos.

En cambio, un **miembro de clase** (también llamado *estático* o `static`) pertenece a la clase en sí misma, no a cada objeto. Solo existe una única copia compartida por todas las instancias. Esto resulta útil para valores comunes, contadores globales o utilidades que no dependen del estado particular de un objeto. En términos conceptuales, sería similar a una variable global en C, pero encapsulada dentro de una clase y con un ámbito más controlado.

Respecto a la ocultación, los miembros de clase **también pueden ocultarse** utilizando modificadores de acceso como `private`. El hecho de que sean estáticos no impide aplicar encapsulación. De hecho, es habitual declarar miembros estáticos como privados y proporcionar métodos públicos estáticos para acceder a ellos, manteniendo así el control sobre su uso. Esto permite que incluso los elementos compartidos sigan las mismas reglas de protección y consistencia que los atributos de instancia.

En conjunto, la diferencia clave radica en si el miembro pertenece a cada objeto o a la clase como entidad única. Sin embargo, ambos tipos pueden encapsularse y gestionarse mediante los mismos mecanismos de visibilidad, lo que refuerza la coherencia del diseño orientado a objetos.



## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene sentido en ciertos diseños que los constructores sean privados. Un constructor privado impide que el código externo cree instancias libremente, lo que permite controlar completamente cómo y cuándo se generan los objetos. Esta técnica se utiliza cuando no interesa que cada parte del programa pueda crear nuevas instancias sin restricciones, ya sea por motivos de coherencia interna o por la necesidad de limitar el número de objetos existentes.

Un caso típico es el **patrón Singleton**, donde solo debe existir una única instancia de la clase. Al declarar el constructor como privado, se evita que otros componentes creen nuevas instancias, y se obliga a usar un método controlado que devuelve siempre la misma. También es útil en clases que solo ofrecen métodos estáticos o en aquellas donde la creación de objetos requiere pasos adicionales que no deben quedar expuestos al exterior.

En resumen, aunque no es lo más habitual en clases comunes, los constructores privados tienen un propósito claro: restringir la creación de objetos para mantener un control estricto sobre el ciclo de vida y la cantidad de instancias. Esto forma parte de las técnicas de encapsulación que permiten diseñar sistemas más robustos y predecibles.



#### 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

En Java, los **miembros de clase** se indican utilizando la palabra clave `static`. Esto hace que el atributo o método pertenezca a la clase en sí y no a cada objeto individual. De este modo, todas las instancias comparten el mismo valor, lo que resulta útil cuando se necesita almacenar información común o llevar un registro global. La sintaxis es la misma que para cualquier miembro, simplemente añadiendo el modificador `static` delante del tipo.

Para ilustrarlo, puede añadirse a la clase `Punto` dos atributos estáticos que registren los valores máximos de `x` e `y` que hayan tenido todos los puntos creados. Cada vez que se construya un nuevo punto, el constructor puede comparar sus coordenadas con los valores almacenados y actualizarlos si es necesario. Así, se mantiene un seguimiento global sin necesidad de estructuras externas.

Un ejemplo posible sería:

```java
public class Punto {
    private int x;
    private int y;

    private static int maxX = Integer.MIN_VALUE;
    private static int maxY = Integer.MIN_VALUE;

    public Punto(int x, int y) {
        this.x = x;
        this.y = y;

        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }

    public static int getMaxX() {
        return maxX;
    }

    public static int getMaxY() {
        return maxY;
    }
}
```

En este diseño, `maxX` y `maxY` son miembros de clase, por lo que no pertenecen a ningún punto concreto, sino a la clase `Punto` como entidad única. Además, pueden encapsularse igual que cualquier otro atributo, utilizando `private` y proporcionando getters estáticos para acceder a ellos de forma controlada. Esto demuestra que los miembros de clase también pueden ocultarse y gestionarse mediante los mismos mecanismos de encapsulación que los miembros de instancia.



##### 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

Para crear un método factoría en Java que devuelva una instancia de la propia clase, es imprescindible utilizar el modificador **`static`**.

En Java, un método estático pertenece a la clase y no a una instancia específica, lo que permite llamarlo para crear objetos sin necesidad de tener un objeto previo. Además, para redondear las coordenadas de tipo `double` al entero más cercano, se utiliza habitualmente `Math.round()`, realizando un *cast* a `int` o `long` según el tipo de los atributos de tu clase.

###### Código del método factoría

Supuniendo que tus atributos internos son de tipo `int`:

```java
public static Punto crearPuntoRedondeado(double x, double y) {
    int xRedondeado = (int) Math.round(x);
    int yRedondeado = (int) Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}

```

---

###### ¿Por qué se usa `static`?

He usado **`static`** por las siguientes razones técnicas:

* **Punto de entrada:** Al ser un método de fabricación, su objetivo es construir el objeto. Si no fuera estático, tendrías el problema del "huevo y la gallina": necesitarías un objeto `Punto` ya existente para poder llamar al método que crea un `Punto`.
* **Encapsulamiento de la lógica:** Permite que la lógica de redondeo ocurra antes de la invocación del constructor, manteniendo el constructor de la clase simple y directo.
* **Semántica:** En Java, los métodos factoría (como `List.of()` o `Optional.of()`) son siempre estáticos por definición, ya que actúan como constructores con nombre.



##### 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

Para adaptar la clase a un array interno sin cambiar la interfaz pública (los métodos que el mundo exterior ve), simplemente cambiamos la declaración del atributo y ajustamos el constructor y los métodos para que accedan a los índices `0` (para X) y `1` (para Y).

Aquí tienes el código del método factoría adaptado a esta nueva estructura interna:

```java
public static Punto crearPuntoRedondeado(double x, double y) {
    // Redondeamos los valores antes de pasarlos al constructor
    int xRedondeado = (int) Math.round(x);
    int yRedondeado = (int) Math.round(y);
    
    // Suponiendo que tu constructor ahora recibe los dos int y los guarda en el array
    return new Punto(xRedondeado, yRedondeado);
}

```

Y para que entiendas cómo quedaría la "cocina" interna de la clase (aunque solo pediste el método), la estructura sería algo así:

```java
public class Punto {
    // Atributo interno: array de 2 posiciones
    private int[] coordenadas = new int[2];

    // Constructor que mantiene la interfaz de recibir dos enteros
    public Punto(int x, int y) {
        this.coordenadas[0] = x;
        this.coordenadas[1] = y;
    }

    // Métodos de acceso (getters) que ocultan el array al usuario
    public int getX() {
        return coordenadas[0];
    }

    public int getY() {
        return coordenadas[1];
    }
}

```

###### Notas para tus apuntes de C++ a Java:

1. **Encapsulamiento**: Al usar un array privado y métodos públicos `getX()`/`getY()`, estás aplicando el principio de ocultación de información. El usuario de la clase no sabe (ni le importa) si usas dos variables sueltas o un array.
2. **Inicialización de arrays**: A diferencia de C++, donde un array en el stack puede contener basura, en Java los arrays se inicializan por defecto a cero (o `null` si son objetos) al usar `new`.
3. **Uso de `static**`: El método sigue siendo `static` porque su función es "fabricar" la instancia; no depende de un estado previo del objeto, sino de los parámetros que recibe.


##### 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?
Esta es una de las dudas más comunes al saltar de C++ (donde a veces se es más laxo con las `struct`) a Java, donde la orientación a objetos es más estricta.

###### ¿Público o Privado?

La respuesta corta es: **No, nunca es mejor declararlo público**, aunque tengas un getter y un setter que parezcan "no hacer nada".

La convención más habitual en Java (y el estándar de la industria) es que **todos los atributos deben ser `private**`. Esto se conoce como **encapsulamiento**.

| Característica | Atributo Público | Getter / Setter (Atributo Privado) |
| --- | --- | --- |
| **Control** | Cualquiera puede cambiar el valor sin que la clase se entere. | La clase puede validar el dato antes de asignarlo. |
| **Abstracción** | Si cambias un `int` por un `long`, rompes todo el código que use la clase. | Puedes cambiar la implementación interna sin cambiar la firma del método. |
| **Depuración** | Es imposible poner un *breakpoint* para saber quién cambió una variable. | Puedes poner un *breakpoint* en el setter. |

---

###### Relación con las "Invariantes de Clase"

Aquí es donde reside la verdadera importancia del encapsulamiento. Una **invariante de clase** es una condición que debe ser verdadera durante toda la vida de un objeto.

Si tus atributos son públicos, la clase **no puede garantizar sus invariantes**, porque cualquier código externo puede violarlas.

**Ejemplo con tu clase `Punto`:**
Imagina que una invariante de tu clase es que las coordenadas no pueden ser negativas (por ejemplo, porque representan píxeles en pantalla).

* **Si el atributo es público:** Alguien puede hacer `miPunto.coordenadas[0] = -500;`. La clase ha perdido su integridad.
* **Si usas un setter:** Puedes proteger la invariante.

```java
public void setX(int x) {
    if (x >= 0) {
        this.coordenadas[0] = x;
    } else {
        // Lanzar una excepción o ignorar el cambio para mantener la invariante
        throw new IllegalArgumentException("La coordenada X no puede ser negativa");
    }
}

```

Al mantener los atributos privados, el objeto es el **único responsable** de mantener su estado consistente. Esto facilita enormemente el mantenimiento de software a gran escala.

###### El caso del array interno

En tu caso anterior, al usar un array `private int[] coordenadas`, si lo hubieras puesto público, cualquier programador despistado podría haber hecho `punto.coordenadas = new int[10];` o `punto.coordenadas = null;`. Al ser privado, tú aseguras que ese array siempre tendrá exactamente dos posiciones y nunca será nulo.




#### 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

En Java, el concepto de inmutabilidad es un pilar del diseño de software robusto, especialmente cuando pasas de C++ donde la gestión de memoria y punteros suele ser más manual.

###### ¿Qué significa que una clase sea inmutable?

Una clase es **inmutable** cuando su estado (sus atributos) no puede ser modificado después de que el objeto ha sido creado. Una vez que invocas el constructor o el método factoría, ese objeto morirá con los mismos valores con los que nació.

Para lograr esto en Java, se suelen seguir estas reglas:

1. Declarar todos los atributos como `private` y `final`.
2. No proporcionar métodos que modifiquen los atributos.
3. Asegurar que la clase no pueda ser heredada (usando la palabra reservada `final` en la declaración de la clase).
4. Si un atributo es una referencia a otro objeto mutable (como un array), no devolver la referencia directa, sino una copia.

###### ¿Qué es un método modificador?

Un **método modificador** (también llamado *mutator*) es cualquier método que cambia el estado interno del objeto.

> **Diferencia clave:** En una clase inmutable, **no existen métodos modificadores**.

###### ¿Un método modificador es siempre un "setter"?

**No.** Aunque el "setter" estándar (`setX(int x)`) es el ejemplo más obvio, cualquier método que altere los datos internos es un modificador. Por ejemplo:

* Un método `desplazar(int deltaX, int deltaY)` que sume valores a tus coordenadas actuales sería un modificador.
* Un método `limpiar()` que ponga las coordenadas a cero también lo sería.

En una clase **inmutable**, si quieres "desplazar" un punto, el método no modificaría el objeto actual, sino que **devolvería un objeto nuevo** con los nuevos valores:

```java
// Versión inmutable de desplazar
public Punto desplazar(int dx, int dy) {
    return new Punto(this.coordenadas[0] + dx, this.coordenadas[1] + dy);
}

```

###### Ventajas de las clases inmutables

Diseñar clases inmutables (como `String` o `Integer` en Java) ofrece beneficios críticos:

1. **Thread-safety (Seguridad en hilos):** Al no poder cambiar, varios hilos pueden leer el mismo objeto simultáneamente sin riesgo de corrupción de datos. No necesitas *mutex* ni bloqueos.
2. **Seguridad y Robustez:** Puedes pasar el objeto a cualquier método externo con la total certeza de que no lo van a modificar "por la espalda".
3. **Simplicidad de uso como llaves:** Son candidatos perfectos para ser usados en colecciones como `HashMap` o `HashSet`, ya que su valor (y por tanto su `hashCode`) nunca cambiará.
4. **Invariantes garantizadas:** Como el estado solo se establece en el constructor, si los datos son válidos al crear el objeto, serán válidos para siempre.

¿Te gustaría que transformáramos tu clase `Punto` actual para que sea totalmente inmutable siguiendo los estándares de Java?


##### 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

La respuesta corta es **no**. Aunque en muchos tutoriales básicos de Java parece que los "getters" y "setters" son obligatorios, seguirlos a ciegas es una mala práctica de diseño.

Aquí tienes los argumentos para tus apuntes:

###### 1. El principio de "Mínimo Privilegio"

En ingeniería de software, solo debes dar los permisos estrictamente necesarios. Si un atributo no necesita ser cambiado después de la creación del objeto, **no le pongas un setter**. Al poner un setter por defecto, estás invitando a otros programadores (o a ti mismo en el futuro) a modificar el estado del objeto de forma innecesaria, aumentando la complejidad y los posibles errores.

###### 2. Rompimiento del Encapsulamiento

Si una clase tiene setters para todos sus atributos, en la práctica estás exponiendo sus tripas casi tanto como si los atributos fueran públicos.

* **El problema:** El código que usa tu clase empieza a contener la lógica que debería estar *dentro* de la clase.
* **La solución:** En lugar de `setX(10)` y `setY(20)`, quizás es mejor un método `trasladar(10, 20)` o `centrarEnPantalla()`. Esto se conoce como **"Tell, Don't Ask"** (Dile al objeto qué hacer, no le pidas sus datos para calcularlo tú fuera).

###### 3. Mutabilidad innecesaria

Como vimos en la pregunta anterior, los objetos inmutables son más seguros. Si pones setters, conviertes automáticamente la clase en **mutable**. Esto significa que:

* No puedes compartir el objeto entre hilos de forma segura sin bloqueos.
* No puedes estar seguro de que el objeto mantenga su validez después de pasarlo como argumento a una función.

---

###### ¿Cuándo SÍ incluirlos?

Existen escenarios específicos donde los setters son la norma:

* **JavaBeans / POJOs (Plain Old Java Objects):** Muchos frameworks de Java (como Hibernate para bases de datos o Jackson para JSON) requieren constructores vacíos y métodos setter para "inyectar" los datos automáticamente.
* **Configuraciones:** Objetos que actúan como "ajustes" donde es natural cambiar un parámetro específico en tiempo de ejecución.

###### Resumen para PROII

| Escenario | Recomendación |
| --- | --- |
| **Clase de lógica/negocio** | **Evitarlos.** Priorizar constructores completos y métodos que representen acciones reales. |
| **Entidades de datos (DTOs)** | **Usarlos con moderación** o solo si el framework lo exige. |
| **Constantes/Valores** | **Prohibidos.** La clase debe ser inmutable. |



##### 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

Esta es una de las preguntas "clásicas" de examen en **PROII**, ya que entender cómo gestiona Java la memoria de los textos es fundamental para el rendimiento.

###### 1. ¿Es `String` mutable o inmutable?

La clase `String` en Java es **estrictamente inmutable**. Una vez que un objeto `String` se crea en la memoria (específicamente en una zona llamada *String Pool*), su contenido no puede cambiar jamás.

###### 2. ¿Qué ocurre al concatenar dos cadenas?

Cuando usas el operador `+` para concatenar, **no estás modificando** ninguna de las cadenas originales. Java realiza los siguientes pasos:

1. Calcula el tamaño necesario para la nueva cadena.
2. Crea un **nuevo objeto** `String` en la memoria.
3. Copia los caracteres de la primera y la segunda cadena en el nuevo objeto.

Si haces esto dentro de un bucle (por ejemplo, 1000 veces), estarás creando 1000 objetos temporales que el *Garbage Collector* (recolector de basura) tendrá que limpiar después, lo cual es muy ineficiente en términos de tiempo y memoria.

###### 3. ¿Qué hacer para concatenaciones masivas?

Si necesitas construir una cadena paso a paso (por ejemplo, leyendo un archivo grande o concatenando en un bucle `for`), **nunca** uses `String`. En su lugar, debes usar la clase **`StringBuilder`**.

`StringBuilder` funciona como un "array dinámico" de caracteres (similar a un `std::vector<char>` en C++) que sí es **mutable**. Permite añadir texto sin crear objetos nuevos constantemente.

###### Ejemplo comparativo:

**Mal (Ineficiente):**

```java
String resultado = "";
for (int i = 0; i < 1000; i++) {
    resultado += "dato "; // Crea un objeto nuevo en cada iteración
}

```

**Bien (Eficiente):**

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append("dato "); // Modifica el buffer interno, no crea objetos nuevos
}
String resultado = sb.toString(); // Solo se crea el String final al terminar

```

---

###### Resumen para tus apuntes:

* **`String`**: Inmutable. Seguro para compartir (thread-safe), pero lento para modificaciones frecuentes.
* **Concatenación (`+`)**: Crea objetos nuevos. Úsala solo para operaciones puntuales.
* **`StringBuilder`**: Mutable. Úsalo siempre que vayas a construir cadenas en bucles o pasos sucesivos.
* **`StringBuffer`**: Es igual que `StringBuilder` pero sincronizado (para entornos multi-hilo), aunque hoy en día se prefiere casi siempre `StringBuilder` por ser más rápido.



##### 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En Programación Orientada a Objetos, y específicamente en Java, la distinción entre **identidad** y **contenido** es el origen de muchos errores cuando vienes de C++.

###### 1. ¿Identidad o Contenido?

* **Identidad (Referencia):** Se refiere a si dos variables apuntan exactamente al mismo objeto en la memoria (la misma dirección). Se comprueba con el operador `==`.
* **Contenido (Estado):** Se refiere a si dos objetos diferentes tienen los mismos valores en sus atributos. Se comprueba con el método `.equals()`.

En Java, cuando comparas objetos con `==`, **siempre estás comparando identidades** (punteros), no el contenido.

---

###### 2. El método `equals` en Java

Todos los objetos en Java heredan de la clase `Object`, la cual incluye el método `public boolean equals(Object obj)`. Su propósito es permitir que cada clase defina qué significa que dos de sus instancias sean "iguales".

**¿Qué hace por defecto?**
La implementación por defecto en la clase `Object` simplemente hace un `==`. Es decir, por defecto, **dos objetos solo son iguales si son el mismo objeto**.

Si quieres que dos puntos con coordenadas $(5, 10)$ sean considerados iguales aunque vivan en posiciones de memoria distintas, **debes sobrescribir (`override`)** el método `equals` en tu clase `Punto`.

---

###### 3. ¿Cómo se deben comparar dos cadenas (Strings)?

Debido a la inmutabilidad y al *String Pool*, usar `==` con cadenas es extremadamente peligroso porque a veces puede funcionar (si Java ha reutilizado la misma instancia) y a veces no.

**Regla de oro:** Las cadenas en Java se comparan **siempre** con el método `.equals()`.

* **MAL (Compara si son el mismo objeto):** `if (nombre == "Juan")`
* **BIEN (Compara el texto):** `if (nombre.equals("Juan"))`
* **IGNORANDO MAYÚSCULAS:** `if (nombre.equalsIgnoreCase("juan"))`

---

###### Ejemplo práctico para tu clase `Punto`

Si quieres comparar tus puntos por su contenido (el array de coordenadas), tu `equals` debería verse así:

```java
@Override
public boolean equals(Object obj) {
    if (this == obj) return true; // Misma identidad
    if (obj == null || getClass() != obj.getClass()) return false; // Tipos distintos
    
    Punto otro = (Punto) obj;
    // Comparamos el contenido del array
    return this.coordenadas[0] == otro.coordenadas[0] && 
           this.coordenadas[1] == otro.coordenadas[1];
}

```

> **Nota importante:** En Java, si sobrescribes `equals`, la buena práctica dicta que también debes sobrescribir `hashCode()`. Si no lo haces, tus objetos no funcionarán correctamente en colecciones como `HashMap`.



##### 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases **wrapper** (o clases envoltorio) son una solución para un dilema de diseño en Java: la existencia de tipos que no son objetos.

###### 1. ¿Qué son y cómo se hace?

En Java, existen **tipos primitivos** (como `int`, `double`, `boolean`) que se heredan del modelo de C por eficiencia. Sin embargo, Java es un lenguaje orientado a objetos donde muchas herramientas (como las colecciones `ArrayList`, `HashMap`, etc.) solo funcionan con **objetos**.

Una clase wrapper es una clase que "envuelve" un valor primitivo dentro de un objeto.

* Para `int`, el wrapper es `Integer`.
* Para `double`, es `Double`.
* Para `char`, es `Character`.

###### 2. ¿Es un proceso automático?

Sí, y tiene nombres específicos: **Autoboxing** y **Unboxing**.

* **Autoboxing:** El paso automático de un primitivo a su clase wrapper.
```java
Integer numero = 10; // Java hace automáticamente: Integer.valueOf(10)

```


* **Unboxing:** El paso automático de un objeto wrapper a su valor primitivo.
```java
int n = numero; // Java hace automáticamente: numero.intValue()

```



###### 3. ¿Qué ventajas tienen?

* **Colecciones:** No puedes crear un `ArrayList<int>`, pero sí un `ArrayList<Integer>`.
* **Métodos de utilidad:** Las clases wrapper contienen métodos estáticos muy útiles para conversión de datos, como `Integer.parseInt("123")` o `Double.isNaN(valor)`.
* **Valores nulos:** Un `int` siempre tiene un valor (por defecto 0), pero un `Integer` puede ser `null`. Esto es vital en bases de datos para representar la ausencia de un dato.
* **Genéricos:** Son imprescindibles para trabajar con tipos genéricos (`<T>`), ya que estos requieren objetos.

---

###### 4. ¿Todos los lenguajes POO tienen tipos primitivos y necesitan wrappers?

**No.** Aquí es donde Java se diferencia de otros lenguajes:

* **Lenguajes Híbridos (como Java o C++):** Mantienen tipos primitivos por rendimiento (son mucho más rápidos y ocupan menos memoria que los objetos). Por eso Java necesita los wrappers como "puente".
* **Lenguajes "Puros" (como Smalltalk, Ruby o Python):** En estos lenguajes, **todo es un objeto**. No existe el `int` como algo ajeno al sistema de objetos; hasta un número literal como `5` tiene métodos.
* **C#:** Tiene una aproximación interesante llamada *Unified Type System*, donde los primitivos pueden comportarse como objetos sin necesidad de una clase wrapper explícita tan diferenciada como en Java.

###### Resumen para tus apuntes

| Tipo Primitivo | Clase Wrapper |
| --- | --- |
| `byte` | `Byte` |
| `short` | `Short` |
| `int` | `Integer` |
| `long` | `Long` |
| `float` | `Float` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |

> **Cuidado de programador:** Aunque el autoboxing es cómodo, abusar de él en bucles muy pesados puede ralentizar tu programa comparado con el uso de primitivos puros, debido a la creación constante de objetos.

¿Te gustaría que viéramos cómo usar estos wrappers para convertir cadenas de texto que recibas por consola a números para tu clase `Punto`?


##### 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un **tipo de dato enumerado** (`enum`) es un tipo de dato especial que permite definir un conjunto de valores constantes y fijos. Se utiliza cuando sabemos de antemano todos los posibles valores que puede tomar una variable (por ejemplo: los días de la semana, los puntos cardinales o los estados de un pedido).

###### 1. ¿En Java, un tipo de dato enumerado es una clase?

**Sí, rotundamente.** A diferencia de C++, donde un `enum` es básicamente una lista de enteros (`int`), en Java un `enum` es una **clase especial**.

Cuando defines un `enum`, Java crea una clase que hereda de `java.lang.Enum`. Esto significa que:

* Puede tener **atributos** (campos).
* Puede tener **métodos**.
* Puede tener **constructores** (aunque siempre son privados).
* Puede implementar interfaces.

###### 2. Ventajas en términos de encapsulación

Al ser clases completas, los enumerados en Java ofrecen ventajas de encapsulación que no existen en lenguajes más simples:

* **Control de estado interno:** Puedes asociar datos a cada constante de forma protegida. Por ejemplo, un enumerado `Color` puede guardar internamente su código hexadecimal y exponerlo mediante un "getter", pero no permitir que se cambie.
* **Validación de tipos (Type Safety):** No puedes asignar un valor que no esté definido en el `enum`. El compilador garantiza que la variable solo contenga uno de los valores permitidos, evitando estados inconsistentes en el objeto.
* **Comportamiento asociado:** Puedes definir métodos abstractos dentro del `enum` que cada constante debe implementar de forma distinta. Esto encapsula la lógica específica de cada valor dentro del propio valor, evitando estructuras `if-else` o `switch` dispersas por todo el código.
* **Sintaxis limpia:** Permite ocultar la complejidad de la configuración de una constante detrás de un nombre sencillo (ej. `Estado.CANCELADO`).

---

###### Ejemplo práctico: Enumerado con lógica

Imagina que en tu proyecto necesitas definir los ejes de coordenadas:

```java
public enum Eje {
    X("Horizontal"), 
    Y("Vertical");

    private final String descripcion; // Atributo encapsulado

    // Constructor privado (Java no permite que sea público)
    Eje(String descripcion) {
        this.descripcion = descripcion;
    }

    public String getDescripcion() {
        return descripcion;
    }
}

```

###### Notas para tus apuntes de C++ a Java:

1. **Comparación:** En Java puedes usar `==` para comparar enumerados de forma segura, ya que las instancias son únicas (Singleton).
2. **Iteración:** Todos los `enum` en Java tienen el método estático `.values()`, que devuelve un array con todas las constantes, facilitando mucho el recorrido.
3. **Orden:** Tienen un método `.ordinal()` que indica su posición, pero se recomienda no usarlo para lógica de negocio para evitar errores si cambias el orden de los elementos.



##### 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

Para implementar este enumerado en Java, aprovecharemos que los `enum` pueden tener constructores y atributos privados, comportándose casi como una clase normal.

Aquí tienes la implementación completa para tus apuntes:

```java
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private final int dias;
    private final int orden;

    // El constructor de un enum siempre es privado o paquete-privado
    Mes(int dias, int orden) {
        this.dias = dias;
        this.orden = orden;
    }

    public int getDias() {
        return this.dias;
    }

    public int getOrden() {
        return this.orden;
    }

    // Métodos de estación con lógica de hemisferio
    
    public boolean esDePrimavera(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            // Norte: Marzo, Abril, Mayo, Junio
            return orden >= 3 && orden <= 6;
        } else {
            // Sur: Septiembre, Octubre, Noviembre, Diciembre
            return orden >= 9 && orden <= 12;
        }
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            // Norte: Junio, Julio, Agosto, Septiembre
            return orden >= 6 && orden <= 9;
        } else {
            // Sur: Diciembre, Enero, Febrero, Marzo
            return orden == 12 || (orden >= 1 && orden <= 3);
        }
    }

    public boolean esDeOtoño(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            // Norte: Septiembre, Octubre, Noviembre, Diciembre
            return orden >= 9 && orden <= 12;
        } else {
            // Sur: Marzo, Abril, Mayo, Junio
            return orden >= 3 && orden <= 6;
        }
    }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            // Norte: Diciembre, Enero, Febrero, Marzo
            return orden == 12 || (orden >= 1 && orden <= 3);
        } else {
            // Sur: Junio, Julio, Agosto, Septiembre
            return orden >= 6 && orden <= 9;
        }
    }
}

```

###### Notas técnicas para PROII:

* **Atributos `final**`: Como los enumerados representan constantes, es una excelente práctica declarar sus atributos como `final` para garantizar la inmutabilidad.
* **Lógica de estaciones**: He simplificado la lógica considerando los meses completos donde hay presencia de esa estación (por ejemplo, Junio tiene días de Primavera y de Verano en el norte).
* **Constructor**: Fíjate que al invocar `ENERO(31, 1)`, Java busca automáticamente el constructor que coincida con esos argumentos. No se usa la palabra `new`.
* **Diferencia con C++**: En C++ tendrías que usar un `switch` externo o un mapa para obtener los días; en Java, el dato viaja "pegado" a la constante.

