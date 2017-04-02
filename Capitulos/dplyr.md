Manipulacion de Data: {dplyr}
================

Gramática Básica
----------------

Esta sección sigue de cerca la exposición y ejemplos en el Capítulo 3 de "R for Data Science" de Wickham y Grolemund.

Se presentarán los seis verbos principales disponibles en el paquete {dplyr} para manipular datos:

1.  Escoger observaciones (filas) de acuerdo a condiciones: **filter()**
2.  Ordenar filas: **arrange()**
3.  Escoger variables (columnas) por nombre: **select()**
4.  Crear nuevas variables a partir de las existentes: **mutate()**
5.  Resumir muchas observaciones en una línea: **summarise()**
6.  Categorizar observaciones para ejecutar los otros verbos: **group\_by()**

Adicionalmente se presentará el uso del conector de funciones de {magrittr} (*pipe operator* en inglés): **%&gt;%**

Todos los verbos de {dplyr} (y en general del {tidyverse}) funcionan de la misma manera:

1.  El primer argumento es la data (como *data.frame* o *tibble*).
2.  Los siguientes argumentos describen qué hacer con la data usando los nombres de las columnas.
3.  El resultado es un *data.frame* o *tibble*.

Como el resultado final es del mismo tipo que el insumo (*data.frame* o *tibble*), esto hace que sea fácil encadenar los verbos para definir acciones complejas con la data (para lo cual usaremos el conector de {magrittr}).

La data que usaremos para ilustrar los verbos se encuentra en el paquete {nycflights}.

### Filtrando con filter()

Luego de indicar la data que debe ser filtrada, los argumentos de la función filter() son expresiones que indican los datos requeridos. Para ello hay que usar los operadores de R:

#### Operadores Relacionales y Lógicos

| Relación      | Sintaxis  |
|:--------------|:----------|
| Mayor         | x &gt; y  |
| Menor         | x &lt; y  |
| Mayor o Igual | x &gt;= y |
| Menor o Igual | x &lt;= y |
| Igual         | x == y    |
| Diferente     | x != y    |

| Logicos | Sintaxis      |
|:--------|:--------------|
| NO      | !x            |
| Y       | x & y, x && y |
| O       | x | y, x || y |
| XOR     | xor(x, y)     |

Por ejemplo si quisieramos conocer los vuelos que despegaron el 4 de febrero:

``` r
# como no se asigna, R imprime el resultado
filter(vuelos, mes == 2, día == 4) 
```

    ## # A tibble: 932 × 19
    ##      año   mes   día hora_salida h_programada_salida demora_salida
    ##    <int> <int> <int>       <int>               <int>         <dbl>
    ## 1   2013     2     4         453                 500            -7
    ## 2   2013     2     4         522                 530            -8
    ## 3   2013     2     4         528                 525             3
    ## 4   2013     2     4         534                 540            -6
    ## 5   2013     2     4         534                 540            -6
    ## 6   2013     2     4         550                 600           -10
    ## 7   2013     2     4         551                 600            -9
    ## 8   2013     2     4         552                 600            -8
    ## 9   2013     2     4         553                 600            -7
    ## 10  2013     2     4         553                 600            -7
    ## # ... with 922 more rows, and 13 more variables: hora_llegada <int>,
    ## #   h_programada_llegada <int>, demora_llegada <dbl>, aerolinea <chr>,
    ## #   vuelo <int>, numerocola <chr>, origen <chr>, distancia <chr>,
    ## #   tiempo_vuelo <dbl>, distance <dbl>, hora <dbl>, minuto <dbl>,
    ## #   fecha_hora <dttm>

``` r
# Cuando se asigna, R NO imprime el resultado
feb4 <- filter(vuelos, mes == 2, día == 4) 

# Para asignar e imprimir se envuelve el comando en paréntesis
(mar5 <- filter(vuelos, mes == 3, día == 5)) 
```

    ## # A tibble: 965 × 19
    ##      año   mes   día hora_salida h_programada_salida demora_salida
    ##    <int> <int> <int>       <int>               <int>         <dbl>
    ## 1   2013     3     5         505                 500             5
    ## 2   2013     3     5         508                 515            -7
    ## 3   2013     3     5         527                 530            -3
    ## 4   2013     3     5         537                 540            -3
    ## 5   2013     3     5         539                 545            -6
    ## 6   2013     3     5         551                 600            -9
    ## 7   2013     3     5         551                 600            -9
    ## 8   2013     3     5         553                 600            -7
    ## 9   2013     3     5         553                 600            -7
    ## 10  2013     3     5         554                 600            -6
    ## # ... with 955 more rows, and 13 more variables: hora_llegada <int>,
    ## #   h_programada_llegada <int>, demora_llegada <dbl>, aerolinea <chr>,
    ## #   vuelo <int>, numerocola <chr>, origen <chr>, distancia <chr>,
    ## #   tiempo_vuelo <dbl>, distance <dbl>, hora <dbl>, minuto <dbl>,
    ## #   fecha_hora <dttm>
