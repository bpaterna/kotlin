# 3. Entrada y salida estándar


<span class="mi_h3">Revisiones</span>

| Revisión | Fecha      | Descripción                             |
|----------|------------|-----------------------------------------|
| 1.0      | 29-06-2026 | Adaptación de los materiales a markdown |



## 1. Introducción

En programación, la **entrada y salida estándar** representa el canal de comunicación directo entre tu programa y la persona que lo está utilizando. A través de la consola, podrás mostrar informes sobre el estado del invernadero (salida) o solicitar que se introduzcan nuevos datos de catalogación de plantas (entrada).



## 2. Salida de datos por consola

Para enviar información y texto hacia la pantalla, Kotlin pone a tu disposición dos funciones muy similares pero con un comportamiento diferente respecto al salto de línea (`print` y `println`).

| Función | Descripción | Ejemplo de uso | Resultado en consola |
| :--- | :--- | :--- | :--- |
| **`print()`** | Imprime el texto tal cual, **sin añadir un salto de línea** al final. El siguiente texto se mostrará pegado a continuación. | `print("Rosa ")`<br>`print("roja")` | `Rosa roja` |
| **`println()`** | Imprime el texto y **añade automáticamente un salto de línea** al final. El siguiente texto comenzará en la línea inferior. | `println("Rosa")`<br>`println("roja")` | `Rosa`<br>`roja` |

**Ejemplo 1: Consola.** Imagina que quieres mostrar una pequeña ficha de registro de tareas de jardinería combinando ambas funciones:

```kotlin
fun main() {
    println("=== EJEMPLO 1: Consola ===")
    print("Estado del riego: ")
    print("COMPLETADO") // Se muestra en la misma línea
    
    println() // Esto genera un salto de línea vacío
    
    println("--- Datos de la parcela ---")
    println("Especie: Lavanda")
    println("Ubicación: Sector Sur")
}
```

Salida por consola:

```text
=== EJEMPLO 1: Consola ===
Estado del riego: COMPLETADO
--- Datos de la parcela ---
Especie: Lavanda
Ubicación: Sector Sur
```

<span class="mi_h3">Formateo de la salida de datos</span>

Los ordenadores operan en binario (base 2) y solo pueden representar de forma exacta fracciones que son potencias de dos (como $0.5$ o $0.25$), por lo que otros números decimales se convierten en binario en fracciones periódicas infinitas, del mismo modo que nos ocurre al intentar escribir un tercio ($\frac{1}{3}$) en nuestro sistema decimal tradicional. Como la memoria física de la CPU es limitada (un `Double` dispone de 64 bits), el procesador se ve obligado a recortar y redondear ese número infinito, acumulando una pérdida microscópica de precisión que se hace visible tras realizar cálculos matemáticos en forma de decimales "fantasma" como `3.3000000000000007`.

Para dar formato a los datos de manera rápida y elegante en tus clases de Kotlin, la forma más cómoda es usar la función de extensión **`.format()`** (que por debajo utiliza los mismos especificadores clásicos de `printf` de Java/C).

Aquí tienes una tabla resumen con las opciones de formateo más útiles y algunos ejemplos:

| Tipo de Dato | Objetivo del Formateo | Especificador de Formato | Código de Ejemplo en Kotlin | Salida en Consola |
| :--- | :--- | :---: | :--- | :--- |
| **Decimal (`Double` / `Float`)** | Limitar a $N$ decimales (ej. 2) | **`%.Nf`** | `"%.2f cm".format(3.30000007)` | `3,30 cm` *(o `3.30` según el idioma del sistema)* |
| **Decimal (`Double` / `Float`)** | Separador de miles y decimales | **`%,.Nf`** | `"%,.1f L".format(1250.75)` | `1.250,8 L` *(en configuración regional de España)* |
| **Entero (`Int` / `Long`)** | Rellenar con ceros a la izquierda | **`%0Nd`** | `"ID-%04d".format(28)` | `ID-0028` |
| **Texto (`String`)** | Forzar ancho fijo alineado a la izquierda | **`%-Ns`** | `println("%-12s \| %s".format("Helecho", "Ok"))` | `Helecho      \| Ok` *(útil para alinear tablas)* |
| **Texto (`String`)** | Forzar ancho fijo alineado a la derecha | **`%Ns`** | `println("%12s \| %s".format("Helecho", "Ok"))` | `     Helecho \| Ok` |
| **Porcentaje** | Mostrar símbolo `%` sin decimales | **`%.0f%%`** | `"Humedad: %.0f%%".format(65.8)` | `Humedad: 66%` *(el doble `%%` escapa el símbolo)* |


## 3. Entrada de datos por teclado

Para que tus programas no sean estáticos y puedan reaccionar a lo que decida el usuario, necesitas capturar lo que se escribe desde el teclado. Para ello se utiliza la función **`readLine()`**.

**Características clave de `readLine()`:**

1. **Siempre devuelve texto:** Aunque el usuario introduzca un número (como un `5`), la función lo capturará como un texto (`"5"`). Su tipo de retorno es **`String?`** (una cadena de texto que puede ser nula).
2. **Control del botón "Enter":** Si el usuario simplemente pulsa la tecla *Enter* sin escribir absolutamente nada, `readLine()` no devolverá un valor nulo (`null`), sino una **cadena vacía** (`""`).
3. **Conversión necesaria:** Si necesitas operar matemáticamente con el dato que introduce el usuario (por ejemplo, para realizar cálculos de crecimiento), tendrás que transformar ese texto al tipo de dato numérico correspondiente (`toInt()`, `toDouble()`, etc.).



<span class="mi_h3">Comprobaciones de seguridad</span> 

Dado que intentar convertir un texto vacío o con letras (como `"mucho"`) a un número entero provocará que tu programa falle y se detenga con un error, **siempre debes realizar una comprobación previa de seguridad**.

La forma segura de realizar esta conversión es utilizando la función toIntOrNull(). Esta función intenta transformar el texto a número: si lo consigue, nos devuelve el valor entero; pero si el usuario introduce letras, espacios en blanco o pulsa Enter directamente, nos devolverá un valor null.


**Ejemplo 2: Cálculo de crecimiento estimado.** Observa este programa que solicita la altura actual de un girasol y calcula cuánto medirá la próxima semana suponiendo un crecimiento estimado de 15 centímetros:

```kotlin
fun main() {
    println("=== EJEMPLO 2: Cálculo de crecimiento estimado ===")
    print("Introduce la altura actual del girasol (en cm): ")
    val entrada = readLine()

    // Intentamos la conversión de forma segura. Si introducen 'p', 'alturaActual' será null.
    val alturaActual = entrada?.toIntOrNull()

    // Ahora solo tenemos que validar que el resultado no sea nulo
    if (alturaActual != null) {
        val alturaEstimada = alturaActual + 15
        println("La próxima semana tu girasol medirá aproximadamente $alturaEstimada cm.")
    } else {
        // Este mensaje ahora cubre tanto si pulsan Enter vacío como si escriben letras o decimales
        println("Altura introducida no válida. Asegúrate de escribir un número entero.")
    }
}
```

Salida por consola (Entrada válida):

```text
=== EJEMPLO 2: Cálculo de crecimiento estimado ===
Introduce la altura actual del girasol (en cm): 45
La próxima semana tu girasol medirá aproximadamente 60 cm.
```

Salida por consola (Entrada vacía o incorrecta):

```text
=== EJEMPLO 2: Cálculo de crecimiento estimado ===
Introduce la altura actual del girasol (en cm): g
Altura introducida no válida. Asegúrate de escribir un número entero.
```


---
<span class="mi_h3">Autoría</span>

<span class="mi_autoria">
Obra realizada por Begoña Paterna Lluch. Publicada bajo licencia [Creative Commons Atribución/Reconocimiento-CompartirIgual 4.0 Internacional](https://creativecommons.org/licenses/by-sa/4.0/)
</span>
---