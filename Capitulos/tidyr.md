Manipulación de Tablas: {tidyr}
================

Tabulación de Datos
-------------------

Una misma colección de datos puede ser tabulada de varias maneras. Este es un punto tan relevante que el paquete {tidyverse} viene pre-cargado con algunas tablas que ilustran específicamente este punto. Los ejemplos que vamos a usar siguen la presentación de Hadley Wickham en el artículo ["Tidy Data"](https://www.jstatsoft.org/article/view/v059i10) y en el capítulo 9 de "R for Data Science" de Wickham y Grolemund.

La data empleada como ejemplo tiene cuatro variables: país, año, casos y población. Cada ejemplo contiene la misma información con otra forma (literalmente):

``` r
# Cada variable con su columna
tabla1
```

    ## # A tibble: 6 × 4
    ##         país   año  casos  población
    ##        <chr> <int>  <int>      <int>
    ## 1 Afganistán  1999    745   19987071
    ## 2 Afganistán  2000   2666   20595360
    ## 3     Brasil  1999  37737  172006362
    ## 4     Brasil  2000  80488  174504898
    ## 5      China  1999 212258 1272915272
    ## 6      China  2000 213766 1280428583

``` r
# Una columna con "tipos de datos" (casos y población) y
# otra columna con los "valores" para cada tipo, año y país.
# IMPORTANTE: nótese como la misma información ocupa más espacio: 
# la tabla tiene más celdas que se llenan repitiendo data.
tabla2
```

    ## # A tibble: 12 × 4
    ##          país   año       tipo   cantidad
    ##         <chr> <int>      <chr>      <int>
    ## 1  Afganistán  1999      cases        745
    ## 2  Afganistán  1999 population   19987071
    ## 3  Afganistán  2000      cases       2666
    ## 4  Afganistán  2000 population   20595360
    ## 5      Brasil  1999      cases      37737
    ## 6      Brasil  1999 population  172006362
    ## 7      Brasil  2000      cases      80488
    ## 8      Brasil  2000 population  174504898
    ## 9       China  1999      cases     212258
    ## 10      China  1999 population 1272915272
    ## 11      China  2000      cases     213766
    ## 12      China  2000 population 1280428583

``` r
# Juntando los valores en una tasa anual por país (casos / población).
tabla3
```

    ## # A tibble: 6 × 3
    ##         país   año              tasa
    ## *      <chr> <int>             <chr>
    ## 1 Afganistán  1999      745/19987071
    ## 2 Afganistán  2000     2666/20595360
    ## 3     Brasil  1999   37737/172006362
    ## 4     Brasil  2000   80488/174504898
    ## 5      China  1999 212258/1272915272
    ## 6      China  2000 213766/1280428583

``` r
# Separando la información en dos tablas: una de casos y otra de población

# Casos
tabla4a
```

    ## # A tibble: 3 × 3
    ##         país `1999` `2000`
    ## *      <chr>  <int>  <int>
    ## 1 Afganistán    745   2666
    ## 2     Brasil  37737  80488
    ## 3      China 212258 213766

``` r
# Población
tabla4b
```

    ## # A tibble: 3 × 3
    ##         país     `1999`     `2000`
    ## *      <chr>      <int>      <int>
    ## 1 Afganistán   19987071   20595360
    ## 2     Brasil  172006362  174504898
    ## 3      China 1272915272 1280428583

``` r
# Desagregando una columna (año) en sus componentes (siglo y año)
tabla5
```

    ## # A tibble: 6 × 4
    ##         país siglo   año              tasa
    ## *      <chr> <chr> <chr>             <chr>
    ## 1 Afganistán    19    99      745/19987071
    ## 2 Afganistán    20    00     2666/20595360
    ## 3     Brasil    19    99   37737/172006362
    ## 4     Brasil    20    00   80488/174504898
    ## 5      China    19    99 212258/1272915272
    ## 6      China    20    00 213766/1280428583

De estas tablas, la única verdaderamente ordenada ("tidy") es la tabla 1:

1.  Cada variable tiene una columna.
2.  Cada observación tiene una fila.
3.  Cada valor está en su propia celda.

Cuando la data está ordenada, es muy simple transformarla a formas que facilitan el análisis. Es decir, la tabla original ordenada es un punto de partida desde el que cualquier transformación es fácil: con frecuencia usaremos tablas desordenadas, es solo que no queremos que esa sea la forma de almacenar la data.

Hay varias alternativas disponibles para la manipulación de bases. La razón por la que me estoy enfocando en usar las herramientas del {tidyverse} es que mantienen una gramática muy similar, la alumna es libre de explorar y usar otros paquetes.

El paquete que se usaremos se llama {tidyr} y dentro de su gramática nos enfocaremos en verbos a dos niveles: columnas y valores.

Columnas: *gather* & *spread*
-----------------------------

Queremos reducir o ampliar columnas de acuerdo a categorías.

### Gather

Con frecuencia (como en las tablas 4a y b), nos pasa que un "valor" relevante para nuestro análisis no está en una celda si no que es el nombre de una columna.

``` r
tabla4b
```

    ## # A tibble: 3 × 3
    ##         país     `1999`     `2000`
    ## *      <chr>      <int>      <int>
    ## 1 Afganistán   19987071   20595360
    ## 2     Brasil  172006362  174504898
    ## 3      China 1272915272 1280428583

En la tabla 4b vemos que hay columnas llamadas "1999" y "2000" que quizás quisieramos que estén en una variable llamada "año". Para eso se emplea el verbo "gather".

La sintaxis de este comando es:

gather(data, <br />             lista de columnas, <br />             key = "nombre\_de\_col\_nueva", <br />             value = "nombre\_de\_col\_con\_valores")

No es necesario escribir cada sección de comando en su propia línea, pero es más fácil de leer que escribir todo corrido en una sola línea. Compara:

gather(data, lista de columnas, key = "nombre\_de\_col\_nueva", value = "nombre\_de\_col\_con\_valores")

Como los nombres de las columnas son números, **hay que usar comillas invertidas** (estas: \` ) para que {tidyr} entienda de qué columnas hablas.

``` r
gather(tabla4b, 
       `1999`, `2000`, 
       key = "año", 
       value = "población")
```

    ## # A tibble: 6 × 3
    ##         país   año  población
    ##        <chr> <chr>      <int>
    ## 1 Afganistán  1999   19987071
    ## 2     Brasil  1999  172006362
    ## 3      China  1999 1272915272
    ## 4 Afganistán  2000   20595360
    ## 5     Brasil  2000  174504898
    ## 6      China  2000 1280428583

### Spread

Este verbo hace lo opuesto que "gather" y lo usas cuando una observación está repartida en varias filas: ver la tabla 2. Una observación en este caso es un país en un año. Para cada observación tenemos dos valores en una fila cada uno: casos y población. La sintaxis de "spread" es la siguiente:

spread(data, <br />               key = "nombre\_de\_col\_con\_variables", <br />               value = "nombre\_de\_col\_con\_valores")

Aplicando esta sintaxis a la tabla 2:

``` r
spread(tabla2, 
       key = tipo, 
       value = cantidad)
```

    ## # A tibble: 6 × 4
    ##         país   año  cases population
    ## *      <chr> <int>  <int>      <int>
    ## 1 Afganistán  1999    745   19987071
    ## 2 Afganistán  2000   2666   20595360
    ## 3     Brasil  1999  37737  172006362
    ## 4     Brasil  2000  80488  174504898
    ## 5      China  1999 212258 1272915272
    ## 6      China  2000 213766 1280428583

Vemos que la columna de la tabla 2 llamada "tipo" tiene dos categorías que ahora son los nombres de dos columnas. Los valores bajo cada columna fueron tomados correlativamente de la columna "cantidad".

Valores: *separate*, *unite* & faltantes
----------------------------------------

Dos verbos principales ("separate" y "unite") nos permiten separar o unir los valores contenidos en celdas de alguna(s) columna. Adicionalmente veremos el tipo de herramientas que podemos usar para administrar data faltante en las tablas.

### Separate

En la tabla 3 arriba vemos que la columna "rate" contiene dos valores separados por un "/". Este forma de almacenamiento de data es muy común, en particular cuando no fueron creadas pensadas en hacer análisis de data. Para separar la información usamos el verbo "separate".

La sintaxis básica de "separate" es:

separate(data, <br />                 columna\_a\_separar, <br />                 into = c("nombre\_col1", "nombre\_col2", ...))

Para la tabla 3 podríamos usar esta sintaxis:

``` r
separate(tabla3, 
         tasa, 
         into = c("casos", "población"))
```

    ## # A tibble: 6 × 4
    ##         país   año  casos  población
    ## *      <chr> <int>  <chr>      <chr>
    ## 1 Afganistán  1999    745   19987071
    ## 2 Afganistán  2000   2666   20595360
    ## 3     Brasil  1999  37737  172006362
    ## 4     Brasil  2000  80488  174504898
    ## 5      China  1999 212258 1272915272
    ## 6      China  2000 213766 1280428583

{tidyr} asume que cualquier símbolo que no sea alfanumérico (como "/") será el separador de la información. Esto puede definirse explícitamente cuando sea necesario:

``` r
separate(tabla3, 
         tasa, 
         into = c("casos", "población"), 
         sep = "/")
```

    ## # A tibble: 6 × 4
    ##         país   año  casos  población
    ## *      <chr> <int>  <chr>      <chr>
    ## 1 Afganistán  1999    745   19987071
    ## 2 Afganistán  2000   2666   20595360
    ## 3     Brasil  1999  37737  172006362
    ## 4     Brasil  2000  80488  174504898
    ## 5      China  1999 212258 1272915272
    ## 6      China  2000 213766 1280428583

Otro asunto importante es que luego de la separación las columnas han sido consideradas como texto (tipo "chr"). Para que R distinga o trate de identificar el tipo se agrega el indicador "convert":

``` r
separate(tabla3, 
         tasa, 
         into = c("casos", "población"),
         convert = TRUE
         )
```

    ## # A tibble: 6 × 4
    ##         país   año  casos  población
    ## *      <chr> <int>  <int>      <int>
    ## 1 Afganistán  1999    745   19987071
    ## 2 Afganistán  2000   2666   20595360
    ## 3     Brasil  1999  37737  172006362
    ## 4     Brasil  2000  80488  174504898
    ## 5      China  1999 212258 1272915272
    ## 6      China  2000 213766 1280428583

### Unite

La tabla 5 tiene la data de "año" separada en dos componentes que nos puede interesar unir. El comando unite() combina columnas y separa los valores unidos con un guión bajo (\_):

``` r
unite(tabla5, nueva_col, siglo, año)
```

    ## # A tibble: 6 × 3
    ##         país nueva_col              tasa
    ## *      <chr>     <chr>             <chr>
    ## 1 Afganistán     19_99      745/19987071
    ## 2 Afganistán     20_00     2666/20595360
    ## 3     Brasil     19_99   37737/172006362
    ## 4     Brasil     20_00   80488/174504898
    ## 5      China     19_99 212258/1272915272
    ## 6      China     20_00 213766/1280428583

Para evitar el guión bajo empleamos la opción *sep = ""*:

``` r
unite(tabla5, nueva_col, siglo, año, sep = "")
```

    ## # A tibble: 6 × 3
    ##         país nueva_col              tasa
    ## *      <chr>     <chr>             <chr>
    ## 1 Afganistán      1999      745/19987071
    ## 2 Afganistán      2000     2666/20595360
    ## 3     Brasil      1999   37737/172006362
    ## 4     Brasil      2000   80488/174504898
    ## 5      China      1999 212258/1272915272
    ## 6      China      2000 213766/1280428583

### Valores Faltantes

Cuando cambiemos la forma de la data los valores que puedan estar ausentes generarán algunos efectos inesperador.

``` r
ventas <- tibble(
    dia    = c("lunes", "lunes", 
               "martes", "martes", 
               "miércoles", 
               "jueves", "jueves", 
               "viernes", "viernes"),
    turno  = c("mañana", "tarde",
               "mañana", "tarde",
               "mañana",
               "mañana", "tarde",
               "mañana", "tarde"),
    ventas = c(12, 13,
               12, 12,
               14,
               NA, 12,
               13, 13)
)

ventas
```

    ## # A tibble: 9 × 3
    ##         dia  turno ventas
    ##       <chr>  <chr>  <dbl>
    ## 1     lunes mañana     12
    ## 2     lunes  tarde     13
    ## 3    martes mañana     12
    ## 4    martes  tarde     12
    ## 5 miércoles mañana     14
    ## 6    jueves mañana     NA
    ## 7    jueves  tarde     12
    ## 8   viernes mañana     13
    ## 9   viernes  tarde     13

En la tabla de ventas hay dos datos que faltan:

-   las ventas del turno *mañana* del día *jueves* faltan **explícitamente** (hay un "NA" en la tabla que así lo indica)
-   las ventas del turno *tarde* del *miércoles* faltan **implícitamente** (no aparece en la data)

Wickham y Grolemund (a quienes estamos siguiendo en esta explicación), presentan una forma poética de hablar de los diferentes tipos de valores faltantes:

<center>
"un valor faltante explícito es la presencia de una ausencia; un valor faltante implícito es la ausencia de una presencia"
</center>
Un valor faltante implícito se puede volver explícito cuando transformamos la data:

``` r
ventas_ancho <- spread(ventas,
       dia,
       ventas)
ventas_ancho
```

    ## # A tibble: 2 × 6
    ##    turno jueves lunes martes miércoles viernes
    ## *  <chr>  <dbl> <dbl>  <dbl>     <dbl>   <dbl>
    ## 1 mañana     NA    12     12        14      13
    ## 2  tarde     12    13     12        NA      13

Eventualmente tener valores faltantes de forma explícita puede comlicar algunas visualizaciones o cálculos. Podemos usar la opción "na.rm = TRUE" dentro del comando gather para volver faltantes explícitos en implícitos:

``` r
gather(ventas_ancho,
       día,
       ventas,
       lunes, martes, miércoles, jueves, viernes,
       na.rm = TRUE)
```

    ## # A tibble: 8 × 3
    ##    turno       día ventas
    ## *  <chr>     <chr>  <dbl>
    ## 1 mañana     lunes     12
    ## 2  tarde     lunes     13
    ## 3 mañana    martes     12
    ## 4  tarde    martes     12
    ## 5 mañana miércoles     14
    ## 6  tarde    jueves     12
    ## 7 mañana   viernes     13
    ## 8  tarde   viernes     13

El desorden de los días se debe a qué R los ordena alfabéticamente. Para mantener el orden de una variable categórica como "día" hay que convertirla en un factor ordenado.

Podemos volver faltantes implícitos en explícitos con el comando "complete" en el que se señalan las variables que identifican las observaciones (vemos que aparece el "NA" para *miércoles tarde*):

``` r
complete(ventas, dia, turno)
```

    ## # A tibble: 10 × 3
    ##          dia  turno ventas
    ##        <chr>  <chr>  <dbl>
    ## 1     jueves mañana     NA
    ## 2     jueves  tarde     12
    ## 3      lunes mañana     12
    ## 4      lunes  tarde     13
    ## 5     martes mañana     12
    ## 6     martes  tarde     12
    ## 7  miércoles mañana     14
    ## 8  miércoles  tarde     NA
    ## 9    viernes mañana     13
    ## 10   viernes  tarde     13

En algunas bases también pasa que cuando un dato se repite en varias filas sólo se incluyen en la primera y se dejan las otras en blanco (el comando "tribble" permite construir tablas columna por columna):

``` r
pagos <- tribble(
    ~ empresa, ~ factura, ~pago,
    "Emp 1", 1, 100,
    NA,      2, 120,
    NA,      3, 110,
    "Emp 2", 1, 150
)

pagos
```

    ## # A tibble: 4 × 3
    ##   empresa factura  pago
    ##     <chr>   <dbl> <dbl>
    ## 1   Emp 1       1   100
    ## 2    <NA>       2   120
    ## 3    <NA>       3   110
    ## 4   Emp 2       1   150

El comando fill() aplicado a una columna completa los valores faltantes con el valor no-faltante más reciente.

``` r
fill(pagos, empresa)
```

    ## # A tibble: 4 × 3
    ##   empresa factura  pago
    ##     <chr>   <dbl> <dbl>
    ## 1   Emp 1       1   100
    ## 2   Emp 1       2   120
    ## 3   Emp 1       3   110
    ## 4   Emp 2       1   150
