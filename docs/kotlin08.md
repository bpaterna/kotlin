# 8. Programación funcional y expresiones Lambda


<span class="mi_h3">Revisiones</span>

| Revisión | Fecha      | Descripción                             |
|----------|------------|-----------------------------------------|
| 1.0      | 29-06-2026 | Adaptación de los materiales a markdown |



## 8.1. Introducción

Las **expresiones lambda** (o funciones lambda) son funciones anónimas, es decir, funciones que se definen sin un nombre. En Kotlin, puedes tratarlas como si fueran valores comunes: guardarlas en variables, pasarlas como argumentos a otras funciones o hacer que una función las devuelva.

Su uso principal es lograr que tu código sea mucho más limpio, legible y expresivo, especialmente al trabajar con listas de datos de tu invernadero.



## 8.2. Sintaxis Básica de una Lambda

La sintaxis siempre va encerrada entre llaves `{}`. Se definen los parámetros de entrada, seguidos de una flecha `->` y, a continuación, el cuerpo o comportamiento de la función:

{ parametro1, parametro2 -> cuerpo_de_la_funcion }

**Ejemplo 1: Lambda básica con un parámetro.**

```kotlin
// Guardamos la lambda en la variable 'describirPlanta'
val describirPlanta = { especie: String -> println("Especie catalogada: $especie") }

fun main() {
    println("=== EJEMPLO 1: Lambda básica con un parámetro ===")
    
    // Se ejecuta llamándola como si fuera una función normal
    describirPlanta("Monstera")
}
```

Salida por consola:

```text
=== EJEMPLO 1: Lambda básica con un parámetro ===
Especie catalogada: Monstera
```


**Ejemplo 2: Lambda sin parámetros.**

```kotlin
val pulverizarAgua = { println("Acción: Pulverizando microgotas de agua...") }

fun main() {
    println("=== EJEMPLO 2: Lambda sin parámetros ===")
    pulverizarAgua()
}
```

Salida por consola:

```text
=== EJEMPLO 2: Lambda sin parámetros ===
Acción: Pulverizando microgotas de agua...
```

**Ejemplo 3: Lambda con múltiples parámetros.**

```kotlin
val calcularCrecimiento = { inicial: Double, final: Double -> final - inicial }

fun main() {
    println("=== EJEMPLO 3: Lambda con múltiples parámetros ===")
    val diferencia = calcularCrecimiento(15.2, 18.5)
    println("Crecimiento neto: ${String.format("%.1f", diferencia)} cm")
}
```

Salida por consola:

```text
=== EJEMPLO 3: Lambda con múltiples parámetros ===
Crecimiento neto: 3,3 cm
```


<span class="mi_h3">Comparativa: Funciones Tradicionales vs. Lambdas</span>

Esta tabla te ayuda a visualizar cómo se simplifica el código al transformar funciones tradicionales a su formato lambda equivalente:

| Función Tradicional | Expresión Lambda Equivalente |
| :--- | :--- |
| `fun regar(planta: String) { println("Regando $planta") }` | `val regar = { planta: String -> println("Regando $planta") }` |
| `fun calcularPh(valor: Double): Double { return valor + 0.5 }` | `val calcularPh = { valor: Double -> valor + 0.5 }` |
| `fun alertarFaltaAgua() = println("¡Falta agua!")` | `val alertarFaltaAgua = { println("¡Falta agua!") }` |



## 8.3. Operaciones avanzadas en colecciones mediante Lambdas

El verdadero potencial de las expresiones lambda en Kotlin se libera cuando las utilizas junto con colecciones utilizando funciones de orden superior del sistema.

A continuación tienes una tabla los operadores que utilizaremos en este curso:

| Operador / Función | ¿Qué hace?                                                                               | Comportamiento en el ejemplo botánico |
| :--- |:-----------------------------------------------------------------------------------------| :--- |
| **`forEach`** | Recorre cada elemento de la colección aplicando una acción.                              | Imprime de forma individual la altura de cada planta. |
| **`map`** | Transforma cada elemento aplicando una regla matemática o lógica y **devuelve una nueva lista con los resultados**.  | Añade 5 cm de crecimiento estimado a cada planta. |
| **`mapIndexed`** | Transforma cada elemento igual que `map`, pero dándote también su índice de posición.    | Genera un texto para etiquetar de forma numerada cada maceta. |
| **`filter`** | Filtra la colección y conserva únicamente los elementos que cumplen una condición.       | Filtra solo las plantas que superan los 30 cm de altura. |
| **`sumOf`** | Suma el resultado de aplicar una transformación o factor de escala a los datos.          | Suma los litros de abono aplicando un 10% de margen de evaporación. |
| **`any`** | Devuelve `true` si **al menos uno** de los elementos cumple con la condición.            | Comprueba si hay riesgo de helada en alguna de las zonas evaluadas. |
| **`all`** | Devuelve `true` si **todos** los elementos cumplen con la condición.                     | Verifica si todas las zonas cumplen con la temperatura óptima. |
| **`none`** | Devuelve `true` si **ninguno** de los elementos cumple con la condición.                 | Comprueba si ninguna de las zonas sufre calor extremo. |
| **`reduce`** | Acumula los valores de izquierda a derecha usando el primer elemento como valor inicial. | Suma las cantidades del lote de nuevos brotes. |
| **`fold`** | Acumula los valores de izquierda a derecha partiendo de un valor inicial personalizado.  | Suma los nuevos brotes a un inventario base existente de 100 plantas. |




**Ejemplo 4: Operaciones avanzadas en colecciones mediante Lambdas.**

```kotlin
fun main() {
    println("=== EJEMPLO 4: Operaciones avanzadas en colecciones mediante Lambdas ===")
    
    // Declaramos todos los conjuntos de datos iniciales
    val alturasCm = intArrayOf(12, 45, 80, 5, 110, 30)
    val abonosLitros = doubleArrayOf(1.5, 2.0, 0.5)
    val temperaturasZonas = doubleArrayOf(18.5, 22.0, 14.0)
    val nuevosBrotes = intArrayOf(5, 8, 12)

    println("=== 1. Recorrido con 'forEach' ===")
    // Recorremos de manera compacta el array de alturas
    alturasCm.forEach { println("Altura de rama registrada: $it cm") }

    println("\n=== 2. Transformación con 'map' ===")
    // Genera una nueva lista sumando 5 cm de crecimiento estimado a cada elemento
    val alturasCrecidas = alturasCm.map { it + 5 } 
    println("Alturas estimadas tras el riego (+5cm): $alturasCrecidas")

    println("\n=== 3. Transformación con 'mapIndexed' ===")
    // Transformamos obteniendo también el índice de posición física de la planta
    val codigosMaceta = alturasCm.mapIndexed { index, altura -> "Maceta #${index + 1}: $altura cm" }
    codigosMaceta.forEach { println(it) }

    println("\n=== 4. Filtrado con 'filter' ===")
    // Filtramos únicamente las plantas que superan los 30 cm
    val listasTrasplante = alturasCm.filter { it > 30 }
    println("Plantas aptas para trasplante (>30cm): $listasTrasplante")

    println("\n=== 5. Suma con transformación ('sumOf') ===")
    // Suma las dosis aplicando un factor del 1.1 (margen extra del 10%)
    val totalAbonoNecesario = abonosLitros.sumOf { it * 1.1 }
    println("Volumen total de abono con margen del 10%: $totalAbonoNecesario litros")

    println("\n=== 6. Consultas lógicas ('any', 'all', 'none') ===")
    val hayRiesgoHelada = temperaturasZonas.any { it < 5.0 }
    val todasZonasOptimas = temperaturasZonas.all { it >= 15.0 }
    val ningunCalorExtremo = temperaturasZonas.none { it > 35.0 }

    println("¿Hay riesgo de helada en alguna zona (<5°C)?: $hayRiesgoHelada")
    println("¿Están todas las zonas a una temperatura óptima (>=15°C)?: $todasZonasOptimas")
    println("¿Ninguna de las zonas supera el umbral de calor extremo (>35°C)?: $ningunCalorExtremo")

    println("\n=== 7. Acumulación ('reduce' y 'fold') ===")
    // reduce: Suma los nuevos brotes partiendo de su primer valor (5 + 8 + 12 = 25)
    val sumaBrotes = nuevosBrotes.reduce { acumulado, brotes -> acumulado + brotes }
    
    // fold: Suma los nuevos brotes partiendo de un stock base ya existente en el invernadero (100)
    val inventarioTotal = nuevosBrotes.fold(100) { acumulador, brotes -> acumulador + brotes }
    
    println("Suma neta del lote de nuevos brotes: $sumaBrotes")
    println("Inventario total final en el sistema: $inventarioTotal")
}
```

Salida por consola:

```text
=== EJEMPLO 4: Operaciones avanzadas en colecciones mediante Lambdas ===
=== 1. Recorrido con 'forEach' ===
Altura de rama registrada: 12 cm
Altura de rama registrada: 45 cm
Altura de rama registrada: 80 cm
Altura de rama registrada: 5 cm
Altura de rama registrada: 110 cm
Altura de rama registrada: 30 cm

=== 2. Transformación con 'map' ===
Alturas estimadas tras el riego (+5cm): [17, 50, 85, 10, 115, 35]

=== 3. Transformación con 'mapIndexed' ===
Maceta #1: 12 cm
Maceta #2: 45 cm
Maceta #3: 80 cm
Maceta #4: 5 cm
Maceta #5: 110 cm
Maceta #6: 30 cm

=== 4. Filtrado con 'filter' ===
Plantas aptas para trasplante (>30cm): [45, 80, 110]

=== 5. Suma con transformación ('sumOf') ===
Volumen total de abono con margen del 10%: 4.4 litros

=== 6. Consultas lógicas ('any', 'all', 'none') ===
¿Hay riesgo de helada en alguna zona (<5°C)?: false
¿Están todas las zonas a una temperatura óptima (>=15°C)?: false
¿Ninguna de las zonas supera el umbral de calor extremo (>35°C)?: true

=== 7. Acumulación ('reduce' y 'fold') ===
Suma neta del lote de nuevos brotes: 25
Inventario total final en el sistema: 125
```


## 8.4. ¿Qué es exactamente la palabra reservada `it`?

En Kotlin, si tu expresión lambda tiene **un único parámetro de entrada**, no es necesario que declares su nombre de forma explícita seguido de la flecha `->`. Kotlin te proporciona automáticamente la variable implícita **`it`** para representar a ese único parámetro.

Esto te permite escribir un código mucho más limpio y conciso:

```kotlin
// Forma explícita (más larga)
alturasCm.filter { altura -> altura > 40 }

// Forma implícita usando 'it' (más corta y recomendada)
alturasCm.filter { it > 40 }
```

**¿Cuándo NO debes usar `it`?**

* Si tu lambda recibe **más de un parámetro** (como en `mapIndexed` o `fold`), estás obligado a nombrar los parámetros explícitamente.
* Si el bloque de código dentro de la lambda es excesivamente largo y anidado, es mejor nombrar la variable de forma explícita (por ejemplo, `altura ->`) para que sea más fácil de leer.


<span class="mi_h3">Tabla Resumen de operaciones con Lambdas</span>

| Operación | Método en Kotlin | ¿Qué devuelve? |
| :--- | :--- | :--- |
| **Recorrer** | `forEach { ... }` | `Unit` (sin valor, solo ejecuta acciones). |
| **Transformar** | `map { ... }`, `mapIndexed { ... }` | Una nueva `List<T>` con los elementos transformados. |
| **Filtrar** | `filter { ... }` | Una nueva `List<T>` filtrada con los que cumplen la condición. |
| **Acumular** | `reduce { ... }`, `fold(valorInicial) { ... }` | Un valor del mismo tipo que el acumulador (`T`). |
| **Consultar** | `any { ... }`, `all { ... }`, `none { ... }` | Un valor de tipo lógico (`Boolean`). |



---
<span class="mi_h3">Autoría</span>

<span class="mi_autoria">
Obra realizada por Begoña Paterna Lluch. Publicada bajo licencia [Creative Commons Atribución/Reconocimiento-CompartirIgual 4.0 Internacional](https://creativecommons.org/licenses/by-sa/4.0/)
</span>
---