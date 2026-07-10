# 4. Estructuras de control


<span class="mi_h3">Revisiones</span>

| Revisión | Fecha      | Descripción                             |
|----------|------------|-----------------------------------------|
| 1.0      | 29-06-2026 | Adaptación de los materiales a markdown |



## 4.1. Introducción

Las estructuras de control permiten romper la ejecución secuencial (de arriba a abajo) de los programas. Con ellas, podrás tomar decisiones lógicas (condicionales) o repetir ciertas acciones varias veces (bucles).


## 4.2. Operadores relacionales y lógicos

Para establecer condiciones o realizar repeticiones, necesitas comparar valores. Estos son los operadores relacionales que utilizarás en Kotlin:

| Operador | Significado | Ejemplo | Resultado |
| :---: | :--- | :---: | :---: |
| **`==`** | Igual a | `5 == 5` | `true` |
| **`!=`** | Distinto de | `5 != 3` | `true` |
| **`>`** | Mayor que | `10 > 7` | `true` |
| **`<`** | Menor que | `3 < 8` | `true` |
| **`>=`** | Mayor o igual que | `6 >= 6` | `true` |
| **`<=`** | Menor o igual que | `4 <= 9` | `true` |

Puedes combinar varias condiciones utilizando **operadores lógicos**:

* **`&&` (AND):** Devuelve `true` solo si **ambas** condiciones son verdaderas.
* **`||` (OR):** Devuelve `true` si al menos **una** de las condiciones es verdadera.
* **`!` (NOT):** Invierte el valor lógico (lo que es `true` pasa a ser `false`).

## 3. Estructuras condicionales

Los condicionales te permiten ejecutar diferentes bloques de código en función de si se cumplen o no determinadas condiciones.

<span class="mi_h3">Estructura `if` / `if-else`</span>

**Ejemplo 1: Condicional simple (`if`).** Se ejecuta un bloque de código únicamente si la condición resulta verdadera.

```kotlin
fun main() {
    println("=== EJEMPLO 1: Condicional simple ===")
    val humedadSuelo = 35

    if (humedadSuelo < 40) {
        println("Alerta: Humedad baja. Activando electroválvula de riego.")
    }
}
```

Salida por consola:

```text
=== EJEMPLO 1: Condicional simple ===
Alerta: Humedad baja. Activando electroválvula de riego.
```


**Ejemplo 2: Condicional doble (`if-else`).** Permite tomar un camino alternativo si la condición no se cumple.

```kotlin
fun main() {
    println("=== EJEMPLO 2: Condicional doble ===")
    val phTierra = 7.5

    if (phTierra < 7.0) {
        println("El sustrato es ácido.")
    } else {
        // Se ejecuta si el pH es 7.0 o superior
        println("El sustrato es neutro o alcalino.")
    }
}
```

Salida por consola:

```text
=== EJEMPLO 2: Condicional doble ===
El sustrato es neutro o alcalino.
```

**Ejemplo 3: Condicionales anidados.** Puedes introducir condicionales dentro de otros condicionales para refinar la lógica de control.

```kotlin
fun main() {
    println("=== EJEMPLO 3: Condicionales anidados ===")
    val temperatura = 24
    val tieneSombra = true
    val requiereSolDirecto = false

    if (temperatura >= 20 && tieneSombra) {
        println("Condiciones ambientales base correctas.")

        if (!requiereSolDirecto) {
            println("La planta está protegida adecuadamente en la sombra.")
        } else {
            println("Aviso: Esta especie requiere sol directo. Mover maceta.")
        }
    } else {
        println("Temperatura fuera de rango o falta de protección de sombra.")
    }
}
```

Salida por consola:

```text
=== EJEMPLO 3: Condicionales anidados ===
Condiciones ambientales base correctas.
La planta está protegida adecuadamente en la sombra.
```

**Ejemplo 4: `if` como expresión.** En Kotlin, el `if` puede devolver un valor directamente. Esto te permite ahorrar líneas de código al asignar valores.

```kotlin
fun main() {
    println("=== EJEMPLO 4: `if` como expresión ===")
    val litrosAgua = 1.5
// El resultado del if se guarda directamente en la variable 'estadoRiego'
    val estadoRiego = if (litrosAgua >= 2.0) "Óptimo" else "Insuficiente"

    println("El riego de hoy ha sido: $estadoRiego")
}
```

Salida por consola:

```text
=== EJEMPLO 4: `if` como expresión ===
El riego de hoy ha sido: Insuficiente
```

<span class="mi_h3">La estructura `when`</span>

La estructura `when` sustituye al clásico `switch` de otros lenguajes. Es mucho más potente y legible cuando tienes múltiples caminos lógicos.


**Ejemplo 5: `when` como selector directo.** Evalúa una variable y toma una acción según su valor coincidente.

```kotlin
fun main() {
    println("=== EJEMPLO 5: `when` como selector directo ===")
    val tipoPlanta = "Cactus"

    val frecuenciaRiego = when (tipoPlanta) {
        "Helecho" -> "Frecuente (cada 2 días)"
        "Cactus" -> "Muy poco frecuente (cada 15 días)"
        "Orquídea" -> "Moderado (cada 7 días)"
        else -> "Frecuencia de riego estándar (cada 5 días)"
    }

    println("Riego recomendado para $tipoPlanta: $frecuenciaRiego")
}
```

Salida por consola:

```text
=== EJEMPLO 5: `when` como selector directo ===
Riego recomendado para Cactus: Muy poco frecuente (cada 15 días)
```

**Ejemplo 6: `when` con condiciones complejas.** Si no le pasas ningún parámetro entre paréntesis a `when`, puedes evaluar condiciones libres en cada rama (similar a una cadena de `if-else if`).

```kotlin
fun main() {
    println("=== EJEMPLO 6: `when` con condiciones complejas ===")
    val humedadSuelo = 45

    val estadoHumedad = when {
        humedadSuelo >= 80 -> "Suelo encharcado (peligro para las raíces)"
        humedadSuelo in 40..79 -> "Humedad óptima"
        humedadSuelo in 15..39 -> "Humedad moderada (se recomienda regar pronto)"
        else -> "Suelo completamente seco (regar inmediatamente)"
    }

    println("Informe del sensor: $estadoHumedad")
}
```

Salida por consola:

```text
=== EJEMPLO 6: `when` con condiciones complejas ===
Informe del sensor: Humedad óptima
```

## 4. Estructuras repetitivas (bucles)

Los bucles te permiten repetir la ejecución de un bloque de código. En Kotlin dispones de cuatro estructuras: `while`, `do-while`, `for` y `repeat`.

**Ejemplo 7: Bucle `while`.** Repite las instrucciones **mientras** la condición sea verdadera. Evalúa la condición **antes** de entrar al bucle; si es falsa desde el principio, el bloque nunca llega a ejecutarse.

```kotlin
fun main() {
    println("=== EJEMPLO 7: Bucle `while` ===")
    var diasSinLluvia = 1

    while (diasSinLluvia <= 5) {
        println("Día $diasSinLluvia sin lluvia: La reserva de agua disminuye.")
        diasSinLluvia++ // Incrementamos para evitar un bucle infinito
    }
}
```

Salida por consola:

```text
=== EJEMPLO 7: Bucle `while` ===
Día 1 sin lluvia: La reserva de agua disminuye.
Día 2 sin lluvia: La reserva de agua disminuye.
Día 3 sin lluvia: La reserva de agua disminuye.
Día 4 sin lluvia: La reserva de agua disminuye.
Día 5 sin lluvia: La reserva de agua disminuye.
```


**Ejemplo 8: Bucle `do-while`.** Es similar al anterior, pero con una diferencia clave: **evalúa la condición al final**. Esto garantiza que el bloque de código se ejecutará **al menos una vez**, sin importar si la condición es falsa desde el inicio.

```kotlin
fun main() {
    println("=== EJEMPLO 8: Bucle `do-while` ===")
    var litrosEnRegadera = 0

    do {
        litrosEnRegadera++
        println("Llenando regadera... Volumen actual: $litrosEnRegadera litros")
    } while (litrosEnRegadera < 5)
}
```

Salida por consola:

```text
=== EJEMPLO 8: Bucle `do-while` ===
Llenando regadera... Volumen actual: 1 litros
Llenando regadera... Volumen actual: 2 litros
Llenando regadera... Volumen actual: 3 litros
Llenando regadera... Volumen actual: 4 litros
Llenando regadera... Volumen actual: 5 litros
```


**Ejemplo 9: Bucle `for`.** Se utiliza para recorrer colecciones, listas o un **rango numérico definido**. Lo emplearás cuando sepas de antemano cuántas veces necesitas repetir una acción.

```kotlin
fun main() {
    println("=== EJEMPLO 9: Bucle `for` ===")
// Recorre los números del 1 al 5, ambos incluidos
    for (dia in 1..5) {
        println("Día de germinación $dia: La semilla absorbe humedad de la tierra.")
    }
}
```

Salida por consola:

```text
=== EJEMPLO 9: Bucle `for` ===
Día de germinación 1: La semilla absorbe humedad de la tierra.
Día de germinación 2: La semilla absorbe humedad de la tierra.
Día de germinación 3: La semilla absorbe humedad de la tierra.
Día de germinación 4: La semilla absorbe humedad de la tierra.
Día de germinación 5: La semilla absorbe humedad de la tierra.
```


**Ejemplo 10: Bucle `repeat`.** Es una función exclusiva de Kotlin para simplificar las repeticiones. Te permite ejecutar un bloque de instrucciones un número exacto de veces, sin necesidad de definir rangos ni variables contadoras adicionales.

```kotlin
fun main() {
    println("=== EJEMPLO 10: Bucle `repeat` ===")
// Simulación de una pulverización de agua automatizada
    repeat(3) {
        println("Pulverizando agua sobre las hojas.")
    }
}
```

Salida por consola:

```text
=== EJEMPLO 10: Bucle `repeat` ===
Pulverizando agua sobre las hojas.
Pulverizando agua sobre las hojas.
Pulverizando agua sobre las hojas.
```


<span class="mi_h3">Comparativa de estructuras repetitivas</span>

Para ayudarte a decidir qué bucle utilizar en el desarrollo de tus aplicaciones, puedes consultar esta tabla resumen:

| Estructura | ¿Cuándo usarla? | ¿Evalúa primero? | ¿Se ejecuta al menos una vez? |
| :--- | :--- | :---: | :---: |
| **`while`** | Cuando no sabes el número exacto de repeticiones y estas dependen de una condición externa que debe cumplirse desde el principio. | Sí | No |
| **`do-while`** | Cuando quieres garantizar que la acción se realice al menos una vez antes de verificar si debe repetirse. | No | Sí |
| **`for`** | Cuando conoces de antemano el número exacto de veces que deseas iterar o necesitas recorrer una lista/rango. | Sí | Depende de si el rango o lista está vacío |
| **`repeat`** | Para tareas sencillas donde solo necesitas repetir un bloque de código un número fijo de veces sin interactuar con un índice. | Sí (valida si N > 0) | No (si N es 0 o menor) |


---
<span class="mi_h3">Autoría</span>

<span class="mi_autoria">
Obra realizada por Begoña Paterna Lluch. Publicada bajo licencia [Creative Commons Atribución/Reconocimiento-CompartirIgual 4.0 Internacional](https://creativecommons.org/licenses/by-sa/4.0/)
</span>
---