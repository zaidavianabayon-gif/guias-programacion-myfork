<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

#### 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C, al no tener un sistema de excepciones (try-catch), la gestión de errores se basa en **convenios**. El objetivo es que la función `raiz` emita una señal y que el código que la llama sea el responsable de decidir qué hacer (mostrar un mensaje, cerrar el programa, etc.).

Aquí tienes las dos estrategias más comunes para resolver este problema:

---

### Opción 1: Valor de retorno especial (Sentinela)

Consiste en devolver un valor que sea físicamente imposible como resultado válido de la operación. Para una raíz cuadrada, cualquier número negativo sirve como indicador de error, ya que el resultado siempre debe ser $\geq 0$.

* **Ventaja:** Muy sencillo de implementar.
* **Desventaja:** Obliga al programador a recordar qué significa cada valor "mágico".

```c
#include <stdio.h>
#include <math.h>

float raiz(float n) {
    if (n < 0) {
        return -1.0; // Valor sentinela que indica error
    }
    return sqrt(n);
}

int main() {
    float num = -5.0;
    float res = r(num);

    if (res == -1.0) {
        printf("Error: No se puede calcular la raiz de un negativo.\n");
    } else {
        printf("Resultado: %f\n", res);
    }
    return 0;
}

```

---

### Opción 2: Paso de parámetros por referencia (Punteros)

En esta opción, la función devuelve un **código de estado** (un entero o booleano) para indicar si hubo éxito o error. El resultado real de la operación se "devuelve" escribiendo en una zona de memoria proporcionada por el usuario a través de un puntero.

* **Ventaja:** Separa claramente el estado del error del valor del resultado. Es el estándar en bibliotecas robustas.
* **Desventaja:** Sintaxis un poco más compleja por el uso de punteros.

```c
#include <stdio.h>
#include <math.h>

// Devuelve 0 si hay error, 1 si hay éxito
int raiz_segura(float n, float *resultado) {
    if (n < 0) {
        return 0; // Indicador de error
    }
    *resultado = sqrt(n);
    return 1; // Indicador de éxito
}

int main() {
    float num = -5.0;
    float res;

    if (!raiz_segura(num, &res)) {
        printf("Error detectado desde fuera: Numero invalido.\n");
    } else {
        printf("Resultado: %f\n", res);
    }
    return 0;
}

```

---

### Resumen para tus apuntes

En C, la gestión de errores es **explícita y manual**. El programador tiene la carga de:

1. Comprobar el valor de retorno en cada llamada.
2. No olvidar gestionar el caso de error antes de usar el resultado.

¿Te gustaría que viéramos cómo Java soluciona este mismo problema de una forma mucho más elegante mediante el uso de excepciones?


#### 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una **excepción** es un evento inusual o un error que ocurre durante la ejecución de un programa y que interrumpe su flujo normal de instrucciones. En términos técnicos, es un **objeto** que encapsula información sobre qué salió mal, dónde y por qué.

A diferencia de los métodos de C (que devolvían códigos de error), una excepción "salta" a través de la pila de llamadas hasta encontrar a alguien que sepa gestionarla.

---

### Objetivos al usar excepciones

Un programador las emplea con dos propósitos principales dependiendo de su rol:

#### 1. Al implementar funciones (Lanzar la excepción)

El objetivo es **delegar la responsabilidad**. La función detecta que no puede cumplir su tarea (por ejemplo, porque la `raiz` recibe un negativo) y, en lugar de devolver un valor "mágico" o imprimir un mensaje por consola (que ensucia la lógica), lanza una señal de alarma.

* **Para qué:** Para garantizar que el error no pase desapercibido y "obligar" a quien usa la función a decidir cómo reaccionar.

#### 2. Al llamar a funciones (Capturar la excepción)

El objetivo es **separar el código principal de la gestión de errores**. Mediante bloques `try-catch`, el programador escribe el "camino feliz" (la lógica normal) en un sitio y el "plan de contingencia" en otro.

* **Para qué:** Para que el programa sea **robusto** (que no se cierre bruscamente ante un fallo) y para mantener el código limpio y legible, evitando llenar todo de condicionales `if (error)`.

¿Te gustaría que viéramos cómo se escribe esa misma función `raiz` en Java usando una excepción propia o una estándar como `IllegalArgumentException`?


#### 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

Para implementar esto en Java, utilizaremos una excepción estándar llamada `IllegalArgumentException`. A diferencia de C, no devolveremos un código de error, sino que "lanzaremos" un objeto que detendrá el flujo normal si no se captura.

### Código en Java

```java
public class Calculadora {

    // Método que calcula la raíz o lanza una excepción si el número es negativo
    public double raiz(double n) {
        if (n < 0) {
            // Lanzamos la excepción. El método termina aquí.
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo: " + n);
        }
        return Math.sqrt(n);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        double numero = -9.0;

        try {
            // Intentamos ejecutar el código "feliz"
            double resultado = calc.raiz(numero);
            System.out.println("El resultado es: " + resultado);
            
        } catch (IllegalArgumentException e) {
            // Este bloque solo se ejecuta si el método 'raiz' lanza la excepción
            System.out.println("Error detectado en el main: " + e.getMessage());
        }
        
        System.out.println("El programa continúa su ejecución normal...");
    }
}

```

---

### Notas para tus apuntes

* **`throw`**: Es la palabra clave para "disparar" la excepción. Es como un `return` de emergencia que solo devuelve objetos de tipo excepción.
* **`try`**: Envuelve el código que podría fallar. Si ocurre una excepción dentro, Java salta inmediatamente al bloque `catch`.
* **`catch (TipoDeExcepcion e)`**: Es el manejador. Aquí defines qué hacer cuando ocurre el error (informar al usuario, registrar un log, etc.).
* **`e.getMessage()`**: Las excepciones son objetos, por lo que tienen métodos. Este en particular devuelve el texto explicativo que pusimos al crearla.

### Diferencia clave con C

En C, si olvidas el `if (res == -1.0)`, el programa sigue con un dato erróneo y falla más tarde de forma silenciosa. En Java, si olvidas el `try-catch`, el programa se detiene (hace "crash") inmediatamente, lo cual es preferible porque evita que el error se propague y corrompa más datos.



#### 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Para entender el ciclo de vida de una excepción, imagina que es una "alarma de incendios" que recorre un edificio (la pila de llamadas).

### 1. ¿Qué es "lanzar" (`throw`)?

Es el acto de detectar un error y generar el objeto de excepción. En nuestro ejemplo, cuando `raiz(-9)` detecta el negativo, ejecuta `throw new IllegalArgumentException(...)`. En ese instante, la ejecución normal del método se **detiene por completo** y no se ejecuta ninguna línea más de ese método. No hay `return`.

### 2. ¿Qué es "propagar" una excepción?

Si el método donde ocurrió el error no tiene un bloque `try-catch`, la excepción no desaparece; "salta" hacia fuera, al método que lo llamó.

* **En la pila de llamadas:** Java mira quién llamó a `raiz()`. Si fue el `main()`, la excepción viaja al `main()`.
* **¿Qué les ocurre a las funciones?:** Las funciones por las que pasa la excepción **mueren prematuramente**. Se sacan de la pila de ejecución (stack) de forma abrupta. **No se reanudan nunca**.

### 3. ¿Qué es "controlar" o "capturar" (`catch`)?

Es detener la propagación. Cuando la excepción llega a un método que tiene un bloque `catch` compatible con ese tipo de error, la "alarma" se apaga. El programa recupera el control y puede seguir ejecutándose, pero **desde el bloque catch hacia abajo**, nunca volviendo atrás al punto donde falló.

---

### Ejemplo de flujo con `Calculadora`

Imagina este escenario de llamadas: `main()` llama a `operacionCompleja()`, y esta llama a `raiz(-9)`.

1. **`raiz(-9)`**: Lanza la excepción. Se detiene inmediatamente. Se elimina de la pila.
2. **`operacionCompleja()`**: Recibe la "bola de fuego". Como no tiene un `try-catch`, ella también se detiene inmediatamente. Se elimina de la pila. La excepción se ha **propagado**.
3. **`main()`**: Tiene un bloque `try { operacionCompleja(); } catch (...)`.
* La excepción llega aquí y es **capturada**.
* Se ejecuta el código dentro del `catch`.
* **Importante:** El programa continúa en la línea siguiente al bloque `catch`.



#### ¿Se reanudan las funciones que no la controlan?

**No.** Si `operacionCompleja()` tenía 10 líneas de código después de llamar a `raiz()`, esas 10 líneas **jamás se ejecutarán**. La única forma de que una función continúe tras un error de una función hija es que ella misma capture la excepción.

---

### Resumen para tus apuntes

* **Lanzar:** Encender la alarma (`throw`).
* **Propagar:** La alarma sube por la pila de llamadas cerrando métodos a su paso.
* **Capturar:** Apagar la alarma (`catch`) para salvar el programa.
* **Estado de la pila:** Cada método que no captura la excepción es "aniquilado" y sus variables locales se pierden.

¿Te gustaría que viéramos qué pasa si la excepción llega hasta el final de la pila (el método `main`) y nadie la captura?


#### 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La ventaja fundamental de la **propagación natural** en Java frente al modelo de C es que el error es **imposible de ignorar por accidente**. En C, el error es un "susurro" que el programador debe elegir escuchar; en Java, la excepción es un "grito" que detiene todo hasta que alguien lo atiende.

Aquí tienes los puntos clave para tus apuntes:

### 1. Limpieza del Código (Separación de incumbencias)

En **C**, si tienes una cadena de 5 funciones donde la última puede fallar, las 4 funciones intermedias deben incluir lógica de control (`if (error) return -1;`) solo para pasar el error hacia arriba. Esto ensucia la lógica de negocio con fontanería de errores.

En **Java**, gracias a la propagación natural, las funciones intermedias **no necesitan ni una sola línea de código de error**. La excepción simplemente "atraviesa" esas funciones hasta llegar al punto donde realmente tiene sentido gestionarla (por ejemplo, el `main` o la interfaz de usuario).

### 2. Robustez: Errores no ignorables

En **C**, si olvidas comprobar un valor de retorno, el programa continúa con datos corruptos, lo que provoca *bugs* silenciosos y difíciles de encontrar.

* **Ejemplo:** Si `raiz(-1)` devuelve `-1.0` y tú usas ese valor para calcular una trayectoria, el programa no falla, pero el resultado será basura.

En **Java**, si no capturas la excepción, el programa explota (termina la ejecución). **Es mejor un fallo ruidoso y temprano que un error silencioso y tardío.** Esto garantiza que nunca procesarás datos inválidos.

### 3. Contexto Completo (Stack Trace)

Cuando una excepción se propaga en Java, va recolectando información de todas las funciones por las que pasa.

* En **C**, cuando recibes un `-1`, solo sabes que algo falló, pero no sabes exactamente qué camino tomó el programa para llegar ahí.
* En **Java**, el objeto excepción contiene el **Stack Trace** (la traza de la pila), que te dice: "El error ocurrió en la línea 5 de `raiz`, llamada por la línea 20 de `operacion`, llamada por la línea 45 de `main`".

### 4. Gestión Centralizada

La propagación permite que un solo bloque `catch` en una función de alto nivel gestione errores ocurridos en cientos de funciones de bajo nivel. Esto es mucho más eficiente que poner miles de `if` repartidos por todo el proyecto.

| Característica | Gestión en C | Propagación en Java |
| --- | --- | --- |
| **Visibilidad** | Fácil de ignorar. | Imposible de ignorar. |
| **Código** | Lleno de condicionales de control. | Limpio, solo lógica de negocio. |
| **Información** | Solo un código numérico. | Objeto con mensaje y traza completa. |
| **Responsabilidad** | Cada función debe "pasar la bola". | La burbuja de error sube sola. |

¿Te gustaría que viéramos cómo crear tu propia clase de excepción (por ejemplo, `NegativeNumberException`) para que el error sea aún más descriptivo?


##### 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En la orientación a objetos, y específicamente en Java, **las excepciones son siempre objetos**. No son simples números ni cadenas de texto; son instancias de clases que heredan de una jerarquía común (siendo `Throwable` la raíz de todas ellas).

### Ventajas de que sean objetos (Encapsulación)

Que una excepción sea un objeto cambia por completo las reglas del juego en comparación con los códigos de error de C:

* **Estado interno (Atributos):** Una excepción puede guardar mucha más información que un simple mensaje. Puede almacenar el valor que causó el error, la hora en que ocurrió, el ID del usuario que realizó la acción o incluso una referencia a otra excepción que ocurrió antes (causa raíz).
* **Comportamiento (Métodos):** Al ser objetos, tienen métodos. Puedes llamar a `e.printStackTrace()` para ver la ruta del error, `e.getMessage()` para obtener una descripción, o métodos personalizados que tú definas para, por ejemplo, traducir el error a otro idioma o enviarlo a una base de datos de logs.
* **Jerarquía y Tipado:** Puedes agrupar errores. Por ejemplo, `FileNotFoundException` es un tipo de `IOException`. Esto permite capturar errores de forma específica o general según te interese, aplicando polimorfismo en los bloques `catch`.

---

### Excepciones personalizadas

**Sí, podemos y debemos crear excepciones personalizadas** cuando las que vienen por defecto en Java (como `IllegalArgumentException` o `NullPointerException`) no describen con suficiente precisión el problema de nuestro dominio de negocio.

Para crear una, simplemente tienes que crear una clase que herede de `Exception` (si quieres que sea obligatoria de capturar) o de `RuntimeException` (si quieres que sea opcional).

#### Ejemplo para tus apuntes:

Imagina que en tu clase `Punto` quieres controlar específicamente cuando una coordenada se sale de un rango permitido.

```java
// Definición de la excepción personalizada
public class CoordenadaFueraDeRangoException extends RuntimeException {
    private int valorErroneo;

    public CoordenadaFueraDeRangoException(int valor) {
        // Pasamos un mensaje a la clase padre
        super("La coordenada " + valor + " está fuera del límite permitido (0-1000)");
        this.valorErroneo = valor;
    }

    public int getValorErroneo() {
        return valorErroneo;
    }
}

```

---

### ¿Cómo se usa esto en tu lógica?

Al lanzar esta excepción, estás encapsulando el dato exacto del error. Quien capture la excepción podrá saber qué valor falló exactamente:

```java
public void setX(int x) {
    if (x < 0 || x > 1000) {
        throw new CoordenadaFueraDeRangoException(x);
    }
    this.coordenadas[0] = x;
}

// En el main...
try {
    punto.setX(-50);
} catch (CoordenadaFueraDeRangoException e) {
    System.out.println("Error: El valor " + e.getValorErroneo() + " no es válido.");
}

```

**Resumen:**

1. **Son objetos:** Tienen datos y métodos.
2. **Encapsulación:** El error "sabe" explicar qué le ha pasado.
3. **Personalización:** Puedes extender la jerarquía de Java para adaptarla a tu programa.

¿Te gustaría que explicáramos ahora la diferencia fundamental entre `Exception` y `RuntimeException` (excepciones comprobadas vs. no comprobadas)?


#### 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Cuando comparamos el valor de retorno de C (un simple número como `-1`) con un objeto excepción en Java, la diferencia en la cantidad de información es abismal. Un objeto excepción no solo dice "algo falló", sino que lleva consigo el "quién, dónde y por qué".

Cualquier objeto que herede de `Throwable` en Java transporta estas **tres piezas de información esencial**:

### 1. El Tipo de la Excepción (La Clase)

A diferencia de C, donde un `-1` puede significar "archivo no encontrado" o "memoria insuficiente" dependiendo de la función, en Java el propio nombre de la clase ya te da la semántica del error.

* **Ventaja:** Permite usar polimorfismo en los manejadores. Puedes tener un `catch` específico para `FileNotFoundException` y otro genérico para `IOException`. Sabes exactamente *qué* tipo de regla se ha roto.

### 2. El Mensaje de Detalle (`String message`)

Es una cadena de texto que se define en el momento de lanzar la excepción (mediante el constructor).

* **Contenido:** Suele incluir datos dinámicos del error, como el nombre del archivo que falta o el valor negativo que causó el fallo.
* **Ventaja:** Facilita enormemente la depuración sin tener que adivinar qué variable causó el problema. En C, tendrías que imprimir tú el error manualmente antes de retornar.

### 3. La Traza de la Pila (`Stack Trace`)

Esta es, posiblemente, la información más valiosa. Es una captura de todos los métodos que estaban activos en el momento del error.

* **Contenido:** Un listado ordenado que incluye el nombre del archivo fuente, el nombre del método y el **número de línea exacto** donde se lanzó la excepción, así como toda la cadena de llamadas que llevaron a ese punto.
* **Ventaja:** En C, si una función de utilidad falla, no sabes quién la llamó. En Java, el objeto excepción "recuerda" todo el camino recorrido.

---

### Comparativa de "encapsulación del error"

| Información | Ejemplo en C (Retorno) | Ejemplo en Java (Objeto Excepción) |
| --- | --- | --- |
| **Identificación** | Un entero (ej: `5`) | Una instancia de clase (ej: `IndexOutOfBoundsException`) |
| **Explicación** | Ninguna (hay que mirar un manual) | Mensaje descriptivo incrustado (ej: `"Index 12 out of bounds for length 10"`) |
| **Origen** | Desconocido (solo sabes que la función falló) | Registro completo de la pila (clase, método y línea exacta) |
| **Causa raíz** | Difícil de trazar | Puede contener una referencia a otra excepción previa (`cause`) |

### Resumen para tus apuntes

La gran ventaja de la **encapsulación** aquí es que el manejador (`catch`) recibe un paquete completo de forense digital. No solo se entera de que hubo un problema, sino que tiene todas las herramientas para reportarlo, registrarlo en un log o incluso intentar una recuperación basada en los datos específicos que viajan dentro del objeto.

¿Quieres que veamos cómo imprimir de forma limpia esta información o cómo añadir tus propios atributos extra a una excepción personalizada?


#### 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?
En Java, un solo bloque `try` puede estar seguido de **múltiples bloques `catch**`. Esta es una de las características más potentes para gestionar diferentes tipos de errores de forma específica.

Aquí tienes los detalles clave para tus apuntes:

### 1. ¿Cuántos bloques `catch` se pueden tener?

**Tantos como necesites.** No hay un límite teórico. Puedes definir un manejador para cada tipo de excepción que esperes que el código dentro del `try` pueda lanzar.

```java
try {
    // Código que puede lanzar varias excepciones
    int[] lista = {1, 2};
    int division = lista[5] / 0; 
} catch (ArithmeticException e) {
    System.out.println("Error: No puedes dividir por cero.");
} catch (IndexOutOfBoundsException e) {
    System.out.println("Error: Te has salido del array.");
} catch (Exception e) {
    System.out.println("Error genérico: Algo salió mal.");
}

```

### 2. ¿Cuántos bloques `catch` se ejecutan?

**Solo se ejecuta uno** (como máximo).

El funcionamiento es similar a un `if-else if-else`:

1. Cuando ocurre una excepción, Java busca de arriba abajo el primer bloque `catch` que sea compatible con el tipo de excepción lanzada.
2. En cuanto encuentra una coincidencia, ejecuta ese bloque y **ignora todos los demás**.
3. Una vez terminado ese bloque `catch`, el programa continúa su ejecución después del último `catch`.

### 3. Regla de oro: El orden importa

Debido a que solo se ejecuta el primero que coincida, debes poner siempre las excepciones **más específicas arriba** y las **más generales abajo**.

Si pones `catch (Exception e)` al principio, este capturará absolutamente todo (ya que todas las excepciones heredan de `Exception`), y los bloques de abajo quedarían "muertos", lo cual provocará un error de compilación en Java (*unreachable code*).

---

### El "Multi-catch" (Bonus de Java moderno)

Si quieres realizar la misma acción para dos tipos de errores distintos, desde Java 7 puedes agruparlos en un solo bloque usando el operador `|`:

```java
try {
    // Código...
} catch (ArithmeticException | IndexOutOfBoundsException e) {
    // Se ejecuta si ocurre cualquiera de las dos
    System.out.println("Error de cálculo o de índice.");
}

```

### Resumen para PROII

* **Cantidad:** Ilimitada.
* **Ejecución:** Exclusiva (solo uno).
* **Orden:** De la clase "hija" (específica) a la clase "padre" (general).

¿Te gustaría que viéramos para qué sirve el bloque **`finally`**, que suele ir acompañando a estos `catch`?




#### 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Para resolver este problema, Java proporciona el bloque **`finally`**. Su característica fundamental es que **se ejecuta siempre**, ocurra una excepción o no, y tanto si la excepción se captura como si se propaga hacia arriba.

Es el lugar sagrado para el código de "limpieza" (cerrar sockets, ficheros o liberar memoria de estructuras externas), garantizando que no dejemos recursos abiertos que puedan colapsar el sistema.

### 1. Ejemplo con `try-catch-finally`

Este es el caso donde queremos gestionar el error y, además, limpiar recursos.

```java
public void procesarFichero() {
    System.out.println("Abriendo fichero...");
    try {
        // Simulamos un error tras abrir el recurso
        int resultado = 10 / 0; 
        System.out.println("Este mensaje nunca se verá.");
    } catch (ArithmeticException e) {
        System.out.println("Error capturado: No se puede dividir por cero.");
    } finally {
        // Este código se ejecuta SIEMPRE
        System.out.println("Cerrando fichero y liberando recursos... (Limpieza completada)");
    }
    System.out.println("El programa sigue tras el bloque try-catch-finally.");
}

```

### 2. Ejemplo con `try-finally` (Sin `catch`)

Este escenario es muy común cuando una función **no sabe cómo arreglar el error** y prefiere que se propague al llamador, pero tiene la responsabilidad de cerrar lo que abrió antes de "morir".

```java
public void metodoQuePropaga() {
    try {
        System.out.println("Iniciando operación crítica...");
        throw new RuntimeException("Error fatal"); // Lanzamos y NO capturamos
    } finally {
        // Antes de que la excepción salga de este método hacia el main,
        // Java ejecuta obligatoriamente este bloque.
        System.out.println("Limpieza de emergencia antes de propagar el error.");
    }
    // Esta línea NUNCA se ejecutará porque la excepción sigue su camino
    System.out.println("Este código es inalcanzable.");
}

```

---

### Puntos clave para tus apuntes:

* **Garantía de ejecución:** El bloque `finally` se ejecuta incluso si hay un `return` dentro del `try` o del `catch`. Java interrumpirá el `return`, ejecutará el `finally` y luego terminará de salir.
* **Propagación segura:** En el ejemplo sin `catch`, la excepción queda en "pausa". Se ejecuta el `finally` y, una vez terminado, la excepción reanuda su viaje por la pila de llamadas.
* **Uso moderno (Try-with-resources):** Aunque `finally` es la base, en Java moderno (desde la versión 7) existe una sintaxis mejorada para cerrar recursos automáticamente llamada `try-with-resources`, que evita tener que escribir el `close()` manualmente.

### ¿Qué ocurre si el propio bloque `finally` lanza otra excepción?

Esta es una pregunta de nota: Si `finally` lanza una excepción, esta **sustituye** a cualquier excepción previa que se estuviera propagando. Por eso, dentro de un `finally` hay que tener mucho cuidado y, a veces, meter otro `try-catch` interno para las operaciones de cierre.

¿Te gustaría que viéramos cómo funciona ese `try-with-resources` que mencioné, que es la forma más profesional de gestionar ficheros hoy en día?


#### 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, el bloque `finally` es extremadamente flexible y tiene un comportamiento muy estricto que lo hace ideal para la gestión de recursos. Aquí tienes las respuestas para tus apuntes:

### 1. ¿Puede el bloque `finally` ir sin `catch`?

**Sí.** La estructura `try-finally` es perfectamente válida en Java.

Se utiliza cuando el método actual **no quiere (o no sabe) cómo manejar la excepción**, por lo que permite que esta se propague hacia el llamador, pero aun así necesita asegurar que ciertos recursos (como un fichero abierto o una conexión a base de datos) se cierren antes de que el método "muera".

```java
public void ejemploSinCatch() {
    try {
        // Código que puede fallar
        abrirRecursoCritico();
        hacerOperacionQueLanzaExcepcion();
    } finally {
        // Esto se ejecuta aunque la excepción salga disparada hacia fuera
        cerrarRecursoCritico();
    }
}

```

### 2. ¿Se ejecuta siempre, ocurra o no una excepción?

**Sí, siempre.** Es la garantía fundamental de Java.

* **Si todo va bien:** Se ejecuta el `try` y, al terminar, se ejecuta el `finally`.
* **Si hay una excepción y se captura:** Se ejecuta el `try` hasta el error, luego el `catch` y finalmente el `finally`.
* **Si hay una excepción y NO se captura:** Se ejecuta el `try` hasta el error y, justo antes de que la excepción se propague al método superior, se ejecuta el `finally`.

### 3. ¿Y si hay un `return` en medio del `try`?

Esta es una de las preguntas "trampa" más habituales. La respuesta es: **Sí, el `finally` se ejecuta incluso si hay un `return**`.

Cuando Java encuentra un `return` dentro de un bloque `try`, hace lo siguiente:

1. Calcula el valor que debe devolver (si lo hay).
2. **Pausa** el retorno.
3. Ejecuta el bloque `finally` por completo.
4. Finalmente, ejecuta el `return` que había quedado pendiente.

#### Ejemplo de la "trampa" del return:

```java
public int pruebaReturn() {
    try {
        return 10;
    } finally {
        System.out.println("Ejecutando finally antes del return...");
    }
}

```

Al llamar a este método, verás el mensaje en la consola y **luego** recibirás el valor `10`.

---

### Casos excepcionales (donde NO se ejecuta)

Existen situaciones extremas, ajenas a la lógica del lenguaje, donde el `finally` podría no ejecutarse:

1. Si el proceso del sistema operativo se mata bruscamente (`kill -9`).
2. Si se corta la corriente o hay un fallo de hardware.
3. Si llamas a `System.exit(0)`, que detiene la Máquina Virtual de Java (JVM) inmediatamente.

### Resumen para PROII

| Escenario | ¿Se ejecuta el `finally`? |
| --- | --- |
| **Flujo normal (sin errores)** | **SÍ** |
| **Excepción capturada** | **SÍ** |
| **Excepción propagada (sin catch)** | **SÍ** |
| **Sentencia `return` en el try** | **SÍ** |
| **Sentencia `break` o `continue**` | **SÍ** |



##### 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

Aquí tienes una versión limpia para apuntes:

---

## Excepciones en Java

### 1. Excepciones controladas (checked)

Son aquellas que el compilador obliga a manejar.
Es necesario capturarlas con `try-catch` o declararlas con `throws`.

Se utilizan cuando el error es previsible y el programa puede recuperarse.

Ejemplos típicos:

* `IOException`
* `SQLException`
* `FileNotFoundException`

Ejemplo:

```java
public void leerArchivo(String ruta) throws IOException {
    FileReader fr = new FileReader(ruta);
}
```

---

### 2. Excepciones no controladas (unchecked)

No es obligatorio manejarlas.
Ocurren en tiempo de ejecución y suelen indicar errores de programación.

Ejemplos típicos:

* `NullPointerException`
* `ArrayIndexOutOfBoundsException`
* `IllegalArgumentException`

Ejemplo:

```java
String texto = null;
texto.length(); // NullPointerException
```

---

### 3. Papel de RuntimeException

`RuntimeException` es la clase base de las excepciones no controladas.

* Si una excepción hereda de `Exception` → es controlada
* Si hereda de `RuntimeException` → es no controlada

Ejemplo:

```java
// No controlada
class MiErrorRuntime extends RuntimeException {
    public MiErrorRuntime(String mensaje) {
        super(mensaje);
    }
}

// Controlada
class MiErrorChecked extends Exception {
    public MiErrorChecked(String mensaje) {
        super(mensaje);
    }
}
```

---

### 4. Cuándo usar cada tipo

#### Usar excepciones controladas cuando:

* Se trabaja con archivos
* Se accede a bases de datos
* Se hacen llamadas a servicios externos
* El error es esperado y se puede gestionar

#### Usar excepciones no controladas cuando:

* Hay errores de programación (null, índices, etc.)
* Los parámetros son inválidos
* El estado del programa es incorrecto
* No tiene sentido forzar al llamador a manejar el error

---

### 5. Resumen

* Checked: obligatorias, errores recuperables
* Unchecked: opcionales, errores de lógica o programación



##### 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

## 12. ¿Qué es y para qué se usa `throws`?

`throws` es una palabra clave que se usa en la **firma de un método** para indicar que ese método puede lanzar una o varias excepciones.

No maneja la excepción, solo **la declara y la propaga** hacia quien llame al método.

Ejemplo:

```java
public void leerArchivo(String ruta) throws IOException {
    FileReader fr = new FileReader(ruta);
}
```

Aquí el método no captura la excepción, sino que avisa:
“oye, si llamas a esto, tú te encargas del problema”.

---

## ¿Para qué se usa?

Se usa para:

* Indicar claramente qué errores pueden ocurrir
* Delegar la responsabilidad al método que llama
* Evitar capturar excepciones en niveles donde no tiene sentido hacerlo

---

## ¿Por qué es alternativa a capturar una excepción controlada?

Porque en Java, las excepciones controladas tienen dos opciones obligatorias:

1. Capturarlas con `try-catch`
2. Declararlas con `throws`

Ejemplo comparando ambas:

### Opción 1: capturar

```java
public void leerArchivo(String ruta) {
    try {
        FileReader fr = new FileReader(ruta);
    } catch (IOException e) {
        System.out.println("Error al leer archivo");
    }
}
```

### Opción 2: propagar con throws

```java
public void leerArchivo(String ruta) throws IOException {
    FileReader fr = new FileReader(ruta);
}
```

---

## Idea clave

* `try-catch` → manejas el error aquí
* `throws` → pasas el problema hacia arriba

---

## Cuándo usar `throws`

* Cuando el método no sabe cómo resolver el error
* Cuando quieres que otro nivel (por ejemplo, el `main` o una capa superior) decida qué hacer
* Cuando estás diseñando APIs y quieres obligar a quien use tu método a tener en cuenta el error

---





#### 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

Aquí tienes un ejemplo claro y típico para apuntes:

## Ejemplo de método con `throws` y `finally`

Este método intenta abrir un fichero, pero **no maneja la excepción** (`FileNotFoundException`), sino que la **propaga hacia arriba** con `throws`.

```java
import java.io.File;
import java.io.FileReader;
import java.io.FileNotFoundException;

public void abrirFichero(String ruta) throws FileNotFoundException {
    FileReader fr = null;

    try {
        fr = new FileReader(new File(ruta));
        System.out.println("Fichero abierto correctamente");
    } finally {
        System.out.println("Bloque finally ejecutado");
        
        // Cierre del recurso si se llegó a abrir
        if (fr != null) {
            try {
                fr.close();
            } catch (Exception e) {
                // Ignorado o tratado de forma simple
            }
        }
    }
}
```

---

## Idea clave del ejemplo

* `throws FileNotFoundException` → el método **no gestiona el error**, lo delega
* `try` → intenta abrir el fichero
* `finally` → se ejecuta siempre, haya error o no
* El `finally` se usa para **liberar recursos** (cerrar el fichero)

---

## Importante

Aunque la excepción se propague:

* El `finally` **siempre se ejecuta**
* Sirve para evitar fugas de recursos (archivos abiertos, conexiones, etc.)

---

Si quisieras manejar la excepción, usarías `catch`, pero aquí justamente se evita para dejar que otro método decida qué hacer.



#### 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

## 14. Excepciones no controladas y `throws`

### ¿Se pueden poner excepciones no controladas en `throws`?

Sí, se puede. No hay ninguna restricción.

Ejemplo:

```java
public void metodo(int x) throws IllegalArgumentException {
    if (x < 0) {
        throw new IllegalArgumentException("Valor negativo");
    }
}
```

Pero **no es obligatorio** hacerlo, porque las excepciones no controladas (las que heredan de `RuntimeException`) no requieren ser declaradas.

---

### ¿Debe el método llamador usar `try-catch`?

No es obligatorio.

El llamador puede:

* Capturar la excepción con `try-catch`
* O no hacer nada y dejar que el programa falle

Ejemplo sin capturar:

```java
metodo(-1); // Puede lanzar IllegalArgumentException
```

Ejemplo capturando:

```java
try {
    metodo(-1);
} catch (IllegalArgumentException e) {
    System.out.println("Error en el argumento");
}
```

---

### ¿Qué sentido tiene usar `throws` con excepciones no controladas?

Principalmente tiene un valor **documentativo**, no funcional.

Sirve para:

* Indicar claramente qué errores puede lanzar el método
* Mejorar la legibilidad del código
* Ayudar a quien usa el método a entender posibles fallos

Pero:

* No obliga a nadie a capturarlas
* No cambia el comportamiento del programa

---

### Idea clave

* Con excepciones **checked** → `throws` es obligatorio si no capturas
* Con excepciones **unchecked** → `throws` es opcional y solo informativo

---

### Resumen

* Sí, puedes usar `throws` con `RuntimeException`
* No obliga al llamador a usar `try-catch`
* Su utilidad es principalmente aclarar el contrato del método



#### 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

## 15. ¿Cuándo usar excepciones controladas y no controladas?

### Uso de excepciones controladas (checked, como `IOException`)

Se recomiendan cuando:

* El error es **esperable**
* El error depende de factores externos (archivo, red, base de datos)
* El programa **puede recuperarse**
* Quieres **forzar** a quien llama al método a manejar el error

Ejemplos típicos:

* Leer un fichero que puede no existir
* Conexión a base de datos
* Llamadas a APIs externas

---

### Uso de excepciones no controladas (unchecked, como `IllegalArgumentException`)

Se recomiendan cuando:

* El error es un **fallo de programación**
* El problema no debería ocurrir si el código está bien hecho
* No tiene sentido obligar al llamador a manejarlo
* El error suele ser **irrecuperable o indica bug**

Ejemplos típicos:

* Pasar argumentos inválidos
* Acceder a índices fuera de rango
* Uso de `null` incorrecto

---

## Idea general

* **Checked** → errores externos y recuperables
* **Unchecked** → errores internos y de lógica

---

## ¿Existen en todos los lenguajes?

No.

Java es de los pocos lenguajes que tiene esta distinción **tan estricta a nivel de compilador**.

---

## ¿Qué pasa en lenguajes con una sola opción?

En la mayoría de lenguajes modernos (como C#, Python, JavaScript):

* Todas las excepciones son equivalentes a las **no controladas**
* No es obligatorio declararlas ni capturarlas

---

## ¿Cuál es la opción más habitual hoy en día?

La más habitual es el modelo de **excepciones no controladas**.

Motivos:

* Menos código repetitivo
* Más flexibilidad
* Menos “ruido” en las firmas de métodos

---

## Resumen

* Checked → errores esperables, obligan a manejar
* Unchecked → errores de programación, no obligan
* No todos los lenguajes tienen ambas
* Hoy en día predomina el modelo tipo `RuntimeException`



## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

## 16. Excepciones dentro de `catch`

### ¿Tiene sentido lanzar excepciones dentro de un `catch`?

Sí, tiene sentido.

Se hace cuando:

* Quieres **transformar** una excepción en otra más adecuada
* Quieres **añadir contexto** (mensaje más claro)
* Tu método no puede manejar realmente el error

Ejemplo (transformar excepción):

```java
import java.io.*;

public void procesarFichero(String ruta) throws MiExcepcion {
    try {
        FileReader fr = new FileReader(ruta);
    } catch (FileNotFoundException e) {
        throw new MiExcepcion("No se pudo procesar el fichero: " + ruta);
    }
}

class MiExcepcion extends Exception {
    public MiExcepcion(String mensaje) {
        super(mensaje);
    }
}
```

---

### ¿Se puede relanzar la misma excepción capturada?

Sí, se puede.

Simplemente se usa `throw e;`

Ejemplo:

```java
import java.io.*;

public void leer(String ruta) throws IOException {
    try {
        FileReader fr = new FileReader(ruta);
    } catch (IOException e) {
        System.out.println("Error al leer");
        throw e; // relanza la misma excepción
    }
}
```

---

### ¿Cuándo tiene sentido relanzar la misma excepción?

Se hace cuando:

* Quieres **registrar (log)** el error pero no manejarlo
* Necesitas ejecutar alguna acción (limpieza, métricas, etc.)
* No puedes resolver el problema en ese nivel

Ejemplo típico (log + relanzar):

```java
public void metodo() throws IOException {
    try {
        hacerAlgo();
    } catch (IOException e) {
        System.out.println("Log del error: " + e.getMessage());
        throw e; // se propaga hacia arriba
    }
}
```

---

### Idea importante

* Lanzar una nueva excepción → cambias el tipo o añades contexto
* Relanzar la misma → haces algo extra pero no la gestionas

---

### Buen detalle (recomendado)

Cuando creas una nueva excepción, es buena práctica **encadenar la original**:

```java
throw new MiExcepcion("Error procesando fichero", e);
```

Así no pierdes la información original del error.

---

## Resumen

* Sí, puedes lanzar excepciones en `catch`
* Sí, puedes relanzar la misma con `throw e`
* Se hace para añadir contexto, transformar errores o registrar información sin resolverlos



## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

## 17. Excepción como "causa" de otra

### ¿En qué consiste?

Que una excepción sea la **causa** de otra significa que:

* Una excepción original (de bajo nivel) provoca otra excepción (de más alto nivel)
* La nueva excepción **envuelve** a la anterior
* Se mantiene la información del error original

Esto se llama **encadenamiento de excepciones**.

---

### ¿Para qué sirve?

* No perder información del error original
* Abstraer detalles internos (por ejemplo, de ficheros o base de datos)
* Dar mensajes más claros a niveles superiores

---

### Ejemplo en Java

Capturamos una excepción de bajo nivel (`IOException`) y la encapsulamos en una excepción propia:

```java id="1t87x0"
import java.io.*;

public void procesar(String ruta) throws MiExcepcion {
    try {
        FileReader fr = new FileReader(ruta);
    } catch (IOException e) {
        throw new MiExcepcion("Error procesando el fichero", e);
    }
}

class MiExcepcion extends Exception {
    public MiExcepcion(String mensaje, Throwable causa) {
        super(mensaje, causa); // aquí se establece la causa
    }
}
```

---

### ¿Qué está pasando aquí?

* `IOException` → error real (bajo nivel)
* `MiExcepcion` → error más abstracto (alto nivel)
* `e` se pasa como **causa** usando `super(mensaje, causa)`

---

### ¿Se ve la causa al mostrar la excepción?

Sí.

Cuando se imprime la excepción (por ejemplo, con `printStackTrace()`), Java muestra:

* La excepción principal
* Y debajo, la causa con el mensaje `Caused by`

Ejemplo de salida:

```
MiExcepcion: Error procesando el fichero
    at ...
Caused by: java.io.FileNotFoundException: fichero.txt (No such file)
    at ...
```

---

### Idea clave

* La “causa” es la excepción original
* Permite mantener trazabilidad del error
* Se usa pasando la excepción original al constructor

---

### Resumen

* Una excepción puede envolver a otra como causa
* Se usa `super(mensaje, causa)`
* La causa sí aparece al imprimir el error
* Es una práctica recomendable para no perder información

