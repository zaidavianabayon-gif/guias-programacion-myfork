<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El **polimorfismo** es la capacidad de tratar objetos de distintos subtipos como si fueran del mismo supertipo, permitiendo invocar sobre ellos métodos comunes definidos en la superclase. Este mecanismo permite escribir código genérico que funciona con cualquier objeto compatible, independientemente de su tipo concreto. Gracias a ello, estructuras como listas o arrays pueden almacenar objetos heterogéneos y operar sobre ellos mediante una interfaz común, lo que incrementa la flexibilidad y extensibilidad del diseño orientado a objetos.

La **sobreescritura de métodos** consiste en que una subclase redefine un método heredado de la superclase, proporcionando una implementación específica que sustituye a la original cuando el objeto real pertenece a la subclase. Esta técnica permite adaptar o especializar el comportamiento heredado sin modificar la superclase, respetando la misma firma del método. La combinación de polimorfismo y sobreescritura permite que una llamada a un método se resuelva dinámicamente según el tipo real del objeto, lo que constituye uno de los pilares fundamentales de la programación orientada a objetos.



## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La **ligadura dinámica** o **enlace tardío** consiste en que la elección del método que realmente se ejecuta no se decide en tiempo de compilación, sino en tiempo de ejecución. Esto significa que, aunque la referencia sea del supertipo, el método que se invoca es el correspondiente al **tipo real del objeto**, no al tipo de la referencia. Este mecanismo es esencial para que el polimorfismo funcione: permite que una misma llamada a un método se resuelva de forma distinta según el subtipo concreto del objeto.

La relación con el polimorfismo es directa. El polimorfismo permite invocar métodos sobre referencias del supertipo, y la ligadura dinámica es el mecanismo que determina qué implementación concreta se ejecuta. Sin enlace tardío, la llamada se resolvería siempre según el tipo de la referencia, lo que impediría la sobreescritura efectiva de métodos y anularía el comportamiento polimórfico. Por tanto, el polimorfismo depende de que el lenguaje implemente ligadura dinámica para los métodos sobreescritos.

En cuanto a si debe indicarse explícitamente, depende del lenguaje. En **Java**, la ligadura dinámica se aplica automáticamente a todos los métodos no estáticos ni privados. El programador no debe hacer nada especial: la sobreescritura y el polimorfismo funcionan de forma implícita. En **C++**, en cambio, la ligadura dinámica solo se activa si el método de la superclase se declara como `virtual`. Si no se marca así, la llamada se resuelve en tiempo de compilación (ligadura estática), incluso aunque exista una sobreescritura en la subclase. Esto hace que el programador deba decidir explícitamente qué métodos participan en el comportamiento polimórfico.

En **Python**, la ligadura dinámica es la norma para todos los métodos. El lenguaje es dinámico y no requiere declaraciones especiales: cualquier método puede ser redefinido en una subclase y la resolución se realiza siempre en tiempo de ejecución. Esto hace que el polimorfismo y la sobreescritura funcionen de manera natural sin necesidad de palabras clave adicionales.



## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

Aquí tienes un ejemplo completamente centrado en lo que pides: una superclase `Soldado`, dos subclases (`Zapador` y `Artillero`), con **sobreescritura real** del método `saludar` en `Zapador`, y un recorrido polimórfico usando un array de referencias de tipo `Soldado`.

---

## Clases

```java
public class Soldado {
    private final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soldado " + nombre + " saluda.");
    }
}
```

```java
public class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void saludar() {
        System.out.println("Zapador en posición, listo para colocar explosivos.");
    }
}
```

```java
public class Artillero extends Soldado {

    public Artillero(String nombre) {
        super(nombre);
    }
}
```

---

## Uso del polimorfismo

```java
public class Principal {
    public static void main(String[] args) {

        Soldado[] escuadra = new Soldado[] {
            new Soldado("Ramírez"),
            new Zapador("López"),
            new Artillero("Gómez")
        };

        for (Soldado s : escuadra) {
            s.saludar();   // polimorfismo: se ejecuta según el tipo real
        }
    }
}
```

---

## Resultado esperado

- Para `Soldado`: se ejecuta el método de `Soldado`.
- Para `Zapador`: se ejecuta la **sobreescritura**, sustituyendo completamente el comportamiento.
- Para `Artillero`: hereda el método de `Soldado` sin modificarlo.

Este ejemplo muestra claramente:

- **Polimorfismo**: una referencia de tipo `Soldado` puede apuntar a distintos subtipos.
- **Ligadura dinámica**: la llamada a `saludar()` se resuelve en tiempo de ejecución según el tipo real del objeto.
- **Sobreescritura**: `Zapador` redefine el método con su propio comportamiento.



## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Sí, cuando **sobreescribes** un método puedes invocar la versión original de la superclase y *añadir* comportamiento extra. Para eso Java proporciona la palabra clave **`super`**, que permite llamar al método heredado antes (o después) de tu código nuevo.

Aquí tienes el ejemplo solicitado:

---

## Superclase

```java
public class Soldado {
    private final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soldado " + nombre + " saluda.");
    }
}
```

---

## Subclase que sobreescribe e invoca al método base

```java
public class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void saludar() {
        super.saludar();   // ← llamada al método de la clase base
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}
```

---

## Respuesta directa a tu pregunta

La palabra clave usada para invocar al método de la clase base es **`super`**.



## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Aquí tienes la explicación completa, clara y académica, tal como te gusta para preparar AEDI.

---

## ✔ Restricciones al **sobreescribir** un método en Java

Cuando una subclase **sobreescribe** un método de la superclase, debe respetar **dos reglas fundamentales**:

### **1. Los parámetros deben ser EXACTAMENTE los mismos**
- Misma cantidad  
- Mismos tipos  
- Mismo orden  

Si cambias los parámetros, **ya no es sobreescritura**, sino *sobrecarga*.

### **2. El tipo de retorno debe ser el mismo o un subtipo (retorno covariante)**
Ejemplo válido:

```java
@Override
public String toString() { ... }
```

Ejemplo también válido (retorno covariante):

```java
@Override
public Estudiante clonar() { ... }   // si el método base devuelve Persona
```

Ejemplo NO válido:

```java
@Override
public int saludar() { ... }   // si el método base devuelve void
```

---

## ✔ Diferencia entre **sobreescritura** (*overriding*) y **sobrecarga** (*overloading*)

### **Sobreescritura (overriding)**
- Ocurre entre **superclase y subclase**.  
- Misma firma (mismo nombre + mismos parámetros).  
- Cambia el comportamiento heredado.  
- Permite **polimorfismo**.

Ejemplo:

```java
@Override
public void saludar() { ... }
```

### **Sobrecarga (overloading)**
- Ocurre **en la misma clase** (o heredada, pero sin relación polimórfica).  
- Mismo nombre, **distintos parámetros**.  
- No tiene relación con polimorfismo.  
- Es solo comodidad sintáctica.

Ejemplo:

```java
public void imprimir(String s) { ... }
public void imprimir(int n) { ... }
```

---

## ✔ ¿Para qué sirve `@Override` y por qué usarla SIEMPRE?

La anotación `@Override`:

1. **Obliga al compilador a comprobar** que realmente estás sobreescribiendo un método existente.
2. Evita errores silenciosos como:
   - Cambiar un parámetro sin querer  
   - Cambiar el nombre del método por error  
   - Cambiar el tipo de retorno incorrectamente  
3. Mejora la legibilidad del código.

Ejemplo de error que `@Override` detectaría:

```java
@Override
public void saluda() { ... }   // ERROR: el método base se llama saludar()
```

Sin `@Override`, este error pasaría desapercibido y **no habría sobreescritura**, rompiendo el polimorfismo.

---

## ✔ Resumen rápido

| Concepto | Qué exige | Para qué sirve |
|---------|------------|----------------|
| **Sobreescritura** | Misma firma, retorno igual o covariante | Polimorfismo |
| **Sobrecarga** | Mismo nombre, parámetros distintos | Comodidad, no polimorfismo |
| **@Override** | Verifica que realmente estás sobreescribiendo | Evita errores y mejora claridad |

---

Si quieres, puedo prepararte un mini‑ejercicio de examen sobre sobreescritura y sobrecarga para practicar.



## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Sí: **desde el primer momento en que en Java sobreescribes un método, ya estás usando polimorfismo**, aunque no siempre seas consciente de ello.

Vamos por partes, de forma precisa y académica.

---

## ✔ ¿Se usa polimorfismo desde el principio al aprender Java?

**Sí.**  
En cuanto defines una clase y sobreescribes un método heredado de `Object` (como `toString()` o `equals()`), ya estás usando **polimorfismo dinámico**.

¿Por qué?

Porque:

- Tienes una **referencia** de tipo `Object` (o de tu supertipo).
- El **objeto real** es de tu clase.
- Cuando llamas a `toString()` o `equals()`, **se ejecuta la versión de tu clase**, no la de `Object`.

Eso es exactamente polimorfismo:  
> *La llamada al método se resuelve según el tipo REAL del objeto, no según el tipo de la referencia.*

---

## ✔ ¿Sobreescribir `toString()` implica polimorfismo?

Sí.

Ejemplo:

```java
Object o = new Estudiante("Ana", 123);
System.out.println(o.toString());
```

Aunque `o` es de tipo **Object**, Java ejecuta **Estudiante.toString()**.

Eso es polimorfismo puro.

---

## ✔ ¿Sobreescribir `equals()` también implica polimorfismo?

También sí.

Cuando haces:

```java
Object a = new Estudiante("Ana", 123);
Object b = new Estudiante("Ana", 123);

a.equals(b);   // se ejecuta Estudiante.equals(...)
```

La referencia es `Object`, pero el método ejecutado es el de `Estudiante`.

---

## ✔ Entonces, ¿por qué no se explica como polimorfismo desde el principio?

Porque al inicio se enseña:

- cómo crear clases,
- cómo sobreescribir métodos básicos,
- cómo imprimir objetos,

y **no se quiere introducir todavía la teoría completa de herencia y polimorfismo**.

Pero técnicamente, sí:  
**cada vez que sobreescribes un método heredado y lo llamas a través de una referencia del supertipo, estás usando polimorfismo.**

---

## ✔ Resumen académico

- Sobreescribir `toString()` y `equals()` **ya activa la ligadura dinámica**.  
- Por tanto, **sí estás usando polimorfismo**, aunque no se mencione explícitamente al principio.  
- El polimorfismo no requiere arrays de soldados ni jerarquías complejas: basta con una referencia de supertipo (`Object`) apuntando a un objeto de subtipo.

---

Si quieres, puedo prepararte un mini‑ejercicio donde compares llamadas polimórficas entre `Object`, `Persona` y `Estudiante` para que veas claramente cómo se resuelven en tiempo de ejecución.



## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Aquí tienes la explicación completa, precisa y orientada a examen, con el ejemplo solicitado.

---

## ✔ ¿Qué es una **clase abstracta**?

Una **clase abstracta** es una clase que:

- **no puede instanciarse**,  
- puede contener **métodos normales** y **métodos abstractos**,  
- sirve como **plantilla** para que las subclases implementen comportamientos comunes.

Se usa cuando quieres definir una **interfaz parcial** + **comportamiento compartido**, pero dejando partes obligatorias para las subclases.

---

## ✔ ¿Qué es un **método abstracto**?

Un método abstracto es un método:

- **sin implementación**,  
- declarado con `abstract`,  
- que **debe ser implementado obligatoriamente** por cada subclase concreta.

Ejemplo:

```java
public abstract void atacar();
```

---

## ✔ ¿Puedo crear instancias de una clase abstracta?

**No.**  
Una clase abstracta **no puede instanciarse** directamente:

```java
Soldado s = new Soldado("Luis");  // ❌ ERROR si Soldado es abstracta
```

Solo puedes instanciar **subclases concretas** que implementen todos los métodos abstractos.

---

## ✔ ¿Dónde debo poner `abstract`?

Debes poner `abstract` en:

1. **La clase** → si contiene métodos abstractos.  
2. **El método** → para indicar que no tiene implementación.

---

## ✔ Ejemplo completo en Java

### Clase abstracta `Soldado`

```java
public abstract class Soldado {
    private final String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soldado " + nombre + " saluda.");
    }

    // Método abstracto: NO tiene cuerpo
    public abstract void atacar();
}
```

---

### Subclase `Zapador`

```java
public class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void atacar() {
        System.out.println("Zapador coloca una carga explosiva.");
    }
}
```

---

### Subclase `Artillero`

```java
public class Artillero extends Soldado {

    public Artillero(String nombre) {
        super(nombre);
    }

    @Override
    public void atacar() {
        System.out.println("Artillero dispara un proyectil.");
    }
}
```

---

## ✔ Uso polimórfico

```java
public class Principal {
    public static void main(String[] args) {

        Soldado[] escuadra = {
            new Zapador("López"),
            new Artillero("Gómez")
        };

        for (Soldado s : escuadra) {
            s.saludar();
            s.atacar();   // polimorfismo sobre método abstracto
        }
    }
}
```

---

## ✔ Resumen académico

| Concepto | Definición |
|---------|------------|
| **Clase abstracta** | Clase que no puede instanciarse y puede contener métodos abstractos |
| **Método abstracto** | Método sin implementación que las subclases deben definir |
| **Instanciación** | No se puede instanciar una clase abstracta |
| **Uso de `abstract`** | En la clase y en los métodos sin implementación |

---

Si quieres, puedo prepararte una variante de este ejercicio al estilo AEDI, con preguntas tipo examen sobre clases abstractas y polimorfismo.


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

Aquí tienes la explicación precisa, académica y orientada a examen, siguiendo tu estilo preferido.

---

## ✔ Efecto de `final` sobre **métodos**

Cuando un método se declara como **`final`**:

- **NO puede ser sobreescrito** en ninguna subclase.
- La implementación queda **cerrada** y no puede modificarse mediante herencia.

Esto afecta directamente al polimorfismo:  
> Si un método es `final`, **no participa en polimorfismo dinámico**, porque no puede haber versiones distintas en subclases.

Ejemplo:

```java
public class Soldado {
    public final void saludar() {
        System.out.println("Soldado saluda.");
    }
}
```

Ninguna subclase podrá redefinir `saludar()`.

---

## ✔ Efecto de `final` sobre **clases**

Cuando una clase se declara como **`final`**:

- **NO puede tener subclases**.
- Queda completamente cerrada a la herencia.

Esto elimina cualquier posibilidad de polimorfismo por especialización, porque no se pueden crear tipos derivados.

Ejemplo:

```java
public final class Utilidades {
    // ...
}
```

Nadie puede hacer:

```java
class MisUtilidades extends Utilidades {}  // ❌ ERROR
```

---

## ✔ Relación con el polimorfismo

`final` **limita o impide** el polimorfismo:

- Una **clase final** no puede ser extendida → no puede haber subtipos → no hay polimorfismo por herencia.
- Un **método final** no puede ser sobreescrito → no puede haber comportamiento polimórfico para ese método.

En resumen:

| Elemento | ¿Permite polimorfismo? |
|----------|-------------------------|
| Clase `final` | ❌ No |
| Método `final` | ❌ No |
| Clase normal + método sobreescrito | ✔ Sí |

---

## ✔ Ejemplos de clases `final` en la API estándar de Java

Hay muchas, pero las más conocidas son:

- **`String`**  
- **`Integer`, `Double`, `Long`** (todas las *wrapper classes*)
- **`LocalDate`, `LocalTime`, `LocalDateTime`** (API de fechas moderna)
- **`Math`**

La razón principal:  
> Son clases diseñadas para ser **inmutables**, seguras y estables.  
> Hacerlas `final` evita que alguien las modifique mediante herencia.

Ejemplo clásico:

```java
public final class String {
    // ...
}
```

---

## ✔ Resumen académico

- `final` en **métodos** → no se pueden sobreescribir → **sin polimorfismo**.  
- `final` en **clases** → no se pueden extender → **sin jerarquía de herencia**.  
- Se usa para garantizar **seguridad, inmutabilidad y estabilidad**.  
- Ejemplos en Java: `String`, `Integer`, `Math`, `LocalDate`, etc.

---

Si quieres, puedo prepararte un ejercicio tipo AEDI donde tengas que identificar qué métodos pueden ser polimórficos según si son `final`, `static`, `private` o normales.



## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Aquí tienes la explicación clara, estructurada y totalmente adecuada para teoría de AEDI.

---

## ✔ ¿Qué son las **interfaces** en Java?

Una **interfaz** es un tipo de referencia en Java que define:

- un **conjunto de métodos abstractos** (antes de Java 8),  
- y desde Java 8 también puede incluir:
  - **métodos por defecto** (`default`)
  - **métodos estáticos**
  - **métodos privados** (desde Java 9)

Una interfaz **no tiene estado** (salvo constantes `public static final`) y **no implementa comportamiento completo**, sino que define un **contrato** que las clases deben cumplir.

En esencia:

> Una interfaz especifica *qué* debe hacer una clase, no *cómo* debe hacerlo.

---

## ✔ ¿Son como clases abstractas?

Se parecen, pero **no son lo mismo**.

| Característica | Clase abstracta | Interfaz |
|----------------|------------------|----------|
| ¿Puede tener estado (atributos)? | ✔ Sí | ❌ No (solo constantes) |
| ¿Puede tener métodos con implementación? | ✔ Sí | ✔ Sí (default y static) |
| ¿Puede tener métodos abstractos? | ✔ Sí | ✔ Sí |
| ¿Puede heredar de varias? | ❌ No | ✔ Sí (una clase puede implementar varias interfaces) |
| ¿Se usa para…? | Compartir código + plantilla | Definir un contrato común |

Una interfaz es más **ligera** y más **flexible** que una clase abstracta.

---

## ✔ ¿Puede una clase implementar más de una interfaz?

**Sí.**  
Y esta es una de las razones principales por las que existen las interfaces en Java.

Ejemplo:

```java
public class Robot implements Movible, Atacante, Reparador {
    // ...
}
```

Java **no permite herencia múltiple de clases**, pero **sí permite implementar múltiples interfaces**, lo que proporciona una forma segura de herencia múltiple de comportamiento.

---

## ✔ Resumen académico

- Una **interfaz** define un conjunto de métodos que una clase debe implementar.  
- Es similar a una clase abstracta, pero **más restrictiva en estado** y **más flexible en herencia**.  
- Una clase puede implementar **todas las interfaces que quiera**, lo que permite un tipo de polimorfismo múltiple sin los problemas de la herencia múltiple clásica.

---

Si quieres, puedo prepararte un ejemplo con `Soldado` y varias interfaces como `Atacante`, `Curable`, `Movible`, para ver cómo se combinan en una clase concreta.



## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

Aquí tienes un diseño **correcto, seguro y académico**, exactamente como se pediría en AEDI: polimorfismo, clases abstractas, `instanceof`, *downcasting* y una `Linea` capaz de trabajar con puntos sin conocer su dimensión.

---

# ✔ 1. Clase abstracta `Punto`

- Define el **contrato**: todas las subclases deben poder calcular la distancia a otro punto.
- El método `calcularDistanciaA` es **abstracto**.
- La clase no sabe si es 2D o 3D.

```java
public abstract class Punto {

    public abstract double calcularDistanciaA(Punto otro);
}
```

---

# ✔ 2. Implementación: `Punto2D`

```java
public class Punto2D extends Punto {

    private final double x;
    private final double y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Se esperaba un Punto2D");
        }

        Punto2D p = (Punto2D) otro;  // downcasting seguro
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

---

# ✔ 3. Implementación: `Punto3D`

```java
public class Punto3D extends Punto {

    private final double x;
    private final double y;
    private final double z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Se esperaba un Punto3D");
        }

        Punto3D p = (Punto3D) otro;  // downcasting seguro
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        double dz = this.z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}
```

---

# ✔ 4. Clase `Linea` (polimórfica y agnóstica de la dimensión)

La clase **no sabe** si los puntos son 2D o 3D.  
Solo sabe que son `Punto`, y confía en el polimorfismo para que cada subtipo calcule su distancia correctamente.

```java
public class Linea {

    private final Punto a;
    private final Punto b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b);  // polimorfismo puro
    }
}
```

---

# ✔ 5. Uso del sistema

```java
public class Principal {
    public static void main(String[] args) {

        Punto p1 = new Punto2D(0, 0);
        Punto p2 = new Punto2D(3, 4);

        Linea l1 = new Linea(p1, p2);
        System.out.println(l1.longitud());  // 5.0

        Punto q1 = new Punto3D(0, 0, 0);
        Punto q2 = new Punto3D(1, 2, 2);

        Linea l2 = new Linea(q1, q2);
        System.out.println(l2.longitud());  // 3.0
    }
}
```

---

# ✔ Explicación académica (resumen)

- `Punto` es **abstracta** → no se puede instanciar.
- `calcularDistanciaA` es **abstracto** → cada subtipo implementa su fórmula.
- Se usa **instanceof + downcasting** para garantizar que se comparan puntos del mismo subtipo.
- `Linea` recibe puntos sin saber si son 2D o 3D → **polimorfismo**.
- La longitud se calcula correctamente porque cada subtipo ejecuta su propia versión del método.

---

Si quieres, puedo extender este diseño para soportar **N dimensiones**, o para evitar `instanceof` usando **double dispatch** o **visitor**, que es un patrón muy elegante para este tipo de problemas.



## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

Aquí tienes la explicación completa, clara y orientada a examen, junto con el ejemplo solicitado.

---

# ✔ ¿Qué es la **herencia de interfaces** en Java?

En Java, una interfaz puede **extender** a otra interfaz usando la palabra clave `extends`.

Ejemplo conceptual:

```java
interface A { }
interface B extends A { }
```

Esto significa:

- `B` **hereda** todos los métodos abstractos de `A`.
- Cualquier clase que implemente `B` también debe implementar los métodos heredados de `A`.

A esto se le llama **herencia de interfaces**.

---

# ✔ ¿Existe **herencia múltiple de interfaces**?

**Sí.**

Una interfaz puede extender **varias interfaces a la vez**, algo que NO se permite con clases.

Ejemplo:

```java
interface C extends A, B { }
```

Y también:

Una **clase** puede implementar **todas las interfaces que quiera**:

```java
class MiClase implements A, B, C { }
```

Esto permite un tipo de *herencia múltiple segura*, sin los problemas de la herencia múltiple de clases.

---

# ✔ Ejemplo solicitado: `Fichero` y `FicheroEscribible`

## Interfaz base: `Fichero`

```java
public interface Fichero {
    String leerContenido();
}
```

---

## Interfaz derivada: `FicheroEscribible`

Extiende a `Fichero` y añade métodos nuevos.

```java
public interface FicheroEscribible extends Fichero {
    void escribirContenido(String texto);
    void eliminar();
}
```

---

# ✔ Explicación académica del diseño

- `Fichero` define el **contrato mínimo**: poder leer.
- `FicheroEscribible` **hereda** ese contrato y añade más capacidades: escribir y eliminar.
- Cualquier clase que implemente `FicheroEscribible` deberá implementar:
  - `leerContenido()` (heredado)
  - `escribirContenido(String)`
  - `eliminar()`

Esto es **herencia de interfaces**.

---

# ✔ Resumen para examen

- Una **interfaz puede extender otra** → herencia de interfaces.  
- Una interfaz puede extender **varias** → herencia múltiple de interfaces.  
- Una clase puede implementar **todas las interfaces que quiera**.  
- Las interfaces definen **contratos**, no implementaciones (salvo métodos `default` o `static`).  

---

Si quieres, puedo completar el ejemplo implementando una clase concreta como `FicheroLocal` que use `java.nio.file` para leer, escribir y borrar ficheros reales.

