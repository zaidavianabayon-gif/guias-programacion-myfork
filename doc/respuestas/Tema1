<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Para comprender las cuatro características fundamentales de la programación orientada a objetos (POO) desde una base en C, es útil verlas como una evolución de las estructuras de datos y la organización del código que ya conoces.

Aquí se describen los pilares básicos:

---

### 1. Abstracción

La abstracción consiste en identificar las características y comportamientos esenciales de un concepto del mundo real, descartando los detalles que no son relevantes para el programa. En C, esto se asemeja a definir un `struct` donde solo se incluyen los campos necesarios. Sin embargo, en la POO, la abstracción va más allá al definir no solo qué datos tiene el objeto, sino qué puede hacer a través de una interfaz simplificada.

Este proceso permite manejar la complejidad enfocándose en el "qué hace" el objeto en lugar de "cómo lo hace". Por ejemplo, al modelar un "Usuario" en un sistema, la abstracción permite definir atributos como el nombre y el correo, ignorando detalles irrelevantes como su estatura o color de ojos, creando así un modelo mental claro y funcional.

### 2. Encapsulamiento

El encapsulamiento es el mecanismo que agrupa los datos (atributos) y el código que los manipula (métodos) en una sola unidad: la clase. A diferencia de C, donde cualquier función puede modificar una estructura si tiene acceso a ella, la POO permite "ocultar" los datos internos. Esto se logra mediante niveles de acceso, asegurando que el estado de un objeto solo pueda ser alterado de manera controlada a través de sus propios métodos.

Esta característica protege la integridad de la información y evita que cambios accidentales en una parte del programa afecten al resto. Se dice que el objeto actúa como una "caja negra" donde el mundo exterior interactúa con él mediante una interfaz pública, pero desconoce los detalles internos de su implementación.

### 3. Herencia

La herencia permite crear nuevas clases a partir de clases ya existentes. La nueva clase (clase hija) hereda los atributos y métodos de la clase original (clase padre), pudiendo añadir nuevas funcionalidades o modificar las existentes. Es un mecanismo fundamental para la reutilización de código, evitando tener que reescribir estructuras comunes.

En C, si se necesitara una versión extendida de un `struct`, se tendría que crear uno nuevo desde cero o anidar estructuras. Con la herencia, se establece una relación jerárquica de tipo "es un". Por ejemplo, si se tiene una clase `Vehiculo`, una clase `Coche` puede heredar de ella, obteniendo automáticamente características como la `velocidad` sin necesidad de definirlas nuevamente.

### 4. Polimorfismo

El polimorfismo es la capacidad que permite que diferentes clases respondan de forma distinta a un mismo mensaje o llamada a un método. En términos prácticos, significa que una sola interfaz puede utilizarse para una familia de acciones relacionadas. Es una de las características más potentes, ya que permite tratar a objetos de diferentes tipos de manera uniforme si comparten un ancestro común.

Por ejemplo, si existe un método llamado `dibujar()`, una clase `Circulo` y una clase `Cuadrado` lo ejecutarán de maneras totalmente distintas. En C, esto requeriría múltiples condicionales (`if` o `switch`) para verificar el tipo de dato antes de llamar a la función correcta; en Java, el sistema decide automáticamente qué método ejecutar según el tipo de objeto en tiempo de ejecución.

---




## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos


### 1. Java

Es el lenguaje por excelencia para aprender POO, ya que fue diseñado casi exclusivamente en torno a este paradigma. En Java, prácticamente todo debe estar dentro de una clase. Es multiplataforma gracias a su Máquina Virtual (JVM) y es el estándar en el desarrollo de aplicaciones empresariales y de backend a gran escala.

### 2. C++

Es una extensión de C (el lenguaje que ya conoces) que añade la capacidad de trabajar con objetos. A diferencia de Java, C++ permite un control de bajo nivel muy similar a C, lo que lo hace ideal para sistemas donde el rendimiento es crítico, como motores de videojuegos (Unreal Engine), navegadores web o sistemas operativos.

### 3. Python

Es un lenguaje de alto nivel muy popular por su sintaxis limpia y fácil de leer. Aunque permite programar de forma imperativa (como en C), es un lenguaje orientado a objetos desde su base. Es el lenguaje líder actualmente en Inteligencia Artificial, Ciencia de Datos y scripting debido a su gran versatilidad y comunidad.

### 4. C# (C-Sharp)

Desarrollado por Microsoft, es la evolución "orientada a objetos" dentro del ecosistema Windows, aunque ahora es multiplataforma. Es muy similar a Java en sintaxis y estructura. Se utiliza masivamente para el desarrollo de aplicaciones de escritorio, servicios en la nube y es el lenguaje principal para el desarrollo de videojuegos con el motor Unity.

---

### Resumen comparativo

| Lenguaje | Uso Principal | Relación con C |
| --- | --- | --- |
| **Java** | Apps móviles (Android) y Backend | Sintaxis parecida, pero sin punteros directos. |
| **C++** | Videojuegos y Sistemas | Es un superconjunto de C; tu código de C funciona aquí. |
| **Python** | IA y Automatización | Sintaxis muy distinta y más simplificada. |
| **C#** | Apps Windows y Unity | Evolución que mezcla conceptos de C++ y Java. |

---



## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Para alguien que ya tiene experiencia con **C**, estos conceptos resultarán muy familiares, ya que C es el lenguaje referente tanto para la programación estructurada como para la modular.

Aquí tienes la explicación detallada para tus apuntes:

---

### 1. Programación Estructurada

La programación estructurada es un paradigma que se basa en dividir la lógica del programa en estructuras de control claras y jerárquicas, eliminando el uso de saltos desordenados (como el famoso `goto`). Se fundamenta en el teorema de la estructura, el cual establece que cualquier programa puede escribirse utilizando solo tres estructuras: **secuencia** (ejecución línea a línea), **selección** (`if`, `switch`) e **iteración** (`for`, `while`).

El objetivo principal de este enfoque es mejorar la claridad y la calidad del código. Al utilizar estas estructuras, el flujo del programa se vuelve predecible y fácil de seguir de arriba hacia abajo. En comparación con los lenguajes antiguos que dependían de saltos incondicionales, la programación estructurada facilita enormemente la depuración y el mantenimiento del software.

### 2. Programación Modular

La programación modular consiste en la técnica de dividir un programa en partes más pequeñas y manejables llamadas **módulos**. Cada módulo es una unidad autónoma que realiza una tarea específica y que puede comunicarse con otros módulos a través de interfaces bien definidas. En el contexto de C, esto se traduce habitualmente en la separación del código en diferentes archivos `.c` y sus correspondientes cabeceras `.h`.

Esta división permite aplicar el principio de "divide y vencerás". Cada módulo puede ser desarrollado, probado y mantenido de forma independiente sin afectar al resto del sistema. Además, fomenta la reutilización de código, ya que un módulo de "operaciones matemáticas" o de "manejo de archivos" puede ser importado y utilizado en múltiples proyectos diferentes sin necesidad de ser reescrito.

---

### Resumen de la evolución

| Paradigma | Unidad de organización | Ventaja principal |
| --- | --- | --- |
| **Estructurada** | Bloques de código (`if`, `while`) | Flujo lógico claro. |
| **Modular** | Archivos y funciones (`.h` / `.c`) | Reutilización y orden. |
| **Orientada a Objetos** | Clases y Objetos | Unión de datos y lógica. |


```




## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Para entender qué define a un objeto, el mejor truco es compararlo con una variable de tipo `struct` en C. Mientras que en C la estructura solo guarda datos, en la programación orientada a objetos (POO) el objeto es una entidad viva que se define por tres elementos inseparables:

---

### 1. Estado (Atributos)

El estado representa los datos o la información que el objeto almacena en un momento determinado. Se define a través de los **atributos** o variables de instancia. En tu experiencia con C, esto equivale exactamente a los campos de un `struct`. Por ejemplo, si tenemos un objeto `CuentaBancaria`, su estado podría estar definido por el `saldo` y el `titular`.

El estado puede cambiar a lo largo de la ejecución del programa (el saldo sube o baja), pero la estructura de qué datos puede guardar permanece fija según lo dictado por su clase. Es la parte pasiva del objeto, lo que el objeto **sabe** o **tiene**.

### 2. Comportamiento (Métodos)

El comportamiento define las operaciones que el objeto puede realizar o las reacciones ante mensajes externos. Se implementa mediante **métodos** (funciones que pertenecen a la clase). A diferencia de C, donde las funciones están separadas de los datos, aquí el objeto "sabe" cómo manipular su propio estado.

Siguiendo el ejemplo anterior, el objeto `CuentaBancaria` tendría comportamientos como `ingresarDinero()` o `retirarDinero()`. Esta es la parte activa del objeto, lo que el objeto **puede hacer**. El comportamiento suele estar vinculado al estado: no puedes retirar dinero si el estado del saldo es cero.

### 3. Identidad

La identidad es lo que permite distinguir a un objeto de otro, incluso si tienen exactamente el mismo estado (los mismos datos). En memoria, cada objeto tiene una dirección única. Dos objetos de la clase `Libro` pueden tener el mismo título y autor, pero son instancias diferentes que ocupan lugares distintos en la memoria RAM.

En C, la identidad sería equivalente a la dirección de memoria de una variable (el puntero). En Java o C++, gracias a la identidad, el sistema sabe que si modificas el objeto `A`, el objeto `B` no se verá afectado, aunque ambos empezaran con los mismos valores. Es lo que hace que cada objeto sea **único**.

---


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Esta es una de las dudas más comunes al empezar, ya que en el habla cotidiana a veces intercambiamos estos términos, pero en programación tienen roles muy distintos.

Aquí tienes la aclaración para tus apuntes:

---

## 1. ¿Qué es una Clase?

Una clase es una **entidad abstracta** que funciona como un plano, molde o plantilla. Define qué datos (atributos) y qué acciones (métodos) tendrán los elementos que se creen a partir de ella. No representa a un elemento concreto, sino que describe las reglas de "cómo debe ser" algo.

Por ejemplo, si se define la clase `Estudiante`, se establece que cualquier estudiante tendrá un `nombre` y una `nota`, y que podrá realizar la acción de `estudiar()`. La clase por sí sola no es un estudiante real, es solo la definición del concepto.

## 2. ¿Es lo mismo que un Objeto?

**No.** La diferencia es fundamental y reside en la existencia física en la memoria del ordenador. La clase es el diseño y el objeto es lo que se construye siguiendo ese diseño. Usando una analogía común: la clase es la receta de una tarta y el objeto es la tarta real que puedes comer.

Mientras la clase es única dentro de un programa, se pueden crear infinitos objetos basados en esa misma clase. Cada objeto tiene su propia copia de los datos definidos en la clase, lo que permite que el objeto `estudianteA` se llame "Ana" y el `estudianteB` se llame "Luis", aunque ambos compartan la misma estructura.

## 3. ¿Qué es una Instancia?

En el contexto de la programación orientada a objetos, **"instancia" y "objeto" son prácticamente sinónimos**. El término proviene de la idea de "instanciar" (crear un ejemplo concreto). Cuando se ejecuta la instrucción para crear un objeto a partir de una clase (como el `new` en Java), se dice que se está creando una "instancia de la clase".

Decir "el objeto `miCoche` es una instancia de la clase `Vehiculo`" es la forma técnicamente más precisa de describirlo. Es el momento en el que la abstracción de la clase se materializa en una dirección de memoria específica.

## 4. ¿Todos los lenguajes POO manejan el concepto de Clase?

Aunque la gran mayoría de los lenguajes populares (Java, C++, C#, Python) se basan en clases, **no todos los lenguajes orientados a objetos las utilizan**. Existe una variante llamada **Programación Orientada a Objetos basada en prototipos**.

En estos lenguajes (como JavaScript en su forma original), no hay clases que sirvan de molde. En su lugar, los objetos se crean clonando otros objetos ya existentes (prototipos) y añadiéndoles o modificándoles características sobre la marcha. Es un enfoque más flexible pero menos estructurado que el que estás estudiando ahora con Java.

---



## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Esta es una de las preguntas donde más se nota el cambio de "chip" de C a Java. En C, tú eras el dueño y señor de la memoria; en Java, hay un asistente (el Garbage Collector) que hace el trabajo sucio por ti.

Aquí tienes los detalles técnicos para tus apuntes:

---

## 1. ¿Dónde se almacenan en memoria los objetos?

En los lenguajes orientados a objetos como Java, la memoria se divide principalmente en dos zonas: el **Stack** (Pila) y el **Heap** (Montículo).

* **El Heap:** Es el lugar donde residen los **objetos** propiamente dichos. Es un área de memoria grande y dinámica. Cuando haces un `new Objeto()`, los datos del objeto se guardan aquí.
* **El Stack:** Aquí se guardan las **referencias** (que son como los punteros de C). Si escribes `Persona p = new Persona();`, la variable `p` vive en el Stack y contiene la dirección de memoria que apunta al objeto real que vive en el Heap.

## 2. ¿Es igual en todos los lenguajes?

**No.** La gestión varía según el lenguaje y su filosofía:

* **En C++:** Tienes la libertad de elegir. Puedes crear un objeto en el Stack (se destruye automáticamente al salir de la función) o en el Heap usando `new` (debes destruirlo manualmente con `delete`).
* **En Java/C#:** Casi todos los objetos van obligatoriamente al Heap. No tienes que preocuparte por "liberar" la memoria manualmente, ya que el programador no tiene acceso a un comando tipo `free()` o `delete`.
* **En Python:** También utiliza el Heap de forma automática para prácticamente todo, gestionando las referencias de manera interna.

## 3. ¿Qué es la Recolección de Basura (Garbage Collection)?

La **recolección de basura** es un proceso automático de gestión de memoria. Su función es identificar qué objetos en el Heap ya no están siendo utilizados por el programa (es decir, ninguna variable en el Stack apunta a ellos) y liberar ese espacio para que pueda ser reutilizado.

En C, si olvidabas usar `free()`, tenías una "fuga de memoria" (*memory leak*). En Java, el **Garbage Collector (GC)** pasa periódicamente, detecta los objetos "huérfanos" y los elimina por ti. Esto evita muchos errores graves de programación, aunque a cambio consume un poco de recursos del sistema para realizar esa vigilancia.

---

### Comparativa para un programador de C

| Concepto | En C (Manual) | En Java (Automático) |
| --- | --- | --- |
| **Creación** | `malloc()` | `new` |
| **Liberación** | `free()` | **Garbage Collector** |
| **Riesgo** | Olvidar liberar memoria. | Casi nulo (el GC se encarga). |
| **Control** | Total sobre el hardware. | Gestionado por la Máquina Virtual. |

---



## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Para completar tu cuestionario, vamos a definir el concepto de "comportamiento" desde la óptica de Java, comparándolo con lo que ya conoces de C.

---

### 1. ¿Qué es un método?

En la programación orientada a objetos, un **método** es un bloque de código que define una acción o comportamiento que un objeto puede realizar. Si lo comparamos con C, un método es básicamente una **función**, con una diferencia clave: el método está "atado" a una clase y tiene acceso automático a los datos (atributos) del objeto que lo invoca.

En C, si querías modificar un `struct`, tenías que pasarle el puntero a la función: `actualizar_saldo(&miCuenta, 100)`. En Java, el método vive dentro de la cuenta, por lo que simplemente invocas `miCuenta.actualizarSaldo(100)`. Esto permite que el código esté mucho más organizado, ya que la lógica y los datos viajan siempre juntos.

### 2. ¿Qué es la sobrecarga de métodos?

La **sobrecarga** es la capacidad de definir en una misma clase varios métodos con el **mismo nombre**, pero con **diferentes parámetros** (ya sea en cantidad o en tipo de datos). Es una forma de polimorfismo que permite realizar una acción similar de distintas maneras sin tener que inventar nombres nuevos para cada función.

Por ejemplo, podrías tener un método llamado `dibujar()` que no reciba nada, otro `dibujar(String color)` y otro `dibujar(int coordenadasX, int coordenadasY)`. El compilador de Java es lo suficientemente inteligente como para saber cuál de ellos ejecutar basándose en los argumentos que le pases al llamarlo.

### 3. Diferencia con C

En C estándar, la sobrecarga **no existe**. Si intentas definir dos funciones llamadas `sumar`, el compilador te dará un error de "redefinición". Tendrías que llamarlas `sumarEnteros(int a, int b)` y `sumarFlotantes(float a, float b)`. La sobrecarga de Java limpia el código permitiendo que ambas se llamen simplemente `sumar`, delegando la responsabilidad de elegir la correcta al lenguaje.

---


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Para completar tu cuestionario con un ejemplo práctico, aquí tienes la implementación de la clase `Punto`.

Como vienes de C, verás que la estructura es muy similar a definir un `struct` y una función asociada, pero todo encapsulado dentro de un mismo bloque de código. Para calcular la distancia, se utiliza el teorema de Pitágoras: .

---

### 1. Definición de la Clase `Punto`

En Java, utilizamos la librería `Math` para las operaciones matemáticas como la raíz cuadrada (`sqrt`) y la potencia (`pow`).

```java
// Archivo: Punto.java
class Punto {
    // Atributos con visibilidad por defecto (package-private)
    int x;
    int y;

    // Método para calcular la distancia al origen (0,0)
    double calculaDistanciaAOrigen() {
        // La fórmula es raíz cuadrada de (x^2 + y^2)
        return Math.sqrt(Math.pow(x, 2) + Math.pow(y, 2));
    }
}

```

### 2. Ejemplo de uso (Instanciación)

Para usar la clase, necesitamos otra clase (normalmente llamada `Main`) con el método `public static void main`, que es el punto de entrada al igual que la función `main` en C.

```java
// Archivo: Principal.java
public class Principal {
    public static void main(String[] args) {
        // 1. Creación de la instancia (Objeto)
        Punto miPunto = new Punto();

        // 2. Asignación de valores a los atributos
        miPunto.x = 3;
        miPunto.y = 4;

        // 3. Uso del método
        double distancia = miPunto.calculaDistanciaAOrigen();

        // 4. Mostrar resultado
        System.out.println("La distancia al origen es: " + distancia);
    }
}

```

---

### Análisis para tus apuntes

* **Atributos:** `x` e `y` definen el **estado** del objeto. Al no escribir `public` ni `private`, tienen visibilidad por defecto, lo que significa que son accesibles desde cualquier clase en el mismo paquete.
* **Método:** `calculaDistanciaAOrigen` define el **comportamiento**. Nota que el método no necesita recibir `x` e `y` como parámetros (a diferencia de C), porque ya "viven" dentro del mismo objeto.
* **Instanciación:** La palabra clave `new` es fundamental. Es la que reserva espacio en el **Heap** para el objeto. Sin el `new`, solo tendríamos una referencia vacía (similar a un puntero `NULL` en C).

---


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Para cerrar tu cuestionario, vamos a analizar la estructura técnica del inicio de cualquier programa en Java y una de sus palabras reservadas más potentes: `static`.

---

## 1. El punto de entrada: El método `main`

Al igual que en C, donde todo comienza en la función `int main()`, en Java el punto de entrada es el método `main`. Sin embargo, en Java debe seguir una firma (estructura) obligatoria y exacta para que la Máquina Virtual (JVM) pueda encontrarlo:

```java
public static void main(String[] args) {
    // El código empieza aquí
}

```

* **`public`**: Permite que la JVM acceda al método desde fuera de la clase.
* **`void`**: Indica que el método no devuelve ningún valor al finalizar.
* **`String[] args`**: Es un array de cadenas de texto que recibe argumentos desde la línea de comandos (equivalente a `char *argv[]` en C).

---

## 2. ¿Qué es `static` y para qué sirve?

La palabra `static` indica que un miembro (ya sea un método o un atributo) **pertenece a la clase en sí** y no a una instancia (objeto) concreta.

Para un programador de C, esto es fácil de entender: los elementos `static` funcionan de forma similar a las variables globales o funciones globales. No necesitas hacer un `new Clase()` para usarlos. En el caso del método `main`, debe ser `static` porque la JVM necesita ejecutarlo antes de que exista cualquier objeto en la memoria.

---

## 3. ¿Solo se emplea para el método `main`?

**No.** Se puede usar en dos casos principales:

1. **Atributos estáticos:** Son variables compartidas por todos los objetos de una clase. Si cambias el valor en un objeto, cambia para todos. Es útil para contadores (ej. saber cuántos objetos se han creado).
2. **Métodos estáticos:** Son funciones de utilidad que no necesitan datos de un objeto específico. Por ejemplo, `Math.sqrt()` es estático; no necesitas crear un objeto "Matemáticas" para calcular una raíz.

---

## 4. ¿Para qué se combina con `final`?

Cuando combinas `static` con `final`, estás creando una **constante de clase**.

* **`static`**: La variable es única para toda la clase (ahorra memoria).
* **`final`**: El valor no puede ser modificado después de su asignación inicial (equivalente al `const` de C o a un `#define`).

Un ejemplo clásico en Java es `Math.PI`. Está definido como `public static final double PI`. Es público para todos, pertenece a la clase y nadie puede cambiar el valor de .

```java
public class Config {
    // Una constante que no cambia y es compartida por todos
    public static final int VERSION = 1;
}

```

---



## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Para un programador de C, el proceso de ejecución de Java puede parecer un poco "indirecto". Mientras que en C compilas para obtener un binario que el procesador entiende directamente (`.exe` o `.out`), en Java compilas para un procesador imaginario.

Aquí tienes la explicación técnica para tus apuntes:

---

### 1. ¿Cómo compilar y ejecutar? (Los comandos)

Supongamos que tienes el archivo `Punto.java` que escribimos antes. Los pasos en la terminal son:

1. **Compilar:** Se usa el comando `javac` (Java Compiler).
```bash
javac Punto.java

```


*Si no hay errores, no verás ningún mensaje, pero aparecerá un nuevo archivo llamado `Punto.class`.*
2. **Ejecutar:** Se usa el comando `java` seguido del nombre de la clase (sin extensión).
```bash
java Principal

```


*Esto lanza la máquina virtual y ejecuta el método `main`.*

---

### 2. ¿Java es compilado? (La respuesta híbrida)

Java es un lenguaje **híbrido**: es tanto compilado como interpretado.

* **Es compilado** porque el código fuente (`.java`) debe pasar por un compilador (`javac`) antes de poder ejecutarse. Sin embargo, no se compila a código máquina real (binario de Windows o Linux).
* **Es interpretado** (en parte) porque el resultado de esa compilación es ejecutado por un software (la Máquina Virtual) que traduce las instrucciones al vuelo para el procesador real.

Esta es la razón por la que se dice: *"Escríbelo una vez, ejecútalo en cualquier lugar"*.

---

### 3. ¿Qué es el Byte-code y los ficheros `.class`?

El **Byte-code** es el lenguaje intermedio que genera Java. Es un conjunto de instrucciones optimizadas que no están diseñadas para un hardware específico, sino para ser ejecutadas por la Máquina Virtual.

Cuando compilas un archivo `.java`, el resultado es un archivo **`.class`**. Este fichero contiene el Byte-code. Si le pasaras tu archivo `.class` a un amigo que usa Mac y tú usas Windows, él podría ejecutarlo sin cambiar ni una sola línea, porque el Byte-code es universal.

---

### 4. ¿Qué es la Máquina Virtual de Java (JVM)?

La **JVM (Java Virtual Machine)** es el corazón de Java. Es un software que simula un ordenador real. Su trabajo es leer el Byte-code de los archivos `.class` y traducirlo a las instrucciones específicas del procesador de tu máquina (ya sea un Intel, un chip de Apple o un móvil).

Para un programador de C, la JVM es como una capa de protección y traducción. Gracias a ella, no tienes que preocuparte de si la arquitectura es de 32 o 64 bits, o de cómo se gestiona la memoria en cada sistema operativo; la JVM se encarga de que tu programa se comporte igual en todos lados.

---

### Resumen para tus apuntes

| Elemento | Función | Equivalente en C |
| --- | --- | --- |
| **`.java`** | Código fuente humano. | `.c` / `.cpp` |
| **`javac`** | Compilador. | `gcc` o `clang` |
| **`.class`** | Byte-code (intermedio). | No tiene (C va directo a binario). |
| **JVM** | Ejecutor / Traductor. | El propio Sistema Operativo. |

---



## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Para cerrar este bloque sobre la creación de objetos, es fundamental entender qué ocurre exactamente en la memoria cuando nace una instancia. En C, usabas `malloc` para reservar bytes; en Java, usamos `new` para invocar a un **constructor**.

---

### 1. ¿Qué es `new`?

La palabra reservada `new` es un operador cuya función principal es **solicitar memoria dinámica en el Heap**. Al ejecutar `new`, ocurren tres cosas de forma automática:

1. Se reserva el espacio necesario para almacenar los atributos del objeto.
2. Se invoca al **constructor** de la clase para inicializar esos atributos.
3. Se devuelve la dirección de memoria (referencia) donde se ha creado el objeto.

En tu experiencia con C, `new` hace el trabajo de `malloc` (reserva de memoria) y de una función de inicialización, todo en un solo paso y de forma mucho más segura.

### 2. ¿Qué es un constructor?

Un constructor es un **método especial** que se ejecuta única y exclusivamente en el momento en que se crea un objeto (al usar `new`). Tiene dos reglas de oro que lo distinguen de cualquier otro método:

* Se llama exactamente igual que la clase (respetando mayúsculas).
* No tiene tipo de retorno (ni siquiera `void`).

Su objetivo principal es dejar al objeto en un "estado válido". Si no defines uno, Java crea un "constructor por defecto" vacío, pero lo ideal es definirlo nosotros para obligar a que el objeto tenga datos desde el principio.

---

### 3. Ejemplo: Clase `Empleado` con Constructor

Aquí se muestra cómo se definiría la clase `Empleado` utilizando un constructor para inicializar el DNI, el nombre y los apellidos de una sola vez.

```java
class Empleado {
    // Atributos
    String dni;
    String nombre;
    String apellidos;

    // Constructor: Recibe los datos y los asigna a los atributos
    Empleado(String dniEntrada, String nombreEntrada, String apellidosEntrada) {
        dni = dniEntrada;
        nombre = nombreEntrada;
        apellidos = apellidosEntrada;
    }

    // Método para mostrar los datos (comportamiento)
    void mostrarDatos() {
        System.out.println("Empleado: " + nombre + " " + apellidos + " (DNI: " + dni + ")");
    }
}

```

### Ejemplo de uso (Instanciación)

Observa cómo ahora, al usar `new`, pasamos los datos directamente entre paréntesis:

```java
public class Main {
    public static void main(String[] args) {
        // Se crea el objeto pasando los valores al constructor
        Empleado emp1 = new Empleado("12345678X", "Juan", "García Pérez");
        
        // El objeto ya nace con sus datos rellenos
        emp1.mostrarDatos();
    }
}

```

---

### Análisis para tus apuntes

* **Inicialización:** En el ejemplo anterior de la clase `Punto`, creamos el objeto y luego asignamos `x` e `y` manualmente. Con el constructor de `Empleado`, el objeto es funcional desde la primera línea de código.
* **Seguridad:** El constructor asegura que no existan objetos "vacíos" o con datos basura en el sistema, algo que en C requería mucho cuidado manual.

---



## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### La referencia `this` es uno de los conceptos más elegantes de la POO, especialmente cuando vienes de un lenguaje como C donde a veces perdemos el rastro de qué estructura estamos modificando.

Aquí tienes la explicación para tus apuntes:

---

### 1. ¿Qué es la referencia `this`?

`this` es una **referencia automática** que apunta al objeto actual que está ejecutando un método o un constructor. Imagina que es un puntero oculto que Java pasa a cada método para decirle: "Oye, estos son *tus* datos, no los de otro objeto".

Su uso principal es eliminar la ambigüedad. Cuando el nombre de un parámetro de un método es igual al nombre de un atributo de la clase, usamos `this` para especificar que nos referimos al atributo de la instancia.

### 2. ¿Se llama igual en todos los lenguajes?

Aunque el concepto es universal en la POO, el nombre cambia según el lenguaje:

* **Java, C++, C#, JavaScript:** Se utiliza `this`.
* **Python:** Se utiliza `self` (y a diferencia de Java, en Python es obligatorio pasarlo como primer argumento en los métodos).
* **PHP:** Se utiliza `$this`.
* **Ruby:** Se utiliza el símbolo `@` antes del nombre de la variable o la palabra `self`.

---

### 3. Ejemplo de uso en la clase `Punto`

Es muy común usar `this` en el **constructor**. Esto permite usar nombres de parámetros claros (como `x` e `y`) sin que se confundan con los atributos de la clase.

```java
class Punto {
    int x; // Atributo de la clase
    int y; // Atributo de la clase

    // Constructor con parámetros que se llaman igual que los atributos
    Punto(int x, int y) {
        this.x = x; // "this.x" es el atributo, "x" es el parámetro que recibimos
        this.y = y; // "this.y" es el atributo, "y" es el parámetro que recibimos
    }

    void mostrarPosicion() {
        // Aquí this es opcional, pero ayuda a entender que hablamos del objeto
        System.out.println("Estoy en (" + this.x + ", " + this.y + ")");
    }
}

```

### 4. ¿Por qué es útil para un programador de C?

En C, si tenías una función para mover un punto, solías hacer algo como `mover(Punto *p, int x, int y)`. Dentro de la función, usabas `p->x = x`.
En Java, ese `p->` es exactamente lo que hace `this.`. Es la forma que tiene el lenguaje de saber a qué dirección de memoria (a qué objeto concreto) le debe asignar los valores.

---


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Para completar este ejercicio, es necesario aplicar la fórmula de la distancia entre dos puntos en un plano cartesiano: .

En este caso,  serán las coordenadas del objeto actual (`this`) y  serán las del punto pasado como parámetro.

---

### 1. Implementación del método `distanciaA`

Se debe añadir este método dentro de la clase `Punto`. Observa cómo se accede a los atributos del punto "ajeno" usando la notación de punto (`otro.x`), mientras que a los del propio se accede mediante `this.x`.

```java
class Punto {
    int x;
    int y;

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    // Nuevo método que recibe otro objeto de la misma clase
    double distanciaA(Punto otro) {
        // Cálculo de la diferencia en cada eje
        int dx = otro.x - this.x;
        int dy = otro.y - this.y;
        
        // Aplicación del teorema de Pitágoras
        return Math.sqrt(Math.pow(dx, 2) + Math.pow(dy, 2));
    }
}

```

### 2. Ejemplo de uso con dos instancias

Para probar este método, es necesario crear dos objetos distintos y llamar al método desde uno de ellos, pasando el otro como argumento.

```java
public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto(0, 0);
        Punto p2 = new Punto(3, 4);

        // Se calcula la distancia desde p1 hacia p2
        double distancia = p1.distanciaA(p2);

        System.out.println("La distancia entre los puntos es: " + distancia);
    }
}

```

---

### Análisis para los apuntes

* **Paso de objetos como parámetros:** En Java, al pasar un objeto a un método, lo que se recibe es una copia de la referencia (puntero). Esto permite que el método `distanciaA` entre en el objeto `otro` y lea sus valores `x` e `y`.
* **Interacción entre objetos:** Este ejemplo es fundamental para entender la POO, ya que muestra cómo dos instancias de la misma clase pueden interactuar entre sí. El objeto `this` es el "sujeto" que realiza la acción, y el objeto `otro` es el "complemento" necesario para el cálculo.
* **Limpieza de código:** Al usar `this.x` y `otro.x`, el código resulta autodocumentado y es imposible confundir qué coordenada pertenece a qué punto.

---



## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Esta es una de las preguntas "trampa" más famosas de Java, especialmente cuando vienes de C y estás acostumbrado a decidir manualmente entre pasar un valor o un puntero.

Aquí tienes la explicación técnica para tus apuntes:

---

### 1. El paso del objeto `Punto`

En Java, **técnicamente todo se pasa por copia**, pero lo que se copia es la **referencia** (el puntero).

Cuando pasas un objeto `Punto` a un método, no estás copiando todo el contenido del objeto, sino la dirección de memoria donde reside. Por tanto, si dentro del método modificas un atributo del punto (ej: `otro.x = 10;`), **sí afectará al objeto fuera del método**. Ambos (el objeto original y el parámetro del método) están apuntando al mismo sitio en el **Heap**.

### 2. El paso de un entero (`int`)

A diferencia de los objetos, los tipos primitivos como `int`, `double` o `boolean` se pasan por **copia de valor**.

Si pasas un `int` a un método y lo modificas dentro, **el cambio no afecta a la variable original**. Se crea una copia exacta en el **Stack** del método y, al terminar la función, esa copia desaparece. En C, esto es exactamente igual a pasar un `int` normal sin usar el operador `&`.

---

### 3. Comparativa técnica para tus apuntes

| Tipo de dato | Cómo se pasa | ¿Afecta al exterior? | Equivalente en C |
| --- | --- | --- | --- |
| **Objetos (Punto)** | Copia de la referencia | **SÍ** (si alteras sus atributos) | Pasar un puntero (`struct Punto *p`) |
| **Primitivos (int)** | Copia del valor | **NO** | Pasar un valor simple (`int x`) |

> **Nota importante:** Si dentro del método haces que el objeto apunte a otro sitio (ej: `otro = new Punto();`), eso **no** cambiará la referencia original fuera del método. Solo habrás cambiado hacia dónde apunta la "copia" de la referencia local.

---

### Ejemplo de código para demostrarlo

```java
void test(Punto p, int n) {
    p.x = 99; // Esto SÍ cambia el objeto original
    n = 99;   // Esto NO cambia la variable original
}

```

---


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### El método `toString()` es uno de los métodos más útiles de Java para la depuración (*debugging*), ya que permite transformar un objeto en una representación textual legible para los humanos.

---

### 1. ¿Qué es el método `toString()`?

En Java, todos los objetos heredan de una clase base llamada `Object`. Esta clase ya trae consigo un método `toString()`. Por defecto, si no lo modificas, este método devuelve una cadena poco útil que incluye el nombre de la clase y una dirección de memoria (ej: `Punto@1b6d3586`).

El objetivo de **sobrescribir** este método es que el objeto pueda "explicarse" a sí mismo. Cuando intentas imprimir un objeto con `System.out.println(miObjeto);`, Java busca automáticamente el método `toString()` de ese objeto para saber qué texto mostrar por pantalla.

### 2. ¿Existe en otros lenguajes?

Sí, el concepto de representar un objeto como cadena es universal, aunque cambia de nombre según el lenguaje:

* **Python:** Se utilizan los métodos especiales `__str__` (para usuarios) y `__repr__` (para desarrolladores).
* **JavaScript:** Se llama exactamente igual, `toString()`.
* **C#:** También se llama `ToString()` (con la primera letra en mayúscula).
* **C++:** No tiene un método predefinido en la clase, sino que se suele sobrecargar el operador de flujo `<<`.

---

### 3. Ejemplo de `toString()` en la clase `Punto`

Para implementarlo, debemos usar la anotación `@Override`, que le indica al compilador que estamos sustituyendo la versión aburrida por defecto de Java por nuestra propia versión personalizada.

```java
class Punto {
    int x;
    int y;

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    // Sobrescribimos el método toString
    @Override
    public String toString() {
        return "Punto[x=" + this.x + ", y=" + this.y + "]";
    }
}

// Ejemplo de uso
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(5, 10);
        
        // Al imprimir el objeto, se invoca automáticamente toString()
        System.out.println(p); 
        // Salida: Punto[x=5, y=10]
    }
}

```

---

### Análisis para tus apuntes

* **Utilidad:** Sin `toString()`, verías algo como `Punto@74a14482`. Con él, ves los valores reales de los atributos, lo cual es vital para saber si tu programa está funcionando bien sin tener que poner mil `println` para cada variable.
* **Concatenación:** Java usa `toString()` automáticamente siempre que concatenas un objeto con una cadena de texto (ej: `"La posición es: " + miPunto`).

---



## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Esta es la reflexión definitiva para alguien que domina **C** y está aterrizando en **Java**. La respuesta corta es: **sí, el `struct` es el antepasado directo de la clase**, pero le falta "autonomía".

Aquí tienes los puntos clave para tu reflexión en los apuntes:

---

### 1. La similitud: El contenedor de datos

Tanto el `struct` como la clase sirven para agrupar variables de diferentes tipos bajo un mismo nombre. Si en C definimos un `struct Punto { int x, y; }`, estamos creando un molde para datos, igual que hacemos en Java con una clase. En ambos casos, el objetivo es organizar la información para no tener variables sueltas por el programa.

### 2. ¿Qué le falta al `struct` para ser una clase?

Para que un `struct` de C se convierta en una clase de Java, le faltan principalmente tres cosas:

* **Métodos (Comportamiento):** Un `struct` solo guarda datos (es pasivo). Para modificarlo, necesitas una función externa. Una clase, en cambio, guarda los datos y **las funciones que los manipulan** (es activa). En Java, el objeto "sabe" cómo calcular su propia distancia o cómo imprimirse, porque el código vive dentro de él.
* **Encapsulamiento (Privacidad):** En C, si tienes acceso al `struct`, puedes cambiar cualquier campo directamente. En una clase, puedes poner "llaves" (`private`, `public`) para decidir quién puede ver o tocar qué cosa, protegiendo la integridad de los datos.
* **Herencia y Polimorfismo:** Un `struct` no puede "heredar" de otro. Si quieres un `struct` que sea como otro pero con un campo más, tienes que copiarlo o anidarlo. Las clases permiten crear jerarquías donde los hijos heredan automáticamente todo el código de sus padres.

### 3. ¿Cuándo las variables pasan a ser instancias?

En C, una variable de tipo `struct` es simplemente un bloque de memoria con datos. Para que pasen a ser **instancias**, necesitan tener una **identidad gestionada** y un **ciclo de vida**:

1. **Construcción automática:** Mientras que en C el `struct` puede nacer con datos basura si no los inicializas a mano, una instancia en Java nace a través de un **constructor** que garantiza un estado inicial válido.
2. **Autorreferencia (`this`):** La instancia tiene conciencia de sí misma. Sabe que es "ella" y no otra la que está ejecutando un método en ese momento.
3. **Gestión de memoria:** En C, tú liberas el `struct`. En Java, la instancia es un objeto vivo que el **Garbage Collector** vigila; cuando nadie la "mira" (referencia), ella misma se retira de la memoria.

---

### Resumen comparativo

| Característica | `struct` en C | Clase en Java |
| --- | --- | --- |
| **Contenido** | Solo variables (Atributos). | Variables + Funciones (Métodos). |
| **Relación** | Datos y funciones están separados. | Datos y funciones están unidos. |
| **Acceso** | Siempre público. | Controlado (`private`, `public`, etc.). |
| **Memoria** | Manual (`malloc`/`free`) o Stack. | Dinámica (`new`) y automática (GC). |

---



## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Esta es la mejor forma de entender la POO: dándote cuenta de que **no es magia**, sino una forma organizada de escribir lo que ya hacías en C.

Para emular la clase `Punto` en C, necesitamos dos cosas: una estructura para los datos y una función que reciba esa estructura.

---

### 1. Emulación en C: El "Punto" artesanal

En C, los datos y la lógica están separados, así que tenemos que pasar el "objeto" manualmente a la función.

```c
#include <stdio.h>
#include <math.h>

// 1. El "Estado" (Atributos)
struct Punto {
    int x;
    int y;
};

// 2. El "Comportamiento" (Método)
// Recibe un puntero a la estructura para actuar sobre ella
double calculaDistanciaAOrigen(struct Punto *self) {
    return sqrt(pow(self->x, 2) + pow(self->y, 2));
}

int main() {
    // 3. La "Instancia"
    struct Punto p1 = {3, 4};

    // Llamada manual pasando la dirección de la estructura
    double d = calculaDistanciaAOrigen(&p1);

    printf("Distancia: %f\n", d);
    return 0;
}

```

---

### 2. ¿Qué ha pasado con `this`?

En el código de arriba, he llamado al parámetro `self` (como en Python). Ese **`struct Punto *self`** es exactamente lo que Java hace por ti en secreto:

* **En C:** Tú tienes que declarar el puntero en la función y pasarlo explícitamente al llamarla: `calculaDistanciaAOrigen(&p1)`.
* **En Java:** El compilador inserta automáticamente ese "puntero" en todos los métodos. Cuando escribes `this.x`, Java simplemente está traduciendo eso al equivalente de `p->x` en C.

---

### 3. La gran diferencia: El "Azúcar Sintáctico"

La POO en Java nos da lo que llamamos **azúcar sintáctico**: una forma más bonita y menos propensa a errores de escribir lo mismo.

1. **Agrupación:** En C, la función `calculaDistanciaAOrigen` podría estar en cualquier parte del código. En Java, **está obligada** a vivir dentro de la clase `Punto`. No puedes llamarla sin un punto.
2. **Invocación:** En lugar de `funcion(puntero, datos)`, usamos `objeto.metodo(datos)`. Es un cambio de perspectiva: el objeto ya no es un trozo de carne que manipula un carnicero (función), sino un ser que sabe hacer cosas por sí mismo.
3. **Seguridad:** En C, podrías pasar un puntero `NULL` a la función por error. En Java, si intentas usar un método sobre una referencia nula, el sistema lanza una excepción controlada (`NullPointerException`).

---

### Resumen de la "Magia" revelada

| Concepto POO | Implementación "Real" en C |
| --- | --- |
| **Clase** | Un `struct` + un grupo de funciones relacionadas. |
| **Objeto** | Una variable de tipo `struct` en memoria. |
| **`this`** | Un parámetro oculto que es un puntero a la estructura (`struct T *`). |
| **Método** | Una función normal que siempre recibe ese puntero como primer argumento. |

---
