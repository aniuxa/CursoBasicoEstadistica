Práctica 10. Post-estimación y Regresión lineal múltiple
================
AE
02/02/2019

-   [Previo: base de trabajo](#previo-base-de-trabajo)
-   [Regresión Lineal múltiple](#regresión-lineal-múltiple)
    -   [Agregando una variable categórica](#agregando-una-variable-categórica)
    -   [Otros supuestos](#otros-supuestos)
    -   [Tabla de modelos estimados](#tabla-de-modelos-estimados)
    -   [Estandarizando](#estandarizando)
-   [Post-estimación](#post-estimación)
    -   [Las predicciones](#las-predicciones)
    -   [Efectos marginales](#efectos-marginales)
-   [Extensiones del modelo de regresión](#extensiones-del-modelo-de-regresión)
    -   [Introducción a las interacciones](#introducción-a-las-interacciones)
    -   [Efectos no lineales](#efectos-no-lineales)
-   [No cumplo los supuestos](#no-cumplo-los-supuestos)
    -   [Heterocedasticidad](#heterocedasticidad)
    -   [Normalidad y Outliers](#normalidad-y-outliers)

En esta Práctica vamos a aprender muchos más paquetes. Listos para instalar. R se trata de conocer los paquetes que nos harán la vida más sencilla y que nos permitan mostrar nuestros resultados de la manera más clara, esto para acompañar un modelo estadístico bien pensado y con evidencia teória de fondo.

Previo: base de trabajo
=======================

¡Recuerda, poner el directorio!

``` r
setwd("/Users/anaescoto/Dropbox/SFP")
```

Vamos a trabajar con la ENVIPE, tal como lo hicimos en la <a href="https://rpubs.com/aniuxa/SFP9">práctica 9</a>.

Cargamos el ambiente (puedes descargarlo de <a href="https://www.dropbox.com/s/hfppwlr2h8qe4po/EnvironmentP9.RData?dl=0">aquí</a>)

``` r
load("EnvironmentP9.RData")
```

Vamos a hacer una sub-base de nuestras posibles variables explicativas. Esto es importante porque sólo podemos comparar modelos con la misma cantidad de observaciones.

``` r
mydata<- TPer_Vic1_Hog[ ,c("gasto_seg_pc","index_inseg0",
                           "index_inseg", "log_gasto_seg_pc"
                           ,"ESTRATO","EDAD", "SEXO"
                           )]
newdata <- na.omit(mydata)
```

Volvemos a correr nuestro modelo, hoy con esta base. Como es nuestro modelo original, le pondremos cero

``` r
modelo0<-lm(log_gasto_seg_pc ~index_inseg, data=newdata, na.action=na.exclude)
summary(modelo0)
```


    Call:
    lm(formula = log_gasto_seg_pc ~ index_inseg, data = newdata, 
        na.action = na.exclude)

    Residuals:
       Min     1Q Median     3Q    Max 
    -6.703 -1.100  0.455  1.662  7.238 

    Coefficients:
                Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  5.39470    0.02892   186.6   <2e-16 ***
    index_inseg  1.30812    0.05130    25.5   <2e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 2.446 on 34672 degrees of freedom
    Multiple R-squared:  0.01841,   Adjusted R-squared:  0.01838 
    F-statistic: 650.3 on 1 and 34672 DF,  p-value: < 2.2e-16

Regresión Lineal múltiple
=========================

Agregando una variable categórica
---------------------------------

Cuando nosotros tenemos una variable categórica para la condición de sexo. \[nota: seguimos haciendo el ejercicio, a pesar de que ya observamos en nuestro diagnóstico el modelo no cumple con los supuestos, pero lo haremos para fines ilustrativos\]

``` r
modelo1<-lm(log_gasto_seg_pc ~index_inseg + SEXO, data=newdata, na.action=na.exclude)
summary(modelo1)
```


    Call:
    lm(formula = log_gasto_seg_pc ~ index_inseg + SEXO, data = newdata, 
        na.action = na.exclude)

    Residuals:
       Min     1Q Median     3Q    Max 
    -6.816 -1.081  0.436  1.634  7.433 

    Coefficients:
                Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  5.55606    0.03100  179.21   <2e-16 ***
    index_inseg  1.37498    0.05137   26.77   <2e-16 ***
    SEXOMujer   -0.37294    0.02634  -14.16   <2e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 2.439 on 34671 degrees of freedom
    Multiple R-squared:  0.02405,   Adjusted R-squared:  0.024 
    F-statistic: 427.3 on 2 and 34671 DF,  p-value: < 2.2e-16

Este modelo tiene coeficientes que deben leerse "condicionados". Es decir, en este caso tenemos que el coeficiente asociado a "index\_inseg", mantiene constante el valor de sexo y viceversa.

¿Cómo saber is ha mejorado nuestro modelo? Podemos comparar el ajuste con la anova, es decir, una prueba F

``` r
pruebaf0<-anova(modelo0, modelo1)
pruebaf0
```

    Analysis of Variance Table

    Model 1: log_gasto_seg_pc ~ index_inseg
    Model 2: log_gasto_seg_pc ~ index_inseg + SEXO
      Res.Df    RSS Df Sum of Sq      F    Pr(>F)    
    1  34672 207479                                  
    2  34671 206286  1      1193 200.51 < 2.2e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Como puedes ver, el resultado muestra un Df de 1 (lo que indica que el modelo más complejo tiene un parámetro adicional) y un valor p muy pequeño (&lt;.001). Esto significa que agregar el sexo al modelo lleva a un ajuste significativamente mejor sobre el modelo original.

Podemos seguir añadiendo variables sólo "sumando" en la función

``` r
modelo2<-lm(log_gasto_seg_pc ~index_inseg + SEXO + EDAD, data=newdata, na.action=na.exclude)
summary(modelo2)
```


    Call:
    lm(formula = log_gasto_seg_pc ~ index_inseg + SEXO + EDAD, data = newdata, 
        na.action = na.exclude)

    Residuals:
       Min     1Q Median     3Q    Max 
    -7.026 -1.072  0.440  1.633  7.374 

    Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  5.2600839  0.0478765 109.868  < 2e-16 ***
    index_inseg  1.3861362  0.0513393  27.000  < 2e-16 ***
    SEXOMujer   -0.3728914  0.0263127 -14.172  < 2e-16 ***
    EDAD         0.0071799  0.0008856   8.107 5.34e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 2.437 on 34670 degrees of freedom
    Multiple R-squared:  0.0259,    Adjusted R-squared:  0.02582 
    F-statistic: 307.3 on 3 and 34670 DF,  p-value: < 2.2e-16

Y podemos ver si introducir esta variable afectó al ajuste global del modelo

``` r
pruebaf1<-anova(modelo1, modelo2)
pruebaf1
```

    Analysis of Variance Table

    Model 1: log_gasto_seg_pc ~ index_inseg + SEXO
    Model 2: log_gasto_seg_pc ~ index_inseg + SEXO + EDAD
      Res.Df    RSS Df Sum of Sq     F   Pr(>F)    
    1  34671 206286                                
    2  34670 205896  1    390.35 65.73 5.34e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Hoy que tenemos más variables podemos hablar de revisar dos supuestos más.

Otros supuestos
---------------

Además de los supuestos de la regresión simple, podemos revisar estos otros. De nuevo, usaremos la librería "car",

1.  Linealidad en los parámetros (será más díficil entre más variables tengamos)

2.  La normalidad también, porque debe ser multivariada

3.  Multicolinealidad La prueba más común es la de Factor Influyente de la Varianza (VIF) por sus siglas en inglés. La lógica es que la multicolinealidad tendrá efectos en nuestro R2, inflándolo. De ahí que observamos de qué variable(s) proviene este problema relacionado con la multicolinealidad.

Si el valor es mayor a 5, tenemos un problema muy grave.

``` r
library(car)
```

    Loading required package: carData

``` r
vif(modelo2)
```

    index_inseg        SEXO        EDAD 
       1.009245    1.008519    1.000727 

Tabla de modelos estimados
--------------------------

``` r
install.packages("memisc", repos = "http://cran.us.r-project.org", 
                          dependencies = TRUE)
```


    The downloaded binary packages are in
        /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//RtmpIpG3C8/downloaded_packages

Comandos para "enchular" la presentación de resultados

``` r
library(memisc)
```

    Loading required package: lattice

    Loading required package: MASS


    Attaching package: 'memisc'

    The following object is masked from 'package:car':

        recode

    The following objects are masked from 'package:stats':

        contr.sum, contr.treatment, contrasts

    The following object is masked from 'package:base':

        as.array

``` r
mtable<-mtable("Modelo 0"=modelo0, "Modelo 1"= modelo1,
               "Modelo 2"=modelo2)
#show_html(mtable)
write.mtable(mtable, file="modelo1.html", format="HTML")
```

Checa todas las medidas de ajuste que nos da este paquete. Va más allá del R2 ajustado. ¿Sabes de dónde provienen y qué miden?

Para los muy avanzados, con el paquete "stargazer" se pueden pasar a LaTeX fácilmente.

``` r
install.packages("stargazer", 
                 repos = "http://cran.us.r-project.org", 
                          dependencies = TRUE)
```


    The downloaded binary packages are in
        /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//RtmpIpG3C8/downloaded_packages

``` r
library(stargazer)
```

``` r
#stargazer(modelo0, modelo1,modelo2, type = 'latex', header=FALSE)
```

``` r
stargazer(modelo0, modelo1,modelo2, type = 'text', header=FALSE)
```


    ====================================================================================================
                                                      Dependent variable:                               
                        --------------------------------------------------------------------------------
                                                        log_gasto_seg_pc                                
                                   (1)                        (2)                        (3)            
    ----------------------------------------------------------------------------------------------------
    index_inseg                  1.308***                   1.375***                   1.386***         
                                 (0.051)                    (0.051)                    (0.051)          
                                                                                                        
    SEXOMujer                                              -0.373***                  -0.373***         
                                                            (0.026)                    (0.026)          
                                                                                                        
    EDAD                                                                               0.007***         
                                                                                       (0.001)          
                                                                                                        
    Constant                     5.395***                   5.556***                   5.260***         
                                 (0.029)                    (0.031)                    (0.048)          
                                                                                                        
    ----------------------------------------------------------------------------------------------------
    Observations                  34,674                     34,674                     34,674          
    R2                            0.018                      0.024                      0.026           
    Adjusted R2                   0.018                      0.024                      0.026           
    Residual Std. Error     2.446 (df = 34672)         2.439 (df = 34671)         2.437 (df = 34670)    
    F Statistic         650.268*** (df = 1; 34672) 427.260*** (df = 2; 34671) 307.282*** (df = 3; 34670)
    ====================================================================================================
    Note:                                                                    *p<0.1; **p<0.05; ***p<0.01

También la librería "sjPlot" tiene el comando "plot\_model()" (instala el comando si no lo tienes)

``` r
library(sjPlot)
plot_model(modelo1)
```

![](Practica10_files/figure-markdown_github/unnamed-chunk-13-1.png)

Estandarizando
--------------

Comparar los resultados de los coeficientes es díficil, porque el efecto está medido en las unidades que fueron medidas. Por lo que no sería tan comparable el efecto que tenemos de nuestro índice sumativo (proporción de lugares con inseguridad declarada) con respecto a la edad (que se mide en años). Por lo que a veces es mejor usar las medida estandarizadas (es decir, nuestra puntajes z).

Podemos hacerlo transormando nuestras variables de origen e introducirlas al modelo. O bien, podemos usar un paquete que lo hace directamente. Los coeficientes calculados se les concoe como "beta"

``` r
install.packages("lm.beta", repos = "http://cran.us.r-project.org",                           dependencies = TRUE)
```


    The downloaded binary packages are in
        /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//RtmpIpG3C8/downloaded_packages

``` r
library("lm.beta")
```

Simplemente aplicamos el comando a nuestros modelos ya calculados

``` r
lm.beta(modelo2)
```


    Call:
    lm(formula = log_gasto_seg_pc ~ index_inseg + SEXO + EDAD, data = newdata, 
        na.action = na.exclude)

    Standardized Coefficients::
    (Intercept) index_inseg   SEXOMujer        EDAD 
     0.00000000  0.14377366 -0.07543694  0.04298967 

Hoy la comparación será mucho más clara y podemos ver qué variable tiene mayor efecto en nuestra dependiente.

``` r
modelo_beta<-lm.beta(modelo2)
modelo_beta
```


    Call:
    lm(formula = log_gasto_seg_pc ~ index_inseg + SEXO + EDAD, data = newdata, 
        na.action = na.exclude)

    Standardized Coefficients::
    (Intercept) index_inseg   SEXOMujer        EDAD 
     0.00000000  0.14377366 -0.07543694  0.04298967 

Para graficarlos, podemos usar de nuevo el comando plot\_model(), con una opción

``` r
plot_model(modelo2, type="std")
```

![](Practica10_files/figure-markdown_github/unnamed-chunk-17-1.png)

Post-estimación
===============

Las predicciones
----------------

Unos de los usos más comunes de los modelos estadísticos es la predicción

``` r
plot_model(modelo2, type="pred", terms = "EDAD")
```

![](Practica10_files/figure-markdown_github/unnamed-chunk-18-1.png)

También podemos incluir la predecciones para los distintos valores de las variables

``` r
plot_model(modelo2, type="pred", terms = c("EDAD", "index_inseg", "SEXO")) + theme_blank()
```

![](Practica10_files/figure-markdown_github/unnamed-chunk-19-1.png)

¿Qué podemos concluir de estos resultados?

Efectos marginales
------------------

Con los efectos marginales, por otro lado medimos el efecto promedio, dejando el resto de variables constantes.

``` r
plot_model(modelo2, type="eff", terms = "EDAD")
```

![](Practica10_files/figure-markdown_github/unnamed-chunk-20-1.png)

``` r
plot_model(modelo2, type="eff", terms = "SEXO")
```

![](Practica10_files/figure-markdown_github/unnamed-chunk-20-2.png) ¿Es el mismo gráfico que con "pred"?

Extensiones del modelo de regresión
===================================

Introducción a las interacciones
--------------------------------

Muchas veces las variables explicativas van a tener relación entre sí. Por ejemplo ¿nuestra percepción de la seguridad tendrá que ver con la edad? Para ello podemos introducir una interacción

``` r
modelo_int1<-lm(log_gasto_seg_pc ~index_inseg * EDAD, data=newdata, na.action=na.exclude)
summary(modelo_int1)
```


    Call:
    lm(formula = log_gasto_seg_pc ~ index_inseg * EDAD, data = newdata, 
        na.action = na.exclude)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -7.1810 -1.0871  0.4533  1.6473  7.2115 

    Coefficients:
                      Estimate Std. Error t value Pr(>|t|)    
    (Intercept)       5.573950   0.079050  70.511   <2e-16 ***
    index_inseg       0.269471   0.150051   1.796   0.0725 .  
    EDAD             -0.004104   0.001757  -2.336   0.0195 *  
    index_inseg:EDAD  0.025156   0.003379   7.444    1e-13 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 2.442 on 34670 degrees of freedom
    Multiple R-squared:  0.02182,   Adjusted R-squared:  0.02174 
    F-statistic: 257.8 on 3 and 34670 DF,  p-value: < 2.2e-16

Esta interaccion lo que asume es que

``` r
plot_model(modelo_int1, type="int", terms = c("index_inseg", "EDAD"))
```

![](Practica10_files/figure-markdown_github/unnamed-chunk-22-1.png)

Efectos no lineales
-------------------

``` r
#Explicitando el logaritmo
index_inseg100<-newdata$index_inseg*100+1
modelo_log<-lm(log_gasto_seg_pc~log(index_inseg100) + SEXO+ EDAD, data=newdata, na.action=na.exclude)
summary(modelo_log)
```


    Call:
    lm(formula = log_gasto_seg_pc ~ log(index_inseg100) + SEXO + 
        EDAD, data = newdata, na.action = na.exclude)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -6.8242 -1.0787  0.4484  1.6354  7.1395 

    Coefficients:
                          Estimate Std. Error t value Pr(>|t|)    
    (Intercept)          4.7623392  0.0620772  76.716   <2e-16 ***
    log(index_inseg100)  0.3182139  0.0126067  25.242   <2e-16 ***
    SEXOMujer           -0.3676068  0.0263429 -13.955   <2e-16 ***
    EDAD                 0.0079350  0.0008882   8.934   <2e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 2.44 on 34670 degrees of freedom
    Multiple R-squared:  0.02337,   Adjusted R-squared:  0.02328 
    F-statistic: 276.5 on 3 and 34670 DF,  p-value: < 2.2e-16

``` r
plot_model(modelo_log, type="pred", terms ="index_inseg100")
```

    Model has log-transformed predictors. Consider using `terms="index_inseg100 [exp]"` to back-transform scale.

![](Practica10_files/figure-markdown_github/unnamed-chunk-24-1.png)

``` r
#Efecto cuadrático (ojo con la sintaxis)
modelo_quadr<-lm(log_gasto_seg_pc ~index_inseg + SEXO+ EDAD + I(EDAD^2), data=newdata, na.action=na.exclude)
summary(modelo_quadr)
```


    Call:
    lm(formula = log_gasto_seg_pc ~ index_inseg + SEXO + EDAD + I(EDAD^2), 
        data = newdata, na.action = na.exclude)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -6.9693 -1.0624  0.4421  1.6293  7.2306 

    Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  4.346e+00  9.705e-02   44.78   <2e-16 ***
    index_inseg  1.298e+00  5.190e-02   25.00   <2e-16 ***
    SEXOMujer   -3.705e-01  2.627e-02  -14.10   <2e-16 ***
    EDAD         5.524e-02  4.530e-03   12.19   <2e-16 ***
    I(EDAD^2)   -5.323e-04  4.921e-05  -10.82   <2e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 2.433 on 34669 degrees of freedom
    Multiple R-squared:  0.02918,   Adjusted R-squared:  0.02906 
    F-statistic: 260.5 on 4 and 34669 DF,  p-value: < 2.2e-16

¿Qué significa que el segundo término es negativo? Quizás con un gráfico de lo predicho tenemos más claro lo que hace ese término

``` r
plot_model(modelo_quadr, type="pred", terms = c("EDAD"))
```

![](Practica10_files/figure-markdown_github/unnamed-chunk-26-1.png)

En efecto, lo que nos da el signo del cuadrático puede hablarnos del comportamiento cóncavo.

No cumplo los supuestos
=======================

Heterocedasticidad
------------------

El problema de la heterocedasticidad es que los errores estándar de subestiman, por lo que si estos están en el cociente de nuestro estadístico de prueba t, esto implicaría que nuestras pruebas podrían estar arrojando valores significativos cuando no lo son.

Una forma muy sencilla es pedir los errores robustos, importando una función <https://economictheoryblog.com/2016/08/08/robust-standard-errors-in-r/>

``` r
library(RCurl)
```

    Loading required package: bitops

``` r
url_robust <- "https://raw.githubusercontent.com/IsidoreBeautrelet/economictheoryblog/master/robust_summary.R"
eval(parse(text = getURL(url_robust, ssl.verifypeer = FALSE)),
     envir=.GlobalEnv)


summary(modelo2, robust=T)
```


    Call:
    lm(formula = log_gasto_seg_pc ~ index_inseg + SEXO + EDAD, data = newdata, 
        na.action = na.exclude)

    Residuals:
       Min     1Q Median     3Q    Max 
    -7.026 -1.072  0.440  1.633  7.374 

    Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  5.2600839  0.0486002 108.232  < 2e-16 ***
    index_inseg  1.3861362  0.0531808  26.065  < 2e-16 ***
    SEXOMujer   -0.3728914  0.0263150 -14.170  < 2e-16 ***
    EDAD         0.0071799  0.0009413   7.627 2.46e-14 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 2.437 on 34670 degrees of freedom
    Multiple R-squared:  0.0259,    Adjusted R-squared:  0.02582 
    F-statistic: 301.1 on 3 and 34670 DF,  p-value: < 2.2e-16

``` r
summary(modelo2, robust=F)
```


    Call:
    lm(formula = log_gasto_seg_pc ~ index_inseg + SEXO + EDAD, data = newdata, 
        na.action = na.exclude)

    Residuals:
       Min     1Q Median     3Q    Max 
    -7.026 -1.072  0.440  1.633  7.374 

    Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  5.2600839  0.0478765 109.868  < 2e-16 ***
    index_inseg  1.3861362  0.0513393  27.000  < 2e-16 ***
    SEXOMujer   -0.3728914  0.0263127 -14.172  < 2e-16 ***
    EDAD         0.0071799  0.0008856   8.107 5.34e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 2.437 on 34670 degrees of freedom
    Multiple R-squared:  0.0259,    Adjusted R-squared:  0.02582 
    F-statistic: 307.3 on 3 and 34670 DF,  p-value: < 2.2e-16

### Matriz varianza-covarianza

``` r
#install.packages("robustbase", repos = "http://cran.us.r-project.org",                           dependencies = TRUE)
library(robustbase)
modelo2rob<-lmrob(log_gasto_seg_pc ~index_inseg + SEXO + EDAD, data=newdata, na.action=na.exclude)
summary(modelo2rob)
```


    Call:
    lmrob(formula = log_gasto_seg_pc ~ index_inseg + SEXO + EDAD, data = newdata, 
        na.action = na.exclude)
     \--> method = "MM"
    Residuals:
         Min       1Q   Median       3Q      Max 
    -7.60464 -1.46816  0.04884  1.23366  6.84128 

    Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  5.5739596  0.0435900  127.87   <2e-16 ***
    index_inseg  1.0544999  0.0489416   21.55   <2e-16 ***
    SEXOMujer   -0.4073613  0.0226376  -18.00   <2e-16 ***
    EDAD         0.0140479  0.0008302   16.92   <2e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Robust residual standard error: 1.924 
    Multiple R-squared:  0.03406,   Adjusted R-squared:  0.03397 
    Convergence in 13 IRWLS iterations

    Robustness weights: 
     2792 weights are ~= 1. The remaining 31882 ones are summarized as
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    0.08291 0.86720 0.94880 0.86380 0.98470 0.99900 
    Algorithmic parameters: 
           tuning.chi                bb        tuning.psi        refine.tol 
            1.548e+00         5.000e-01         4.685e+00         1.000e-07 
              rel.tol         scale.tol         solve.tol       eps.outlier 
            1.000e-07         1.000e-10         1.000e-07         2.884e-06 
                eps.x warn.limit.reject warn.limit.meanrw 
            1.764e-10         5.000e-01         5.000e-01 
         nResample         max.it       best.r.s       k.fast.s          k.max 
               500             50              2              1            200 
       maxit.scale      trace.lev            mts     compute.rd fast.s.large.n 
               200              0           1000              0           2000 
                      psi           subsampling                   cov 
               "bisquare"         "nonsingular"         ".vcov.avar1" 
    compute.outlier.stats 
                     "SM" 
    seed : int(0) 

### Regresión robusta

No es lo mismo la regresión robusta que los errores robustos. La regresión robusta es más robusta a los outliers. No confundir.

``` r
stargazer(modelo2, modelo2rob, type = 'text', header=FALSE)
```


    =====================================================================
                                             Dependent variable:         
                                     ------------------------------------
                                               log_gasto_seg_pc          
                                                OLS              MM-type 
                                                                 linear  
                                                (1)                (2)   
    ---------------------------------------------------------------------
    index_inseg                               1.386***          1.054*** 
                                              (0.051)            (0.049) 
                                                                         
    SEXOMujer                                -0.373***          -0.407***
                                              (0.026)            (0.023) 
                                                                         
    EDAD                                      0.007***          0.014*** 
                                              (0.001)            (0.001) 
                                                                         
    Constant                                  5.260***          5.574*** 
                                              (0.048)            (0.044) 
                                                                         
    ---------------------------------------------------------------------
    Observations                               34,674            34,674  
    R2                                         0.026              0.034  
    Adjusted R2                                0.026              0.034  
    Residual Std. Error (df = 34670)           2.437              1.924  
    F Statistic                      307.282*** (df = 3; 34670)          
    =====================================================================
    Note:                                     *p<0.1; **p<0.05; ***p<0.01

Normalidad y Outliers
---------------------

Se pueden usar métodos no paramétricos, como la regresión mediana. O como ya vimos podemos transformar la variable a logaritmo, seleccionar casos.

### Quitar casos

En nuestro caso podríamos quitar los gastos igual a cero. No obstante, esto resuelve un problema estadístico, pero nos supone una decisión metodológica díficil de atender.

``` r
outlierTest(modelo2)
```

    No Studentized residuals with Bonferonni p < 0.05
    Largest |rstudent|:
          rstudent unadjusted p-value Bonferonni p
    43905 3.026539          0.0024755           NA

``` r
newdata$rownames<-rownames(newdata)
newdata1<-subset(newdata, rownames!=43905)


modelo_no_out<-lm(log_gasto_seg_pc ~index_inseg + SEXO+ EDAD, data=newdata1, na.action=na.exclude)

outlierTest(modelo_no_out)
```

    No Studentized residuals with Bonferonni p < 0.05
    Largest |rstudent|:
           rstudent unadjusted p-value Bonferonni p
    46120 -2.884242           0.003926           NA

``` r
qqPlot(modelo_no_out)
```

![](Practica10_files/figure-markdown_github/unnamed-chunk-29-1.png)

    26296 46120 
    10340 17586 

### Quitar ceros

``` r
newdata2<-subset(newdata1, gasto_seg_pc!=0)
newdata2<-subset(newdata2, rownames!=c(26296,46120,10340,17586 ))

modelo_normal<-lm(log_gasto_seg_pc ~index_inseg + SEXO+ EDAD, data=newdata2, na.action=na.exclude)
hist(modelo_normal$residuals)
```

![](Practica10_files/figure-markdown_github/unnamed-chunk-30-1.png)

¿Cuando parar?

``` r
qqPlot(modelo_normal)
```

![](Practica10_files/figure-markdown_github/unnamed-chunk-31-1.png)

     7362 13730 
     2801  5304 

``` r
outlierTest(modelo_normal)
```

    No Studentized residuals with Bonferonni p < 0.05
    Largest |rstudent|:
          rstudent unadjusted p-value Bonferonni p
    13730 3.830974         0.00012788           NA

¡Este puede ser un proceso infinito! Si quitamos lo anormal, esto mueve nuestros rangos y al quitar un outlier, otra variable que antes no era outlier en el ajuste se puede convertir en outlier.

La regresión robusta, es esto, robusta a los outliers, porque pesa el valor de las observaciones de tal manera que los outliers tenga menor influencia.

Por ello es recomendable mejor utilizar otro tipo de modelos más robustos a la presencia de outliers(regresión robusta) y menos dependientes de una distribución normal(regresión mediana).
