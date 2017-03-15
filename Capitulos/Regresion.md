Regresion
================

Ejemplos de Regresion Lineal
----------------------------

En este docuemnto tenemos ejemplos hechos en clase de regresion lineal.

El primer ejemplo es el ejemplo 2 de la introduccion de Kleiber:

``` r
reg.lineal <- lm(log(wage) ~ experience + I(experience^2) + education,
                 data = cps)

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
kable(tidy(reg.lineal))
```

| term            |    estimate|  std.error|  statistic|   p.value|
|:----------------|-----------:|----------:|----------:|---------:|
| (Intercept)     |   0.5203218|  0.1236163|   4.209170|  3.01e-05|
| experience      |   0.0349403|  0.0056492|   6.184994|  0.00e+00|
| I(experience^2) |  -0.0005362|  0.0001245|  -4.307068|  1.97e-05|
| education       |   0.0897561|  0.0083205|  10.787317|  0.00e+00|

Kleiber analiza los datos haciendo una regresion por quintiles y evaluando el efecto del salario inicial sobre las tendencias del salario final.

``` r
reg.quintil <- rq(log(wage) ~ experience + I(experience^2) + education,
                 data = cps, tau = seq(0.2,0.8, by = 0.15))

summary(reg.quintil)
```

    ## Warning in rq.fit.br(x, y, tau = tau, ci = TRUE, ...): Solution may be
    ## nonunique

    ## Warning in rq.fit.br(x, y, tau = tau, ci = TRUE, ...): Solution may be
    ## nonunique

    ## 
    ## Call: rq(formula = log(wage) ~ experience + I(experience^2) + education, 
    ##     tau = seq(0.2, 0.8, by = 0.15), data = cps)
    ## 
    ## tau: [1] 0.2
    ## 
    ## Coefficients:
    ##                 coefficients lower bd upper bd
    ## (Intercept)      0.45093      0.03337  0.65111
    ## experience       0.03718      0.02616  0.04889
    ## I(experience^2) -0.00070     -0.00090 -0.00049
    ## education        0.06600      0.04976  0.09704
    ## 
    ## Call: rq(formula = log(wage) ~ experience + I(experience^2) + education, 
    ##     tau = seq(0.2, 0.8, by = 0.15), data = cps)
    ## 
    ## tau: [1] 0.35
    ## 
    ## Coefficients:
    ##                 coefficients lower bd upper bd
    ## (Intercept)      0.31347      0.08176  0.44357
    ## experience       0.04756      0.03610  0.05910
    ## I(experience^2) -0.00083     -0.00112 -0.00059
    ## education        0.08531      0.07408  0.10263
    ## 
    ## Call: rq(formula = log(wage) ~ experience + I(experience^2) + education, 
    ##     tau = seq(0.2, 0.8, by = 0.15), data = cps)
    ## 
    ## tau: [1] 0.5
    ## 
    ## Coefficients:
    ##                 coefficients lower bd upper bd
    ## (Intercept)      0.19387     -0.06344  0.53006
    ## experience       0.04496      0.03266  0.05877
    ## I(experience^2) -0.00073     -0.00105 -0.00043
    ## education        0.10776      0.08272  0.12101
    ## 
    ## Call: rq(formula = log(wage) ~ experience + I(experience^2) + education, 
    ##     tau = seq(0.2, 0.8, by = 0.15), data = cps)
    ## 
    ## tau: [1] 0.65
    ## 
    ## Coefficients:
    ##                 coefficients lower bd upper bd
    ## (Intercept)      0.42973      0.20244  0.72517
    ## experience       0.04305      0.02701  0.05434
    ## I(experience^2) -0.00060     -0.00092 -0.00026
    ## education        0.10266      0.08749  0.11923
    ## 
    ## Call: rq(formula = log(wage) ~ experience + I(experience^2) + education, 
    ##     tau = seq(0.2, 0.8, by = 0.15), data = cps)
    ## 
    ## tau: [1] 0.8
    ## 
    ## Coefficients:
    ##                 coefficients lower bd upper bd
    ## (Intercept)      0.61292      0.49682  0.91475
    ## experience       0.03915      0.02421  0.04723
    ## I(experience^2) -0.00051     -0.00073 -0.00026
    ## education        0.10560      0.09150  0.11279

``` r
# Luego se crea una base para simular el efecto de el salario por quintil. Se mantiene la educación constante y se recorre todos los niveles de experiencia

cps2 <- data.frame(education = mean(cps$education), 
                   experience = min(cps$experience):max(cps$experience))

cps2 <- cbind(cps2, predict(reg.lineal, newdata = cps2, interval = "prediction"))
cps2 <- cbind(cps2, predict(reg.quintil, newdata = cps2, type = ""))

plot(log(wage) ~ experience, data = cps)

for(i in 6:10) lines(cps2[,i] ~ experience, data = cps2, col = "red")
```

![](Regresion_files/figure-markdown_github/unnamed-chunk-2-1.png)
