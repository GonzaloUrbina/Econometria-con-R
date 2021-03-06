Objetos en R
================

Programación Orientada a Objetos
--------------------------------

Hay diferentes formas de hacer programación. Entre las muchas diferencias entre tipos de lenguajes se encuentran las diferencias en el manejo de data. Por ejemplo hay lenguajes en los que se define un espacio de data a emplear y luego se ejecutan una serie de procedimientos sobre esa data. Este tipo de lenguajes (como BASIC o MATLAB) son conocidos como lenguajes procedimentales.

Algunos programas, como R, Python, Java o C++, manejan su data creando múltiples clases de **objetos** en los que se almacena de diferentes clases de información de acuerdo a ciertas reglas (no solo data, en R se pueden guardar funciones, ecuaciones, opciones y, basicamente, lo que se te ocurra). Luego la programación realiza operaciones sobre los objetos. Todo en R se puede entender como un **objeto** que consiste de *nombre* y *atributos* con una serie de reglas de *conducta*.

### Crear (asignar valor a) un Objeto

Para asignar datos a un nombre y crear un objeto, usaremos la función *assign()*, para luego "llamar" al objeto escribiendo su nombre en la consola:

``` r
# Creación del objeto
assign("x", 1)

# Llamado
x
```

    ## [1] 1

Para conocer el detalle de la sintaxis de la función *assign()*, escribir en la consola *?assign*. En general obedece el siguiente formato: *assign("nombre", valores)* - nótese que el nombre se escribe entre comillas. Si queremos asignar **varios** valores a x tendremos que *combinarlos* con la función *c()*, o si fueran correlativos usar la operación de generación de secuencias (*a:b* genera una secuencia de 'a' hasta 'b'):

``` r
assign("x", c(1,2,3,4,5,6,7,8,9,10))

x
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10

``` r
assign("y", 1:30)

y
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
    ## [24] 24 25 26 27 28 29 30

Como escribir la función *assign* y sus paréntesis todas las veces puede ser mucho trabajo, existe un atajo gramatical:

"nombre del objeto" &lt;- "valores"

``` r
w <- 1:20

w
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

R también entiende la función de asignar usando '-&gt;' (el nombre va a la derecha en este caso) o '=' que sería un símbolo más natural para la mayoría de personas. Sin embargo el '=' está desaconsejado seriamente (mis alumnos perderan puntos por mala gramática: si los profesores de lenguaje hablado pueden quitar puntos por ello, ¿por qué no podría hacerlo yo en un lenguaje de programación?).

La razón por la que evitamos '=' es simple: evitar ambigüedad (por menor que sea) a como de lugar. El término '=' se emplea en muchas funciones para definir argumentos en la función. Por ejemplo se podría tener este escenario:

a = funcion(data = x, modelo = 'tipo1')

En la que el '=' significa "asignar valores a un objeto" y "asignar atributos a los argumentos de una función". Esta doble función (que normalmente R maneja bien y solo en raras ocasiones genera problemas reales), es considerada una mala práctica en programación en general y no solo en R: se deben favorecer métodos que no tengan potencial ambiguedad (recordar que la Ley de Murphy estará en plena operación mientras ustedes escriban código y a más complicado el código peor será: si puede salir mal, saldrá mal).

### Tipos de Data

Los valores o datos en R siempre deben tener asignados un tipo o modo.

#### *numeric* / *double*

Son **Números Reales** con [precisión de 53 bits](https://en.wikipedia.org/wiki/Double-precision_floating-point_format) o unos 16 decimales. R asume por defecto que cualquier número será de este tipo.

``` r
# Creamos un objeto con un número, automáticamente R asume que será una variable contínua
a <- 2

# Este comando nos permite ver la estructura de un objeto
class(a)
```

    ## [1] "numeric"

``` r
# El comando genérico is.tipo(objeto) verifica rápidamente si un objeto es de cierto tipo
is.numeric(a)
```

    ## [1] TRUE

``` r
is.integer(a)
```

    ## [1] FALSE

#### *integer*

Comprende los **Números Enteros**. Se debe asignar este tipo para que R lo reconozca.

``` r
# Podemos crear una nueva variable o reemplazar la variable anterior
b <- as.integer(a)

class(b)
```

    ## [1] "integer"

``` r
a <- as.integer(a)

class(a)
```

    ## [1] "integer"

``` r
a
```

    ## [1] 2

``` r
# R no redondea si fuerzas el entero sobre decimales

a <- 2.7

a <- as.integer(a)

a
```

    ## [1] 2

``` r
class(a)
```

    ## [1] "integer"

#### *character*

Es simplemente **texto** que puede incluir valores númericos tratados como texto.

``` r
# Si no se usan comillas R buscaría un objeto llamado 'texto' y al no encontrarlo daría un error
c <- "texto incluso con espacios"

c
```

    ## [1] "texto incluso con espacios"

``` r
class(c)
```

    ## [1] "character"

#### *factor*

Variables *Categóricas* cardinales u ordinales. Cuando R lee bases de datos, los vectores de caracteres usualmente se almacenan como factores porque es más eficiente con el uso de memoria: cada nombre de categoría es almacenado una vez y luego se usa una vector de números enteros.

``` r
# Para crear un factor
a <- c("Categoría 1", "Categoría 1", "Categoría 2", "Categoría 1", "Categoría 2", "Categoría 1")
a <- factor(a)

# No solo muestra el contenido del objeto pero también los *niveles* o *categorías* del factor
a
```

    ## [1] Categoría 1 Categoría 1 Categoría 2 Categoría 1 Categoría 2 Categoría 1
    ## Levels: Categoría 1 Categoría 2

``` r
# Para ver un resumen de las frecuencias de las categorías (también funciona con variables lógicas)
table(a)
```

    ## a
    ## Categoría 1 Categoría 2 
    ##           4           2

``` r
# Crear un factor ordenado

mes <- factor(c(1, 1, 1, 2, 2, 3, 4, 5, 6, 6, 7, 8, 9, 10, 11, 12), ordered = TRUE)
mes
```

    ##  [1] 1  1  1  2  2  3  4  5  6  6  7  8  9  10 11 12
    ## Levels: 1 < 2 < 3 < 4 < 5 < 6 < 7 < 8 < 9 < 10 < 11 < 12

``` r
# Renombrar los niveles del factor. Se debe hacer en orden por lo que se desaconseja hacer por separado. 
# En la sección de Gestión de Datos veremos como manipular factores más a fondo.

levels(mes) <- c("Enero",
                 "Febrero",
                 "Marzo",
                 "Abril",
                 "Mayo",
                 "Junio",
                 "Julio",
                 "Agosto",
                 "Septiembre",
                 "Octubre",
                 "Noviembre",
                 "Diciembre")

table(mes)
```

    ## mes
    ##      Enero    Febrero      Marzo      Abril       Mayo      Junio 
    ##          3          2          1          1          1          2 
    ##      Julio     Agosto Septiembre    Octubre  Noviembre  Diciembre 
    ##          1          1          1          1          1          1

#### *logical*

Variables dicotómicas de tipo *Verdadero/Falso*.

``` r
# No es necesario usar comillas
logica <- c(TRUE, TRUE, FALSE, TRUE)

logica
```

    ## [1]  TRUE  TRUE FALSE  TRUE

### Tipos de Objeto

Vamos a empezar conociendo cuatro tipos de objetos que emplearemos con frecuencia.

#### Vector Atómico

Es un vector compuesto de átomos. Los átomos de un vector *tienen que ser del mismo tipo de data*. Es decir, el vector será numérico, entero, etc.

#### Lista

Es un vector atómico generalizado. Permite que los elementos sean de diferentes tipos.

#### Matriz

Un conjunto de vectores del mismo tamaño. La matriz debe tener el mismo tipo de data dentro. El estándar es: logical &lt; integer &lt; double. Una matriz con algunos valores enteros y otros dobles será una matriz doble.

#### *Data Frame*

Una **lista** (en el sentido de R) de vectores (o listas) del mismo tamaño pero que pueden tener diferentes tipos de data cada uno. Este es uno de los tipos de objeto más útiles en R y la base para la mayor parte de nuestro trabajo.
