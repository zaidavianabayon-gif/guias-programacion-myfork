<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas, y sin emojis.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.


En orientación a objetos, la **herencia** es el mecanismo por el cual una clase adquiere automáticamente los atributos y métodos definidos en otra clase. La relación conceptual que la guía es “A es‑un B”: si `Artillero` es‑un `Soldado`, entonces un objeto de tipo `Artillero` puede utilizarse allí donde se espere un `Soldado`. Esta relación no implica que ambos sean idénticos, sino que la subclase amplía o especializa el comportamiento de la superclase manteniendo su esencia. Desde el punto de vista del diseño, la herencia permite factorizar código común en una clase base y evitar duplicación.

La primera implicación importante es la **compatibilidad de tipos**. Si una clase hereda de otra, cualquier instancia de la subclase puede tratarse como si fuera de la superclase. Esto permite almacenar objetos de distintos subtipos en estructuras homogéneas, como arrays o listas, y operar sobre ellos mediante la interfaz común definida en la superclase. Esta compatibilidad es fundamental para el polimorfismo, ya que permite invocar métodos comunes sin conocer el subtipo concreto.

La segunda implicación es la **herencia de estado y comportamiento**. Las subclases reciben automáticamente los atributos y métodos definidos en la superclase. Aunque un atributo sea privado, sigue formando parte del estado heredado y puede utilizarse mediante los métodos públicos o protegidos de la superclase. A partir de esa base común, cada subtipo puede añadir su propio estado y sus propios métodos, especializando el comportamiento sin romper la interfaz heredada.

---

## Ejemplo en Java

### Superclase `Soldado`
```java
public class Soldado {
    private final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy " + nombre + ", a sus órdenes.");
    }

    public String getNombre() {
        return nombre;
    }
}
```

### Subclase `Artillero`
```java
public class Artillero extends Soldado {
    private final int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }

    public int getNumCohetes() {
        return numCohetes;
    }

    public void disparar() {
        System.out.println(getNombre() + " dispara un cohete.");
    }
}
```

### Subclase `Zapador`
```java
public class Zapador extends Soldado {
    private final int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }

    public void ponerMina() {
        System.out.println(getNombre() + " coloca una mina.");
    }
}
```

---

## Compatibilidad de tipos en acción

```java
public class Main {
    public static void main(String[] args) {

        Soldado[] escuadra = new Soldado[] {
            new Soldado("Ramírez"),
            new Artillero("Gómez", 5),
            new Zapador("López", 3)
        };

        for (Soldado s : escuadra) {
            s.saludar();
        }
    }
}
```







## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 



Cuando se crea un objeto de una subclase, se ejecutan **dos constructores**: primero el de la **superclase** y después el de la **subclase**. Este orden es obligatorio porque la parte heredada del objeto debe inicializarse antes de que la subclase pueda completar su propia construcción. En Java, la primera instrucción de cualquier constructor de una subclase es siempre una llamada implícita o explícita al constructor de la superclase. Si no se escribe nada, el compilador inserta automáticamente `super()` como primera línea.

La palabra clave `super` dentro de un constructor sirve para invocar explícitamente un constructor concreto de la clase base. Esto permite pasar parámetros necesarios para inicializar el estado heredado. En el ejemplo anterior, tanto `Artillero` como `Zapador` llaman a `super(nombre)` porque la clase `Soldado` exige un nombre para poder construirse. Sin esta llamada, la subclase no podría inicializar correctamente la parte heredada del objeto.

Si la clase base **no tiene visible** el constructor sin parámetros (por ejemplo, porque no existe o es privado), entonces la llamada a `super` debe realizarse de forma explícita y con los argumentos adecuados. En estos casos, no es posible confiar en la llamada implícita `super()`, ya que el compilador no puede encontrar un constructor válido. Por tanto, siempre que la superclase requiera parámetros, la subclase debe invocar explícitamente el constructor correspondiente mediante `super(...)`.




## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Sí, los atributos privados de la superclase **forman parte** de cada instancia de la subclase en memoria. Cuando se crea un objeto de una subclase, este contiene físicamente tanto la parte correspondiente a la superclase como la parte añadida por la propia subclase. En el caso de `Artillero` o `Zapador`, cada objeto incluye internamente el atributo `nombre` definido en `Soldado`, aunque dicho atributo no sea accesible directamente desde el código de la subclase. La herencia implica que el objeto completo incorpora el estado heredado, no que la subclase vuelva a declararlo.

Sin embargo, que un atributo privado forme parte del objeto **no implica** que pueda utilizarse directamente desde la subclase. La privacidad se respeta estrictamente: un atributo declarado como `private` en la superclase solo puede ser accedido por métodos de esa superclase. Por ejemplo, `Artillero` no puede escribir `this.nombre`, ya que el atributo está encapsulado dentro de `Soldado`. La subclase debe utilizar los métodos públicos o protegidos que la superclase proporcione, como `getNombre()` en el ejemplo.

En el caso concreto de `Soldado`, el atributo `nombre` está declarado como privado, pero `Artillero` y `Zapador` pueden usarlo indirectamente a través del método `getNombre()` o mediante el comportamiento heredado, como `saludar()`. Esto demuestra que el estado heredado existe físicamente en cada instancia de la subclase, pero su acceso está controlado por las reglas de encapsulación. De este modo, la herencia y la encapsulación cooperan: se hereda el estado, pero se mantiene el control sobre cómo puede utilizarse.


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La compatibilidad de tipos implica que el código que trabaja con la superclase puede seguir funcionando sin modificaciones aunque se añadan nuevas subclases. Esto ocurre porque dicho código no depende de los detalles concretos de cada subtipo, sino únicamente de la interfaz común definida en la superclase. En términos de extensibilidad, esto permite ampliar el sistema añadiendo nuevos comportamientos especializados sin necesidad de alterar el código existente que opera sobre colecciones o estructuras de la superclase. De este modo, la herencia favorece un diseño abierto a la extensión pero cerrado a la modificación.

Al introducir un nuevo subtipo de `Soldado`, el resto del programa no necesita cambios siempre que el nuevo tipo respete la interfaz heredada. Por ejemplo, si se añade un `Francotirador` con un número de balas y un método propio para disparar, este podrá integrarse en cualquier estructura que espere objetos de tipo `Soldado`. El método `saludar()`, heredado de la superclase, garantiza que el código que recorre un conjunto de soldados pueda invocarlo sin conocer el tipo concreto de cada elemento.

### Nuevo subtipo: `Francotirador`
```java
public class Francotirador extends Soldado {
    private final int numBalas;

    public Francotirador(String nombre, int numBalas) {
        super(nombre);
        this.numBalas = numBalas;
    }

    public int getNumBalas() {
        return numBalas;
    }

    public void disparar() {
        System.out.println(getNombre() + " dispara con precisión.");
    }
}
```

### Código que no necesita modificarse
```java
Soldado[] escuadra = new Soldado[] {
    new Soldado("Ramírez"),
    new Artillero("Gómez", 5),
    new Zapador("López", 3),
    new Francotirador("Santos", 12)
};

for (Soldado s : escuadra) {
    s.saludar();
}
```

El bucle que recorre la escuadra permanece idéntico, ya que solo depende del tipo `Soldado` y de su método `saludar()`. La incorporación del nuevo subtipo demuestra que el diseño es extensible: se amplía el sistema sin modificar el código que ya funcionaba correctamente.



## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Sí, es posible tener una referencia del **supertipo** que apunte a un objeto real de un **subtipo**. Esto ocurre porque una instancia de la subclase es compatible con el tipo de la superclase. Por ejemplo, una variable de tipo `Soldado` puede contener un `Artillero`, un `Zapador` o cualquier otro subtipo. Esta compatibilidad permite escribir código genérico que opere sobre la interfaz común definida en la superclase, independientemente del tipo concreto del objeto almacenado en la referencia.

Sin embargo, una referencia del supertipo solo permite invocar los métodos **visibles en el supertipo**, incluso si el objeto real tiene métodos adicionales. Esto significa que, si se tiene una referencia de tipo `Soldado`, no es posible llamar directamente a métodos específicos de `Artillero` como `getNumCohetes()`. Para acceder a esos métodos, es necesario realizar un **downcasting**, es decir, convertir explícitamente la referencia del supertipo al subtipo correspondiente. El proceso inverso, convertir una referencia de subtipo a supertipo, se denomina **upcasting** y siempre es seguro, ya que no pierde coherencia de tipos.

La operación `instanceof` permite comprobar en tiempo de ejecución si el objeto real al que apunta una referencia pertenece a un subtipo concreto. Esta comprobación es útil antes de realizar un downcasting, ya que evita errores de conversión. En un recorrido de un array de `Soldado`, puede utilizarse `instanceof` para detectar si un elemento es un `Artillero` y, en ese caso, acceder a su número de cohetes mediante un downcast seguro.

### Ejemplo con `instanceof` y downcasting

```java
Soldado[] escuadra = new Soldado[] {
    new Soldado("Ramírez"),
    new Artillero("Gómez", 5),
    new Zapador("López", 3)
};

for (Soldado s : escuadra) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;   // downcasting seguro
        System.out.println("Cohetes: " + a.getNumCohetes());
    }
}
```

Este ejemplo muestra cómo una referencia del supertipo puede apuntar a distintos subtipos y cómo, mediante `instanceof` y downcasting, es posible acceder a la funcionalidad específica de cada subtipo sin comprometer la seguridad del programa.


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.
El acceso protegido permite que ciertos atributos o métodos de una clase sean visibles para sus subclases, incluso aunque permanezcan ocultos para el resto del código externo. En términos de ocultación de información, se sitúa entre el acceso público y el privado: no expone el estado al exterior, pero sí permite que las clases derivadas utilicen directamente ese estado heredado. Esto resulta útil cuando la superclase desea ofrecer a sus subclases un acceso más directo a ciertos datos sin romper la encapsulación hacia el exterior.

En Java, el acceso protegido se implementa mediante la palabra clave `protected`. Un atributo o método declarado como protegido puede ser utilizado por cualquier subclase, incluso si se encuentra en un paquete distinto. De este modo, la superclase controla qué partes de su estado interno pueden ser empleadas por las subclases sin necesidad de recurrir a getters o métodos auxiliares. Esta capacidad facilita la especialización del comportamiento en las clases derivadas sin comprometer la integridad del diseño.

Si se desea que el nombre del `Soldado` pueda ser utilizado directamente por el `Zapador` en su método de colocar minas, basta con declarar el atributo como protegido en la superclase. De este modo, el `Zapador` podrá acceder a él sin necesidad de un método accesor. El siguiente ejemplo ilustra esta situación:

### Superclase con atributo protegido
```java
public class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy " + nombre + ", a sus órdenes.");
    }
}
```

### Subclase que usa el atributo protegido
```java
public class Zapador extends Soldado {
    private final int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public void ponerMina() {
        System.out.println(nombre + " coloca una mina.");
    }
}
```

En este ejemplo, el atributo `nombre` forma parte del estado heredado y, al ser protegido, puede utilizarse directamente en la subclase sin violar la encapsulación hacia el exterior.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?
En los lenguajes orientados a objetos no existe una regla universal sobre la existencia de una **clase base común** para todos los objetos. Algunos lenguajes optan por un modelo jerárquico único, mientras que otros permiten que las clases no tengan una raíz común. Esto depende del diseño del lenguaje y de las decisiones tomadas sobre tipado, reflexión y mecanismos internos de objetos. Por tanto, no puede asumirse que todos los lenguajes compartan este enfoque.

En lenguajes como C++ no existe una clase base obligatoria para todos los objetos. Una clase solo hereda de otra si el programador lo especifica, y si no se indica nada, la clase no tiene superclase. Esto implica que no hay un conjunto universal de métodos comunes a todos los objetos, y que características como la reflexión o la comparación genérica deben implementarse de forma distinta o no están disponibles de manera uniforme.

En Java, en cambio, **todas las clases heredan de forma implícita de `java.lang.Object`**, salvo que se indique explícitamente otra superclase. Esto significa que cualquier objeto en Java dispone de un conjunto mínimo de métodos comunes, como `toString()`, `equals()`, `hashCode()` y `getClass()`. Esta clase base única proporciona un comportamiento estándar que facilita la interoperabilidad entre objetos, el uso de colecciones genéricas y la integración con el sistema de tipos del lenguaje.



## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?
La **herencia múltiple** es un mecanismo por el cual una clase puede heredar simultáneamente de **más de una superclase**, obteniendo atributos y métodos de todas ellas. Este modelo permite combinar comportamientos procedentes de varias jerarquías, pero también introduce problemas potenciales, como ambigüedades en la resolución de métodos o conflictos cuando dos superclases definen el mismo atributo o el mismo método. Este tipo de situaciones se conoce tradicionalmente como el “problema del diamante”, muy estudiado en lenguajes que permiten herencia múltiple directa.

No todos los lenguajes orientados a objetos admiten herencia múltiple. Por ejemplo, C++ sí permite que una clase derive de varias clases base, lo que ofrece flexibilidad pero también exige un mayor control por parte del programador para evitar conflictos. Otros lenguajes, como C#, Java o Smalltalk, optan por evitarla en las clases para simplificar el modelo de herencia y reducir la posibilidad de ambigüedades. En estos lenguajes, la extensibilidad se consigue mediante otros mecanismos como interfaces o composición.

En Java **no existe herencia múltiple de clases**. Una clase solo puede extender a una única superclase, lo que garantiza una jerarquía lineal y evita los problemas derivados de múltiples ancestros directos. Sin embargo, Java sí permite implementar múltiples **interfaces**, lo que proporciona una forma controlada de herencia múltiple de comportamiento sin heredar estado. Este enfoque combina la simplicidad de la herencia simple con la flexibilidad de definir capacidades adicionales mediante interfaces.



## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

Una excepción personalizada es simplemente una clase que extiende alguna clase de excepción existente. Si se desea que sea **no controlada**, debe heredar de `RuntimeException` o de alguna de sus subclases. Al ser objetos, las excepciones pueden componerse con otros objetos, de modo que la excepción pueda transportar información adicional relevante para el diagnóstico del error. En este caso, la excepción contendrá un objeto `Usuario` que permitirá identificar qué usuario provocó el problema.

Para permitir que la excepción incluya una causa subyacente, basta con **sobrecargar el constructor** y llamar al constructor correspondiente de `RuntimeException` mediante `super(...)`. Esto permite encadenar excepciones, de modo que la traza de error preserve el origen real del fallo. La composición con el objeto `Usuario` se logra añadiendo un atributo privado y un getter para consultarlo.

### Ejemplo de excepción personalizada no controlada

```java
public class UsuarioNoEncontradoException extends RuntimeException {

    private final Usuario usuarioProblema;

    public UsuarioNoEncontradoException(Usuario usuarioProblema) {
        super("Usuario no encontrado: " + usuarioProblema.getNombre());
        this.usuarioProblema = usuarioProblema;
    }

    public UsuarioNoEncontradoException(Usuario usuarioProblema, Throwable causa) {
        super("Usuario no encontrado: " + usuarioProblema.getNombre(), causa);
        this.usuarioProblema = usuarioProblema;
    }

    public Usuario getUsuarioProblema() {
        return usuarioProblema;
    }
}
```

### Clase `Usuario` de ejemplo

```java
public class Usuario {
    private final String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

En este diseño, la excepción es no controlada porque extiende `RuntimeException`, incorpora un objeto `Usuario` para aportar contexto adicional y permite incluir una causa mediante un constructor alternativo. Esto facilita la depuración y mantiene un estilo coherente con el sistema de excepciones de Java.



## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

La herencia no debe emplearse únicamente como mecanismo de reutilización de código porque introduce una relación estructural fuerte entre clases. Cuando una clase hereda de otra, queda ligada a su diseño, a sus decisiones internas y a su evolución futura. Esto implica que cualquier cambio en la superclase puede afectar de forma no deseada a todas las subclases, incluso aunque estas no utilicen directamente la parte modificada. La herencia establece una dependencia rígida que puede dificultar el mantenimiento y la evolución del sistema si se utiliza sin una verdadera relación conceptual del tipo “A es‑un B”.

Además, la herencia transmite no solo comportamiento, sino también estado y contrato. Esto significa que la subclase hereda métodos que quizá no necesita o que no encajan con su semántica, lo que puede llevar a redefiniciones forzadas o a comportamientos incoherentes. Cuando la motivación principal es simplemente reutilizar código, esta transmisión completa resulta excesiva y puede generar diseños frágiles. La composición, en cambio, permite reutilizar funcionalidad sin imponer una jerarquía rígida ni heredar elementos que no son necesarios.

Por estas razones, se recomienda emplear herencia únicamente cuando exista una relación conceptual clara entre superclase y subclase, y no como un simple atajo para evitar duplicación de código. La composición ofrece una alternativa más flexible y segura, ya que permite construir objetos complejos combinando otros objetos, delegando en ellos el comportamiento deseado sin acoplarse a una jerarquía. De este modo, se favorece un diseño más modular, extensible y fácil de mantener.



## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

La recomendación de *favorecer la composición frente a la herencia* se basa en que la composición produce diseños más flexibles, menos acoplados y más fáciles de mantener. La herencia establece una relación rígida entre clases: la subclase depende de la estructura interna y de las decisiones de diseño de la superclase. Esto significa que cualquier cambio en la superclase puede propagarse de forma no deseada a todas las subclases, lo que dificulta la evolución del sistema. Además, la herencia transmite estado y comportamiento incluso cuando la subclase no necesita parte de ellos, lo que puede generar jerarquías artificiales o incoherentes.

La composición, en cambio, permite construir clases complejas combinando objetos que colaboran entre sí. En lugar de heredar comportamiento, se delega en otros objetos, lo que reduce el acoplamiento y evita depender de detalles internos de otra clase. Este enfoque facilita sustituir componentes, modificar comportamientos o extender funcionalidades sin alterar jerarquías completas. La composición también evita los problemas derivados de herencias profundas o mal planteadas, como la sobrescritura forzada de métodos o la aparición de efectos secundarios inesperados.

Por estas razones, la composición se considera una alternativa más segura y modular cuando el objetivo es reutilizar funcionalidad sin imponer una relación conceptual del tipo “A es‑un B”. La herencia debe reservarse para casos en los que exista una verdadera relación jerárquica y semántica entre clases, mientras que la composición permite mantener un diseño más limpio, adaptable y resistente a cambios futuros.



## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

Se afirma que la herencia puede romper la encapsulación porque una subclase queda expuesta a detalles internos de la superclase que, en principio, deberían permanecer ocultos. Aunque los atributos privados siguen siendo inaccesibles, la subclase hereda toda la estructura interna y el comportamiento de la superclase, incluidos métodos que quizá no estaban pensados para ser utilizados o redefinidos fuera de su contexto original. Esto hace que la superclase ya no pueda modificar libremente ciertos aspectos internos sin arriesgarse a afectar de manera inesperada a las subclases que dependen de ellos.

Además, la herencia establece un acoplamiento fuerte: la subclase depende de la implementación concreta de la superclase, no solo de su interfaz pública. Si la superclase cambia su lógica interna, la subclase puede verse afectada incluso aunque el cambio no altere la interfaz externa. Esto contradice el principio de encapsulación, que busca aislar los detalles internos para que puedan evolucionar sin impactar en el resto del sistema. Con herencia, esa frontera se vuelve más difusa, ya que la subclase hereda y, en cierto modo, queda ligada a esos detalles.

Por este motivo, se considera que la herencia debe utilizarse únicamente cuando existe una relación conceptual clara del tipo “A es‑un B”, y no como un mecanismo general de reutilización de código. La composición, al delegar comportamiento en objetos internos sin exponer su implementación, preserva mejor la encapsulación y reduce el riesgo de dependencias indeseadas entre clases.



## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

A continuación se muestran las dos alternativas solicitadas, una basada en **herencia** y otra basada en **composición**, manteniendo la misma información común (DNI y nombre) pero estructurándola de forma distinta según el enfoque.

---

## Alternativa 1: Herencia (superclase `Persona`)

En este diseño, `Estudiante` y `Trabajador` heredan de una superclase `Persona`, que contiene los datos comunes. Este enfoque establece una relación del tipo “A es‑un B”: un estudiante es‑una persona y un trabajador es‑una persona. La reutilización de código se logra mediante la herencia, ya que ambos subtipos reciben automáticamente los atributos y métodos de la superclase. Este modelo es adecuado cuando existe una relación conceptual clara entre las clases.

```java
public class Persona {
    private final String dni;
    private final String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

public class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
```

---

## Alternativa 2: Composición (clase `DatosPersonales`)

En este diseño, `Estudiante` y `Trabajador` **no heredan** de una superclase, sino que contienen internamente un objeto `DatosPersonales`. Este enfoque evita la rigidez de la herencia y permite reutilizar código sin imponer una jerarquía. La composición favorece un diseño más flexible, ya que los datos personales se encapsulan en un objeto independiente que puede reutilizarse en distintos contextos sin establecer relaciones del tipo “es‑un”.

```java
public class DatosPersonales {
    private final String dni;
    private final String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Estudiante {
    private final DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}

public class Trabajador {
    private final DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}
```

---

Ambas soluciones modelan correctamente el problema, pero cada una refleja un enfoque distinto: la herencia establece una jerarquía conceptual, mientras que la composición permite reutilizar datos sin imponer relaciones estructurales rígidas.

