# 9. Gestión de errores (Excepciones y recursos)


<span class="mi_h3">Revisiones</span>

| Revisión | Fecha      | Descripción                             |
|----------|------------|-----------------------------------------|
| 1.0      | 29-06-2026 | Adaptación de los materiales a markdown |
| 1.1      | 10-07-2026 | Renumeración de apartados |


## 9.1. Introducción

Una **excepción** es un error que ocurre en tiempo de ejecución (mientras el programa se está ejecutando) e interrumpe el flujo normal de tu aplicación. Ejemplos típicos son intentar dividir un número entre cero (como repartir abono entre cero macetas) o intentar abrir un archivo de registros del invernadero que no existe en el disco.

La gestión de errores consiste en detectar, capturar y recuperarse de estos fallos para evitar que la aplicación se cuelgue. En Kotlin dispones de dos herramientas fundamentales para lograrlo.


## 9.2. Captura explícita de errores: `try-catch-finally`

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


## 9.3. Gestión automática de recursos con `use`

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

Salida por consola:

```text
=== EJEMPLO 3: Leer el archivo de inventario de forma segura ===
ID: 1 | Monstera deliciosa | Seccion Sombra
ID: 2 | Lavanda dentata    | Seccion Exterior
ID: 3 | Helecho real       | Seccion Humedad
```


## 9.4. Comportamiento de la consola de IntelliJ

Cuando haces pruebas por consola, la forma en que el IDE (IntelliJ) y su motor de construcción (Gradle) interpretan tus textos puede alterar lo que ves en pantalla. 

Si utilizas en tus mensajes una línea de texto plano que comienza con la palabra `"Error:"` el plugin de Gradle en IntelliJ puede asumir que la ejecución del código ha fallado y el resultado en consola será unicamente la línea con ese texto en color rojo (como cuando el programa se detiene inesperadamente).

Esto ocurre porque, para ahorrar espacio en pantalla, Gradle recoge en un desplegable todas las líneas que considera informativas o exitosas (en este caso, los `println` anteriores y posteriores a nuestro mensaje de error).


**Ejemplo 4: Con palabra reservada `Error:` en el mensaje.** 

```kotlin
fun main() {
    println("--- ESTADO DEL SENSOR ---")
    println("Sensor de humedad conectado.")
    println("Error: Fallo en el sensor de luz.")
    println("Chequeo del sistema de riego finalizado.")
}
```

Salida por consola (en color rojo):

```text
Error: Fallo en el sensor de luz.
```

Para que tus mensajes de control de errores no confundan al motor de Gradle, una buena práctica es redactar tus advertencias sin utilizar `Error:` al inicio del mensaje. Puedes utilizar cualquier otro caracter que llame la atención.


**Ejemplo 5: Sin palabra reservada `Error:` en el mensaje.**

```kotlin
fun main() {
    println("--- ESTADO DEL SENSOR ---")
    println("Sensor de humedad conectado.")
    println("**** Fallo en el sensor de luz.")
    println("Chequeo del sistema de riego finalizado.")
}
```

Salida por consola:

```text
--- ESTADO DEL SENSOR ---
Sensor de humedad conectado.
**** Fallo en el sensor de luz.
Chequeo del sistema de riego finalizado.
```



---
<span class="mi_h3">Autoría</span>

<span class="mi_autoria">
Obra realizada por Begoña Paterna Lluch. Publicada bajo licencia [Creative Commons Atribución/Reconocimiento-CompartirIgual 4.0 Internacional](https://creativecommons.org/licenses/by-sa/4.0/)
</span>
---