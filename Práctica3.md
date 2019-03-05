Práctica 3- Dataframes y gráficos
================
AE
10/01/2018

Matrices vrs. Data Frames
-------------------------

Matrices
========

Anteriormente, aprendimos a instalar paquetes y a importar bases de datos. Las matrices por lo general sólo almacenan un tipo de datos mientras que las data frames puede almacenar varios tipos de datos.

``` r
m <-matrix(1:20, ncol=2)
m
```

    ##       [,1] [,2]
    ##  [1,]    1   11
    ##  [2,]    2   12
    ##  [3,]    3   13
    ##  [4,]    4   14
    ##  [5,]    5   15
    ##  [6,]    6   16
    ##  [7,]    7   17
    ##  [8,]    8   18
    ##  [9,]    9   19
    ## [10,]   10   20

En general, los datos de una matriz son numéricos. R es muy potente para hacer operaciones con matrices.

Para poder llegar a ver los elementos individuales de una matriz, tenemos que usar los corchetes y revisar por ejemplo el elemento 5 de la columna 1

``` r
m[5,1] 
```

    ## [1] 5

``` r
m[5,2]
```

    ## [1] 15

``` r
m[, 1] # si queremos todas las filas, no ponemos nada
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10

``` r
m[1, ] # si queremos todas las columnas no ponemos nada
```

    ## [1]  1 11

``` r
m[1:2,]
```

    ##      [,1] [,2]
    ## [1,]    1   11
    ## [2,]    2   12

``` r
m[c(1,5, 6),]
```

    ##      [,1] [,2]
    ## [1,]    1   11
    ## [2,]    5   15
    ## [3,]    6   16

``` r
n<-m[1,]

class(m)
```

    ## [1] "matrix"

``` r
class(n)
```

    ## [1] "integer"

Estos resultados se pueden asignar también a nuevos objetos, lo cual es muy útil cuando estemos filtrando caso.

Dataframes
==========

Vamos a instalar la librería "MASS" que nos da acceso a un grupo amplio de bases de datos que nos ayudaran a revisar y analizar algunas técnicas sencillas.

``` r
install.packages("MASS", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//RtmpsIcEcQ/downloaded_packages

``` r
library(MASS)
```

Vamos a llamar a la base "painters" que contiene información sobre pintores del siglo XVIII

``` r
data()
data(painters)
class(painters)
```

    ## [1] "data.frame"

Para revisar el contenido de un data frame podemos usar, como lo hicimos en el ejercicio de la ENVIPE, el formado basededatos$var o usar corchete, checa como estas tres formas tan el mismo resultado

``` r
painters$School
```

    ##  [1] A A A A A A A A A A B B B B B B C C C C C C D D D D D D D D D D E E E
    ## [36] E E E E F F F F G G G G G G G H H H H
    ## Levels: A B C D E F G H

``` r
painters[["School"]]  # ¡Ojo con las comillas! 
```

    ##  [1] A A A A A A A A A A B B B B B B C C C C C C D D D D D D D D D D E E E
    ## [36] E E E E F F F F G G G G G G G H H H H
    ## Levels: A B C D E F G H

``` r
painters[,5]  
```

    ##  [1] A A A A A A A A A A B B B B B B C C C C C C D D D D D D D D D D E E E
    ## [36] E E E E F F F F G G G G G G G H H H H
    ## Levels: A B C D E F G H

Vamos a pedir cosas más especificas y podemos seleccionar observaciones o files

``` r
painters[painters$School=="A",]
```

    ##                 Composition Drawing Colour Expression School
    ## Da Udine                 10       8     16          3      A
    ## Da Vinci                 15      16      4         14      A
    ## Del Piombo                8      13     16          7      A
    ## Del Sarto                12      16      9          8      A
    ## Fr. Penni                 0      15      8          0      A
    ## Guilio Romano            15      16      4         14      A
    ## Michelangelo              8      17      4          8      A
    ## Perino del Vaga          15      16      7          6      A
    ## Perugino                  4      12     10          4      A
    ## Raphael                  17      18     12         18      A

``` r
painters[(painters$School=="A" & painters$Composition>10 ),]
```

    ##                 Composition Drawing Colour Expression School
    ## Da Vinci                 15      16      4         14      A
    ## Del Sarto                12      16      9          8      A
    ## Guilio Romano            15      16      4         14      A
    ## Perino del Vaga          15      16      7          6      A
    ## Raphael                  17      18     12         18      A

``` r
subset1<- painters[(painters$School=="A" & painters$Composition>10 ),]
```

También podemos seleccionar columnas

``` r
painters[, c("Colour", "Expression")]
```

    ##                 Colour Expression
    ## Da Udine            16          3
    ## Da Vinci             4         14
    ## Del Piombo          16          7
    ## Del Sarto            9          8
    ## Fr. Penni            8          0
    ## Guilio Romano        4         14
    ## Michelangelo         4          8
    ## Perino del Vaga      7          6
    ## Perugino            10          4
    ## Raphael             12         18
    ## F. Zucarro           8          8
    ## Fr. Salviata         8          8
    ## Parmigiano           6          6
    ## Primaticcio          7         10
    ## T. Zucarro          10          9
    ## Volterra             5          8
    ## Barocci              6         10
    ## Cortona             12          6
    ## Josepin              6          2
    ## L. Jordaens          9          6
    ## Testa                0          6
    ## Vanius              12         13
    ## Bassano             17          0
    ## Bellini             14          0
    ## Giorgione           18          4
    ## Murillo             15          4
    ## Palma Giovane       14          6
    ## Palma Vecchio       16          0
    ## Pordenone           17          5
    ## Tintoretto          16          4
    ## Titian              18          6
    ## Veronese            16          3
    ## Albani              10          6
    ## Caravaggio          16          0
    ## Corregio            15         12
    ## Domenichino          9         17
    ## Guercino            10          4
    ## Lanfranco           10          5
    ## The Carraci         13         13
    ## Durer               10          8
    ## Holbein             16         13
    ## Pourbus              6          6
    ## Van Leyden           6          4
    ## Diepenbeck          14          6
    ## J. Jordaens         16          6
    ## Otho Venius         10         10
    ## Rembrandt           17         12
    ## Rubens              17         17
    ## Teniers             13          6
    ## Van Dyck            17         13
    ## Bourdon              8          4
    ## Le Brun              8         16
    ## Le Suer              4         15
    ## Poussin              6         15

``` r
subset2<- painters[, c("Colour", "Expression")]
```

podemos combinar los dos tipos de selección

``` r
painters[painters$School=="A", c("Colour", "Expression")]
```

    ##                 Colour Expression
    ## Da Udine            16          3
    ## Da Vinci             4         14
    ## Del Piombo          16          7
    ## Del Sarto            9          8
    ## Fr. Penni            8          0
    ## Guilio Romano        4         14
    ## Michelangelo         4          8
    ## Perino del Vaga      7          6
    ## Perugino            10          4
    ## Raphael             12         18

``` r
subset4<- painters[, c("Colour", "Expression")]
```

Vamos a revisar cuántos pintores estudiaron en cada escuela. Si recuerdas, eso se puede hacer con el comando table. Como estamos trabajando

``` r
table(painters$School)
```

    ## 
    ##  A  B  C  D  E  F  G  H 
    ## 10  6  6 10  7  4  7  4

Este resultado lo podemos asignar a un objeto nuevo

``` r
freq.school<-table(painters$School)
```

El tenerlo como un objeto nos sirve sobre todo porque podemos aplicarle funciones a esta tabla de frecuencias, las funciones m´ás importantes que podemos aplicar son las gráficas

``` r
plot(freq.school) # el gráfico más sencillo 
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-11-1.png)

``` r
barplot(freq.school) # gráficos de barra
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-11-2.png)

``` r
pie(freq.school) # gráfico de pastel
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-11-3.png)

Vamos a modificar ligeramente los colores

``` r
barplot(freq.school, col=gray(0:8/8)) # gráficos de barra con escala de grises de oscuro a claros, (8 categorías)
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-12-1.png)

``` r
barplot(freq.school, col=gray(8:0/8)) # gráficos de barra con escala de grises de claro a oscuros
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-12-2.png)

``` r
barplot(freq.school, col=rainbow(8)) # gráfico de barras () ponemos el número de categorías
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-12-3.png)

``` r
barplot(freq.school, col=heat.colors(8)) # gráfico de barras () ponemos el número de categorías
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-12-4.png)

También podemos hacer nuestro propio vector de colores, para la paleta de nombres de colores que usa R, revisar <http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf>

``` r
my_colors1<-c("lightblue","lightblue1", "lightblue2", "lightblue3", "lightblue4", "lightcyan4", "lightcyan3", "lightcyan2")

my_colors2<-c("mediumorchid","mediumorchid1", "mediumorchid2", "mediumorchid3", "mediumorchid4", "mediumpurple4", "mediumpurple3", "mediumpurple2")

barplot(freq.school, col=my_colors1) # gráficos de barra con colores personalizados 1
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-13-1.png)

``` r
barplot(freq.school, col=my_colors2) # gráficos de barra con colores personalizados 2
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-13-2.png)

Para poner título

``` r
barplot(freq.school, col=gray.colors(8)) 
title(main = list("Escuelas de pintores", font = 4))
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-14-1.png)

``` r
# border :
barplot(freq.school, border = "dark blue", col=my_colors2,  legend = rownames(freq.school))
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-14-2.png)

Para hacer las barras horizontales le ponemos la opción "horiz=TRUE"

``` r
barplot(freq.school, border = "dark blue", col=my_colors2,  legend = rownames(freq.school), horiz=TRUE )
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-15-1.png)

Gráficos bivariados
===================

Igual hacemos un objeto con las frecuencias de una tabla, pero en este caso será una tabla bivariada. Vamos a cargar otra base de datos. Nuestra base de pintores sólo tenía una sóla variable categórica.

``` r
data()
data("Cars93")
freq.cars<-table(Cars93$AirBags, Cars93$Type)
freq.cars
```

    ##                     
    ##                      Compact Large Midsize Small Sporty Van
    ##   Driver & Passenger       2     4       7     0      3   0
    ##   Driver only              9     7      11     5      8   3
    ##   None                     5     0       4    16      3   6

Con este objeto vamos a graficar.

Para <b>barras apiladas</b>

``` r
barplot(freq.cars, main="Carros según bolsas de aire y tipo",
                   col=heat.colors(3),
                      legend = rownames(freq.cars))
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-17-1.png)

Para <b>barras agrupadas</b> Para estos gráficos usamos el mismo código, pero le activamos la opción "beside"

``` r
barplot(freq.cars, main="Carros según bolsas de aire y tipo",
                   col=heat.colors(3),
                     legend = rownames(freq.cars),
                   beside=TRUE )
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-18-1.png)

Para las barras apiladas es útil que lo veamos en términos de proporciones.

Para eso usabmos "prop.table" la opción 1, es para que nos dé las proporciones por fila la opción 2, es para que nos dé las proporciones por columna Sin opción, da la proporción para el total de observaciones

``` r
prop.cars<-prop.table(freq.cars,2)
prop.cars
```

    ##                     
    ##                        Compact     Large   Midsize     Small    Sporty
    ##   Driver & Passenger 0.1250000 0.3636364 0.3181818 0.0000000 0.2142857
    ##   Driver only        0.5625000 0.6363636 0.5000000 0.2380952 0.5714286
    ##   None               0.3125000 0.0000000 0.1818182 0.7619048 0.2142857
    ##                     
    ##                            Van
    ##   Driver & Passenger 0.0000000
    ##   Driver only        0.3333333
    ##   None               0.6666667

``` r
barplot(prop.cars, main="Carros según bolsas de aire y tipo",
                   col=heat.colors(3),
                      legend = rownames(prop.cars))
```

![](Práctica3_files/figure-markdown_github/unnamed-chunk-20-1.png)
