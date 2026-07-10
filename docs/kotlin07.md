# 7. Programación Orientada a Objetos (POO)


<span class="mi_h3">Revisiones</span>

| Revisión | Fecha      | Descripción                             |
|----------|------------|-----------------------------------------|
| 1.0      | 29-06-2026 | Adaptación de los materiales a markdown |
| 1.1      | 10-07-2026 | Renumeración de apartados |


## 7.1. Introducción

La **Programación Orientada a Objetos** es un paradigma que te permite estructurar tus programas simulando elementos del mundo real.

Para entenderlo, imagina una **planta** real en tu invernadero:
* Tiene características o **propiedades** (especie, altura, nivel de humedad, edad).
* Puede realizar acciones o **métodos** (crecer, florecer, marchitarse).

En la POO, utilizamos las **clases** como moldes o planos de diseño, y los **objetos** como los ejemplares reales (instancias) creados a partir de ese molde.



## 7.2. Clases, constructores y atributos

Kotlin simplifica enormemente la creación de clases en comparación con otros lenguajes. Dispones de tres formas de estructurar tus constructores según la complejidad que busques.

**Ejemplo 1: Constructor primario (La forma recomendada).** Define las propiedades directamente en la cabecera de la clase.

```kotlin
class Planta(val especie: String, var alturaCm: Double) {
    
    // Método de la clase
    fun imprimirFicha() {
        println("Especie: $especie | Altura: $alturaCm cm")
    }

    fun necesitaRiego(humedad: Int): Boolean {
        return humedad < 40
    }
}

fun main() {
    println("=== EJEMPLO 1: CONSTRUCTOR PRIMARIO ===")
    
    // Creamos dos objetos (instancias) de la clase Planta
    val planta1 = Planta("Monstera deliciosa", 45.0)
    val planta2 = Planta("Cactus de Pascua", 12.0)

    planta1.imprimirFicha()
    println("¿Necesita riego planta 2? ${planta2.necesitaRiego(25)}")
}
```

Salida por consola:

```text
=== EJEMPLO 1: CONSTRUCTOR PRIMARIO ===
Especie: Monstera deliciosa | Altura: 45.0 cm
¿Necesita riego planta 2? true
```


**Ejemplo 2: Propiedades dentro del cuerpo.** Si quieres dar valores por defecto por omisión sin que se pasen obligatoriamente en el constructor.

```kotlin
class Flor() {
    var especie: String = "Desconocida"
    var colorPetalos: String = "Blanco"
}

fun main() {
    println("=== EJEMPLO 2: PROPIEDADES EN EL CUERPO ===")

    // 1. Instanciamos una flor sin pasar parámetros (se aplican los valores por defecto)
    val florPredeterminada = Flor()
    println("Flor por defecto -> Especie: ${florPredeterminada.especie}, Color: ${florPredeterminada.colorPetalos}")
    
    // 2. Creamos otra flor y modificamos sus propiedades de forma manual
    val miFlor = Flor()
    miFlor.especie = "Tulipán"
    miFlor.colorPetalos = "Rojo"

    println("Flor modificada -> Especie: ${miFlor.especie}, Color: ${miFlor.colorPetalos}")
    
}
```

Salida por consola:

```text
=== EJEMPLO 2: PROPIEDADES EN EL CUERPO ===
Flor por defecto -> Especie: Desconocida, Color: Blanco
Flor modificada -> Especie: Tulipán, Color: Rojo
```


**Ejemplo 3: Constructor secundario.** A veces necesitas ofrecer varias formas de crear el objeto. Se utiliza la palabra reservada `constructor` y este debe delegar en el constructor primario usando `: this(...)`.

```kotlin
class PlantaExotica(val especie: String, var alturaCm: Double, val paisOrigen: String) {
    
    // Constructor secundario que asume origen desconocido si no se especifica
    constructor(especie: String, alturaCm: Double) : this(especie, alturaCm, "Desconocido")
}

fun main() {
    println("=== EJEMPLO 3: CONSTRUCTOR SECUNDARIO ===")

    // 1. Usamos el constructor primario (pasamos los 3 argumentos necesarios)
    val plantaConOrigen = PlantaExotica("Orquídea de Madagascar", 35.0, "Madagascar")

    println("Planta 1 (Constructor Primario):")
    println("- Especie: ${plantaConOrigen.especie}")
    println("- Altura: ${plantaConOrigen.alturaCm} cm")
    println("- Origen: ${plantaConOrigen.paisOrigen}") // Imprime: Madagascar

    println("-------------------------------------------")

    // 2. Usamos el constructor secundario (solo pasamos 2 argumentos)
    // El constructor secundario delegará en el primario y asignará "Desconocido" al país
    val plantaOrigenDesconocido = PlantaExotica("Cactus Cebra", 12.5)

    println("Planta 2 (Constructor Secundario):")
    println("- Especie: ${plantaOrigenDesconocido.especie}")
    println("- Altura: ${plantaOrigenDesconocido.alturaCm} cm")
    println("- Origen: ${plantaOrigenDesconocido.paisOrigen}") // Imprime: Desconocido
}
```

Salida por consola:

```text
=== EJEMPLO 3: CONSTRUCTOR SECUNDARIO ===
Planta 1 (Constructor Primario):
- Especie: Orquídea de Madagascar
- Altura: 35.0 cm
- Origen: Madagascar
-------------------------------------------
Planta 2 (Constructor Secundario):
- Especie: Cactus Cebra
- Altura: 12.5 cm
- Origen: Desconocido
```

## 7.3. Propiedades, getters y setters

En Kotlin, todas las propiedades mutables (`var`) generan automáticamente un **getter** (para leer su valor) y un **setter** (para modificarlo) por detrás. Si necesitas aplicar lógica de negocio o validación al asignar o leer un dato, puedes personalizarlos usando la variable implícita `field` (que hace referencia al campo físico de memoria).

**Ejemplo 4: Validación personalizada de humedad.** Evitamos que se asigne un porcentaje de humedad que esté fuera del rango lógico del 0% al 100%.

```kotlin
class SensorSuelo {
    var porcentajeHumedad: Int = 50
        set(value) {
            // Validamos que el porcentaje sea coherente
            field = if (value in 0..100) value else 50
        }
}

fun main() {
    println("=== EJEMPLO 4: VALIDACIÓN PERSONALIZADA ===")

    // 1. Instanciamos el objeto del sensor
    val miSensor = SensorSuelo()
    println("Humedad inicial del suelo: ${miSensor.porcentajeHumedad}%") // Imprime: 50%

    println("-------------------------------------------")

    // 2. Intentamos asignar un valor correcto (dentro del rango 0..100)
    miSensor.porcentajeHumedad = 75
    println("Asignando 75%...")
    println("Humedad registrada: ${miSensor.porcentajeHumedad}%") // Imprime: 75%

    println("-------------------------------------------")

    // 3. Intentamos asignar un valor incorrecto (por encima del 100%)
    // El setter interceptará este valor, detectará que está fuera de rango y aplicará el fallback de 50
    miSensor.porcentajeHumedad = 150
    println("Intentando asignar 150% (Valor fuera de rango)...")
    println("Humedad registrada: ${miSensor.porcentajeHumedad}%") // Imprime: 50%

    println("-------------------------------------------")

    // 4. Intentamos asignar otro valor incorrecto (negativo)
    miSensor.porcentajeHumedad = -10
    println("Intentando asignar -10% (Valor fuera de rango)...")
    println("Humedad registrada: ${miSensor.porcentajeHumedad}%") // Imprime: 50%
}
```


Salida por consola:

```text
=== EJEMPLO 4: VALIDACIÓN PERSONALIZADA ===
Humedad inicial del suelo: 50%
-------------------------------------------
Asignando 75%...
Humedad registrada: 75%
-------------------------------------------
Intentando asignar 150% (Valor fuera de rango)...
Humedad registrada: 50%
-------------------------------------------
Intentando asignar -10% (Valor fuera de rango)...
Humedad registrada: 50%
```

**Ejemplo 5: Propiedad de solo lectura con cálculo automático.** Si declaras una propiedad con `val`, solo tendrá *getter*. Es útil para campos calculados dinámicamente.

```kotlin
class Invernadero(val largoMetros: Double, val anchoMetros: Double) {
    // Getter personalizado. No almacena el dato en memoria, lo calcula al consultarlo
    val superficieMetrosCuadrados: Double
        get() = largoMetros * anchoMetros
}

fun main() {
    println("=== EJEMPLO 5: PROPIEDAD SOLO LECTURA Y CÁLCULO AUTOMÁTICO ===")
    val miInvernadero = Invernadero(10.0, 4.0)
    println("Superficie útil: ${miInvernadero.superficieMetrosCuadrados} m²") // 40.0 m²
}
```


Salida por consola:

```text
=== EJEMPLO 5: PROPIEDAD SOLO LECTURA Y CÁLCULO AUTOMÁTICO ===
Superficie útil: 40.0 m²
```

**Ejemplo 6: Setter privado (Encapsulamiento).** Si quieres que una propiedad pueda leerse públicamente desde cualquier parte de tu código, pero que **solo pueda modificarse desde dentro de la propia clase**, puedes privatizar su *setter*.

* **Metáfora:** La edad en días de una planta solo debería incrementarse mediante un método controlado, nunca asignándole valores arbitrarios desde fuera.

```kotlin
class PlantaInvernadero(val especie: String) {
    var edadDias: Int = 0
        private set // El setter es privado, nadie puede hacer 'planta.edadDias = 90' desde fuera

    fun pasarDia() {
        edadDias++ // Solo la propia clase puede modificarlo
    }
}

fun main() {
    println("=== EJEMPLO 6: SETTER PRIVADO (ENCAPSULAMIENTO) ===")

    // 1. Instanciamos la planta
    val miPlanta = PlantaInvernadero("Helecho Espada")

    // 2. Comprobamos que podemos LEER la propiedad de forma pública (el getter es público)
    println("Especie: ${miPlanta.especie}")
    println("Edad inicial de la planta: ${miPlanta.edadDias} días") // Imprime: 0 días

    println("--------------------------------------------------")

    // 3. Simulamos el paso del tiempo usando el método público controlado
    miPlanta.pasarDia()
    miPlanta.pasarDia()
    miPlanta.pasarDia()

    println("Tras simular el paso del tiempo...")
    println("Edad actual de la planta: ${miPlanta.edadDias} días") // Imprime: 3 días

    println("--------------------------------------------------")

    // 4. Intento de modificación directa desde fuera (DESCOMENTAR PARA PROBAR EL ERROR)
    // Si tus alumnos quitan las barras de comentario de la línea de abajo, el proyecto NO compilará.
    // miPlanta.edadDias = 45 

    println("Nota: Si intentas hacer 'miPlanta.edadDias = 45',")
    println("IntelliJ mostrará el error: 'Cannot assign to 'edadDias': the setter is private in...'")
}
```


Salida por consola:

```text
=== EJEMPLO 6: SETTER PRIVADO (ENCAPSULAMIENTO) ===
Especie: Helecho Espada
Edad inicial de la planta: 0 días
--------------------------------------------------
Tras simular el paso del tiempo...
Edad actual de la planta: 3 días
--------------------------------------------------
Nota: Si intentas hacer 'miPlanta.edadDias = 45',
IntelliJ mostrará el error: 'Cannot assign to 'edadDias': the setter is private in...'
```



## 7.4. Relación entre clases

En el mundo real, los objetos contienen otros objetos o se relacionan entre sí. La composición (o relación entre clases) consiste en definir propiedades dentro de una clase que son instancias de otra clase.

**Ejemplo 7: Relación entre clases.**

```kotlin
class Maceta(val material: String, val volumenLitros: Double)

class PlantaDomestica(val especie: String, val contenedor: Maceta) {
    fun describirCultivo() {
        println("La planta '$especie' está plantada en una maceta de ${contenedor.material} de ${contenedor.volumenLitros} litros.")
    }
}

fun main() {
    println("=== EJEMPLO 7: RELACIÓN ENTRE CLASES ===")
    val macetaBarro = Maceta("Barro cocido", 5.0)
    val helecho = PlantaDomestica("Helecho real", macetaBarro)
    helecho.describirCultivo()
}
```

Salida por consola:

```text
=== EJEMPLO 7: RELACIÓN ENTRE CLASES ===
La planta 'Helecho real' está plantada en una maceta de Barro cocido de 5.0 litros.
```


## 7.5. Modificadores de acceso

En Kotlin, todo es público por defecto. No obstante, puedes restringir la visibilidad de tus clases, métodos y propiedades para proteger la integridad de tus datos.

| Modificador | Visibilidad |
| :--- | :--- |
| **`public`** *(por defecto)* | Accesible desde cualquier parte del proyecto. |
| **`private`** | Accesible únicamente dentro de la misma clase o archivo que lo contiene. |
| **`protected`** | Accesible dentro de la misma clase y de sus subclases (herencia). |
| **`internal`** | Accesible dentro del mismo módulo de compilación (útil en librerías). |



## 7.6. Herencia y polimorfismo

En Kotlin, por seguridad, **todas las clases son cerradas (`final`) por defecto**, lo que significa que no se pueden heredar a menos que las marques explícitamente con la palabra reservada **`open`**. Los métodos que desees sobrescribir en las subclases también deben ser `open`.

**Ejemplo 8: Jerarquía de plantas.**

```kotlin
// Clase base marcada como open
open class Vegetal(val especie: String) {
    // Método open para permitir que las subclases lo personalicen
    open fun regar() {
        println("Regando $especie de manera estándar.")
    }
}

// Subclase Helecho que hereda de Vegetal
class Helecho : Vegetal("Helecho espada") {
    // Sobrescribimos el comportamiento específico de riego
    override fun regar() {
        super.regar() // Ejecuta el riego estándar opcionalmente
        println("- Cuidado extra: Pulverizar agua sobre las frondas para emular humedad alta.")
    }
}

// Subclase Cactus que hereda de Vegetal
class Cactus : Vegetal("Cactus de barril") {
    override fun regar() {
        println("Regar $especie con muy poca agua, garantizando suelo seco entre riegos.")
    }
}

fun main() {
    println("=== EJEMPLO 8: JERARQUÍA DE PLANTAS (HERENCIA) ===")

    // 1. Instanciamos un objeto de cada clase
    val vegetalGenerico = Vegetal("Planta silvestre")
    val miHelecho = Helecho()
    val miCactus = Cactus()

    println("\n--- 1. Llamadas Individuales Controladas ---")

    // Llamada a la clase base
    vegetalGenerico.regar()
    println("-------------------------------------------")

    // Llamada a Helecho (reutiliza super y añade lógica propia)
    miHelecho.regar()
    println("-------------------------------------------")

    // Llamada a Cactus (reemplaza por completo el comportamiento)
    miCactus.regar()
    println("-------------------------------------------")


    println("\n--- 2. Demostración de Polimorfismo en Colecciones ---")
    // Agrupamos todos los objetos en una lista de tipo List<Vegetal>
    val inventarioJardin: List<Vegetal> = listOf(vegetalGenerico, miHelecho, miCactus)

    for (planta in inventarioJardin) {
        // El compilador sabe de forma dinámica qué versión de 'regar()' debe ejecutar para cada elemento
        planta.regar()
        println("-----------------------------------")
    }
}
```

Salida por consola:

```text
=== EJEMPLO 8: JERARQUÍA DE PLANTAS (HERENCIA) ===

--- 1. Llamadas Individuales Controladas ---
Regando Planta silvestre de manera estándar.
-------------------------------------------
Regando Helecho espada de manera estándar.
- Cuidado extra: Pulverizar agua sobre las frondas para emular humedad alta.
-------------------------------------------
Regar Cactus de barril con muy poca agua, garantizando suelo seco entre riegos.
-------------------------------------------

--- 2. Demostración de Polimorfismo en Colecciones ---
Regando Planta silvestre de manera estándar.
-------------------------------------------
Regando Helecho espada de manera estándar.
- Cuidado extra: Pulverizar agua sobre las frondas para emular humedad alta.
-------------------------------------------
Regar Cactus de barril con muy poca agua, garantizando suelo seco entre riegos.
-------------------------------------------
```


**Ejemplo 9: Polimorfismo en colecciones de objetos.** El polimorfismo te permite tratar objetos de diferentes subclases como si fueran del tipo de la clase base común, ejecutando el método correcto correspondiente a cada instancia en tiempo de ejecución.

En el ejemplo anterior demostramos el polimorfismo usando una lista dinámica (List). A continuación, veremos que el comportamiento polimórfico es idéntico si agrupamos los objetos en un array de tamaño fijo.

```kotlin
fun main() {
    println("=== EJEMPLO 9: POLIMORFISMO EN COLECCIONES DE OBJETOS ===")
    
    // Array polimórfico que almacena diferentes vegetales
    val jardin: Array<Vegetal> = arrayOf(
        Helecho(),
        Cactus(),
        Vegetal("Planta genérica")
    )

    // Recorremos el jardín y cada planta responde de forma única al riego
    for (planta in jardin) {
        planta.regar()
        println("-----------------------------------")
    }
}
```

Salida por consola:

```text
=== EJEMPLO 9: POLIMORFISMO EN COLECCIONES DE OBJETOS ===
Regando Helecho espada de manera estándar.
- Cuidado extra: Pulverizar agua sobre las frondas para emular humedad alta.
-----------------------------------
Regar Cactus de barril con muy poca agua, garantizando suelo seco entre riegos.
-----------------------------------
Regando Planta genérica de manera estándar.
-----------------------------------
```

## 7.7. Data classes

Las clases de datos (`data class`) son clases diseñadas específicamente para **almacenar información**. Al anteponer la palabra `data` a una clase, el compilador de Kotlin genera automáticamente por detrás una serie de funciones de utilidad:
* Un formato de texto legible para consola (`toString()`).
* Comparación de igualdad basada en sus datos internos (`equals()`).
* Generador de códigos únicos de dispersión (`hashCode()`).
* Clonación del objeto permitiendo modificar propiedades específicas (`copy()`).

**Ejemplo 10: Data classes.**

```kotlin
// Clase de datos para modelar semillas
data class LoteSemillas(val variedad: String, val cantidadSemillas: Int, val anyoCosecha: Int)

fun main() {
    println("=== EJEMPLO 10: DATA CLASSES ===")
    
    val lote1 = LoteSemillas("Girasol gigante", 150, 2025)
    val lote2 = LoteSemillas("Girasol gigante", 150, 2025)

    // 1. toString() automático
    println(lote1) // Salida legible: LoteSemillas(variedad=Girasol gigante, cantidadSemillas=150, anyoCosecha=2025)

    // 2. equals() automático (compara valores, no referencias de memoria)
    println("¿Son lotes idénticos? ${lote1 == lote2}") // Salida: true

    // 3. copy() automático (clona el lote pero actualizando el año de cosecha)
    val loteNuevoAnual = lote1.copy(anyoCosecha = 2026)
    println(loteNuevoAnual)
}
```


Salida por consola:

```text
=== EJEMPLO 10: DATA CLASSES ===
LoteSemillas(variedad=Girasol gigante, cantidadSemillas=150, anyoCosecha=2025)
¿Son lotes idénticos? true
LoteSemillas(variedad=Girasol gigante, cantidadSemillas=150, anyoCosecha=2026)
```

## 7.8. Sobrecarga de operadores

Kotlin te permite personalizar el comportamiento de operadores matemáticos o lógicos (`+`, `-`, `*`, `==`) cuando interactúan con tus propios objetos. Para ello, debes definir funciones utilizando la palabra clave **`operator`**.

* **Metáfora botánica:** Si sumas dos sacos de tierra con diferentes litros, el resultado debería ser un nuevo saco de tierra con la suma total del volumen.

**Ejemplo 11: Sobrecarga de operadores.**

```kotlin
data class SacoTierra(val volumenLitros: Double) {
    // Sobrecargamos el operador '+' (asociado a la función 'plus')
    operator fun plus(otroSaco: SacoTierra): SacoTierra {
        return SacoTierra(this.volumenLitros + otroSaco.volumenLitros)
    }
}

fun main() {
    println("=== EJEMPLO 11: SOBRECARGA DE OPERADORES ===")
    
    val sacoPequenyo = SacoTierra(15.0)
    val sacoGrande = SacoTierra(50.0)

    // Usamos el operador '+' directamente de forma intuitiva
    val mezclaSustrato = sacoPequenyo + sacoGrande

    println("Has combinado la tierra. El saco resultante tiene: ${mezclaSustrato.volumenLitros} litros.") 
    // Salida: 65.0 litros
}
```

Salida por consola:

```text
=== EJEMPLO 11: SOBRECARGA DE OPERADORES ===
Has combinado la tierra. El saco resultante tiene: 65.0 litros.
```


## 7.9. Objetos sin clases

En ocasiones, necesitas una estructura que actúe como un gestor o base de datos centralizada de la cual **solo debe existir una única instancia en toda tu aplicación**. En Kotlin, esto se resuelve de forma directa sin necesidad de configurar patrones complejos de diseño; simplemente declaras un **`object`** en lugar de una `class`.

**Ejemplo 12: Objetos sin clases.**

```kotlin
data class FlorExotica(val nombre: String, val color: String)

// Singleton para el registro de especies raras del Jardín Botánico
object RegistroBotanicoUnico {
    private val floresRegistradas = mutableListOf<FlorExotica>()

    fun anyadirFlor(flor: FlorExotica) {
        floresRegistradas.add(flor)
        println("Flor '${flor.nombre}' anyadida al herbario nacional.")
    }

    fun mostrarHerbario() {
        println("--- HERBARIO OFICIAL ---")
        floresRegistradas.forEach { println("- ${it.nombre} (${it.color})") }
    }
}

fun main() {
    println("=== EJEMPLO 12: OBJETOS SIN CLASES ===")
    
    // No usamos constructores ni 'new'. Accedemos directamente por su nombre
    RegistroBotanicoUnico.anyadirFlor(FlorExotica("Orquídea Negra", "Negro azabache"))
    RegistroBotanicoUnico.anyadirFlor(FlorExotica("Flor de Jade", "Turquesa"))

    RegistroBotanicoUnico.mostrarHerbario()
}
```

Salida por consola:

```text
=== EJEMPLO 12: OBJETOS SIN CLASES ===
Flor 'Orquídea Negra' anyadida al herbario nacional.
Flor 'Flor de Jade' anyadida al herbario nacional.
--- HERBARIO OFICIAL ---
- Orquídea Negra (Negro azabache)
- Flor de Jade (Turquesa)
```

## 7.10. Jerarquía de clases y arrays de objetos

Utilizaremos esta jerarquía de clases base para los ejemplos:

```kotlin
// Clase base
open class Vegetal(val nombre: String) {
    open fun aplicarCuidados() {
        println("Aplicando cuidados estándar para la planta '$nombre'.")
    }
}

// Subclase Flor
class Flor(nombre: String, val colorPetalos: String) : Vegetal(nombre) {
    override fun aplicarCuidados() {
        println("Riego moderado para la flor '$nombre' y protección de sus pétalos de color $colorPetalos.")
    }

    // Método específico de esta subclase
    fun fertilizarFloracion() {
        println("¡Aplicando abono especial de fósforo para estimular la floración de la flor '$nombre'!")
    }
}

// Subclase Arbol
class Arbol(nombre: String, val alturaMaxMetros: Double) : Vegetal(nombre) {
    override fun aplicarCuidados() {
        println("Poda estructural para el árbol '$nombre' (Altura máxima esperada: $alturaMaxMetros m).")
    }
}
```

**Ejemplo 13: Array de objetos.** En este ejemplo verás cómo declarar un array de tamaño fijo que almacena diferentes tipos de vegetales, cómo recorrerlo aprovechando el polimorfismo, cómo modificar elementos por índice y cómo inicializar un array vacío para llenarlo más tarde.

```kotlin
fun main() {
    println("=== EJEMPLO 13: ARRAY DE OBJETOS ===")
    
    // 1. Declaramos un array con diferentes subclases de Vegetal
    val jardin: Array<Vegetal> = arrayOf(
        Flor("Rosa", "Rojo"),
        Arbol("Pino", 20.0),
        Flor("Girasol", "Amarillo"),
        Vegetal("Planta silvestre")
    )

    // 2. Recorremos el array llamando al método polimórfico
    println("--- Cuidados diarios del jardín ---")
    for (planta in jardin) {
        planta.aplicarCuidados() // Llama automáticamente al método correcto de cada clase
    }

    println("\n--- Operaciones de mantenimiento por índice ---")
    // Acceso directo a un elemento por su índice
    jardin[1].aplicarCuidados() // Llama al cuidado del Pino

    // Modificar un elemento del array por otro objeto
    jardin[3] = Arbol("Roble joven", 15.0)
    println("Se ha reemplazado la planta silvestre por un Roble.")

    // 3. Crear un array inicialmente vacío de tamaño fijo y luego asignarle elementos
    val estanteInvernadero = arrayOfNulls<Vegetal>(3) // Array de tamaño 3 que permite nulos
    estanteInvernadero[0] = Flor("Tulipán", "Rosa")
    estanteInvernadero[1] = Arbol("Bonsái de Olivo", 1.5)
    // El índice 2 quedará como null hasta que asignemos un objeto
}
```

Salida por consola:

```text
=== EJEMPLO 13: ARRAY DE OBJETOS ===
--- Cuidados diarios del jardín ---
Riego moderado para la flor 'Rosa' y protección de sus pétalos de color Rojo.
Poda estructural para el árbol 'Pino' (Altura máxima esperada: 20.0 m).
Riego moderado para la flor 'Girasol' y protección de sus pétalos de color Amarillo.
Aplicando cuidados estándar para la planta 'Planta silvestre'.

--- Operaciones de mantenimiento por índice ---
Poda estructural para el árbol 'Pino' (Altura máxima esperada: 20.0 m).
Se ha reemplazado la planta silvestre por un Roble.
```


**Ejemplo 14: ArrayList de objetos y filtrado.** En este ejemplo trabajarás con una lista dinámica (`MutableList`), que es el equivalente nativo en Kotlin al `ArrayList` de Java. Verás cómo llenarla y cómo utilizar la función **`filterIsInstance`** para extraer únicamente un tipo de objeto de la lista y ejecutar sus métodos específicos de clase.

```kotlin
fun main() {
    println("=== EJEMPLO 14: ARRAYLIST DE OBJETOS Y FILTRADO ===")
    
    // 1. Creamos una lista dinámica (MutableList) de vegetales
    val inventarioInvernadero = mutableListOf<Vegetal>(
        Flor("Orquídea", "Blanco"),
        Arbol("Naranjo", 4.0),
        Flor("Azalea", "Rosa"),
        Arbol("Limonero", 3.0)
    )

    // 2. Mostrar todas las plantas del invernadero
    println("--- Catálogo Completo del Invernadero ---")
    for (planta in inventarioInvernadero) {
        planta.aplicarCuidados()
    }

    // 3. FILTRADO CON filterIsInstance: Extraer SOLO las flores de la lista general
    println("\n--- Tarea especial: Abonar únicamente las flores ---")
    
    // Filtramos la lista para obtener una de tipo List<Flor>
    val soloFlores: List<Flor> = inventarioInvernadero.filterIsInstance<Flor>()
    
    for (flor in soloFlores) {
        // Al estar seguros de que son de tipo Flor, podemos usar sus métodos exclusivos
        flor.fertilizarFloracion() 
    }
}
```

Salida por consola:

```text
=== EJEMPLO 14: ARRAYLIST DE OBJETOS Y FILTRADO ===
--- Catálogo Completo del Invernadero ---
Riego moderado para la flor 'Orquídea' y protección de sus pétalos de color Blanco.
Poda estructural para el árbol 'Naranjo' (Altura máxima esperada: 4.0 m).
Riego moderado para la flor 'Azalea' y protección de sus pétalos de color Rosa.
Poda estructural para el árbol 'Limonero' (Altura máxima esperada: 3.0 m).

--- Tarea especial: Abonar únicamente las flores ---
¡Aplicando abono especial de fósforo para estimular la floración de la flor 'Orquídea'!
¡Aplicando abono especial de fósforo para estimular la floración de la flor 'Azalea'!
```


**Ejemplo 15: Operaciones funcionales adicionales en listas de objetos.** Kotlin permite simplificar el procesamiento de estas listas de objetos utilizando las expresiones lambda.

```kotlin
fun main() {
    println("=== EJEMPLO 15: OPERACIONES ADICIONALES EN LISTAS DE OBJETOS ===")
    
    val invernadero = listOf(
        Flor("Orquídea", "Blanco"),
        Arbol("Naranjo", 4.0),
        Flor("Azalea", "Rosa")
    )

    // A) Recorrer de forma compacta con forEach
    invernadero.forEach { it.aplicarCuidados() }

    // B) Transformar los objetos en una lista de nombres con map
    val nombresEspecies: List<String> = invernadero.map { it.nombre }
    println("\nListado de especies: $nombresEspecies") 
    // Salida: [Orquídea, Naranjo, Azalea]
}
```

Salida por consola:

```text
=== EJEMPLO 15: OPERACIONES ADICIONALES EN LISTAS DE OBJETOS ===
Riego moderado para la flor 'Orquídea' y protección de sus pétalos de color Blanco.
Poda estructural para el árbol 'Naranjo' (Altura máxima esperada: 4.0 m).
Riego moderado para la flor 'Azalea' y protección de sus pétalos de color Rosa.

Listado de especies: [Orquídea, Naranjo, Azalea]
```

---
<span class="mi_h3">Autoría</span>

<span class="mi_autoria">
Obra realizada por Begoña Paterna Lluch. Publicada bajo licencia [Creative Commons Atribución/Reconocimiento-CompartirIgual 4.0 Internacional](https://creativecommons.org/licenses/by-sa/4.0/)
</span>
---