Objetos en R
================

Programación Orientada a Objetos
--------------------------------

Hay diferentes formas de hacer programación. Entre las muchas diferencias entre tipos de lenguajes se encuentran las diferencias en el manejo de data. Por ejemplo hay lenguajes en los que se define un espacio de data a emplear y luego se ejecutan una serie de procedimientos sobre esa data. Este tipo de lenguajes (como BASIC o MATLAB) son conocidos como lenguajes procedimentales.

Algunos programas, como R, Python, Java o C++, manejan su data creando múltiples clases de **objetos** en los que se almacena de diferentes clases de información de acuerdo a ciertas reglas (no solo data, en R se pueden guardar funciones, ecuaciones, opciones y, basicamente, lo que se te ocurra). Luego la programación realiza operaciones sobre los objetos. Todo en R se puede entender como un **objeto** que consiste de *nombre* y *atributos* con una serie de reglas de *conducta*.

### Crear un Objeto

Para asignar datos a un nombre y crear un objeto, usaremos la función *assign()*, para luego "llamar" al objeto escribiendo su nombre en la consola:

``` r
assign("x", 1)

x
```

    ## [1] 1

Para conocer el detalle de la sintaxis de la función *assign()*, escribir en la consola *?assign*. En general obedece el siguiente formato: *assign("nombre", valores)* - nótese que el nombre se escribe entre comillas. Sin embargo si queremos asignar varios valores a x tendremos que *combinarlos* con la función *c()*, o si fueran correlativos usar la operación de generación de secuencias (*a:b* genera una secuencia de 'a' hasta 'b'):

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

Como escribir la función *assign* y sus paréntesis cada vez es pesado, existe un atajo gramatical:

"nombre del objeto" &lt;- "valores"

``` r
w <- 1:20

w
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

R también entiende la función de asignar usando '-&gt;' (el nombre va a la derecha en este caso) o '=' que sería un símbolo más natural para la mayoría de personas. Sin embargo el '=' está desaconsejado seriamente (mis alumnos perderan puntos por mala gramática: si los profesores de lenguaje hablado pueden quitar puntos por ello, ¿por qué no podría hacerlo yo en un lenguaje de programación?).

La razón por la que evitamos '=' es simple: evitar ambigüedad (por menor que sea) a como de lugar: el término '=' se emplea en muchas funciones para definir argumentos en la función. Por ejemplo se podría tener este escenario:

a = funcion(data = x, modelo = 'tipo1')

En la que el '=' significa "asignar valores a un objeto" y "asignar atributos a los argumentos de una función". Esta doble función (que normalmente R maneja bien y solo en raras ocasiones genera problemas reales), es considerada una mala práctica en programación en general y no solo en R: se deben favorecer métodos que no tengan potencial ambiguedad (recordar que la Ley de Murphy estará en plena operación mientras ustedes escriban código y a más complicado el código peor será: si puede salir mal, saldrá mal).

Voy a introducir cuatro tipos de objetos que emplearemos con frecuencia. Luego introduciré los diferentes tipos de data que puede manejar R:

### Vector

Un vector en R (tipea en la consola ?vector) tiene dos atributos: longitud (número de observaciones/dimensiones) y modo (tipo de información).