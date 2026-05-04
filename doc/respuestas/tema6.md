<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

Aquí tienes un ejemplo **académico, claro y directo**, mostrando cómo crear una estructura de datos que pueda almacenar **cualquier tipo de dato** usando:

- `void*` en **C**
- `Object` en **Java**

En ambos casos se usa un **array primitivo** como contenedor.

---

# ✔ Ejemplo en C usando `void*`

En C, `void*` permite almacenar **punteros a cualquier tipo**.  
El array es de tipo `void*`, y cada elemento apunta a un dato real.

```c
#include <stdio.h>

#define MAX 10

typedef struct {
    void* datos[MAX];
    int size;
} ArrayGenerico;

void agregar(ArrayGenerico* arr, void* elemento) {
    arr->datos[arr->size++] = elemento;
}

int main() {
    ArrayGenerico arr = { .size = 0 };

    int x = 42;
    double y = 3.14;
    char* texto = "Hola";

    agregar(&arr, &x);
    agregar(&arr, &y);
    agregar(&arr, texto);

    printf("Entero: %d\n", *(int*)arr.datos[0]);
    printf("Double: %f\n", *(double*)arr.datos[1]);
    printf("Texto: %s\n", (char*)arr.datos[2]);
}
```

### ✔ Características
- El array es **primitivo**: `void* datos[MAX]`.
- Se puede almacenar **cualquier tipo**.
- Para recuperar los datos hay que hacer **downcasting manual**.

---

# ✔ Ejemplo equivalente en Java usando `Object`

En Java, `Object` es la superclase de todos los tipos de referencia, así que un array de `Object` puede almacenar **cualquier objeto**.

```java
public class ArrayGenerico {

    private final Object[] datos;
    private int size;

    public ArrayGenerico(int capacidad) {
        datos = new Object[capacidad];
        size = 0;
    }

    public void agregar(Object o) {
        datos[size++] = o;
    }

    public Object obtener(int i) {
        return datos[i];
    }

    public static void main(String[] args) {

        ArrayGenerico arr = new ArrayGenerico(10);

        arr.agregar("Hola");
        arr.agregar(123);        // autoboxing → Integer
        arr.agregar(3.14);       // autoboxing → Double

        String s = (String) arr.obtener(0);
        int n = (Integer) arr.obtener(1);
        double d = (Double) arr.obtener(2);

        System.out.println(s);
        System.out.println(n);
        System.out.println(d);
    }
}
```

### ✔ Características
- El array es **primitivo**: `new Object[capacidad]`.
- Puede almacenar **cualquier objeto**.
- Para recuperar el valor se hace **downcasting**, igual que con `void*`.

---

# ✔ Resumen comparativo

| Lenguaje | Tipo genérico | Array | Recuperación |
|----------|----------------|--------|--------------|
| **C** | `void*` | `void* datos[]` | Cast manual |
| **Java** | `Object` | `Object[] datos` | Cast manual |

Ambos ejemplos ilustran cómo simular un contenedor genérico **sin usar genéricos**, solo con un array primitivo y el tipo más general del lenguaje.

---

Si quieres, puedo extender esto a una versión **segura con genéricos en Java**, o a una versión **con realloc en C** para que el array crezca dinámicamente.


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

Aquí va la explicación breve, precisa y totalmente orientada a teoría de AEDI.

---

# ✔ ¿Qué significa **programación genérica**?

La **programación genérica** es un estilo de programación en el que las estructuras de datos y los algoritmos se escriben **sin fijar el tipo de dato**, de forma que puedan trabajar con *cualquier tipo* manteniendo **seguridad de tipos en tiempo de compilación**.

En Java esto se hace con **genéricos**:

```java
List<String> lista = new ArrayList<>();
```

En C++ se hace con **templates**:

```cpp
vector<int> v;
```

La idea clave:

> El tipo se parametriza, en lugar de usar un tipo universal como `Object` o `void*`.

---

# ✔ ¿Es el ejemplo anterior un ejemplo de programación genérica?

**No.**  
El ejemplo anterior **no es programación genérica**, sino **programación no tipada** usando el tipo más general del lenguaje:

- `void*` en C  
- `Object` en Java  

Eso permite almacenar cualquier cosa, pero:

- **no hay seguridad de tipos**  
- hay que hacer **casts manuales**  
- los errores se detectan **en tiempo de ejecución**, no en compilación  

La programación genérica **auténtica** evita todo eso.

---

# ✔ Diferencia clara y directa

| Técnica | ¿Seguridad de tipos? | ¿Necesita cast? | ¿Cuándo falla? |
|--------|------------------------|------------------|----------------|
| `void*` en C | ❌ No | ✔ Sí | Ejecución |
| `Object` en Java | ❌ No | ✔ Sí | Ejecución |
| **Genéricos** | ✔ Sí | ❌ No | Compilación |

---

# ✔ Resumen académico

- La **programación genérica** permite escribir código independiente del tipo, pero **seguro**.  
- El ejemplo anterior **no** es programación genérica: es un contenedor universal sin tipos.  
- Los genéricos **evitan errores** y permiten que el compilador verifique los tipos.

---

Si quieres, puedo reescribir el ejemplo anterior usando **genéricos reales en Java**, para que veas la diferencia exacta entre `Object[]` y `T[]`.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

Aquí tienes la explicación breve, clara y académica sobre **los problemas de chequeo de tipos** cuando se usan `void*` en C o `Object` en Java para crear estructuras de datos “genéricas”.

---

# ✔ Problemas al usar `void*` (C) u `Object` (Java) en estructuras genéricas

Cuando una estructura de datos almacena elementos usando el **tipo más general posible**, aparecen varios problemas importantes relacionados con la **seguridad de tipos**.

---

## 1. ❌ **Pérdida total de información de tipos**

Al almacenar datos como:

- `void*` en C  
- `Object` en Java  

la estructura **no sabe qué tipo real** tiene cada elemento.

Esto implica:

- No se puede verificar si el tipo es correcto.
- No se puede garantizar que el cast posterior sea válido.

---

## 2. ❌ **Necesidad de *downcasting* manual**

Para recuperar un elemento, el programador debe hacer un cast explícito:

- `(int*)` en C  
- `(Integer)` en Java  

Esto introduce riesgo porque:

- El compilador **no puede comprobar** si el cast es correcto.
- Si el tipo no coincide, el error aparece **en tiempo de ejecución**.

Ejemplos:

- C: comportamiento indefinido (posible crash).
- Java: `ClassCastException`.

---

## 3. ❌ **Errores que deberían detectarse en compilación pasan a ejecución**

Con genéricos reales, el compilador detecta errores de tipo.  
Con `void*` u `Object`, no.

Ejemplo en Java:

```java
Object[] arr = new Object[10];
arr[0] = "Hola";
Integer n = (Integer) arr[0];   // ERROR en ejecución, no en compilación
```

Ejemplo en C:

```c
void* arr[10];
arr[0] = "Hola";
int x = *(int*)arr[0];   // comportamiento indefinido
```

---

## 4. ❌ **Mayor probabilidad de errores silenciosos**

En C, un cast incorrecto puede:

- leer memoria inválida,
- corromper datos,
- provocar fallos difíciles de depurar.

En Java, aunque más seguro, igualmente:

- el error aparece tarde,
- puede romper la ejecución del programa.

---

## 5. ❌ **No hay restricciones sobre qué se puede insertar**

La estructura acepta *cualquier cosa*:

- En C: cualquier puntero.
- En Java: cualquier objeto.

Esto impide imponer reglas como:

- “solo números”
- “solo elementos comparables”
- “solo objetos de un tipo concreto”

Con genéricos reales sí se puede:

```java
List<Integer> lista = new ArrayList<>();
```

---

# ✔ Resumen académico

| Problema | C (`void*`) | Java (`Object`) |
|----------|-------------|------------------|
| Pérdida de información de tipos | ✔ | ✔ |
| Necesidad de cast manual | ✔ | ✔ |
| Errores en tiempo de ejecución | ✔ (grave) | ✔ (ClassCastException) |
| Sin restricciones de tipo | ✔ | ✔ |
| Seguridad de tipos | ❌ | ❌ |

---

# ✔ Conclusión

Usar `void*` u `Object` permite crear estructuras “genéricas”, pero:

> **No hay seguridad de tipos**, y los errores aparecen tarde, en ejecución.  
> La programación genérica real (templates en C++, genéricos en Java) evita todos estos problemas.




## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Aquí va la explicación breve, precisa y totalmente alineada con teoría de AEDI.

---

# ✔ ¿Qué son los **parámetros de tipo**?

Los **parámetros de tipo** son *variables de tipo* que se usan para definir clases, interfaces o métodos **genéricos**, permitiendo que trabajen con distintos tipos **sin perder seguridad de tipos en tiempo de compilación**.

Se escriben entre `<>` y representan un tipo que será **sustituido** por un tipo concreto cuando se use la clase o el método.

Ejemplo típico en Java:

```java
class Caja<T> {
    private T valor;

    public void set(T v) { valor = v; }
    public T get() { return valor; }
}
```

Aquí:

- `T` es un **parámetro de tipo**.
- No es un tipo real, sino un **marcador** que será reemplazado por un tipo concreto al usar la clase:

```java
Caja<String> c1 = new Caja<>();
Caja<Integer> c2 = new Caja<>();
```

---

# ✔ ¿Para qué sirven?

Los parámetros de tipo permiten:

### 1. **Escribir código genérico seguro**
El compilador verifica que los tipos son correctos.

### 2. **Evitar casts**
Sin genéricos:

```java
Object o = lista.get(0);
String s = (String) o;   // cast obligatorio
```

Con genéricos:

```java
String s = lista.get(0); // sin cast
```

### 3. **Detectar errores en compilación**
Si intentas meter un tipo incorrecto:

```java
Caja<String> c = new Caja<>();
c.set(123);   // ERROR en compilación
```

---

# ✔ ¿Por qué son una mejora respecto a `Object` o `void*`?

Porque:

- **mantienen la información del tipo**,  
- **evitan errores en tiempo de ejecución**,  
- **eliminan la necesidad de downcasting**,  
- y permiten que el compilador garantice la corrección del tipo.

Son la solución formal y segura al problema que teníamos con:

- `void*` en C  
- `Object` en Java  

---

# ✔ Resumen académico

- Un **parámetro de tipo** es una variable que representa un tipo.  
- Se usa para crear **clases, interfaces y métodos genéricos**.  
- Permite escribir código **reutilizable y seguro**, sin casts.  
- Es la base de la **programación genérica moderna**.

---

Si quieres, en la siguiente pregunta podemos ver **métodos genéricos**, **múltiples parámetros de tipo**, o **restricciones con `extends`**, que suelen caer en examen.



## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

Aquí tienes **exactamente** lo que pides: un ejemplo claro, académico y comparativo de **programación genérica** en Java (generics) y en C++ (templates), creando una lista/vector que **solo admite `String`**, insertando valores y recorriéndolos con **seguridad de tipos**.

---

# ✔ Java — *Generics*

En Java, los genéricos se implementan con **parámetros de tipo** entre `<>`.

```java
import java.util.ArrayList;
import java.util.List;

public class EjemploJava {
    public static void main(String[] args) {

        // Lista genérica que SOLO admite String
        List<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Mundo");
        lista.add("Genéricos");

        // Recorrido seguro: cada elemento es String
        for (String s : lista) {
            System.out.println(s.toUpperCase());
        }
    }
}
```

### ✔ Seguridad de tipos
- No puedes hacer `lista.add(123)` → **error en compilación**.
- No necesitas *downcasting*.
- Cada elemento es **garantizado** como `String`.

---

# ✔ C++ — *Templates*

En C++ los genéricos se implementan con **templates**.

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {

    // Vector genérico que SOLO admite std::string
    std::vector<std::string> v;

    v.push_back("Hola");
    v.push_back("Mundo");
    v.push_back("Templates");

    // Recorrido seguro: cada elemento es std::string
    for (const std::string& s : v) {
        std::cout << s << std::endl;
    }

    return 0;
}
```

### ✔ Seguridad de tipos
- No puedes hacer `v.push_back(3.14)` → **error en compilación**.
- No hay *casts*.
- Cada elemento es un `std::string`.

---

# ✔ Comparación directa

| Lenguaje | Mecanismo | Sintaxis | Seguridad de tipos | Cast necesario |
|----------|-----------|----------|---------------------|----------------|
| **Java** | Generics | `List<String>` | ✔ Sí | ❌ No |
| **C++** | Templates | `vector<string>` | ✔ Sí | ❌ No |

Ambos mecanismos permiten:

- parametrizar el tipo,
- evitar errores en tiempo de ejecución,
- escribir estructuras de datos reutilizables y seguras.

---

Si quieres, puedo ampliar esto con:

- **métodos genéricos** en ambos lenguajes,  
- **clases genéricas propias**,  
- o un ejemplo de **restricciones de tipo** (`extends` en Java, `concepts` en C++20).



## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

Aquí tienes la explicación **académica, precisa y comparativa** entre Java y C++, exactamente como se pide en AEDI. Este punto es clave porque muestra que **Java y C++ usan mecanismos completamente distintos** para implementar la programación genérica.

---

# ✔ ¿Qué hace el compilador cuando se instancia una clase genérica?

Depende del lenguaje:

---

# 🟦 **En Java: *type erasure*** (borrado de tipos)

Cuando usas una clase genérica en Java, por ejemplo:

```java
List<String> lista = new ArrayList<>();
```

el compilador **elimina la información del tipo** (`String`) durante la compilación.  
Esto se llama **type erasure**.

### ✔ ¿Qué implica el type erasure?

1. **En tiempo de compilación**:
   - El compilador usa los tipos genéricos para verificar que todo es correcto.
   - Detecta errores de tipo (por ejemplo, intentar meter un `Integer` en un `List<String>`).

2. **En tiempo de ejecución**:
   - La información del tipo **desaparece**.
   - `List<String>` y `List<Integer>` se convierten en **la misma clase**: `List`.

3. Los tipos genéricos se sustituyen por:
   - `Object` (si no hay restricciones)
   - el límite superior (`extends`) si existe

Ejemplo conceptual:

```java
class Caja<T> {
    T valor;
}
```

Tras el *type erasure* se convierte en algo equivalente a:

```java
class Caja {
    Object valor;
}
```

### ✔ Consecuencia importante

En Java **no existen genéricos reales en tiempo de ejecución**.  
Por eso no puedes hacer:

```java
if (obj instanceof List<String>)  // ❌ ERROR
```

---

# 🟥 **En C++: instanciación de plantillas**

En C++, cuando usas una plantilla:

```cpp
std::vector<std::string> v;
```

el compilador **genera una versión nueva y distinta del código** para ese tipo concreto.

Esto se llama **instanciación de plantillas** (*template instantiation*).

### ✔ ¿Qué implica?

1. Para cada tipo usado, el compilador **crea una copia especializada** del código.
2. `vector<int>` y `vector<string>` son **clases distintas**, con código distinto.
3. La información del tipo **se conserva en tiempo de compilación y ejecución**.

Ejemplo conceptual:

```cpp
template <typename T>
class Caja {
    T valor;
};
```

El compilador genera:

```cpp
class Caja_int { int valor; };
class Caja_string { std::string valor; };
```

### ✔ Consecuencia importante

- C++ tiene **genéricos reales**, con tipos distintos para cada instanciación.
- Esto permite optimizaciones y elimina casts.
- Pero también puede generar **mucho código duplicado** (*code bloat*).

---

# 🟩 Comparación directa Java vs C++

| Característica | Java (Generics) | C++ (Templates) |
|----------------|------------------|------------------|
| Mecanismo | **Type erasure** | **Instanciación de plantillas** |
| ¿Se conserva el tipo en ejecución? | ❌ No | ✔ Sí |
| ¿Se genera código nuevo por tipo? | ❌ No | ✔ Sí |
| ¿Hay casts implícitos? | ✔ Sí (tras el borrado) | ❌ No |
| ¿Seguridad de tipos? | ✔ En compilación | ✔ En compilación |
| ¿Rendimiento? | Igual para todos los tipos | Especializado y optimizable |
| ¿Se puede hacer `instanceof List<String>`? | ❌ No | ✔ Sí (equivalente con plantillas) |

---

# ✔ Resumen académico

- **Java** usa *type erasure*:  
  Los tipos genéricos existen solo en compilación; en ejecución desaparecen.  
  → No hay genéricos reales en tiempo de ejecución.

- **C++** usa *template instantiation*:  
  El compilador genera una versión distinta del código para cada tipo.  
  → Genéricos reales, especializados y con tipos distintos.

---

Si quieres, puedo prepararte un ejercicio comparativo tipo examen donde tengas que identificar qué operaciones son posibles en Java y cuáles en C++ según su modelo de genéricos.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

Aquí tienes la solución **clara, académica y perfectamente alineada con programación genérica en Java**, creando una clase `Par<A,B>` con dos tipos distintos y usándola para devolver **media** y **desviación típica** de un array de `double`.

---

# ✔ Clase genérica `Par<A,B>`

Dos parámetros de tipo: uno para el primer valor y otro para el segundo.

```java
public class Par<A, B> {

    private final A primero;
    private final B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() {
        return primero;
    }

    public B getSegundo() {
        return segundo;
    }
}
```

### ✔ Características
- `A` y `B` son **parámetros de tipo**.
- La clase puede almacenar **dos valores de tipos distintos**.
- El tipo concreto se fija al instanciarla.

---

# ✔ Función que devuelve un `Par<Double, Double>`  
Media y desviación típica de un array de `double`.

```java
public static Par<Double, Double> calcularEstadisticas(double[] datos) {

    double suma = 0;
    for (double d : datos) {
        suma += d;
    }
    double media = suma / datos.length;

    double sumaCuadrados = 0;
    for (double d : datos) {
        sumaCuadrados += Math.pow(d - media, 2);
    }
    double desviacion = Math.sqrt(sumaCuadrados / datos.length);

    return new Par<>(media, desviacion);
}
```

---

# ✔ Ejemplo de uso

```java
public class Principal {
    public static void main(String[] args) {

        double[] valores = { 1.0, 2.0, 3.0, 4.0 };

        Par<Double, Double> resultado = calcularEstadisticas(valores);

        double media = resultado.getPrimero();
        double desviacion = resultado.getSegundo();

        System.out.println("Media: " + media);
        System.out.println("Desviación típica: " + desviacion);
    }
}
```

### ✔ Seguridad de tipos
- `resultado` es un `Par<Double,Double>`.
- No hay *casts*.
- El compilador garantiza que ambos valores son `Double`.

---

# ✔ Resumen académico

- `Par<A,B>` es una clase genérica con **dos parámetros de tipo**.
- Permite almacenar dos valores de tipos distintos con **seguridad de tipos**.
- Se usa para devolver múltiples valores sin crear una clase específica.
- El ejemplo muestra cómo devolver **media** y **desviación típica** en un único objeto.

---

Si quieres, puedo extender este ejemplo para que `Par` implemente `Comparable`, o convertirlo en un `Registro` (record) de Java moderno.



## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

Aquí tienes la explicación **precisa, comparativa y orientada a examen**, mostrando por qué un **método genérico** es superior a usar `Object` cuando quieres que dos argumentos sean del mismo tipo y evitar *downcasting*.

---

# ✔ 1. Versión sin genéricos (con `Object`)  
**No hay seguridad de tipos** y **obliga a hacer cast**.

```java
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}
```

### ❌ Problema (i): obliga a *downcasting*

```java
String s = (String) seleccionaUno("hola", "adiós");
```

Si te equivocas en el cast, el error aparece **en ejecución**, no en compilación.

### ❌ Problema (ii): NO fuerza que ambos objetos sean del mismo tipo

Esto **compila**, pero es peligrosísimo:

```java
String s = (String) seleccionaUno("hola", 123);   // compila
```

Si la función devuelve `123`, obtienes un **ClassCastException**.

---

# ✔ 2. Versión correcta: método genérico `<T>`  
El compilador garantiza que ambos argumentos son del **mismo tipo** y evita *downcasting*.

```java
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}
```

---

# ✔ Ventajas del método genérico

## (i) Evita *downcasting*
El tipo de retorno es **exactamente el mismo** que el de los argumentos:

```java
String s = seleccionaUno("rojo", "azul");   // sin cast
```

El compilador sabe que `s` es un `String`.

---

## (ii) Fuerza que ambos objetos sean del mismo tipo  
Esto **NO compila**:

```java
seleccionaUno("hola", 123);   // ❌ ERROR de compilación
```

El compilador detecta que `"hola"` es `String` y `123` es `Integer`, y rechaza la llamada.

---

# ✔ Ejemplo completo

```java
public class Principal {

    public static <T> T seleccionaUno(T a, T b) {
        return Math.random() < 0.5 ? a : b;
    }

    public static void main(String[] args) {

        // Correcto: ambos son String
        String color = seleccionaUno("rojo", "azul");
        System.out.println("Elegido: " + color);

        // Incorrecto: tipos distintos → error de compilación
        // seleccionaUno("hola", 123);
    }
}
```

---

# ✔ Resumen académico

| Versión | Seguridad de tipos | ¿Evita cast? | ¿Fuerza tipos iguales? |
|--------|---------------------|--------------|-------------------------|
| `Object seleccionaUno(Object,Object)` | ❌ No | ❌ No | ❌ No |
| `<T> T seleccionaUno(T,T)` | ✔ Sí | ✔ Sí | ✔ Sí |

---

Si quieres, puedo continuar con **métodos genéricos con múltiples parámetros de tipo**, o con **restricciones (`<T extends Comparable<T>>`)**, que suelen caer en examen.





## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Aquí tienes la explicación **académica, precisa y orientada a AEDI**, con las dos soluciones pedidas y la relación con el *type erasure* de Java.

---

# ✔ ¿Se pueden establecer restricciones en los parámetros de tipo?

Sí.  
En Java puedes restringir un parámetro de tipo usando **bounded generics**:

```java
<T extends Number>
```

Esto significa:

> “T debe ser un subtipo de Number”.

Así puedes garantizar que `T` es un número y que puedes usar métodos como `doubleValue()`.

---

# ✔ SOLUCIÓN 1 — Sin genéricos: usar directamente `Number`

Aquí `Punto` funciona con **cualquier subtipo de `Number`**, pero **pierdes información del tipo concreto**.

```java
public class Punto {

    private final Number x;
    private final Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(Punto otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

### ✔ Ventajas
- Muy simple.
- Acepta `Integer`, `Double`, `Float`, etc.

### ❌ Inconvenientes
- No sabes si un punto usa `Integer`, `Double`, etc.
- No hay seguridad de tipos entre puntos: puedes mezclar `Punto(Integer)` con `Punto(Double)`.

---

# ✔ SOLUCIÓN 2 — Con genéricos: `Punto<T extends Number>`

Aquí reforzamos el chequeo de tipos:  
**todos los valores del punto son del mismo tipo numérico**.

```java
public class Punto<T extends Number> {

    private final T x;
    private final T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

### ✔ Ventajas
- **Seguridad de tipos**: si creas `Punto<Integer>`, todos los valores serán `Integer`.
- No puedes mezclar `Punto<Integer>` con `Punto<Double>` en `calcularDistanciaA`.

### ✔ Ejemplo de uso

```java
Punto<Integer> p1 = new Punto<>(3, 4);
Punto<Integer> p2 = new Punto<>(0, 0);
double d1 = p1.calcularDistanciaA(p2);

Punto<Double> q1 = new Punto<>(1.5, 2.5);
Punto<Double> q2 = new Punto<>(4.0, 6.0);
double d2 = q1.calcularDistanciaA(q2);
```

### ❌ Esto NO compila (y es bueno que no compile)

```java
Punto<Integer> a = new Punto<>(1, 2);
Punto<Double> b = new Punto<>(1.0, 2.0);

a.calcularDistanciaA(b);   // ERROR: tipos incompatibles
```

---

# ✔ ¿Qué ocurre tras la compilación? (*type erasure*)

En Java, los genéricos **desaparecen** en tiempo de ejecución.

Para la clase:

```java
public class Punto<T extends Number> { ... }
```

tras el *type erasure*, el compilador genera algo equivalente a:

```java
public class Punto {

    private final Number x;
    private final Number y;

    public Punto(Number x, Number y) { ... }

    public Number getX() { ... }
    public Number getY() { ... }

    public double calcularDistanciaA(Punto otro) { ... }
}
```

### ✔ Conclusión del *type erasure*
- El tipo final en ejecución es **Punto** con atributos de tipo **Number**.
- La información de `T` **no existe en tiempo de ejecución**.
- Toda la seguridad de tipos ocurre **solo en compilación**.

---

# ✔ Resumen académico

- Sí, se pueden establecer restricciones: `T extends Number`.
- **Solución 1**: usar `Number` directamente → simple pero menos seguro.
- **Solución 2**: usar genéricos → seguridad de tipos y coherencia interna.
- En Java, tras el *type erasure*, el tipo real es siempre `Punto` con `Number`, independientemente del parámetro de tipo.

---

Si quieres, puedo extender esto a un `Punto3D<T extends Number>` o a una `Linea<P extends Punto<?>>` para ver cómo se combinan genéricos con herencia.



## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

Vamos a comparar **las dos soluciones** (sin genéricos y con genéricos) desde el punto de vista del **chequeo de tipos**, que es exactamente lo que te interesa.

---

# ✔ 1. ¿Permiten ambas soluciones crear un punto con coordenadas de tipos distintos?

## 🔹 **Solución 1: usando `Number` directamente (sin genéricos)**  
Sí, **permite mezclar tipos**:

```java
Punto p = new Punto(3, 4.5);   // ✔ Integer y Double mezclados
```

Esto es válido porque ambos son `Number`.

➡️ **No hay refuerzo de tipos**: cada coordenada puede ser un subtipo distinto de `Number`.

---

## 🔹 **Solución 2: usando genéricos `Punto<T extends Number>`**  
No, **NO permite mezclar tipos**:

```java
Punto<Integer> p = new Punto<>(3, 4.5);   // ❌ ERROR
```

El compilador exige que **ambas coordenadas sean del mismo tipo `T`**.

➡️ **Sí hay refuerzo de tipos**: si `T` es `Integer`, ambas coordenadas deben ser `Integer`.  
Si `T` es `Double`, ambas deben ser `Double`.

---

# ✔ 2. ¿Qué tipo devuelve `getX()` en cada solución?

## 🔹 **Solución sin genéricos (atributos `Number`)**

```java
public Number getX()
```

Siempre devuelve un **Number**, independientemente del tipo real almacenado.

Ejemplo:

```java
Number x = p.getX();
```

Si quieres usarlo como `int`, `double`, etc., debes llamar a:

```java
x.intValue();
x.doubleValue();
```

➡️ **El tipo estático es siempre `Number`.**

---

## 🔹 **Solución con genéricos `Punto<T extends Number>`**

```java
public T getX()
```

El tipo devuelto es **exactamente el tipo numérico concreto** con el que se instanció el punto.

Ejemplos:

```java
Punto<Integer> p1 = new Punto<>(3, 4);
Integer x1 = p1.getX();   // ✔ devuelve Integer

Punto<Double> p2 = new Punto<>(1.5, 2.5);
Double x2 = p2.getX();    // ✔ devuelve Double
```

➡️ **El tipo estático es `T`, no `Number`.**  
➡️ El compilador sabe exactamente qué tipo numérico devuelve.

---

# ✔ 3. ¿Qué ocurre tras la compilación? (*type erasure*)

Aunque uses:

```java
Punto<Integer>
Punto<Double>
```

tras el *type erasure*, **ambas versiones se convierten en la misma clase**:

```java
class Punto {
    Number x;
    Number y;
}
```

➡️ **En tiempo de ejecución, el tipo real es siempre `Number`.**  
➡️ Toda la seguridad de tipos ocurre **solo en compilación**.

---

# ✔ Resumen académico

| Aspecto | Sin genéricos (`Number`) | Con genéricos (`<T extends Number>`) |
|---------|---------------------------|---------------------------------------|
| ¿Permite mezclar tipos? | ✔ Sí | ❌ No |
| Tipo devuelto por `getX()` | `Number` | `T` (tipo concreto) |
| Seguridad de tipos | ❌ Baja | ✔ Alta |
| ¿Detecta errores en compilación? | ❌ No | ✔ Sí |
| Tipo tras *type erasure* | `Number` | `Number` |

---

Si quieres, puedo continuar con una versión **3D**, o con una `Linea<P extends Punto<?>>` para ver cómo se combinan restricciones de tipo con herencia.



## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

Perfecto, Zaida. Vamos a transformar tu diseño original —que usa `instanceof` y *downcasting*— en una solución **totalmente tipada**, **sin comprobaciones en tiempo de ejecución**, usando **generics con autolimitación** (*F‑bounded polymorphism*).  
Este es el patrón correcto cuando quieres que:

- `Punto2D` solo calcule distancias con **Punto2D**  
- `Punto3D` solo calcule distancias con **Punto3D**  
- El compilador **impida mezclar tipos**  
- No exista ni un solo `instanceof` ni *downcasting*

---

# ✔ 1. Interfaz `Punto<T extends Punto<T>>`  
El truco es que la interfaz recibe como parámetro de tipo **su propio subtipo**.

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T otro);
}
```

### Qué significa esto
- `T` es “el mismo tipo que implementa esta interfaz”.
- Cada subtipo fijará `T` a sí mismo.
- El método `distanciaA` **solo acepta puntos del mismo subtipo**.

---

# ✔ 2. Implementación correcta de `Punto2D`  
Sin `instanceof`, sin cast, sin errores en ejecución.

```java
public class Punto2D implements Punto<Punto2D> {

    private final double x;
    private final double y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

### Observa
- El método recibe **Punto2D**, no `Punto`.
- El compilador garantiza que no puedes pasar un `Punto3D`.

---

# ✔ 3. Implementación correcta de `Punto3D`

```java
public class Punto3D implements Punto<Punto3D> {

    private final double x;
    private final double y;
    private final double z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        double dz = this.z - otro.z;
        return Math.sqrt(dx*dx + dy*dy + dz*dz);
    }
}
```

---

# ✔ 4. ¿Qué hemos conseguido?

## ✔ Seguridad de tipos en compilación  
Esto **NO compila**:

```java
Punto2D a = new Punto2D(1,2);
Punto3D b = new Punto3D(1,2,3);

a.distanciaA(b);   // ❌ ERROR de compilación
```

El compilador detecta que `distanciaA` espera un `Punto2D`.

---

## ✔ No hay `instanceof` ni *downcasting*  
Cada clase recibe directamente su tipo concreto.

---

## ✔ Polimorfismo limpio  
Cada subtipo implementa su propia distancia sin lógica condicional.

---

# ✔ 5. ¿Qué ocurre con el *type erasure*?

Aunque uses:

```java
Punto<Punto2D>
Punto<Punto3D>
```

tras la compilación, **todos los parámetros de tipo desaparecen**.

El interfaz se convierte en algo equivalente a:

```java
public interface Punto {
    double distanciaA(Punto otro);
}
```

Pero:

- El compilador **ya ha verificado** que `Punto2D` solo llama a `distanciaA(Punto2D)`
- El borrado de tipos **no afecta a la seguridad**, porque los errores ya se detectaron antes

➡️ **En tiempo de ejecución, el tipo es simplemente `Punto`, pero el código es seguro porque el compilador lo garantizó.**

---

# ✔ Resumen académico

| Aspecto | Código original | Código con generics F‑bounded |
|---------|-----------------|-------------------------------|
| ¿Necesita `instanceof`? | ✔ Sí | ❌ No |
| ¿Necesita downcasting? | ✔ Sí | ❌ No |
| ¿Se puede mezclar 2D y 3D? | ✔ Sí (error en ejecución) | ❌ No (error en compilación) |
| ¿Seguridad de tipos? | ❌ Baja | ✔ Máxima |
| ¿Qué queda tras *type erasure*? | — | `Punto` con métodos sobre `Punto` |

---




## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

Vamos a resolverlo con precisión académica y con la claridad que necesitas para AEDI. Esta pregunta es **clave** porque mezcla:

- subtipado entre clases,
- subtipado entre arrays,
- subtipado entre tipos genéricos,
- y las nociones de **covarianza**, **contravarianza** e **invariancia**.

---

# ✔ 1. ¿Es `List<String>` subtipo de `List<Object>`?

**NO.**

En Java, los tipos genéricos son **invariantes**.

Eso significa:

> Aunque `String` sea subtipo de `Object`,  
> **`List<String>` NO es subtipo de `List<Object>`**.

Ejemplo:

```java
List<String> ls = new ArrayList<>();
List<Object> lo = ls;   // ❌ ERROR de compilación
```

### ¿Por qué?

Porque si esto fuera legal:

```java
List<Object> lo = ls;
lo.add(123);   // válido para List<Object>
```

pero `ls` es una `List<String>`, así que ahora tendrías un `Integer` dentro de una lista de `String`.

➡️ **El compilador lo prohíbe para evitar errores en tiempo de ejecución.**

---

# ✔ 2. ¿Es `String[]` subtipo de `Object[]`?

**SÍ.**

Los arrays en Java son **covariantes**.

```java
String[] ss = new String[10];
Object[] oo = ss;   // ✔ permitido
```

### ¿Por qué es diferente?

Porque los arrays existen **en tiempo de ejecución**, y Java decidió (por compatibilidad histórica) que fueran covariantes.

Pero esto introduce un problema grave.

---

# ✔ 3. ¿Qué problema aparece con los arrays?

El siguiente código **compila**, pero falla en ejecución:

```java
String[] ss = new String[10];
Object[] oo = ss;     // permitido
oo[0] = 123;          // ❌ ArrayStoreException en tiempo de ejecución
```

El array real es un `String[]`, así que no puede almacenar un `Integer`.

➡️ **Los arrays son covariantes pero no seguros.**  
➡️ Por eso los genéricos se diseñaron como **invariantes**, para evitar este tipo de errores.

---

# ✔ 4. Covarianza, contravarianza e invariancia

Vamos a definirlo de forma clara y formal.

---

## ✔ Covarianza

Un tipo genérico `F<T>` es **covariante** si:

> Si `A` es subtipo de `B`, entonces `F<A>` es subtipo de `F<B>`.

Ejemplo en Java:

- Arrays: `String[]` es subtipo de `Object[]`.

Problema: **no es seguro**.

---

## ✔ Contravarianza

Un tipo genérico `F<T>` es **contravariante** si:

> Si `A` es subtipo de `B`, entonces `F<B>` es subtipo de `F<A>`.

Ejemplo en Java (solo con wildcards):

```java
List<? super Number>   // contravariante respecto a Number
```

---

## ✔ Invariancia

Un tipo genérico `F<T>` es **invariante** si:

> Aunque `A` sea subtipo de `B`,  
> **`F<A>` NO es subtipo de `F<B>` ni viceversa.**

Ejemplo:

```java
List<String>  ≠  List<Object>
```

Esto es lo que ocurre con **todos los tipos genéricos normales en Java**.

---

# ✔ 5. Resumen académico

| Tipo | ¿Covariante? | ¿Contravariante? | ¿Invariante? | ¿Seguro? |
|------|--------------|------------------|--------------|----------|
| **Arrays (`T[]`)** | ✔ Sí | ❌ No | ❌ No | ❌ No (ArrayStoreException) |
| **Genéricos (`List<T>`)** | ❌ No | ❌ No | ✔ Sí | ✔ Sí |

---

# ✔ Conclusión clara

- `String` es subtipo de `Object`.  
- **Pero `List<String>` NO es subtipo de `List<Object>`** → los genéricos son **invariantes**.  
- **Sí, `String[]` es subtipo de `Object[]`** → los arrays son **covariantes**, pero esto causa errores en ejecución.  
- La covarianza/contravarianza/invariancia describe **cómo se comporta un tipo genérico respecto a su parámetro de tipo**.

---

Si quieres, puedo continuar con:

- ejemplos de `? extends T` (covarianza segura),
- ejemplos de `? super T` (contravarianza segura),
- o un ejercicio de examen donde tengas que decidir si un tipo es válido o no.



## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Vamos a por ello, Zaida. Esta pregunta es **clave** para entender cómo Java recupera *covarianza* y *contravarianza* de forma **segura**, a diferencia de los arrays.

---

# ✔ 1. ¿Qué es un *wildcard* (`?`)?

Un **wildcard** es un comodín que representa **un tipo desconocido** dentro de un tipo genérico.

Ejemplo:

```java
List<?> lista;
```

Significa:

> “lista es una lista de *algo*, pero no sé de qué”.

Los wildcards permiten expresar relaciones de subtipado **sin romper la seguridad de tipos**.

---

# ✔ 2. `List<? extends T>` — **Covarianza controlada**

Significa:

> “Una lista de algún subtipo de `T`”.

Ejemplo:

```java
List<? extends Number> l;
```

Puede referirse a:

- `List<Integer>`
- `List<Double>`
- `List<Float>`

### ✔ ¿Para qué sirve?

Para **leer** valores de la lista como `T`, pero **no para escribir**.

Regla mnemotécnica:

> **PECS**: *Producer Extends, Consumer Super*  
> Si la lista **produce** valores (la lees), usa `extends`.

---

# ✔ 3. `List<? super T>` — **Contravarianza controlada**

Significa:

> “Una lista de algún supertipo de `T`”.

Ejemplo:

```java
List<? super Integer> l;
```

Puede referirse a:

- `List<Integer>`
- `List<Number>`
- `List<Object>`

### ✔ ¿Para qué sirve?

Para **escribir** valores de tipo `T` en la lista, pero **no para leerlos como `T`**.

Regla mnemotécnica:

> **PECS**: *Consumer Super*  
> Si la lista **consume** valores (le añades cosas), usa `super`.

---

# ✔ 4. Ejemplo (i): sumar una lista de números → `? extends Number`

Queremos **leer** números, no escribirlos.

```java
public static double sumar(List<? extends Number> lista) {
    double suma = 0;
    for (Number n : lista) {
        suma += n.doubleValue();
    }
    return suma;
}
```

Uso:

```java
sumar(List.of(1, 2, 3));        // List<Integer>
sumar(List.of(1.5, 2.5, 3.5));  // List<Double>
```

✔ Funciona con cualquier subtipo de `Number`.  
✔ No permite añadir elementos (seguro).

---

# ✔ 5. Ejemplo (ii): añadir enteros a una lista → `? super Integer`

Queremos **escribir** valores `Integer`.

```java
public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
    lista.add(30);
}
```

Uso:

```java
List<Object> lo = new ArrayList<>();
List<Number> ln = new ArrayList<>();
List<Integer> li = new ArrayList<>();

añadirEnteros(lo);   // ✔
añadirEnteros(ln);   // ✔
añadirEnteros(li);   // ✔
```

✔ Puedes añadir `Integer` sin riesgo.  
✔ No puedes leer como `Integer` (solo como `Object`).

---

# ✔ 6. Resumen académico

| Tipo | Significado | Permite leer | Permite escribir | Uso típico |
|------|-------------|--------------|------------------|------------|
| `List<?>` | tipo desconocido | ✔ como `Object` | ❌ | métodos muy genéricos |
| `List<? extends T>` | subtipo de T | ✔ como T | ❌ | listas que **producen** datos |
| `List<? super T>` | supertipo de T | ❌ | ✔ valores T | listas que **consumen** datos |

Regla **PECS** (muy de examen):

> **Producer Extends, Consumer Super**

---

# ✔ Conclusión

- Los wildcards permiten expresar **covarianza** (`extends`) y **contravarianza** (`super`) de forma **segura**, evitando los problemas de los arrays.
- `? extends` → leer, no escribir.  
- `? super` → escribir, no leer.

---

Si quieres, puedo prepararte un ejercicio tipo examen donde tengas que decidir si un método debe usar `extends`, `super` o ninguno.

