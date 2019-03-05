Práctica 1. Introducción
================
AE
11/01/2018

Primer acercamiento al uso del programa
=======================================

En RStudio podemos tener varias ventanas que nos permiten tener más control de nuestro "ambiente", el historial, los "scripts" o códigos que escribimos y por supuesto, tenemos nuestra consola, que también tiene el símbolo "&gt;" con R. Podemos pedir operaciones básicas

``` r
2+5
```

    ## [1] 7

``` r
5*3
```

    ## [1] 15

``` r
#Para escribir comentarios y que no los lea como operaciones ponemos el símbolo de gato
# Lo podemos hacer para un comentario en una línea o la par de una instrucción
1:5               # Secuencia 1-5
```

    ## [1] 1 2 3 4 5

``` r
seq(1, 10, 0.5)   # Secuencia con incrementos diferentes a 1
```

    ##  [1]  1.0  1.5  2.0  2.5  3.0  3.5  4.0  4.5  5.0  5.5  6.0  6.5  7.0  7.5
    ## [15]  8.0  8.5  9.0  9.5 10.0

``` r
c('a','b','c')  # Vector con caracteres
```

    ## [1] "a" "b" "c"

``` r
1:7             # Entero
```

    ## [1] 1 2 3 4 5 6 7

``` r
40<80           # Valor logico
```

    ## [1] TRUE

``` r
2+2 == 5        # Valor logico
```

    ## [1] FALSE

``` r
T == TRUE       # T expresion corta de verdadero
```

    ## [1] TRUE

R es un lenguaje de programación por objetos. Por lo cual vamos a tener objetos a los que se les asigna su contenido. Si usamos una flechita "&lt;-" o "-&gt;" le estamos asignando algo al objeto que apunta la felcha.

``` r
x <- 24         # Asignacion de valor 24 a la variable x para su uso posterior (OBJETO)
x/2             # Uso posterior de variable u objeto x
```

    ## [1] 12

``` r
x               # Imprime en pantalla el valor de la variable u objeto
```

    ## [1] 24

``` r
x <- TRUE       # Asigna el valor logico TRUE a la variable x OJO: x toma el ultimo valor que se le asigna
x
```

    ## [1] TRUE

Vectores
--------

Los vectores son uno de los objetos más usados en R.

``` r
y <- c(2,4,6)     # Vector numerico
y <- c('Primaria', 'Secundaria') # Vector caracteres
```

Dado que poseen elementos, podemos también observar y hacer operaciones con sus elementos, usando "\[ \]" para acceder a ellos

``` r
y[2]              # Acceder al segundo valor del vector y
```

    ## [1] "Secundaria"

``` r
y[3] <- 'Preparatoria y más' # Asigna valor a la tercera componente del vector
sex <-1:2         # Asigna a la variable sex los valores 1 y 2
names(sex) <- c("Femenino", "Masculino") # Asigna nombres al vector de elementos sexo
sex[2]            # Segundo elemento del vector sex
```

    ## Masculino 
    ##         2

Matrices
========

Las matrices son muy importantes, porque nos permiten hacer operaciones y casi todas nuestras bases de datos tendran un aspecto de matriz.

``` r
m <- matrix (nrow=2, ncol=3, 1:6, byrow = TRUE) # Matrices Ejemplo 1
m
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    2    3
    ## [2,]    4    5    6

``` r
m <- matrix (nrow=2, ncol=3, 1:6, byrow = FALSE) # Matrices Ejemplo 1
m
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    3    5
    ## [2,]    2    4    6

``` r
dim(m)
```

    ## [1] 2 3

``` r
attributes(m)
```

    ## $dim
    ## [1] 2 3

``` r
n <- 1:6     # Matrices Ejemplo 2
dim(n) <- c(2,3)
n
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    3    5
    ## [2,]    2    4    6

``` r
xx <-10:12   # Matrices Ejemplo 3
yy<-14:16
cbind(xx,yy) # Une vectores por Columnas
```

    ##      xx yy
    ## [1,] 10 14
    ## [2,] 11 15
    ## [3,] 12 16

``` r
rbind(xx,yy) # Une vectores por Renglones
```

    ##    [,1] [,2] [,3]
    ## xx   10   11   12
    ## yy   14   15   16

``` r
mi_matrix<-cbind(xx,yy) # este resultado lo puedo asignar a un objeto
```

Funciones
---------

Algunas funciones básicas son las siguientes. Vamos a ir viendo más funciones, pero para entender cómo funcionan, haremos unos ejemplos y cómo pedir ayuda sobre ellas.

``` r
sum (10,20,30)    # Función suma
```

    ## [1] 60

``` r
rep('R', times=3) # Repite la letra R el numero de veces que se indica
```

    ## [1] "R" "R" "R"

``` r
sqrt(9)           # Raiz cuadrada de 9
```

    ## [1] 3

Ayuda
-----

Pedir ayuda es indispensable para aprender a escribir nuestros códigos. A prueba y error, es el mejor sistema para aprender. Podemos usar la función help, example y ?

``` r
help(sum)         # Ayuda sobre función sum
example(sum)      # Ejemplo de función sum
```

    ## 
    ## sum> ## Pass a vector to sum, and it will add the elements together.
    ## sum> sum(1:5)
    ## [1] 15
    ## 
    ## sum> ## Pass several numbers to sum, and it also adds the elements.
    ## sum> sum(1, 2, 3, 4, 5)
    ## [1] 15
    ## 
    ## sum> ## In fact, you can pass vectors into several arguments, and everything gets added.
    ## sum> sum(1:2, 3:5)
    ## [1] 15
    ## 
    ## sum> ## If there are missing values, the sum is unknown, i.e., also missing, ....
    ## sum> sum(1:5, NA)
    ## [1] NA
    ## 
    ## sum> ## ... unless  we exclude missing values explicitly:
    ## sum> sum(1:5, NA, na.rm = TRUE)
    ## [1] 15

Mi ambiente
-----------

Todos los objetos que hemos declarado hasta ahora son parte de nuestro "ambiente" (enviroment). Para saber qué está en nuestro ambiente usamos el comando

``` r
ls()
```

    ## [1] "m"         "mi_matrix" "n"         "sex"       "x"         "xx"       
    ## [7] "y"         "yy"

``` r
gc()           # Garbage collection, reporta memoria en uso
```

    ##          used (Mb) gc trigger (Mb) limit (Mb) max used (Mb)
    ## Ncells 481743 25.8    1061157 56.7         NA   630502 33.7
    ## Vcells 940472  7.2    8388608 64.0      16384  1768006 13.5

Para borrar todos nuestros objetos, usamos el siguiente comando, que equivale a usar la escobita de la venta de environment

``` r
rm(list=ls())  # Borrar objetos actuales
```

Directorio de trabajo
---------------------

Es muy útil saber dónde estamos trabajando y donde queremos trabajar. Por eso podemos utilizar los siguientes comandos para saberlo

Ojo, checa, si estás desdes una PC, cómo cambian las "" por "/" o por "\\"

``` r
getwd()           # Directorio actual
```

    ## [1] "/Users/anaescoto/Dropbox/SFP"

``` r
setwd("/Users/anaescoto/Dropbox/SFP")# Cambio de directorio

list.files()      # Lista de archivos en ese directorio
```

    ##   [1] "2014_employment.dta"                                         
    ##   [2] "Administrativo"                                              
    ##   [3] "Bases de datos"                                              
    ##   [4] "cropped-patkc3b3_workers_19301.jpg"                          
    ##   [5] "Curso"                                                       
    ##   [6] "Día 3.pptx"                                                  
    ##   [7] "Día 4.pptx"                                                  
    ##   [8] "Día 5.pptx"                                                  
    ##   [9] "Día 6.pptx"                                                  
    ##  [10] "Día 7.ppt"                                                   
    ##  [11] "Día 7.pptx"                                                  
    ##  [12] "Día 8.ppt"                                                   
    ##  [13] "Día1.pptx"                                                   
    ##  [14] "Día2.pptx"                                                   
    ##  [15] "dummy.R"                                                     
    ##  [16] "envipe0.png"                                                 
    ##  [17] "envipe1.png"                                                 
    ##  [18] "envipe3.png"                                                 
    ##  [19] "envipe4.png"                                                 
    ##  [20] "EnviromentD3.RData"                                          
    ##  [21] "EnvironmentP9.RData"                                         
    ##  [22] "Excel_2014_IMCO.xlsx"                                        
    ##  [23] "Excel_2016_IMCO.xlsx"                                        
    ##  [24] "fd_envipe2018.pdf"                                           
    ##  [25] "InegiR.R"                                                    
    ##  [26] "kupdf.net_estadiacutestica-aplicada-basica-david-s-moore.pdf"
    ##  [27] "lca_2014.sav"                                                
    ##  [28] "lrtest.png"                                                  
    ##  [29] "Mapas.R"                                                     
    ##  [30] "Material_completo.pdf"                                       
    ##  [31] "Material_completo.Rmd"                                       
    ##  [32] "merge.png"                                                   
    ##  [33] "mi_exportación.dta"                                          
    ##  [34] "mi_exportacion.sav"                                          
    ##  [35] "Mi_Exportación.xlsx"                                         
    ##  [36] "mi_output.csv.csv"                                           
    ##  [37] "modelo1.html"                                                
    ##  [38] "Nube de Palabras.R"                                          
    ##  [39] "P3.pptx"                                                     
    ##  [40] "paired.xlsx"                                                 
    ##  [41] "Practica 5.R"                                                
    ##  [42] "Practica 6.R"                                                
    ##  [43] "Práctica 7.R"                                                
    ##  [44] "Práctica 9.R"                                                
    ##  [45] "Práctica Día 1.Rmd"                                          
    ##  [46] "Práctica Día 2.Rmd"                                          
    ##  [47] "Práctica intermedia.Rmd"                                     
    ##  [48] "Práctica_Día_1.log"                                          
    ##  [49] "Práctica_Día_1.md"                                           
    ##  [50] "Práctica_Día_1.Rmd"                                          
    ##  [51] "Práctica_Día_1.tex"                                          
    ##  [52] "Práctica_intermedia.md"                                      
    ##  [53] "Practica10_files"                                            
    ##  [54] "Practica10.docx"                                             
    ##  [55] "Práctica10.html"                                             
    ##  [56] "Práctica10.log"                                              
    ##  [57] "Practica10.md"                                               
    ##  [58] "Práctica10.md"                                               
    ##  [59] "Practica10.pdf"                                              
    ##  [60] "Práctica10.R"                                                
    ##  [61] "Practica10.Rmd"                                              
    ##  [62] "Práctica10.Rmd"                                              
    ##  [63] "Práctica10.tex"                                              
    ##  [64] "Practica11_files"                                            
    ##  [65] "Practica11.md"                                               
    ##  [66] "Práctica11.R"                                                
    ##  [67] "Practica11.Rmd"                                              
    ##  [68] "Práctica2.md"                                                
    ##  [69] "Práctica2.Rmd"                                               
    ##  [70] "Práctica3_files"                                             
    ##  [71] "Práctica3.md"                                                
    ##  [72] "Práctica3.Rmd"                                               
    ##  [73] "Práctica4_files"                                             
    ##  [74] "Práctica4.md"                                                
    ##  [75] "Práctica4.Rmd"                                               
    ##  [76] "Práctica5 continuación.R"                                    
    ##  [77] "Práctica5_files"                                             
    ##  [78] "Práctica5.md"                                                
    ##  [79] "Práctica5.Rmd"                                               
    ##  [80] "Práctica6_files"                                             
    ##  [81] "Práctica6.md"                                                
    ##  [82] "Práctica6.Rmd"                                               
    ##  [83] "Práctica7_files"                                             
    ##  [84] "Práctica7.md"                                                
    ##  [85] "Práctica7.Rmd"                                               
    ##  [86] "Práctica8.md"                                                
    ##  [87] "Práctica8.Rmd"                                               
    ##  [88] "Practica9_files"                                             
    ##  [89] "Practica9-TUFTE_files"                                       
    ##  [90] "Practica9-TUFTE.pdf"                                         
    ##  [91] "Practica9-TUFTE.Rmd"                                         
    ##  [92] "Practica9-TUFTE.tex"                                         
    ##  [93] "Práctica9.log"                                               
    ##  [94] "Practica9.md"                                                
    ##  [95] "Práctica9.md"                                                
    ##  [96] "Practica9.pdf"                                               
    ##  [97] "Practica9.Rmd"                                               
    ##  [98] "Práctica9.Rmd"                                               
    ##  [99] "Programa.docx"                                               
    ## [100] "rsconnect"                                                   
    ## [101] "Sesión 7 asistentes.xlsx"                                    
    ## [102] "Sesión 7 asistentes.xltx"                                    
    ## [103] "svm-latex-ms.tex"                                            
    ## [104] "THogar.dbf"                                                  
    ## [105] "TMod_Vic.dbf"                                                
    ## [106] "TPer_Vic1_mod.dbf"                                           
    ## [107] "TPer_Vic1.dbf"                                               
    ## [108] "TSDem.dbf"                                                   
    ## [109] "TVivienda.dbf"

Checar que esto también se puede hacer desde el menú

Instalación de librerías
========================

Las librerías son útiles para realizar funciones especiales. La especialización de paquetes es más rápida en R que en otros programas por ser un software libre.

Vamos a instalar el paquete "foreign", como su nombre lo indica, nos permite leer elementos "extranjeros" en R. Es sumamente útil porque nos permite leer casi todos los formatos, sin necesidad de usar paquetes especializados como StatTransfer.

Para instalar las paqueterías usamos el siguiente comando "install.packages()" Checa que adentro del paréntesis va el nombre de la librería, con comillas. Si estamos trabajando en la computadora no es necesario poner la opción repos = "<http://cran.us.r-project.org>"."

Con la opción "dependencies = TRUE" R nos instalará no sólo la librería o paquete que estamos pidiendo, sino todo aquellos paquetes que necesite la librería en cuestión. Muchas veces los diseños de los paquetes implican el uso de algún otro anterior. Por lo que poner esta sentencia nos puede ahorrar errores cuando estemos usando el paquete. Piensa que esto es similar a cuando enciendes tu computadora y tu sistema operativo te pide que mantengas las actualizaciones.

Vamos a instalar dos librerías que nos permiten importar formatos.

``` r
install.packages("foreign", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//RtmplKHIKu/downloaded_packages

``` r
install.packages("haven", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//RtmplKHIKu/downloaded_packages
