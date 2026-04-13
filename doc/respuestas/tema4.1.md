<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

A continuación se muestra un ejemplo sencillo en C donde se ilustra la **composición**: una línea está formada por dos puntos, y cada punto contiene dos coordenadas. Este tipo de diseño refleja la idea de “A tiene‑un B”, muy habitual cuando se construyen estructuras más complejas a partir de otras más simples. El código se mantiene dentro del paradigma estructurado de C, sin orientación a objetos, pero permite ver claramente cómo se agrupan datos relacionados.

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto inicio;
    Punto fin;
} Linea;

double distancia(Punto a, Punto b) {
    double dx = b.x - a.x;
    double dy = b.y - a.y;
    return sqrt(dx*dx + dy*dy);
}

double longitudLinea(Linea l) {
    return distancia(l.inicio, l.fin);
}

int main() {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};
    Linea l = {p1, p2};

    printf("Distancia entre puntos: %.2f\n", distancia(p1, p2));
    printf("Longitud de la línea: %.2f\n", longitudLinea(l));

    return 0;
}
```

En este ejemplo, la estructura `Punto` encapsula dos valores numéricos que representan coordenadas en un plano. La estructura `Linea` se compone de dos puntos, lo que permite describir un segmento definido por un origen y un final. Esta composición facilita la organización de datos y permite reutilizar estructuras más pequeñas para construir otras más complejas.

Las funciones `distancia` y `longitudLinea` ilustran cómo operar sobre estas estructuras. La primera calcula la distancia euclidiana entre dos puntos, mientras que la segunda reutiliza esa lógica para obtener la longitud de una línea. Este enfoque modular favorece la claridad del código y permite extender fácilmente la funcionalidad si se quisieran añadir más operaciones geométricas en el futuro.



## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

A continuación se muestra una posible versión orientada a objetos en Java, donde la **composición** se expresa mediante una clase `Linea` que contiene dos objetos `Punto`. La encapsulación permite ocultar los detalles internos y, además, se garantiza la **inmutabilidad** tanto de los puntos como de la línea. Esto se logra declarando los atributos como `private final` y evitando exponer métodos que permitan modificarlos después de la construcción.

```java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx*dx + dy*dy);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}

public final class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.distancia(fin);
    }

    public Punto getInicio() { return inicio; }
    public Punto getFin() { return fin; }
}
```

En este diseño, la clase `Punto` encapsula sus coordenadas y no ofrece ningún método para modificarlas, lo que garantiza que una vez creado un punto, sus valores permanecen constantes. El método `distancia` permite operar entre dos puntos sin comprometer su estado interno. Esta aproximación refleja una clara separación entre datos y operaciones, manteniendo la integridad de los objetos.

La clase `Linea` se compone de dos puntos inmutables, lo que asegura que la línea no pueda alterarse después de su creación. Su método `longitud` reutiliza la lógica ya definida en `Punto`, mostrando cómo la composición permite construir comportamientos más complejos a partir de objetos más simples. Este enfoque orientado a objetos supera al de C en términos de seguridad y claridad, ya que evita modificaciones accidentales y facilita el razonamiento sobre el estado del programa.



## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La **multiplicidad** en composición indica cuántas instancias de un tipo de objeto participan en la relación con otro tipo. En otras palabras, expresa cuántos objetos “componen” a otro y, a la inversa, cuántos objetos pueden estar contenidos dentro de uno. Esta idea es fundamental en los diagramas de clases y en el diseño orientado a objetos, porque permite razonar sobre la estructura interna de los objetos y sobre las restricciones que deben cumplirse durante su creación y uso.

En el ejemplo de `Linea` y `Punto`, una línea está formada **exactamente por dos puntos**, ni más ni menos. Por tanto, la multiplicidad desde `Linea` hacia `Punto` es **2**, o expresado en notación UML: `Linea → Punto : 2`. Esto refleja que toda línea debe tener dos puntos obligatoriamente, y que no puede existir una línea con un número distinto de puntos.

En la dirección contraria, un `Punto` puede formar parte de **cero o más líneas**. Un punto puede no pertenecer a ninguna línea, puede ser el extremo de una sola, o puede ser compartido por varias líneas (por ejemplo, en una figura geométrica). Por tanto, la multiplicidad desde `Punto` hacia `Linea` es `0..*`, indicando que no hay límite superior en cuántas líneas pueden usar el mismo punto.

En conjunto, la relación de composición queda descrita así:  
- **De `Linea` a `Punto`: multiplicidad fija 2**.  
- **De `Punto` a `Linea`: multiplicidad `0..*`**.  

Esta asimetría es habitual en composición: el todo impone restricciones estrictas sobre sus partes, mientras que las partes pueden existir independientemente o participar en múltiples estructuras.



## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La **composición fuerte** describe una relación en la que el objeto “todo” es dueño exclusivo de sus partes, de modo que estas no pueden existir de manera independiente. En este tipo de composición, las partes se crean junto con el todo y desaparecen cuando el todo deja de existir. El ciclo de vida de los objetos queda, por tanto, completamente ligado: si el objeto contenedor se destruye, también lo hacen sus componentes. Esta relación expresa una dependencia total y suele emplearse cuando las partes no tienen sentido fuera del conjunto que las contiene.

La **composición débil**, por el contrario, indica que el objeto “todo” utiliza o agrupa otros objetos, pero sin poseerlos en un sentido estricto. Las partes pueden existir antes, durante o después del todo, y no dependen de él para su ciclo de vida. En este caso, la destrucción del objeto contenedor no implica la destrucción de los objetos que lo componen. Esta relación es más flexible y se emplea cuando los objetos pueden ser compartidos o reutilizados en distintos contextos.

En terminología habitual, la composición débil se conoce como **asociación** o **agregación**, ya que los objetos se relacionan sin que exista una dependencia vital entre ellos. La composición fuerte, en cambio, es la que se denomina **composición** propiamente dicha, porque establece una relación de pertenencia estricta y un ciclo de vida compartido. Esta distinción resulta fundamental para diseñar estructuras de clases coherentes y para decidir qué objetos deben gestionarse de manera conjunta y cuáles pueden mantenerse independientes.



## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase utiliza a otra únicamente **como parámetro**, **como valor de retorno**, **como variable local** o incluso creando instancias puntuales con `new` dentro de un método, no se está ante un caso de composición, sino ante una relación de **dependencia**. Esta relación indica que una clase necesita a otra para realizar alguna operación concreta, pero no la “posee” ni controla su ciclo de vida. La dependencia es, por tanto, una relación mucho más débil y temporal, limitada al ámbito del método donde se produce el uso.

En este tipo de relación, los objetos utilizados pueden existir antes, durante o después de la ejecución del método, sin que la clase que los usa tenga responsabilidad sobre su creación o destrucción. La clase dependiente simplemente “se apoya” en ellos para realizar una tarea, pero no los integra como parte de su estructura interna. Esto contrasta con la composición, donde los objetos forman parte del estado permanente de la clase y su ciclo de vida queda ligado al del objeto contenedor.

Por tanto, cuando el uso de otra clase se limita a parámetros, retornos, variables locales o instanciaciones internas de métodos, se habla estrictamente de **dependencia**, no de composición. Esta distinción es esencial para comprender el diseño orientado a objetos, ya que permite diferenciar entre relaciones estructurales duraderas y relaciones funcionales temporales.



## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

A continuación se muestran dos formas distintas de modelar la relación entre `Linea` y `Punto` en Java: una como **composición fuerte**, donde los puntos pertenecen exclusivamente a la línea y su ciclo de vida está ligado al de ella, y otra como **composición débil**, donde la línea solo referencia puntos que existen independientemente. Ambas versiones ilustran cómo cambia el diseño según el tipo de relación que se quiera expresar.

---

# **Composición fuerte (la línea “posee” sus puntos)**

En este caso, la línea crea internamente sus propios puntos y no recibe puntos externos. Los puntos no pueden existir sin la línea, y no se exponen referencias directas para evitar que otros objetos los utilicen. El ciclo de vida queda completamente ligado: si la línea deja de existir, también lo hacen sus puntos. Esta es la composición en sentido estricto.

```java
public final class LineaFuerte {
    private final Punto inicio;
    private final Punto fin;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
```

Este diseño refleja que la línea controla totalmente la creación de sus puntos. No se permite que otros objetos compartan estos puntos ni que los modifiquen, reforzando la idea de pertenencia exclusiva. La línea es responsable de su estructura interna y no depende de objetos externos para existir.

---

# **Composición débil (la línea usa puntos externos)**

Aquí la línea recibe puntos creados fuera de ella y simplemente los referencia. Los puntos pueden existir antes, durante o después de la vida de la línea, y pueden ser compartidos por otras líneas u objetos. No existe dependencia vital entre ellos, por lo que se trata de una agregación o asociación.

```java
public final class LineaDebil {
    private final Punto inicio;
    private final Punto fin;

    public LineaDebil(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
```

En este diseño, la línea no controla el ciclo de vida de los puntos. Estos pueden ser reutilizados, modificados o eliminados por otros objetos sin afectar directamente a la línea. La relación es más flexible y menos restrictiva, adecuada cuando los puntos tienen sentido por sí mismos y no dependen de la línea para existir.

---

Si quieres, puedo añadir un diagrama UML comparando ambas relaciones o ayudarte a decidir cuál conviene usar en distintos escenarios de diseño.



## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java, incluso cuando se modela una **composición fuerte**, el contenedor no destruye explícitamente los objetos que contiene. Esto se debe a que Java no ofrece destrucción manual de objetos, ya que no existe un equivalente al `free()` de C ni un destructor como en C++. En su lugar, Java utiliza un **recolector de basura (Garbage Collector)** que se encarga de liberar memoria automáticamente cuando detecta que un objeto ya no es accesible desde ninguna parte del programa. Por tanto, aunque conceptualmente se diga que el ciclo de vida de los puntos está ligado al de la línea, esa “destrucción” no se expresa mediante código explícito.

En una composición fuerte, lo que realmente ocurre es que los objetos contenidos dejan de ser accesibles cuando el contenedor deja de serlo. Si una instancia de `Linea` desaparece porque ya no se guarda ninguna referencia a ella, también desaparecen las referencias a sus `Punto`, y el recolector de basura podrá liberar ambos tipos de objetos. La relación de ciclo de vida se expresa, por tanto, **a nivel de diseño**, no mediante instrucciones de destrucción explícita.

Este comportamiento implica que la composición fuerte en Java se basa en **propiedad lógica**, no en control manual de memoria. La línea “posee” sus puntos porque los crea internamente y no los expone para que otros objetos los gestionen, pero no porque ejecute ninguna acción de destrucción. El recolector de basura se encarga de eliminar tanto la línea como sus puntos cuando ya no son necesarios, manteniendo la coherencia del modelo de composición sin intervención del programador.

En resumen, Java no destruye objetos de forma explícita; simplemente deja que el recolector de basura actúe cuando los objetos quedan fuera de uso. La composición fuerte se expresa mediante encapsulación, inmutabilidad y control de referencias, no mediante destrucción manual.



## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

A continuación se muestra un ejemplo completo de **composición débil** entre un `Departamento` y sus `Profesor`, cumpliendo todas las restricciones pedidas:  
- El departamento mantiene una lista de hasta 50 profesores.  
- El director es siempre un profesor del departamento.  
- Se puede añadir y eliminar profesores sin romper la encapsulación.  
- Se garantiza la invariante: *el director siempre pertenece al departamento*.  
- Se lanzan excepciones cuando se viola la invariante.  

---

# **Clase `Profesor` (simple e inmutable)**

```java
public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede ser vacío");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

Esta clase es sencilla e inmutable, lo que facilita su uso en composiciones débiles. Los profesores existen independientemente del departamento, y pueden ser compartidos por otros objetos si fuera necesario. No hay relación de propiedad fuerte, solo de uso.

---

# **Clase `Departamento` con dos composiciones débiles**

```java
public class Departamento {

    private static final int MAX_PROFESORES = 50;

    private final Profesor[] profesores;
    private int numProfesores;

    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe haber un director inicial");
        }
        this.profesores = new Profesor[MAX_PROFESORES];
        this.numProfesores = 0;

        // Añadir el director como primer profesor
        profesores[numProfesores++] = directorInicial;
        this.director = directorInicial;
    }

    public int getNumeroProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        return profesores[posicion];
    }

    public void añadirProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("Profesor no puede ser null");
        }
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("No caben más profesores");
        }
        profesores[numProfesores++] = p;
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }

        Profesor aEliminar = profesores[posicion];

        // No se puede eliminar al director
        if (aEliminar == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento");
        }

        // Compactar el array
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--numProfesores] = null;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }

        // Comprobar que el nuevo director pertenece al departamento
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }

        if (!encontrado) {
            throw new IllegalStateException("El nuevo director debe ser un profesor del departamento");
        }

        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }
}
```

---

# **Explicación conceptual**

En esta implementación, la relación entre `Departamento` y `Profesor` es una **composición débil**, porque el departamento no crea ni destruye profesores: simplemente los referencia. Los profesores pueden existir antes o después del departamento, y podrían incluso pertenecer a otros contextos si se quisiera. Sin embargo, el departamento mantiene una estructura interna que controla estrictamente su lista de profesores y garantiza la invariante de que el director siempre forma parte de ella.

La gestión del director introduce una segunda composición débil: el departamento no “posee” al director, pero sí exige que sea uno de los profesores que contiene. Esto obliga a validar cuidadosamente tanto el cambio de director como la eliminación de profesores, evitando inconsistencias. El uso de un array encapsulado permite cumplir el requisito de no exponer la estructura interna, ofreciendo solo métodos seguros para consultar, añadir y eliminar profesores.

---

Si quieres, puedo añadir un diagrama UML, una versión con `ArrayList`, o una variante con profesores inmutables y roles adicionales.



## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

A continuación se muestra cómo quedaría el ejemplo anterior usando **`List<Profesor>`** en lugar de arrays primitivos. El uso de listas simplifica notablemente el código, ya que ya no es necesario gestionar manualmente el tamaño, desplazar elementos al eliminar ni controlar posiciones libres. La estructura interna se vuelve más flexible y expresiva, manteniendo la encapsulación y las invariantes de clase.

---

# **Versión con `List<Profesor>`**

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Departamento {

    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe haber un director inicial");
        }
        this.profesores = new ArrayList<>();
        profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public int getNumeroProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        return profesores.get(pos);
    }

    public void añadirProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("Profesor no puede ser null");
        }
        profesores.add(p);
    }

    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }

        Profesor aEliminar = profesores.get(pos);

        if (aEliminar == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }

        profesores.remove(pos);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalStateException("El director debe ser un profesor del departamento");
        }
        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }

    // Alternativa segura para devolver todos los profesores
    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}
```

---

# **¿Qué parte del código original se ha ahorrado?**

El uso de `List` elimina varias responsabilidades que antes recaían sobre el programador:

- Ya no es necesario gestionar un tamaño máximo ni un contador manual (`numProfesores`).
- No hace falta desplazar elementos al eliminar un profesor.
- No se requiere comprobar si queda espacio libre en el array.
- No es necesario inicializar un array fijo ni mantener posiciones nulas.

En conjunto, desaparece toda la lógica de mantenimiento del array, lo que reduce el riesgo de errores y hace el código más claro y expresivo.

---

# **¿Por qué no se debe devolver directamente la lista interna?**

Si se devolviera la lista interna tal cual, cualquier código externo podría:

- Añadir profesores sin pasar por las validaciones del departamento.
- Eliminar profesores, incluso al director.
- Modificar el orden o vaciar la lista.
- Romper la invariante de que el director debe pertenecer al departamento.

Esto violaría completamente la encapsulación y permitiría dejar el objeto en un estado inconsistente.

---

# **¿Cómo se resuelve?**

La solución correcta es devolver una **vista inmodificable** de la lista interna:

```java
public List<Profesor> getProfesores() {
    return Collections.unmodifiableList(profesores);
}
```

De este modo, el cliente puede consultar la lista, pero no modificarla.  
Si se necesitara una copia independiente, también sería válido:

```java
return new ArrayList<>(profesores);
```

Ambas opciones protegen la invariante y mantienen la encapsulación del objeto.

---

Si quieres, puedo añadir una versión con `Set`, una variante con profesores identificados por DNI, o un diagrama UML que muestre ambas composiciones débiles.



## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

A continuación se muestra un ejemplo de **composición recursiva** en Java: una `Persona` inmutable que contiene una referencia a su **madre**, que es otra `Persona`. Esta relación es recursiva porque una persona puede tener una madre, que a su vez es otra persona con su propia madre, y así sucesivamente. La inmutabilidad garantiza que, una vez creada la estructura familiar, no pueda modificarse, preservando la coherencia del modelo.

```java
public final class Persona {
    private final String nombre;
    private final Persona madre; // composición recursiva

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede ser vacío");
        }
        this.nombre = nombre;
        this.madre = madre; // puede ser null si no se conoce
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
}
```

Un ejemplo de uso podría construir una pequeña genealogía desde la abuela hasta el nieto. Cada persona se crea con su madre correspondiente, formando una cadena recursiva. Este tipo de estructura permite recorrer la ascendencia simplemente siguiendo las referencias, sin necesidad de estructuras auxiliares. El siguiente `main` muestra cómo se instancian los objetos y cómo se puede navegar por la relación.

```java
public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("María", null);
        Persona madre = new Persona("Laura", abuela);
        Persona nieto = new Persona("Carlos", madre);

        System.out.println("Nieto: " + nieto.getNombre());
        System.out.println("Madre: " + nieto.getMadre().getNombre());
        System.out.println("Abuela: " + nieto.getMadre().getMadre().getNombre());
    }
}
```

Este patrón de composición recursiva aparece en numerosos contextos. Un ejemplo clásico es el de los **árboles**, donde cada nodo contiene referencias a sus hijos, que a su vez son nodos del mismo tipo. Otro caso habitual es el de los **directorios y archivos** en un sistema de ficheros, donde un directorio puede contener otros directorios, formando una estructura jerárquica. También se observa en expresiones matemáticas o lógicas, donde cada expresión puede contener subexpresiones del mismo tipo, permitiendo representar estructuras complejas de forma natural.

Si quieres, puedo extender el ejemplo a un árbol genealógico completo, añadir métodos de recorrido o mostrar cómo representar estas composiciones recursivas en UML.


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Las **relaciones de composición bidireccionales** son aquellas en las que **cada objeto conoce al otro**: el “todo” conoce a sus partes y, simultáneamente, cada parte mantiene una referencia al “todo” al que pertenece. Esto crea una relación en ambos sentidos, lo que permite navegar desde el contenedor hacia los elementos y desde los elementos hacia el contenedor. Este tipo de diseño puede ser útil, pero también introduce riesgos: ciclos de referencias, mayor acoplamiento y la necesidad de mantener invariantes en ambas direcciones para evitar estados inconsistentes.

En el ejemplo de `Profesor` y `Departamento`, actualmente la relación es **unidireccional**: el departamento conoce a sus profesores, pero los profesores no saben a qué departamento pertenecen. Para convertirla en **bidireccional**, cada `Profesor` debería tener un atributo `Departamento departamento`, que se establezca cuando el profesor se añade al departamento. Esto implica modificar los métodos `añadirProfesor`, `eliminarProfesor` y `cambiarDirector` para mantener la coherencia: al añadir un profesor, se debe asignar su departamento; al eliminarlo, se debe poner su departamento a `null`; y al cambiar el director, no habría que hacer nada extra porque ya pertenece al departamento.

Además, habría que garantizar que un profesor no pueda pertenecer a dos departamentos simultáneamente, lo que obligaría a validar en `añadirProfesor` que `p.getDepartamento()` sea `null` antes de añadirlo. También sería necesario evitar que el usuario modifique directamente el departamento desde el profesor, ya que eso rompería la invariante; por tanto, el método `setDepartamento` debería ser privado o con acceso restringido. En conjunto, la relación bidireccional exige más disciplina en el mantenimiento de la coherencia, pero permite navegar en ambas direcciones dentro del modelo.

