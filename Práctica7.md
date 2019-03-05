Práctica 7. Introducción a la inferencia
================
AE
19/01/2019

Previo
------

Siempre: revisar el directorio

``` r
setwd("~/Dropbox/SFP")
```

Llamamos nuestra base

``` r
library(foreign)
TPer_Vic1 <- read.dbf("TPer_Vic1_mod.dbf") # esta base tiene las etiquetas del día 3

TPer_Vic1$seguridad_casa<-TPer_Vic1$seguridad_
```

Hipótesis e intervalos de confianza
===================================

t-test
------

Este comando nos sirve para calcular diferentes tipos de test, que tienen como base la distribución t

<b>Univariado para estimación</b>

``` r
t.test(TPer_Vic1$gasto_seg)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  TPer_Vic1$gasto_seg
    ## t = 63.376, df = 34819, p-value < 2.2e-16
    ## alternative hypothesis: true mean is not equal to 0
    ## 95 percent confidence interval:
    ##  6550.497 6968.602
    ## sample estimates:
    ## mean of x 
    ##  6759.549

<b>Univariado para hipótesis específica</b>

``` r
t.test(TPer_Vic1$gasto_seg, mu=200)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  TPer_Vic1$gasto_seg
    ## t = 61.501, df = 34819, p-value < 2.2e-16
    ## alternative hypothesis: true mean is not equal to 200
    ## 95 percent confidence interval:
    ##  6550.497 6968.602
    ## sample estimates:
    ## mean of x 
    ##  6759.549

``` r
t.test(TPer_Vic1$gasto_seg, mu=200, alternative = "two.sided") #default y de dos colas
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  TPer_Vic1$gasto_seg
    ## t = 61.501, df = 34819, p-value < 2.2e-16
    ## alternative hypothesis: true mean is not equal to 200
    ## 95 percent confidence interval:
    ##  6550.497 6968.602
    ## sample estimates:
    ## mean of x 
    ##  6759.549

``` r
t.test(TPer_Vic1$gasto_seg, mu=200, alternative = "less") # cola izquierda
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  TPer_Vic1$gasto_seg
    ## t = 61.501, df = 34819, p-value = 1
    ## alternative hypothesis: true mean is less than 200
    ## 95 percent confidence interval:
    ##     -Inf 6934.99
    ## sample estimates:
    ## mean of x 
    ##  6759.549

``` r
t.test(TPer_Vic1$gasto_seg, mu=200, alternative = "greater") #cola derecha 
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  TPer_Vic1$gasto_seg
    ## t = 61.501, df = 34819, p-value < 2.2e-16
    ## alternative hypothesis: true mean is greater than 200
    ## 95 percent confidence interval:
    ##  6584.109      Inf
    ## sample estimates:
    ## mean of x 
    ##  6759.549

prop.test
=========

Si recordamos un poco de la práctica 3, podemos guardar en un objeto la tabla de frecuencias. Podemos también obtener la estimación puntual de nuestra estimación y también podemos obtener de ahí nuestra prueba que incluye la inferencia.

``` r
freq.sex<-table(TPer_Vic1$SEXO)
prop.table(freq.sex)
```

    ## 
    ##    Hombre     Mujer 
    ## 0.4620116 0.5379884

``` r
freq.sex
```

    ## 
    ## Hombre  Mujer 
    ##  42293  49248

``` r
prop.test(freq.sex)
```

    ## 
    ##  1-sample proportions test with continuity correction
    ## 
    ## data:  freq.sex, null probability 0.5
    ## X-squared = 528.27, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: true p is not equal to 0.5
    ## 95 percent confidence interval:
    ##  0.4587781 0.4652482
    ## sample estimates:
    ##         p 
    ## 0.4620116

Por default, toma los valores de la primera categoría, también podemos hacer la estimación con los datos del total de "éxitos" y el total de "intentos". Calculemos para las mujeres

``` r
prop.test(49248,91541)
```

    ## 
    ##  1-sample proportions test with continuity correction
    ## 
    ## data:  49248 out of 91541, null probability 0.5
    ## X-squared = 528.27, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: true p is not equal to 0.5
    ## 95 percent confidence interval:
    ##  0.5347518 0.5412219
    ## sample estimates:
    ##         p 
    ## 0.5379884

Para hacer la prueba con respecto a un nivel de proporción.

``` r
prop.test(freq.sex, p=0.5,alternative = "two.sided")
```

    ## 
    ##  1-sample proportions test with continuity correction
    ## 
    ## data:  freq.sex, null probability 0.5
    ## X-squared = 528.27, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: true p is not equal to 0.5
    ## 95 percent confidence interval:
    ##  0.4587781 0.4652482
    ## sample estimates:
    ##         p 
    ## 0.4620116

``` r
prop.test(freq.sex, p=0.5, alternative = "greater")
```

    ## 
    ##  1-sample proportions test with continuity correction
    ## 
    ## data:  freq.sex, null probability 0.5
    ## X-squared = 528.27, df = 1, p-value = 1
    ## alternative hypothesis: true p is greater than 0.5
    ## 95 percent confidence interval:
    ##  0.4592969 1.0000000
    ## sample estimates:
    ##         p 
    ## 0.4620116

``` r
prop.test(freq.sex, p=0.5, alternative = "less")
```

    ## 
    ##  1-sample proportions test with continuity correction
    ## 
    ## data:  freq.sex, null probability 0.5
    ## X-squared = 528.27, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: true p is less than 0.5
    ## 95 percent confidence interval:
    ##  0.0000000 0.4647285
    ## sample estimates:
    ##         p 
    ## 0.4620116

La corrección ¿qué hace?

``` r
prop.test(49248,91541, alternative = "greater", p=0.5, correct = F)
```

    ## 
    ##  1-sample proportions test without continuity correction
    ## 
    ## data:  49248 out of 91541, null probability 0.5
    ## X-squared = 528.42, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: true p is greater than 0.5
    ## 95 percent confidence interval:
    ##  0.535277 1.000000
    ## sample estimates:
    ##         p 
    ## 0.5379884

La función prop.test no realiza una prueba z, como casi todos los libros de estadística establece. ¡Hace una prueba de Chi cuadrado, basada en que hay una variable categórica con dos estados (éxito y fracaso)! Por ello vemos la línea que comienza con "X-cuadrado" La corrección de continuidad de Yates, que se ajusta a las diferencias que surgen al utilizar una aproximación normal a la distribución binomial, también se aplica automáticamente. Esto elimina 0.5 / n del límite inferior del intervalo de confianza y agrega 0.5 / n al límite superior. El intervalo de confianza dado por la prueba de propiedades no está la estimación de la muestra, p-hat. ¡Oh no! Pero, de nuevo, esto no es preocupante, ya que prop.test usa el intervalo de puntuación de Wilson para construir el intervalo de confianza. Esto da como resultado un intervalo de confianza asimétrico, pero presumiblemente más preciso (con respecto a la población real).

Estimaciones bivariadas
=======================

Diferencias de medias por grupos
--------------------------------

¿Podemos decir, con significancia estadística que los valores medios de una variable son diferentes entre los grupos?

``` r
tapply(TPer_Vic1$gasto_seg,TPer_Vic1$SEXO, mean, na.rm=T)
```

    ##   Hombre    Mujer 
    ## 7544.987 6042.628

``` r
t.test(TPer_Vic1$gasto_seg~TPer_Vic1$SEXO)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  TPer_Vic1$gasto_seg by TPer_Vic1$SEXO
    ## t = 7.1378, df = 33300, p-value = 9.678e-13
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  1089.811 1914.906
    ## sample estimates:
    ## mean in group Hombre  mean in group Mujer 
    ##             7544.987             6042.628

### No tan bonito, las proporciones para dos poblaciones

``` r
#Prueba de proporciones
table(TPer_Vic1$seguridad_)
```

    ## 
    ##  Inseguro No Aplica    Seguro 
    ##     22620        17     68843

``` r
TPer_Vic1$seguridad_casa2<-TPer_Vic1$seguridad_
TPer_Vic1$seguridad_casa2[TPer_Vic1$seguridad_=="No Aplica"]<-NA
table(TPer_Vic1$seguridad_casa2)
```

    ## 
    ##  Inseguro No Aplica    Seguro 
    ##     22620         0     68843

``` r
addmargins(table(TPer_Vic1[TPer_Vic1$SEXO=="Hombre",]$seguridad_casa2))
```

    ## 
    ##  Inseguro No Aplica    Seguro       Sum 
    ##      9262         0     32991     42253

``` r
addmargins(table(TPer_Vic1[TPer_Vic1$SEXO=="Mujer",]$seguridad_casa2))
```

    ## 
    ##  Inseguro No Aplica    Seguro       Sum 
    ##     13358         0     35852     49210

``` r
prop.test(c(9262,13358),c(42253, 49210))
```

    ## 
    ##  2-sample test for equality of proportions with continuity
    ##  correction
    ## 
    ## data:  c(9262, 13358) out of c(42253, 49210)
    ## X-squared = 333.07, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.05783515 -0.04665590
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.2192034 0.2714489

``` r
prop.test(c(9262,13358), c(42253, 49210), alternative= "two.sided")
```

    ## 
    ##  2-sample test for equality of proportions with continuity
    ##  correction
    ## 
    ## data:  c(9262, 13358) out of c(42253, 49210)
    ## X-squared = 333.07, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.05783515 -0.04665590
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.2192034 0.2714489

``` r
prop.test(c(9262,13358), c(42253, 49210), alternative= "greater")
```

    ## 
    ##  2-sample test for equality of proportions with continuity
    ##  correction
    ## 
    ## data:  c(9262, 13358) out of c(42253, 49210)
    ## X-squared = 333.07, df = 1, p-value = 1
    ## alternative hypothesis: greater
    ## 95 percent confidence interval:
    ##  -0.05694002  1.00000000
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.2192034 0.2714489

``` r
prop.test(c(9262,13358), c(42253, 49210), alternative= "less")
```

    ## 
    ##  2-sample test for equality of proportions with continuity
    ##  correction
    ## 
    ## data:  c(9262, 13358) out of c(42253, 49210)
    ## X-squared = 333.07, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: less
    ## 95 percent confidence interval:
    ##  -1.00000000 -0.04755103
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.2192034 0.2714489

Estimación de varianzas y sus pruebas de hipótesis
--------------------------------------------------

Para poder hacer inferencia sobre la varianza utilizamos el comando varTest() del paquete "EnvStats"

``` r
install.packages("EnvStats", repos = "http://cran.us.r-project.org", dependencies = T)
```

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//RtmpEZjU6l/downloaded_packages

``` r
library(EnvStats)
```

    ## 
    ## Attaching package: 'EnvStats'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     predict, predict.lm

    ## The following object is masked from 'package:base':
    ## 
    ##     print.default

``` r
varTest(TPer_Vic1$gasto_seg)
```

    ## Warning in is.not.finite.warning(x): There were 56721 nonfinite values in
    ## x : 56721 NA's

    ## Warning in varTest(TPer_Vic1$gasto_seg): 56721 observations with NA/NaN/Inf
    ## in 'x' removed.

    ## 
    ##  Chi-Squared Test on Variance
    ## 
    ## data:  TPer_Vic1$gasto_seg
    ## Chi-Squared = 1.3792e+13, df = 34819, p-value < 2.2e-16
    ## alternative hypothesis: true variance is not equal to 1
    ## 95 percent confidence interval:
    ##  390288504 402057627
    ## sample estimates:
    ##  variance 
    ## 396107208

Podemos también decir algo sobre el valor objetivo de nuestra hipótesis

``` r
varTest(TPer_Vic1$gasto_seg, sigma.squared = 150000)
```

    ## Warning in is.not.finite.warning(x): There were 56721 nonfinite values in
    ## x : 56721 NA's

    ## Warning in varTest(TPer_Vic1$gasto_seg, sigma.squared = 150000): 56721
    ## observations with NA/NaN/Inf in 'x' removed.

    ## 
    ##  Chi-Squared Test on Variance
    ## 
    ## data:  TPer_Vic1$gasto_seg
    ## Chi-Squared = 91947000, df = 34819, p-value < 2.2e-16
    ## alternative hypothesis: true variance is not equal to 150000
    ## 95 percent confidence interval:
    ##  390288504 402057627
    ## sample estimates:
    ##  variance 
    ## 396107208

Guardar como objeto nuestros resultados, siempres muy conveniente para pedir después o para realizar operaciones con ellos

``` r
test2<-varTest(TPer_Vic1$gasto_seg)
```

    ## Warning in is.not.finite.warning(x): There were 56721 nonfinite values in
    ## x : 56721 NA's

    ## Warning in varTest(TPer_Vic1$gasto_seg): 56721 observations with NA/NaN/Inf
    ## in 'x' removed.

``` r
test2$conf.int
```

    ##       LCL       UCL 
    ## 390288504 402057627 
    ## attr(,"conf.level")
    ## [1] 0.95

``` r
sqrt(test2$conf.int) ## sacamos la raíz cuadrada para tener las
```

    ##      LCL      UCL 
    ## 19755.72 20051.37 
    ## attr(,"conf.level")
    ## [1] 0.95

``` r
#desviaciones estándar y sea más fácil de interpretar
```

Estimación de diferencias de varianzas y sus pruebas de hipótesis
-----------------------------------------------------------------

Para comparar varianza, usamos su "ratio", esto nos da un estadístico de prueba F, para comparar dos muestras de poblaciones normales.

``` r
var.test(TPer_Vic1$gasto_seg, TPer_Vic1$EDAD, ratio=1)
```

    ## 
    ##  F test to compare two variances
    ## 
    ## data:  TPer_Vic1$gasto_seg and TPer_Vic1$EDAD
    ## F = 1418900, num df = 34819, denom df = 91081, p-value < 2.2e-16
    ## alternative hypothesis: true ratio of variances is not equal to 1
    ## 95 percent confidence interval:
    ##  1394406 1443972
    ## sample estimates:
    ## ratio of variances 
    ##            1418924

``` r
var.test(TPer_Vic1$gasto_seg, TPer_Vic1$EDAD, ratio=0.5, conf.level = 0.99)
```

    ## 
    ##  F test to compare two variances
    ## 
    ## data:  TPer_Vic1$gasto_seg and TPer_Vic1$EDAD
    ## F = 2837800, num df = 34819, denom df = 91081, p-value < 2.2e-16
    ## alternative hypothesis: true ratio of variances is not equal to 0.5
    ## 99 percent confidence interval:
    ##  1386798 1451942
    ## sample estimates:
    ## ratio of variances 
    ##            1418924

¿Tendrá sentido comparar la varianza del gasto y de la edad? Piénsalo.

Si lo que queremos es comparar la varianza entre dos grupos, usamos el signo ~

``` r
var.test(TPer_Vic1$gasto_seg ~TPer_Vic1$SEXO, ratio=1, conf.level = 0.99)
```

    ## 
    ##  F test to compare two variances
    ## 
    ## data:  TPer_Vic1$gasto_seg by TPer_Vic1$SEXO
    ## F = 0.53733, num df = 16615, denom df = 18203, p-value < 2.2e-16
    ## alternative hypothesis: true ratio of variances is not equal to 1
    ## 99 percent confidence interval:
    ##  0.5167369 0.5587532
    ## sample estimates:
    ## ratio of variances 
    ##          0.5373264

Para complementar... los gráficos y resolver missings
=====================================================

Gráficamente lo podemos ver en un "boxplot" \[Recuerda que si has seguido las prácticas, no tienes que volver a instalar las librerías\]

``` r
library(ggplot2)
```

Ocupamos qplot para graficarlos como un boxplot

``` r
qq<-qplot(SEXO, gasto_seg, data = TPer_Vic1, geom = "boxplot")
qq #imprimo mi objeto
```

    ## Warning: Removed 56721 rows containing non-finite values (stat_boxplot).

![](Práctica7_files/figure-markdown_github/unnamed-chunk-17-1.png)

``` r
qq2<-qplot(seguridad_casa, gasto_seg, data = TPer_Vic1, geom = "boxplot")
qq2 #imprimo mi objeto
```

    ## Warning: Removed 56721 rows containing non-finite values (stat_boxplot).

![](Práctica7_files/figure-markdown_github/unnamed-chunk-17-2.png)

Podemos también ver estas diferencias por sexo, con una tercera variable, que serán nuestros "facets""

``` r
qplot(SEXO, gasto_seg, data = TPer_Vic1, geom = "boxplot") + facet_grid(. ~seguridad_casa)
```

    ## Warning: Removed 56721 rows containing non-finite values (stat_boxplot).

![](Práctica7_files/figure-markdown_github/unnamed-chunk-18-1.png)

Si no queremos usar el grid (que da control de qué va en horizontal y en lo vertical), podemos usar facet\_wrap

``` r
qplot(SEXO, gasto_seg, data = TPer_Vic1, geom = "boxplot") + facet_wrap(~seguridad_casa)
```

    ## Warning: Removed 56721 rows containing non-finite values (stat_boxplot).

![](Práctica7_files/figure-markdown_github/unnamed-chunk-19-1.png)

Missings
--------

``` r
mydata<- TPer_Vic1[, c("gasto_seg", "ESTRATO","EDAD", "SEXO", "seguridad_casa")]

newdata <- na.omit(mydata)
qplot(SEXO, gasto_seg, data = newdata, geom = "boxplot") + facet_wrap(~seguridad_casa)
```

![](Práctica7_files/figure-markdown_github/unnamed-chunk-20-1.png)

Prueba chi-cuadrado chi-sq. Una aplicación más común
====================================================

Cuando tenemos dos variables cualitativas o nominales podemos hacer esta la prueba chi-cuadrado, o prueba de independencia. Esta tiene una lógica un poco diferente a las pruebas que hacemos, porque proviene de comparar la distribución de los datos dado que no hay independencia entre las variables y los datos que tenemos. La hipótesis nula postula una distribución de probabilidad totalmente especificada como el modelo matemático de la población que ha generado la muestra, por lo que si la rechazamos hemos encontrado evidencia estadística sobre la dependencia de las dos variables.

``` r
table(newdata$seguridad_casa, newdata$SEXO)
```

    ##            
    ##             Hombre Mujer
    ##   Inseguro    5065  6644
    ##   No Aplica      3     0
    ##   Seguro     11472 11472

``` r
chisq.test(newdata$seguridad_casa, newdata$SEXO)
```

    ## Warning in chisq.test(newdata$seguridad_casa, newdata$SEXO): Chi-squared
    ## approximation may be incorrect

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  newdata$seguridad_casa and newdata$SEXO
    ## X-squared = 144.56, df = 2, p-value < 2.2e-16

Cuando hay categorías con ceros nos da este warning de mala estimación

``` r
newdata2<-newdata[newdata$seguridad_casa!="No Aplica",]
table(newdata2$seguridad_casa, newdata2$SEXO)
```

    ##            
    ##             Hombre Mujer
    ##   Inseguro    5065  6644
    ##   No Aplica      0     0
    ##   Seguro     11472 11472

``` r
chisq.test(newdata2$seguridad_casa, newdata2$SEXO)
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  newdata2$seguridad_casa and newdata2$SEXO
    ## X-squared = 141.01, df = 1, p-value < 2.2e-16

Nuevo
=====

Intro intro intro a evaluación
------------------------------

La prueba t que vimos al inicio puede también servir para comparar una misma población en dos momentos diferentes.

Para eso importaremos una nueva base llamada paired, que contiene la serie de las tasa de homicidios por estado para 2015 y 2016

``` r
library(readxl)
paired <- read_excel("paired.xlsx")
#View(paired)
```

Hoy que ya la tenemos podemos volver a establecer una prueba de t de diferencias, pero hoy de diferencias de una población en dos momentos del tiempo, tenemos que activar el argumento "paired", para decirle que lo que tenemos son muestras apareadas

``` r
t.test(paired$Homicidios_2016,paired$Homicidios_2015, paired = T)
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  paired$Homicidios_2016 and paired$Homicidios_2015
    ## t = 2.5177, df = 31, p-value = 0.01719
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  0.7627398 7.2687956
    ## sample estimates:
    ## mean of the differences 
    ##                4.015768

Checa la diferencia de los grados de libertad, como se trata de una sola población y un parámetro, la media, tenemos 31

Las opciones para el nivel de confianza se mantienen.
