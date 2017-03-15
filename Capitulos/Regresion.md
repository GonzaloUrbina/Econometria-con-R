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
