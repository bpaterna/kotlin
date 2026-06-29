# 8. Programación funcional y expresiones Lambda


<span class="mi_h3">Revisiones</span>

| Revisión | Fecha      | Descripción                             |
|----------|------------|-----------------------------------------|
| 1.0      | 29-06-2026 | Adaptación de los materiales a markdown |



## 1. Introducción

Las **expresiones lambda** (o funciones lambda) son funciones anónimas, es decir, funciones que se definen sin un nombre. En Kotlin, puedes tratarlas como si fueran valores comunes: guardarlas en variables, pasarlas como argumentos a otras funciones o hacer que una función las devuelva.

Su uso principal es lograr que tu código sea mucho más limpio, legible y expresivo, especialmente al trabajar con listas de datos de tu invernadero.



## 2. Sintaxis Básica de una Lambda

La sintaxis siempre va encerrada entre llaves `{}`. Se definen los parámetros de entrada, seguidos de una flecha `->` y, a continuación, el cuerpo o comportamiento de la función:

$$\{\text{parámetro1}, \text{parámetro2} \rightarrow \text{cuerpo de la función}\}$$

**Ejemplo 1: Lambda básica con un parámetro**

```kotlin
// Guardamos la lambda en la variable 'describirPlanta'
val describirPlanta = { especie: String -> println("Especie catalogada: $especie") }

fun main() {
    // Se ejecuta llamándola como si fuera una función normal
    describirPlanta("Monstera")
}
```

**Ejemplo 2: Lambda sin parámetros**

```kotlin
val pulverizarAgua = { println("Acción: Pulverizando microgotas de agua...") }

fun main() {
    pulverizarAgua()
}
```

**Ejemplo 3: Lambda con múltiples parámetros**

```kotlin
val calcularCrecimiento = { inicial: Double, final: Double -> final - inicial }

fun main() {
    val diferencia = calcularCrecimiento(15.2, 18.5)
    println("Crecimiento neto: $diferencia cm") // 3.3 cm
}
```



**Comparativa: Funciones Tradicionales vs. Lambdas**

Esta tabla te ayuda a visualizar cómo se simplifica el código al transformar funciones tradicionales a su formato lambda equivalente:

| Función Tradicional | Expresión Lambda Equivalente |
| :--- | :--- |
| `fun regar(planta: String) { println("Regando $planta") }` | `val regar = { planta: String -> println("Regando $planta") }` |
| `fun calcularPh(valor: Double): Double { return valor + 0.5 }` | `val calcularPh = { valor: Double -> valor + 0.5 }` |
| `fun alertarFaltaAgua() = println("¡Falta agua!")` | `val alertarFaltaAgua = { println("¡Falta agua!") }` |



## 3. Operaciones avanzadas en colecciones mediante Lambdas

El verdadero potencial de las expresiones lambda en Kotlin se libera cuando las utilizas junto con colecciones (como `Arrays` o `Lists`) utilizando funciones de orden superior del sistema.

Para los siguientes ejemplos, utilizaremos como base este registro de alturas de plantas:
```kotlin
val alturasCm = intArrayOf(12, 45, 80, 5, 110, 30)
```

**`forEach` (Recorrer elementos)**

Permite realizar una acción para cada elemento de la colección de forma compacta.

```kotlin
// 'it' es el nombre que Kotlin da por defecto al elemento único que se está procesando
alturasCm.forEach { println("Altura de rama registrada: $it cm") }
```

**`map` (Transformar datos)**

Transforma cada elemento de la colección aplicando una regla matemática o lógica y **devuelve una nueva lista con los resultados**.

```kotlin
// Simula el crecimiento de 5 cm en todas las plantas
val alturasCrecidas = alturasCm.map { it + 5 } 
println(alturasCrecidas) // [17, 50, 85, 10, 115, 35]
```

**`mapIndexed` (Transformar usando el índice)**

Es idéntico a `map`, pero además de darte el elemento (`it`), te proporciona su posición o índice en la colección.

```kotlin
val codigosMaceta = alturasCm.mapIndexed { index, altura -> "Maceta #${index + 1}: $altura cm" }
println(codigosMaceta) // [Maceta #1: 12 cm, Maceta #2: 45 cm, ...]
```

**`filter` (Filtrar elementos)**

Filtra la colección conservando **únicamente** aquellos elementos que cumplan una condición (es decir, que la lambda devuelva `true`).

```kotlin
// Filtramos solo aquellas plantas listas para trasplante (que midan más de 30 cm)
val listasTrasplante = alturasCm.filter { it > 30 }
println(listasTrasplante) // [45, 80, 110]
```

**`sumOf` (Sumar transformaciones)**
Suma los valores de la colección aplicando opcionalmente una transformación o un factor de escala.

```kotlin
val abonosLitros = doubleArrayOf(1.5, 2.0, 0.5)
// Sumamos los litros estimando un desperdicio del 10% por evaporación (factor 1.1)
val totalAbonoNecesario = abonosLitros.sumOf { it * 1.1 }
println("Volumen total requerido: $totalAbonoNecesario litros")
```

**`any`, `all` y `none` (Consultas lógicas)**

* **`any`:** Devuelve `true` si **al menos una** planta cumple la condición.
* **`all`:** Devuelve `true` si **todas** las plantas cumplen la condición.
* **`none`:** Devuelve `true` si **ninguna** planta cumple la condición (o si todas la incumplen).

```kotlin
val temperaturasZonas = doubleArrayOf(18.5, 22.0, 14.0)

val hayRiesgoHelada = temperaturasZonas.any { it < 5.0 } // false (ninguna baja de 5°C)
val todasZonasOptimas = temperaturasZonas.all { it >= 15.0 } // false (la de 14.0°C no lo cumple)
val ningunCalorExtremo = temperaturasZonas.none { it > 35.0 } // true (ninguna supera los 35°C)
```

**`reduce` y `fold` (Acumuladores)**

* **`reduce`:** Acumula los valores de la colección de izquierda a derecha. Utiliza el primer elemento de la lista como valor inicial acumulado de forma automática.
* **`fold`:** Es idéntico a `reduce`, pero te permite **definir un valor inicial de partida** personalizado para el acumulador.

```kotlin
val nuevosBrotes = intArrayOf(5, 8, 12)

// Sumamos los nuevos brotes partiendo de un stock inicial de 100 plantas ya existentes
val inventarioTotal = nuevosBrotes.fold(100) { acumulador, brotes -> acumulador + brotes }
println("Total inventario invernadero: $inventarioTotal") // 100 + 5 + 8 + 12 = 125
```



## 4. ¿Qué es exactamente la palabra reservada `it`?

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



**Tabla Resumen de operaciones con Lambdas**

| Operación | Método en Kotlin | ¿Qué devuelve? |
| :--- | :--- | :--- |
| **Recorrer** | `forEach { ... }` | `Unit` (sin valor, solo ejecuta acciones). |
| **Transformar** | `map { ... }`, `mapIndexed { ... }` | Una nueva `List<T>` con los elementos transformados. |
| **Filtrar** | `filter { ... }` | Una nueva `List<T>` filtrada con los que cumplen la condición. |
| **Acumular** | `reduce { ... }`, `fold(valorInicial) { ... }` | Un valor del mismo tipo que el acumulador (`T`). |
| **Consultar** | `any { ... }`, `all { ... }`, `none { ... }` | Un valor de tipo lógico (`Boolean`). |



---

<span class="mi_h3">Autoría</span>

Obra realizada por Begoña Paterna Lluch. Publicada bajo licencia [Creative Commons Atribución/Reconocimiento-CompartirIgual 4.0 Internacional](https://creativecommons.org/licenses/by-sa/4.0/)

---