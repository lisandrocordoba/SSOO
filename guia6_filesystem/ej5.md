# Ej 2
Asumiendo las mismas condiciones del ejercicio 3, tenemos:
- Cada entrada indirecta simple direcciona a $512$ bloques de datos.
- Cada entrada indirecta doble direcciona a $512$ entradas indirectas simples, por lo que direcciona a $512 * 512 = 262.144$ bloques de datos.

Tambien se sabe que tanto la FAT como el inodo del archivo correspondiente ya estan en memoria.
## I) 1, 2, 3, 5, 7, 11, 13, 17, 23
### FAT
Debemos iterar en la tabla empezando por el primero bloque del archivo, para obtener cada uno de los bloques que nos interesan del archivo.  

En total se accede a $9$ bloques.

### ext2

Los bloques ``1, 2, 3, 5, 7, 11`` están direccionados directamente, pero los ``13, 17 y 23`` debemos buscarlos en el primer bloque de indirectas simples.    

Asi, en total se accede a $6 + 1 + 3 = 10$ bloques.

## II) 1, 2, 3, 4, 5, 6, 10.001
### FAT
Igual que antes, hay que iterar por la FAT y traer cada uno de los bloques que nos interesan del archivo.

En total se accede a $7$ bloques.

### ext2
Los bloques ``1, 2, 3, 4, 5, 6`` están direccionados directamente, veamos que ocurre con el ``10.001``:  
Los indirectos simples permiten direccionar $512$ bloques y habiendo 3 de estos, podemos direccionar $3 * 512 = 1536$ bloques, que no alcanza para llegar al bloque $10.001$.  
El indirecto doble permite direccionar $512 * 512 = 262.144$ bloques, por lo que alcanza para llegar al bloque $10.001$.  
De esta manera, debemos traer el bloque de indirectas dobles, luego el bloque de indirectas simples correspondiente y finalmente el bloque de datos.  

En total se accede a $6 + 1 + 1 + 1 = 9$


## III) 13, 10.000, 1.000.000
### FAT
Igual que antes.

En total se accede a $3$ bloques.

### ext2
Veamos cada bloque:
- El bloque ``13``  
No llega a estar direccionado directamente, pues hay 12 directas.   
Traemos el primer indirecto y luego el bloque de datos.  
2 bloques.
- El bloque ``10.000``  
Ya vimos que los 3 indirectos simples no alcanzan para llegar, por lo que debemos usar el indirecto doble.  
Traemos el bloque de indirectos dobles, luego el bloque de indirectos simples correspondiente y finalmente el bloque de datos.  
3 bloques.
- El bloque ``1.000.000``  
Con la doble direccionamos $512 * 512 = 262.144$ bloques, no alcanza.  
Debemos traer el bloque de indirectos triples, luego el bloque de indirectos dobles correspondiente, luego el bloque de indirectos simples correspondiente y finalmente el bloque de datos.  
4 bloques.

En total se accede a $2 + 3 + 4 = 9$ bloques.


## 14, 15, ..., 50
### FAT
Igual que antes.

En total se accede a 37$ bloques.

### ext2
Todos los bloques buscados estan en la primer indirecta simples, por lo que debemos traer el bloque de indirectas simples y luego los bloques de datos.  

En total se accede a $1 + 37 = 38$ bloques.