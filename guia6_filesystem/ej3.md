# Ejercicio 3
Tenemos un filesystem parecido a ext2:
- Bloques de $4$ KB
- Direccionamiento a bloques de disco (LBA) de $8$ B
- Entradas del inodo:
    - $5$ directas
    - $2$ indirectas
    - $1$ doblemente indirecta

## a) Tamaño maximo de archivo que soporta
Separemos en los tipos de entradas:
#### Directa
Hay $5$ y cada uno direcciona a un bloque de datos de $4$ KB.  
En total direccionan $20$ KB.

### Indirecta
Direcciona a un bloque de LBAs. Cada una es de $8$ B y el bloque es de $4$ KB, por lo que en un bloque de direcciones entran: 

$\frac{4 \text{ KB}}{8 \text{ B}} = 512 \text{ LBAs}$. 

Se direccionan $512$ bloques de datos, cada uno de $4$ KB, por lo que en total una entrada indirecta direcciona:  

$512 * 4 \text{ KB} = 2048 \text{ KB} = 2 \text{ MB}$. 

Hay $2$ entradas indirectas, por lo que en total direccionan:

$2 * 2 \text{ MB} = 4 \text{ MB}$.

### Indirecta doble
Direcciona a un bloque de LBAs, pero cada una de esas direcciona a un indirecto simple.  
Sabemos que en un bloque de LBAs entran $512$ LBAs y que cada bloque indirecto simple direcciona $2$ MB.  
Por lo que en total la entrada indirecta doble direcciona:

$512 * 2 \text{ MB} = 1024 \text{ MB} = 1 \text{ GB}$.

### Total
Sumando todo lo direccionado por las entradas del inodo, tenemos que el tamaño maximo de archivo es:

$1 \text{ GB} + 4 \text{ MB} + 20 \text{ KB} = 1.004.020$ KB

## b) 
Como los bloques son de $4$ KB, los archivos que ocupen $2$ KB desperdician la mitad del bloque. La mitad de los archivos pesan $2$ KB, por lo que el 25% del espacio total se desperdicia.  
Por otro lado, los archivos que ocupan $4$ KB y $8$ KB no desperdician espacio pues utilizan bloques enteros.

## c)
Para procesar un archivo de $5$ MB, necesitamos:  

$\frac{5 \text{ MB}}{4 \text{ KB}} = 1280$ bloques.

### Entradas directas
Usamos las $5$ entradas directas, que nos permiten direccionar $5$ bloques de datos.  
Quedan $1275$ bloques por direccionar.

### Entradas indirectas
Usamos las dos entradas indirectas, que nos permiten direccionar $2 * 512 = 1024$ bloques de datos.  
Quedan $251$ bloques por direccionar.
Ademas, tendremos que traer los dos bloques de LBAs correspondientes a las entradas indirectas.

### Entrada indirecta doble
Usamos la entrada indirecta doble, unicamente con la primer LBA (correspondiente a la primer entrada indirecta simple) alcanza para direccionar los $251$ bloques de datos.
Ademas, tendremos que traer el bloque de LBAs correspondiente a la entrada indirecta doble y el bloque de LBAs correspondiente a la primer entrada indirecta simple.

### Total de bloques leidos
Para procesar el archivo de $5$ MB, necesitamos leer:
- $1280$ bloques de datos
- $2$ bloques de LBAs de las entradas indirectas
- $1$ bloque de LBAs de la entrada indirecta doble
- $1$ bloque de LBAs de la primer entrada indirecta simple dentro de la indirecta doble

En total, $1284$ bloques.