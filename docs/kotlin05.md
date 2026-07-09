# 5. Funciones


<span class="mi_h3">Revisiones</span>

| Revisión | Fecha      | Descripción                             |
|----------|------------|-----------------------------------------|
| 1.0      | 29-06-2026 | Adaptación de los materiales a markdown |



## 1. Introducción

Las **funciones** (conocidas como *métodos* en lenguajes como Java) son bloques de código independientes diseñados para realizar una tarea específica. Te sirven para organizar tu código, evitar la duplicación de instrucciones (principio *DRY - Don't Repeat Yourself*) y hacer que tus programas sean mucho más legibles y fáciles de mantener.



## 2. Definición y sintaxis básica

Para declarar una función en Kotlin, debes utilizar la palabra reservada **`fun`**, seguida del nombre de la función (en camello o *camelCase*), paréntesis para los parámetros (si los tiene) y llaves `{}` que delimitan las instrucciones de la función.

**Parámetro vs. Argumento**

Es común confundir estos términos al principio. Esta tabla te ayudará a diferenciarlos claramente:

| Concepto | ¿Qué es? | Ejemplo botánico |
| :--- | :--- | :--- |
| **Parámetro** | La variable que se define en la firma o declaración de la función. Actúa como un contenedor. | `fun regar(planta: String) { ... }`<br>*(aquí `planta` es el parámetro)* |
| **Argumento** | El valor real que le envías a la función cuando la llamas. | `regar("Girasol")`<br>*(aquí `"Girasol"` es el argumento)* |

**Ejemplo 1: Función simple sin parámetros ni retorno.** Ideal para realizar acciones fijas, como imprimir una cabecera de informe.

```kotlin
// Declaración de la función
fun mostrarCabeceraInvernadero() {
    println("=====================================")
    println("   SISTEMA DE CONTROL BOTÁNICO v2   ")
    println("=====================================")
}

fun main() {
    println("=== EJEMPLO 1: Función simple sin parámetros ni retorno ===")
    // Llamada a la función
    mostrarCabeceraInvernadero()
}
```

Salida por consola:

```text
=== EJEMPLO 1: Función simple sin parámetros ni retorno ===
=====================================
   SISTEMA DE CONTROL BOTÁNICO v2   
=====================================
```


## 3. Funciones con parámetros

Las funciones suelen necesitar información del exterior para realizar su trabajo. Puedes pasarle una o varias variables indicando siempre su nombre y su tipo de dato.

**Ejemplo 2: Función con un parámetro (sin retorno).**

```kotlin
fun registrarRegado(especie: String) {
    println("Acción: Se ha suministrado agua a la planta: $especie")
}

fun main() {
    println("=== EJEMPLO 2: Función con un parámetro (sin retorno) ===")
    // Al llamar a la función, le enviamos un argumento de tipo String
    registrarRegado("Orquídea")
    registrarRegado("Helecho")
}
```

Salida por consola:

```text
=== EJEMPLO 2: Función con un parámetro (sin retorno) ===
Acción: Se ha suministrado agua a la planta: Orquídea
Acción: Se ha suministrado agua a la planta: Helecho
```

## 4. Funciones que devuelven un valor

Si necesitas que una función realice un cálculo y te devuelva el resultado para poder guardarlo en una variable u operar con él, debes indicar el **tipo de retorno** al final de la cabecera (usando dos puntos `:`) y emplear la palabra clave **`return`** dentro de la función.

**Ejemplo 3: Calcular el consumo anual de agua.**

```kotlin
// Indicamos con ': Double' al final que esta función devolverá un número decimal
fun calcularConsumoAnual(litrosSemanales: Double): Double {
    val semanasAño = 52.0
    val totalConsumo = litrosSemanales * semanasAño
    return totalConsumo
}

fun main() {
    println("=== EJEMPLO 3: Calcular el consumo anual de agua ===")
    val consumoMacetas = calcularConsumoAnual(2.5)
    println("Consumo anual estimado para el lote de macetas: $consumoMacetas litros.")
}
```

Salida por consola:

```text
=== EJEMPLO 3: Calcular el consumo anual de agua ===
Consumo anual estimado para el lote de macetas: 130.0 litros.
```


## 5. Funciones simplificadas

Si una función tiene una sola línea de código que simplemente devuelve un valor, Kotlin te permite prescindir de las llaves `{}` y de la palabra `return`. En su lugar, puedes estructurarla utilizando el operador de asignación **`=`**.

**Ejemplo 4: Formatear nombre familiar de una especie.**

```kotlin
// Función simplificada de expresión única (single-expression function)
fun obtenerNombreFamiliar(nombreComun: String) = "Familia de las $nombreComun"

fun main() {
    println("=== EJEMPLO 4: Formatear nombre familiar de una especie ===")
    val texto = obtenerNombreFamiliar("Rosáceas")
    println(texto) // Salida: Familia de las Rosáceas
}
```

Salida por consola:

```text
=== EJEMPLO 4: Formatear nombre familiar de una especie ===
Familia de las Rosáceas
```


## 6. Parámetros con valores por defecto

En Kotlin, puedes asignar valores predeterminados a los parámetros en la firma de la función. Si al llamar a la función decides no enviar ese argumento, el programa utilizará automáticamente el valor por defecto que hayas configurado. Esto evita tener que escribir múltiples funciones sobrecargadas.

**Ejemplo 5: Registro de especies con ubicación predeterminada.**

```kotlin
// Por defecto, asumimos que las plantas se ubican en el 'Invernadero A'
fun registrarEspecie(nombre: String, ubicacion: String = "Invernadero A") {
    println("Especie: $nombre | Ubicación asignada: $ubicacion")
}

fun main() {
    println("=== EJEMPLO 5: Registro de especies con ubicación predeterminada ===")
    // Llamada usando el valor por defecto de la ubicación
    registrarEspecie("Cactus de Navidad") // Salida: Ubicación asignada: Invernadero A
    
    // Llamada especificando una ubicación diferente
    registrarEspecie("Monstera deliciosa", "Sector Sombra") // Salida: Ubicación asignada: Sector Sombra
}
```

Salida por consola:

```text
=== EJEMPLO 5: Registro de especies con ubicación predeterminada ===
Especie: Cactus de Navidad | Ubicación asignada: Invernadero A
Especie: Monstera deliciosa | Ubicación asignada: Sector Sombra
```


## 7. Argumentos nombrados

Cuando llamas a una función que tiene muchos parámetros (algunos de ellos con valores por defecto), puede ser confuso recordar el orden exacto de las variables. Kotlin te permite solucionar esto indicando explícitamente el nombre del parámetro al que le estás asignando el valor. Además, te permite cambiar el orden de los argumentos en la llamada.

**Ejemplo 6: Programación de tareas de abono.**

```kotlin
fun programarTratamiento(especie: String, mililitros: Double, repetirSemanas: Int = 1) {
    println("Tratamiento programado para '$especie':")
    println("- Cantidad: $mililitros ml")
    println("- Duración: $repetirSemanas semana(s)")
}

fun main() {
    println("=== EJEMPLO 6: Programación de tareas de abono ===")
    // 1. Llamada tradicional (respetando el orden estricto de los parámetros)
    programarTratamiento("Bonsái Ficus", 15.0, 4)
    
    // 2. Llamada utilizando argumentos nombrados en desorden (facilita la legibilidad)
    programarTratamiento(
        mililitros = 25.5, 
        especie = "Hiedra inglesa", 
        repetirSemanas = 2
    )
    
    // 3. Llamada combinando argumentos nombrados y valores por defecto
    programarTratamiento("Sansevieria", mililitros = 10.0) // Utiliza el valor por defecto: 1 semana
}
```

Salida por consola:

```text
=== EJEMPLO 6: Programación de tareas de abono ===
Tratamiento programado para 'Bonsái Ficus':
- Cantidad: 15.0 ml
- Duración: 4 semana(s)
Tratamiento programado para 'Hiedra inglesa':
- Cantidad: 25.5 ml
- Duración: 2 semana(s)
Tratamiento programado para 'Sansevieria':
- Cantidad: 10.0 ml
- Duración: 1 semana(s)
```


## 8. Funciones de extensión

Las **funciones de extensión** te permiten añadir nuevas funciones a clases ya existentes (sean clases tuyas, de librerías de terceros, o clases estándar de Kotlin como `String`, `Int` o `Double`) **sin necesidad de heredar de ellas ni modificar su código original**.

Para definirlas, debes anteponer el nombre de la clase que quieres extender al nombre de tu nueva función, separado por un punto `.`. Dentro de la función, la palabra clave **`this`** hace referencia al objeto real sobre el que se hace la llamada.


**Ejemplo 7: Extensión sobre la clase estándar `String`.** Vamos a crear una función para formatear los nombres de las especies eliminando espacios innecesarios y poniendo la primera letra en mayúscula de forma segura:

```kotlin
// Extendemos la clase estándar String
fun String.formatoEspecie(): String {
    // 'this' es el String original sobre el que aplicamos la función
    return this.trim().lowercase().replaceFirstChar { it.uppercase() }
}

fun main() {
    println("=== EJEMPLO 7: Extensión sobre la clase estándar `String` ===")
    val entradaSucia = "  mOnStErA dElIcIoSa  "
    // Usamos nuestra nueva función como si siempre hubiera existido en los Strings
    val especieLimpia = entradaSucia.formatoEspecie()
    
    println(especieLimpia) // Salida: Monstera deliciosa
}
```

Salida por consola:

```text
=== EJEMPLO 7: Extensión sobre la clase estándar `String` ===
Monstera deliciosa
```


**Ejemplo 8: Extensión sobre tu propia clase personalizada.**

```kotlin
class Maceta(val material: String, val volumenLitros: Double)

// Añadimos una función de extensión a nuestra clase Maceta
fun Maceta.mostrarDetalles(): String {
    return "Contenedor de $material con capacidad de $volumenLitros litros de sustrato."
}

fun main() {
    println("=== EJEMPLO 8: Extensión sobre tu propia clase personalizada ===")
    val miMaceta = Maceta("Terracota", 12.5)
    println(miMaceta.mostrarDetalles())
}
```

Salida por consola:

```text
=== EJEMPLO 8: Extensión sobre tu propia clase personalizada ===
Contenedor de Terracota con capacidad de 12.5 litros de sustrato.
```


## 9. Funciones con cantidad variable de argumentos

A veces necesitas que una función pueda recibir un número indeterminado de argumentos del mismo tipo (por ejemplo, registrar tres plantas en una tanda, o registrar diez). Para no verte obligado a crear una lista o un array previamente, puedes usar la palabra reservada **`vararg`** en la definición del parámetro.

Internamente, Kotlin tratará ese parámetro variable como si fuera un array, permitiéndote acceder a su tamaño (`.size`), recorrerlo o usar sus elementos.

**Ejemplo 9: Registro por lotes de plantas de exterior.**

```kotlin
// Declaramos un parámetro variable usando vararg
fun registrarLoteExteriores(vararg plantas: String) {
    println("--- REGISTRO DE PARCELA (Total: ${plantas.size} especies) ---")
    for (planta in plantas) {
        println("- Especie anyadida: $planta")
    }
}

fun main() {
    println("=== EJEMPLO 9: Registro por lotes de plantas de exterior ===")
    // 1. Llamada pasando argumentos individuales directamente
    registrarLoteExteriores("Lavanda", "Romero", "Tomillo")
    
    // 2. Pasar un array existente usando el OPERADOR SPREAD (*)
    // El asterisco '*' indica a Kotlin que debe "descomprimir" el array en argumentos individuales
    val plantasSocio = arrayOf("Menta", "Albahaca")
    registrarLoteExteriores(*plantasSocio)
}
```

Salida por consola:

```text
=== EJEMPLO 9: Registro por lotes de plantas de exterior ===
--- REGISTRO DE PARCELA (Total: 3 especies) ---
- Especie anyadida: Lavanda
- Especie anyadida: Romero
- Especie anyadida: Tomillo
--- REGISTRO DE PARCELA (Total: 2 especies) ---
- Especie anyadida: Menta
- Especie anyadida: Albahaca
```


## 10. Funciones locales

Una **función local** es una función que se declara **dentro de otra función**. Solo es visible y utilizable de manera interna por la función que la rodea. Te sirve para:
* Organizar y dividir el código de funciones excesivamente largas.
* Evitar la repetición de lógica que solo tiene sentido dentro de ese bloque concreto.
* Encapsular operaciones de validación de manera limpia.

> **Regla de oro:** Las funciones locales tienen acceso a las variables locales y parámetros de la función principal que las envuelve.

**Ejemplo 10: Registro de fichas botánicas con validación interna.**

```kotlin
fun registrarFichaBotanica(nombreComun: String, phSuelo: Double) {
    
    // Función local para validar que los nombres no tengan espacios de más
    fun limpiarTexto(texto: String): String {
        return texto.trim().uppercase()
    }
    
    val nombreLimpio = limpiarTexto(nombreComun)
    val estadoPh = if (phSuelo < 7.0) "Suelo ácido" else "Suelo alcalino/Neutro"
    
    println("Ficha generada -> [$nombreLimpio] | Diagnóstico: $estadoPh")
}

fun main() {
    println("=== EJEMPLO 10: Registro de fichas botánicas con validación interna ===")
    registrarFichaBotanica("   Hortensia azul  ", 5.2)
    // limpiarTexto("Test") // ¡Error! No puedes llamar a la función local desde aquí
}
```

Salida por consola:

```text
=== EJEMPLO 10: Registro de fichas botánicas con validación interna ===
Ficha generada -> [HORTENSIA AZUL] | Diagnóstico: Suelo ácido
```


## 11. Funciones de orden superior

Una **función de orden superior** es aquella que **recibe otra función como parámetro** o que **devuelve una función** como resultado. Es un concepto clave de la programación funcional que ya has estado utilizando de manera indirecta en el Bloque 9 al trabajar con colecciones.

**Ejemplo 11: Calculadora dinámica de dosificación de abono.** Vamos a crear una función que aplica una mezcla de agua y abono líquido, pero delega la fórmula matemática exacta a una función lambda externa que recibe como parámetro:

```kotlin
// El tercer parámetro es una función 'operacion' que toma dos decimales y devuelve otro decimal
fun aplicarTratamiento(aguaLitros: Double, abonoMl: Double, formula: (Double, Double) -> Double): Double {
    return formula(aguaLitros, abonoMl)
}

fun main() {
    println("=== EJEMPLO 11: Calculadora dinámica de dosificación de abono ===")
    // 1. Definimos una fórmula para un riego suave (concentración baja)
    val concentracionSuave = aplicarTratamiento(5.0, 10.0) { agua, abono -> 
        (abono / agua) * 0.8 
    }
    
    // 2. Definimos otra fórmula para un riego intensivo de choque
    val concentracionIntensiva = aplicarTratamiento(5.0, 10.0) { agua, abono -> 
        (abono / agua) * 1.5 
    }

    println("Dosificación suave: $concentracionSuave mg/L")
    println("Dosificación intensiva: $concentracionIntensiva mg/L")
}
```

Salida por consola:

```text
=== EJEMPLO 11: Calculadora dinámica de dosificación de abono ===
Dosificación suave: 1.6 mg/L
Dosificación intensiva: 3.0 mg/L
```


---
<span class="mi_h3">Autoría</span>

<span class="mi_autoria">
Obra realizada por Begoña Paterna Lluch. Publicada bajo licencia [Creative Commons Atribución/Reconocimiento-CompartirIgual 4.0 Internacional](https://creativecommons.org/licenses/by-sa/4.0/)
</span>
---