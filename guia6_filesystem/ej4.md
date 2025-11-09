# EJERCICIO 4
Filesystem ext2 con bloques de $4$ KB

## a) Archivo de 40 KB
En ext2 contamos con:
- $12$ entradas directas
- $1$ entrada indirecta simple
- $1$ entrada indirecta doble
- $1$ entrada indirecta triple

El archivo necesita:

$\frac{40 \text{ KB}}{4 \text{ KB}} = 10$ bloques.

Alcanza con las primeras $10$ entradas directas, por lo que se necesitan $0$ bloques adicionales para direccionamiento.

Total bloques: $10$ bloques de datos.

## b) Archivo de 80 KB

El archivo necesita:

$\frac{80 \text{ KB}}{4 \text{ KB}} = 20$ bloques.

### Entradas directas
Utiliza las primeras 10 entradas directas, direccionando $12$ bloques de datos.  
Quedan $8$ bloques por direccionar.

### Entrada indirecta simple
Deberemos leer el bloque de indireccion simple y luego los $8$ bloques de datos que faltan.

### Total
$21$ bloques leidos.
