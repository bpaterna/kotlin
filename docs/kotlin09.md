# 9. Gestión de errores (Excepciones y recursos)


<span class="mi_h3">Revisiones</span>

| Revisión | Fecha      | Descripción                             |
|----------|------------|-----------------------------------------|
| 1.0      | 29-06-2026 | Adaptación de los materiales a markdown |



## 1. Introducción

Una **excepción** es un error que ocurre en tiempo de ejecución (mientras el programa se está ejecutando) e interrumpe el flujo normal de tu aplicación. Ejemplos típicos son intentar dividir un número entre cero (como repartir abono entre cero macetas) o intentar abrir un archivo de registros del invernadero que no existe en el disco.

La gestión de errores consiste en detectar, capturar y recuperarse de estos fallos para evitar que la aplicación se cuelgue. En Kotlin dispones de dos herramientas fundamentales para lograrlo.

---

## 2. Captura explícita de errores: `try-catch-finally`

Se utiliza cuando deseas ejecutar un bloque de código que es propenso a fallar y quieres definir de manera controlada qué debe hacer el programa si el error ocurre.

* **`try`:** Contiene el bloque de código que puede lanzar la excepción.
* **`catch`:** Captura el error y te permite manejarlo (mostrar un aviso, usar un valor por defecto, registrar el fallo, etc.). Puedes encadenar varios bloques `catch` para manejar diferentes tipos de errores.
* **`finally`:** Contiene código que **se ejecutará siempre**, tanto si ha ocurrido un error como si no. Es opcional y se suele usar para tareas de limpieza.

**Ejemplo 1: Dosificación de abono entre macetas.** Si intentamos dividir una dosis total de fertilizante entre cero macetas, el sistema lanzará un error matemático (`ArithmeticException`).

```kotlin
fun main() {
    val dosisTotalMl = 150
    val cantidadMacetas = 0 

    try {
        println("=== EJEMPLO 1: Dosificación de abono entre macetas ===")
        
        // Esta línea lanzará una excepción al dividir entre cero
        val dosisPorMaceta = dosisTotalMl / cantidadMacetas
        println("Cada maceta recibirá: $dosisPorMaceta ml de abono.")
        
    } catch (e: ArithmeticException) {
        // Capturamos el error específico y damos una solución alternativa segura
        println("No puedes dosificar abono si tienes 0 macetas.")
        
    } catch (e: Exception) {
        // Captura cualquier otro error genérico no previsto
        println("Ha ocurrido un error inesperado: ${e.message}")
        
    } finally {
        // Este bloque se ejecuta pase lo que pase
        println("Fin del chequeo de dosificación de nutrientes.")
    }
}
```

Salida por consola:

```text
=== EJEMPLO 1: Dosificación de abono entre macetas ===
No puedes dosificar abono si tienes 0 macetas.
Fin del chequeo de dosificación de nutrientes.
```


Si en el mismo ejemplo modificamos el valor de cantidadMacetas y le asignamos un 10:

```kotlin
    val cantidadMacetas = 10
```

Salida por consola:

```text
=== EJEMPLO 1: Dosificación de abono entre macetas ===
Cada maceta recibirá: 15 ml de abono.
Fin del chequeo de dosificación de nutrientes.
```


## 3. Gestión automática de recursos con `use`

Cuando trabajas con recursos externos del sistema operativo (como archivos de texto en el disco, sensores físicos o flujos de red), estás obligado a **abrirlos, utilizarlos y, al finalizar, cerrarlos obligatoriamente**. Si olvidas cerrarlos, esos archivos o recursos quedan bloqueados en memoria, provocando fugas de rendimiento.

En Java, esto se gestiona con el bloque *try-with-resources*. En Kotlin, dispones de una herramienta mucho más elegante llamada **`use`**.

**¿Cómo funciona `use`?**
Se puede aplicar sobre cualquier objeto que implemente la interfaz `Closeable` (como `BufferedReader` o `FileWriter`). El bloque `use` abre el recurso, ejecuta el código de su interior y, al terminar, **se encarga de cerrar el recurso automáticamente por ti, incluso si ocurre un error o una excepción dentro del bloque**.



**Ejemplo 2: Escribir el inventario del invernadero en un archivo.** Utilizaremos `FileWriter` para guardar de forma persistente un listado de plantas en un archivo llamado `inventario_plantas.txt`.

```kotlin
import java.io.FileWriter

fun main() {
    try {
        println("=== EJEMPLO 2: Escribir el inventario del invernadero en un archivo ===")
        // Abrimos el archivo de texto. Al usar '.use', el archivo se cerrará solo al finalizar.
        FileWriter("inventario_plantas.txt").use { escritor ->
            escritor.write("ID: 1 | Monstera deliciosa | Seccion Sombra\n")
            escritor.write("ID: 2 | Lavanda dentata    | Seccion Exterior\n")
            escritor.write("ID: 3 | Helecho real       | Seccion Humedad\n")
        }
        println("Inventario botánico escrito en disco correctamente.")
        
    } catch (e: Exception) {
        println("Error al intentar guardar el archivo: ${e.message}")
    }
}
```

Salida por consola:

```text
=== EJEMPLO 2: Escribir el inventario del invernadero en un archivo ===
Inventario botánico escrito en disco correctamente.
```


**Ejemplo 3: Leer el archivo de inventario de forma segura.** Ahora leeremos el archivo que acabamos de crear utilizando `BufferedReader`. Controlaremos de manera segura la posibilidad de que el archivo haya sido borrado o no exista.

```kotlin
import java.io.File

fun main() {
    val archivo = File("inventario_plantas.txt")

    try {
        // '.use' se asegura de liberar el lector y cerrar el archivo pase lo que pase
        archivo.bufferedReader().use { lector ->
            println("=== EJEMPLO 3: Leer el archivo de inventario de forma segura ===")
            var linea = lector.readLine()
            
            while (linea != null) {
                println(linea)
                linea = lector.readLine() // Leemos la siguiente línea
            }
        }
    } catch (e: Exception) {
        // Se ejecutará si el archivo no existe o se interrumpe la lectura física del disco
        println("Error al intentar leer el archivo de inventario: ${e.message}")
    }
}
```

Salida por consola de la lectura del archivo:

```text
=== EJEMPLO 3: Leer el archivo de inventario de forma segura ===
ID: 1 | Monstera deliciosa | Seccion Sombra
ID: 2 | Lavanda dentata    | Seccion Exterior
ID: 3 | Helecho real       | Seccion Humedad
```



---

por revisar


> 💡 **Nota de depuración: ¿Qué pasa si tu consola de IntelliJ oculta líneas?**
>
> Si al ejecutar este ejemplo con `cantidadMacetas = 0` notas que solo se imprime la línea del error y desaparecen tanto la cabecera del `try` como el bloque `finally`, no te preocupes, tu código está bien. Se debe a un comportamiento del IDE por dos posibles razones:
>
> 1. **Filtro de búsqueda activo:** Comprueba que no tengas escrita la palabra *"Error"* en la barra de búsqueda (icono de la lupa) de la consola de ejecución de IntelliJ. Si está puesta, el IDE ocultará automáticamente cualquier línea que no contenga esa palabra.
> 2. **Ocultación automática de Gradle:** Si tu proyecto usa Gradle, el compilador puede "inteligir" que al imprimir un texto que empieza por `"Error:"` ha ocurrido un fallo de sistema y agrupará (plegará) de forma automática todas las líneas exitosas anteriores para limpiar la pantalla. Puedes desplegarlas manualmente en la consola o evitar empezar tus mensajes de texto plano con la palabra exacta `"Error:"`.





Aquí tienes una propuesta de contenido estructurado en Markdown (`.md`) para colocar al final del **Bloque 11: Gestión de errores**, diseñado como un subapartado independiente.

Está redactado dirigiéndose al alumno de **tú** e incluye dos ejemplos sencillos y específicos para que ellos mismos fuercen este comportamiento en su IntelliJ y aprendan a identificarlo.

***

## 4. Nota técnica: Comportamiento y filtros de la consola de IntelliJ

Cuando programas y haces pruebas por consola, la forma en que el IDE (IntelliJ) y su motor de construcción (Gradle) interpretan tus textos puede alterar lo que ves en pantalla. Si utilizas palabras clave como `"Error:"` o `"Exception:"` en tus mensajes, es muy común que ocurran dos fenómenos visuales que conviene que conozcas para no pensar que tu código está fallando.

---

### 4.1. El Filtro de Búsqueda Activo (Ocultación de líneas)

La consola de ejecución de IntelliJ incluye una barra de búsqueda rápida (un icono de lupa). Si dejas cualquier texto escrito en ella, el IDE aplicará un filtro en tiempo real.

#### Ejemplo independiente para probar en clase:
Escribe y ejecuta este código sencillo:

```kotlin
fun main() {
    println("--- ESTADO DEL SENSOR ---")
    println("Sensor de humedad encharcado conectado.")
    println("Error: Conexión intermitente en el Sector Sombra.")
    println("Chequeo del sistema de riego finalizado.")
}
```

#### ¿Qué debes observar?
1. Ejecuta el programa normalmente. Verás las **4 líneas** en tu consola.
2. Ahora, busca la pequeña **lupa de búsqueda** en la esquina superior derecha del panel de la consola de IntelliJ y escribe la palabra **"Error"**.
3. Verás que, al instante, las líneas 1, 2 y 4 **desaparecen de la pantalla**, quedando únicamente visible la línea del error.
4. **La lección:** Si alguna vez ejecutas un bloque `try-catch` y notas que solo ves el mensaje del `catch` omitiendo la cabecera y el `finally`, comprueba siempre que no tengas ningún texto escrito en la barra de búsqueda de la consola de IntelliJ [25].

---

### 4.2. El Plegado Automático de Gradle (*Console Folding*)

Si creaste tu proyecto en IntelliJ utilizando **Gradle** (el sistema de construcción estándar en el desarrollo moderno de Kotlin), el IDE analiza activamente el texto de tus impresiones en busca de problemas lógicos de compilación o ejecución [10].

#### Ejemplo independiente para probar en clase:
Crea un archivo nuevo y ejecuta el siguiente código:

```kotlin
fun main() {
    println("Paso 1: Abriendo electroválvulas principales...")
    println("Paso 2: Comprobando caudalímetro...")
    // Al empezar el texto estrictamente por "Error:", Gradle puede clasificar el proceso como fallido
    println("Error: Caudal de agua por debajo del umbral mínimo.")
    println("Paso 3: Cerrando sistema por seguridad.")
}
```

#### ¿Qué debes observar?
* Al detectar una línea de texto plano que comienza de forma estricta con la palabra `"Error:"` (o `"Exception:"`), el plugin de Gradle en IntelliJ puede asumir de manera automática que esa tarea ha fallado o tiene un reporte crítico.
* Para ahorrar espacio en pantallas con mucho texto, Gradle tiende a **plegar y colapsar (*folding*)** bajo un desplegable gris todas las líneas previas que considera informativas o exitosas (en este caso, los "Paso 1" y "Paso 2").
* **Cómo evitarlo:** Si quieres que tus mensajes de control de errores no se plieguen ni confundan al motor de Gradle, una buena práctica es redactar tus advertencias de forma más narrativa (por ejemplo, omitiendo los dos puntos pegados a la palabra: `"No se pudo conectar el sensor..."`) o utilizar herramientas específicas de traza de logs.







---
<span class="mi_h3">Autoría</span>

<span class="mi_autoria">
Obra realizada por Begoña Paterna Lluch. Publicada bajo licencia [Creative Commons Atribución/Reconocimiento-CompartirIgual 4.0 Internacional](https://creativecommons.org/licenses/by-sa/4.0/)
</span>
---