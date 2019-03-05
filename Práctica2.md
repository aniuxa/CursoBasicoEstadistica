Práctica 2 - Más importación y manipulación de datos
================
AE
11/1/2019

Importando más datos
====================

<i>¡Recuerda establecer tu directorio!</i>

``` r
setwd("/Users/anaescoto/Dropbox/SFP")
```

Hay muchos formatos de almacenamiento de bases de datos. Vamos a aprender a importar información desde ellos

Desde Excel
-----------

El paquete más compatible con RStudio es readxl. A veces, otros paquetes tienen más problemas de configuración entre R y el Java.

``` r
install.packages("readxl", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//Rtmpb8yoUM/downloaded_packages

``` r
library(readxl) # Recuerda que hay llamar al paquete
```

Vamos a trabajar con el "Índice de Competitividad Estatal" (mayor info <a href="http://imco.org.mx/indices/un-puente-entre-dos-mexicos/">aquí</a>).

``` r
Excel_2014_IMCO <- read_excel("Excel_2014_IMCO.xlsx")
```

    ## New names:
    ## * `` -> `..103`
    ## * `` -> `..104`
    ## * `` -> `..105`

``` r
#View(Excel_2014_IMCO)
```

Como el nombre de paquete lo indica, sólo lee. Para escribir en este formato, recomiendo el paquete "openxlsx".

``` r
install.packages("openxlsx", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//Rtmpb8yoUM/downloaded_packages

``` r
library(openxlsx)
```

Si quisiéramos exportar un objeto a Excel

``` r
openxlsx::write.xlsx(Excel_2014_IMCO, file = "Mi_Exportación.xlsx")
```

Desde STATA y SPSS
------------------

Si bien también se puede realizar desde el paquete foreign. Pero este no importa algunas característica como las etiquetas y tampoco funciona con las versiones más nuevas de STATA. Vamos a instalar otro paquete, compatible con el mundo tidyverse.

``` r
install.packages("haven", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//Rtmpb8yoUM/downloaded_packages

``` r
library(haven) #¡Recuerda llamar el paquete!
```

Recuerda que este proceso no hay que hacerlo siempre. Si no que sólo la primera vez. Una vez instalado un paquete, lo llamamos con el comando "library"

Importemos una base ejemplo de stata, que podemos descargar <a href="https://www.dropbox.com/s/qov2urlyai3xbhn/2014_employment.dta?dl=0 ">aquí</a>

``` r
X2014_employment <- read_dta("2014_employment.dta")
```

Checa que podemos hacer las versiones compatibles. Se debe señalar que porque en STATA los missings se escriben con un punto, suele uno tener que cambiar el formato.

``` r
class(X2014_employment$id_trabajo)
```

    ## [1] "character"

Esto se puede solucionar, usando los comandos "as." Podemos cambiar fácilmente las propiedades de las variables

``` r
X2014_employment$id_trabajo_bis<-as.numeric(X2014_employment$id_trabajo)
```

!Importante, a R no le gustan los objetos con nombres que empiezan en números

El paquete haven sí exporta información.

``` r
write_dta(X2014_employment , "mi_exportación.dta", version = 14)
```

Con SSPS es muy parecido. Dentro de "haven" hay una función específica para ello. Importemos una base ejemplo de SPSS, que podemos descargar <a href="https://www.dropbox.com/s/8g23zgb3n2am3fm/lca_2014.sav?dl=0 ">aquí</a>

``` r
lca_2014 <- read_sav("lca_2014.sav")
```

Para escribir

``` r
write_sav(lca_2014 , "mi_exportacion.sav")
```

Checa que en todas las exportaciones en los nombres hay que incluir la extensión del programa. Si quieres guardar en un lugar diferente al directorio del trabajo, hay que escribir toda la ruta dentro de la computadora.
