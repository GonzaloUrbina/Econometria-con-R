Regresion
================

Ejemplos de Regresion Lineal
----------------------------

La sintaxis de regresiones lineales en R requiere de dos cosas: una función y un objeto con la data.

La función se define de la siguiente forma: "variable dependiente ~ variables dependientes" y es factible crear un objeto que contenga tu formula.

``` r
ecuacion <- y ~ x1 + x2

class(ecuacion)
```

    ## [1] "formula"

La sintaxis estándar de una regresión lineal (y = b1 + b1 \* x) para data contenida en un objeto llamado "foo" es:

objeto\_con\_resultados &lt;- lm(formula = y ~ x, data = foo)

Este comando generará varios tipos de data que pueden (o no) ser almacenados en un objeto. Como los datos tienen diferentes niveles de observación (hay un estimador de pendiente para cada variable pero un solo R2 para toda la función), las tablas de regresión pueden ser difíciles de presentar. El paquete {broom} que usaremos en breve simplifica mucho este problema.

No es necesario escribir "formula =" o "data =" para el comando "lm". Ni siquiera hay que escribirlas en ese orden, ya que la ecuación siempre será vista como una fórmula por R a menos que esté mal escrita en cuyo caso dará error. A pesar de que no es necesario escribir esto, mientras aprendemos puede ayudar a mantener el orden y entender mejor nuestro código.

Para mostrar rápidamente esta sintaxis usamos el ejemplo 2 de la introduccion de Kleiber:

``` r
# Creamos un objeto llamado "reg.lineal" con toda la data generada por el cálculo
reg.lineal <- lm(log(wage) ~ experience + I(experience^2) + education, data = cps)

# Los resultados en formato normal:
summary(reg.lineal)
```

    ## 
    ## Call:
    ## lm(formula = log(wage) ~ experience + I(experience^2) + education, 
    ##     data = cps)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -2.12709 -0.31543  0.00671  0.31170  1.98418 
    ## 
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)      0.5203218  0.1236163   4.209 3.01e-05 ***
    ## experience       0.0349403  0.0056492   6.185 1.24e-09 ***
    ## I(experience^2) -0.0005362  0.0001245  -4.307 1.97e-05 ***
    ## education        0.0897561  0.0083205  10.787  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.4619 on 530 degrees of freedom
    ## Multiple R-squared:  0.2382, Adjusted R-squared:  0.2339 
    ## F-statistic: 55.23 on 3 and 530 DF,  p-value: < 2.2e-16

``` r
# Diferentes partes de los resultados utilizando el paquete "broom" incluido en tidyverse

# A nivel de variable (coeficientes, p-values, etc)
tidy(reg.lineal)
```

    ##              term      estimate    std.error statistic      p.value
    ## 1     (Intercept)  0.5203217710 0.1236162526  4.209170 3.010737e-05
    ## 2      experience  0.0349403392 0.0056492113  6.184994 1.242179e-09
    ## 3 I(experience^2) -0.0005362401 0.0001245024 -4.307068 1.971719e-05
    ## 4       education  0.0897560821 0.0083205199 10.787317 1.160240e-24

``` r
# A nivel de observación (genera estimaciones, residuales, etc.)
head(augment(reg.lineal))
```

    ##   .rownames log.wage. experience I.experience.2. education  .fitted
    ## 1         1  1.629241         21             441         8 1.735636
    ## 2      1100  1.599388         42            1764         9 1.849693
    ## 3         2  1.897620          1               1        12 1.631799
    ## 4         3  1.386294          4              16        12 1.728576
    ## 5         4  2.014903         17             289        12 2.036407
    ## 6         5  2.570320          9              81        13 1.958178
    ##      .se.fit      .resid        .hat    .sigma      .cooksd  .std.resid
    ## 1 0.05261865 -0.10639514 0.012975435 0.4623457 1.766412e-04 -0.23183504
    ## 2 0.05356309 -0.25030568 0.013445402 0.4622393 1.014041e-03 -0.54554606
    ## 3 0.04909659  0.26582101 0.011296534 0.4622230 9.566966e-04  0.57873206
    ## 4 0.03795644 -0.34228191 0.006751704 0.4621279 9.393949e-04 -0.74349201
    ## 5 0.02949342 -0.02150412 0.004076550 0.4623682 2.226726e-06 -0.04664767
    ## 6 0.02519169  0.61214108 0.002974110 0.4616002 1.313499e-03  1.32714869

``` r
# A nivel de regresión (R cuadrado, prueba F, Suma de Residuales, etc.)
glance(reg.lineal)
```

    ##   r.squared adj.r.squared     sigma statistic      p.value df    logLik
    ## 1 0.2381624     0.2338501 0.4619327  55.22876 4.459215e-31  4 -343.2782
    ##        AIC      BIC deviance df.residual
    ## 1 696.5564 717.9584 113.0924         530

Kleiber analiza los datos haciendo una regresion por quintiles y evaluando el efecto del salario inicial sobre las tendencias del salario final. Es necesario usar el paquete {quantreg} para regresiones por estratos.

``` r
reg.quintil <- rq(log(wage) ~ experience + I(experience^2) + education,
                 data = cps, tau = seq(0.2,0.8, by = 0.15))

reg.quintil
```

    ## Call:
    ## rq(formula = log(wage) ~ experience + I(experience^2) + education, 
    ##     tau = seq(0.2, 0.8, by = 0.15), data = cps)
    ## 
    ## Coefficients:
    ##                     tau= 0.20     tau= 0.35     tau= 0.50    tau= 0.65
    ## (Intercept)      0.4509293695  0.3134682434  0.1938720519  0.429725904
    ## experience       0.0371833086  0.0475639449  0.0449586672  0.043050616
    ## I(experience^2) -0.0006970546 -0.0008288489 -0.0007251398 -0.000598682
    ## education        0.0660013111  0.0853064797  0.1077608566  0.102662617
    ##                     tau= 0.80
    ## (Intercept)      0.6129246830
    ## experience       0.0391506313
    ## I(experience^2) -0.0005141205
    ## education        0.1056037756
    ## 
    ## Degrees of freedom: 534 total; 530 residual

``` r
# Luego se crea una base para simular el efecto de el salario por quintil. 
# En este nuevo objeto (cps2) se mantiene la educación constante (igual al promedio) 
# y se listan todos los niveles posibles de experiencia para generar un estimado de
# ingresos para cada nivel de experiencia.

cps2 <- data.frame(education = mean(cps$education), 
                   experience = min(cps$experience):max(cps$experience))

cps2 <- cbind(cps2, predict(reg.lineal, newdata = cps2, interval = "prediction"))
cps2 <- cbind(cps2, predict(reg.quintil, newdata = cps2, type = ""))

plot(log(wage) ~ experience, data = cps)

for(i in 6:10) lines(cps2[,i] ~ experience, data = cps2, col = "red")
```

![](Regresion_files/figure-markdown_github/unnamed-chunk-3-1.png)
