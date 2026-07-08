# 1. Introducción y entorno de desarrollo


<span class="mi_h3">Revisiones</span>

| Revisión | Fecha      | Descripción                             |
|----------|------------|-----------------------------------------|
| 1.0      | 29-06-2026 | Adaptación de los materiales a markdown |



## 1. Instalación de IntelliJ IDEA Community

Para el desarrollo de nuestras aplicaciones utilizaremos el Entorno de Desarrollo Integrado (IDE) **IntelliJ IDEA** en su versión **Community Edition**.


<span class="mi_h3">1.1. Instalación en el ordenadores de clase</span>

Para instalar IntelliJ en el ordenador del aula, sigue estos pasos:

1. **Descargar los archivos:** Accede a la web oficial [https://www.jetbrains.com/idea/download/other/](https://www.jetbrains.com/idea/download/other/) y busca la Version 2025.2 Community Edition para Linux, luego descarga el archivo `.tar.gz`.
2. **Descomprimir y ubicar:** Extrae el archivo descargado (que habitualmente se guarda en la carpeta `Descargas`) y mueve la carpeta resultante a tu directorio de usuario (fuera de la carpeta temporal de descargas).
3. **Configurar permisos de ejecución:**
    * Navega hasta la carpeta `bin` dentro del directorio extraído.
    * Localiza el archivo ejecutable `idea.sh`.
    * Haz clic derecho sobre él, selecciona **Propiedades > Permisos** y asegúrate de que la opción **Permitir ejecutar el archivo como un programa** (o *Permet executar*) esté marcada.
    * Ejecuta el archivo desde la terminal o haciendo doble clic sobre él.

<span class="mi_h3">1.2. Instalación en Windows</span>

Para realizar la instalación en tu equipo personal con Windows:

1. **Descargar el instalador:** Accede a la web oficial [https://www.jetbrains.com/idea/download/other/](https://www.jetbrains.com/idea/download/other/) y busca la Version 2025.2 Community Edition para Windows, luego descarga el archivo `.exe`.
2. **Asistente de instalación:** Ejecuta el instalador descargado y sigue los pasos indicados en pantalla:
    * **Directorio de destino:** Por defecto se instalará en `C:\Program Files\JetBrains\...` (requiere aproximadamente 3.2 GB de espacio en disco).
    * **Opciones de instalación:** Se recomienda marcar la opción para crear un acceso directo en el escritorio y, opcionalmente, asociar las extensiones de archivo `.kt` (Kotlin).
    * **Menú Inicio:** Selecciona la carpeta de destino del menú (por defecto *JetBrains*) y pulsa **Install**.
3. **Primer inicio:** Al abrir la aplicación por primera vez, acepta los términos de uso y elige si deseas importar configuraciones previas en caso de que ya tuvieras otra versión instalada.



## 2. Gestión y configuración de proyectos

<span class="mi_h3">2.1. Organización del espacio de trabajo</span>

Antes de empezar a crear programas, es importante que mantengas tu espacio de trabajo organizado. Es recomendable que crees una carpeta raíz en tu unidad de almacenamiento (por ejemplo, una carpeta llamada `kot` dentro de tu unidad de trabajo) donde guardar de forma ordenada los proyectos del curso.

> **Consejo de personalización:** Puedes cambiar la apariencia del entorno pulsando en el icono de la **rueda dentada (Ajustes)** en la esquina inferior izquierda de la pantalla de bienvenida. Para estas explicaciones utilizaremos el modo de color claro (*Light Mode*).


<span class="mi_h3">2.2. Creación de un nuevo proyecto</span>

Para crear tu primer proyecto en Kotlin:

1. Abre IntelliJ IDEA y haz clic en **New Project** en la ventana de inicio.
2. Configura los parámetros en la ventana de configuración:
    * **Name:** Indica el nombre de tu ptoyecto (en el caso del ejemplo es `control_plantas`)
    * **Location:** Selecciona tu directorio de trabajo (por ejemplo, `F:\kot` o la ruta de tu carpeta de usuario).
    * **Language:** Asegúrate de marcar **Kotlin** en la columna de la izquierda.
    * **Build system:** Selecciona el sistema nativo de **IntelliJ**.
    * **JDK:** Selecciona la versión disponible de Java instalada en el sistema (por ejemplo, JDK 24 o la versión del aula).
    * **Opciones adicionales:** Asegúrate de tener activada la opción **Add sample code** (para disponer de un archivo inicial de ejemplo) y **Use compact project structure**.
3. Haz clic en **Create**.

<span class="mi_h3">2.3. Estructura del proyecto y creación de archivos</span>

Al crearse el proyecto, la interfaz mostrará una estructura de carpetas en el margen izquierdo:

* **`.idea` / `out`:** Carpetas de configuración interna del IDE y de compilación (no debes modificarlas manualmente).
* **`src` (Source):** Es la carpeta más importante. Aquí se almacena todo el código fuente de nuestra aplicación.
* **`Main.kt`:** Archivo que contiene el punto de entrada de nuestro programa.


Si necesitas crear una clase o un nuevo archivo de Kotlin en el futuro sigue estos pasos: 

* Haz clic derecho sobre la carpeta **`src`**. 
* Selecciona **New > Kotlin File/Class**. 
* Escribe el nombre deseado para el archivo y selecciona el tipo correspondiente (File, Class, Interface, etc.).


<span class="mi_h3">2.4. Primer programa y su jecución</span>

Para que un proyecto en Kotlin pueda ejecutarse, requiere como mínimo tener un punto de entrada definido por una función llamada `main`. Kotlin utiliza la palabra reservada `fun` para declarar funciones.

Reemplaza el código autogenerado del archivo `Main.kt` con el siguiente programa inicial (que simula un sistema de riego de una planta):

```kotlin
fun main() {
    val planta = "Orquídea"
    println("¡Bienvenido al sistema de control botánico!")
    println("Planta seleccionada en el sistema: $planta")

    // Simulación del registro de crecimiento diario (5 días)
    for (dia in 1..5) {
        println("Día $dia: La planta '$planta' ha sido regada correctamente.")
    }
}
```


> **Ejecución del programa:** Para ejecutar el código escrito, haz clic en el icono verde de **Play** situado en el margen izquierdo de la función `main` (o en la barra de herramientas superior).

El resultado de la ejecución se mostrará inmediatamente en la consola y es el siguiente:

```text
¡Bienvenido al sistema de control botánico!
Planta seleccionada en el sistema: Orquídea
Día 1: La planta 'Orquídea' ha sido regada correctamente.
Día 2: La planta 'Orquídea' ha sido regada correctamente.
Día 3: La planta 'Orquídea' ha sido regada correctamente.
Día 4: La planta 'Orquídea' ha sido regada correctamente.
Día 5: La planta 'Orquídea' ha sido regada correctamente.

Process finished with exit code 0
```

<span class="mi_h3">2.5. Compartir y entregar proyectos</span>

Cuando debas entregar una tarea de programación o compartir un proyecto con el profesor, evita subir archivos sueltos. El procedimiento correcto se realiza desde el explorador de archivos de tu sistema operativo:

1. Cierra el IDE IntelliJ IDEA para asegurarte de que no haya procesos de escritura activos.
2. Localiza la carpeta de tu proyecto (por ejemplo, la carpeta `control_plantas` que se encuentra dentro de tu espacio de trabajo `kot`).
3. Comprime la carpeta del proyecto en un único archivo comprimido:
    * **En Windows:** Haz clic derecho sobre la carpeta del proyecto y haz clic en  **Enviar a carpeta comprimida (en zip)**.
    * **En Ubuntu:** Haz clic derecho sobre la carpeta y haz clic en **Comprimir... Seleccionar formato .zip**.
4. Envía o sube a la plataforma del aula el archivo `.zip` resultante.


## 3. Organización del código (Packages e Imports)

A medida que tus proyectos crecen, escribir todo el código en un único archivo (`Main.kt`) se vuelve insostenible. En Kotlin, para mantener el proyecto limpio, ordenado y escalable, dividimos nuestro código en diferentes carpetas utilizando los conceptos de **package** (paquete) e **import** (importación).


<span class="mi_h3">3.1. ¿Qué es un Package (Paquete)?</span>

Un **package** es una forma de agrupar clases, funciones, objetos y otras estructuras relacionadas bajo un mismo espacio de nombres físico y lógico. Sirve para organizar tus archivos y evitar conflictos en caso de que declares dos funciones con el mismo nombre.

**Reglas clave de los paquetes:**

* La declaración del paquete debe ir en la **primera línea de tu archivo de código**, antes de cualquier otra cosa.
* Su sintaxis utiliza la palabra reservada `package` seguida de la ruta lógica de la carpeta, por ejemplo: `package util.botanica`.
* Los nombres de los paquetes se escriben siempre en minúsculas y se separan por puntos `.`.

**Ejemplo de archivo dentro de un paquete:**

```kotlin
// Archivo: src/util/botanica/MisCalculos.kt
package util.botanica

fun calcularConsumo(dias: Int) = dias * 1.5
```


<span class="mi_h3">3.2. ¿Qué es un Import?</span>

La palabra clave **`import`** se utiliza para poder acceder y utilizar clases o funciones que están guardadas en otros paquetes diferentes, sin necesidad de escribir la ruta completa de carpetas cada vez que las uses.

**Ejemplo de uso básico:**

Si tu archivo principal está en el paquete `botanica.app` y deseas utilizar la función `calcularConsumo` que está en `botanica.util`:

```kotlin
// Archivo: src/app/botanica/Main.kt
package app.botanica

// Importamos la función específica indicando su paquete de origen
import util.botanica.calcularConsumo

fun main() {
    val consumoTotal = calcularConsumo(7) // Podemos usar la función directamente
    println("Consumo acumulado: $consumoTotal litros.")
}
```



**Alias en importaciones**

A veces puede ocurrir que necesites importar dos clases o funciones de diferentes paquetes que se llaman exactamente igual. Para evitar conflictos, puedes renombrar una de ellas de manera temporal en tu archivo actual utilizando la palabra clave **`as`** (alias).

```kotlin
import util.botanica.saludarJardinero as saludarBreve
import admin.botanica.saludarJardinero as saludarCompleto

fun main() {
    saludarBreve("Pol")     // Llama a la función del paquete util
    saludarCompleto("Pol")  // Llama a la función del paquete administracion
}
```



**Importaciones con comodín**

Si un paquete contiene muchas funciones u objetos y necesitas utilizarlos casi todos en un mismo archivo, puedes evitar escribir una línea de importación para cada uno de ellos utilizando el comodín asterisco (`*`).

```kotlin
// Importa absolutamente todas las funciones y clases públicas del paquete util
import botanica.util.*
```

<span class="mi_h3">3.3. Tabla resumen de organización</span>

| Término | ¿Qué hace? | Ejemplo de uso |
| :--- | :--- | :--- |
| **`package`** | Define el espacio lógico y la ubicación del archivo actual. | `package botanica.modelo` |
| **`import`** | Trae una herramienta (clase o función) de otro paquete para poder usarla. | `import botanica.util.evaluarHumedad` |
| **`import ... as`**| Trae una herramienta y le asigna un nombre temporal para evitar conflictos. | `import botanica.util.regar as regadoAutomatico` |


<span class="mi_h3">3.4. Ejemplo completo multiarchivo</span>

Para entender cómo encaja todo esto de manera profesional en un proyecto de software, vamos a estructurar un asistente de control de humedad multiarchivo utilizando tres paquetes separados: `modelo`, `util` y `app`.

**Estructura de carpetas de tu proyecto:**

```text
control_botanico/
└── src/
    ├── app/
    │   └── Main.kt
    ├── modelo/
    │   └── Planta.kt
    └── util/
        └── AsistenteRiego.kt
```



**Archivo 1: `modelo/Planta.kt`**

Este archivo define la estructura de datos base para nuestras plantas.

```kotlin
// src/modelo/Planta.kt
package modelo

data class Planta(val especie: String, val humedadIdeal: Int)
```



**Archivo 2: `util/AsistenteRiego.kt`**

Este archivo contiene la lógica de asistencia y cálculo para el invernadero.

```kotlin
// src/util/AsistenteRiego.kt
package util

fun evaluarHumedad(actual: Int, ideal: Int): String {
    return when {
        actual < ideal - 15 -> "Urgente: Requiere riego inmediato."
        actual > ideal + 15 -> "Atención: Suelo encharcado, suspender el riego."
        else -> "Humedad adecuada."
    }
}

fun bienvenida(nombreJardinero: String): String {
    return "¡Hola, $nombreJardinero! Iniciando asistente de control..."
}
```



**Archivo 3: `app/Main.kt`**
Este es el archivo principal que coordina toda la aplicación e importa las clases y funciones de los otros dos paquetes.

```kotlin
// src/app/Main.kt
package app

// 1. Importamos la data class del paquete modelo
import modelo.Planta

// 2. Importamos la función de evaluación del paquete util
import util.evaluarHumedad

// 3. Importamos la función de bienvenida usando un alias para abreviarla
import util.bienvenida as saludar

fun main() {
    // Usamos el alias importado
    println(saludar("Pol"))
    println("----------------------------------------")

    // Creamos la planta utilizando la clase del paquete modelo
    val miHelecho = Planta("Helecho real", 75)
    
    // Evaluamos la humedad actual (por ejemplo, leída de un sensor: 50%) frente a su ideal (75%)
    val lecturaSensor = 50
    val diagnostico = evaluarHumedad(lecturaSensor, miHelecho.humedadIdeal)

    // Mostramos el informe final por consola
    println("Planta: ${miHelecho.especie}")
    println("Humedad registrada: $lecturaSensor% (Humedad objetivo: ${miHelecho.humedadIdeal}%)")
    println("Diagnóstico del sistema: $diagnostico")
}
```

**Salida por consola de tu proyecto multiarchivo:**

```text
¡Hola, Pol! Iniciando asistente de control...
----------------------------------------
Planta: Helecho real
Humedad registrada: 50% (Humedad objetivo: 75%)
Diagnóstico del sistema: Urgente: Requiere riego inmediato.
```



---

<span class="mi_h3">Autoría</span>

Obra realizada por Begoña Paterna Lluch. Publicada bajo licencia [Creative Commons Atribución/Reconocimiento-CompartirIgual 4.0 Internacional](https://creativecommons.org/licenses/by-sa/4.0/)

---