# Ej 6
Ruta del archivo: ``/home/aprobar.txt``

## a) FAT
A partir del directorio raiz `/`, buscamos la dirEntry correspondiente a `home` para obtener el bloque inicial del directorio.
Los pasos son:

```
Leer el bloque correspondiente a `/` para obtener su contenido.

while(aun no encotramos home){
    Buscar en el bloque la dirEntry correspondiente a `home` para obtener el bloque inicial del directorio.

    Si no está en el bloque
        buscar el siguiente bloque del directorio `/` usando la tabla FAT y repetir .

    Si recorrimos todo `/`
        salir pues no existe `home`. 
}

Una vez encontrado `home`, obtener el bloque inicial del directorio.  
Repetir el while para buscar la dirEntry correspondiente a `aprobar.txt` dentro de `home`.

Una vez encontrado `aprobar.txt`, obtener el bloque inicial del archivo.  
Luego leer todos los bloques del archivo siguiendo la tabla FAT hasta llegar al final del archivo.
```

En total, se necesitan leer los bloques del directorio `/` hasta encontrar la dirEntry de `home`. Luego leer los bloques de `home` hasta encontrar la dirEntry de `aprobar.txt` y luego leer todos los bloques del archivo `aprobar.txt`.  

Como maximo leeremos:

$\text{Cantidad de bloques de / } + \text{Cantidad de bloques de home } + \text{ Cantidad de bloques de aprobar}$ 

= $\frac{\text{ rootSize}}{\text{blockSize}}$ + $\frac{\text{ homeSize}}{\text{blockSize}}$ + $\frac{\text{aprobarSize}}{\text{blockSize}}$


## b) ext2
Queremos leer el archivo `/pepe.txt`, que es un enlace simbolico a `/home/aprobar.txt`.

Los pasos son:

```
Buscar el inodo de `/` en la tabla de inodos.
Recorrer las dirEntrys del inodo hasta encontrar la dirEntry correspondiente a `pepe.txt` de la siguiente forma:

Para cada entrada directa del inodo (hay 12):
    Leer el bloque correspondiente a la entrada directa.
    Buscar en el bloque la dirEntry correspondiente a `pepe.txt`.

Para cada entrada indirecta simple del inodo (hay 3):
    Leer el bloque de LBAs correspondiente a la entrada indirecta simple.
    Para cada LBA en el bloque:
        Leer el bloque correspondiente a la LBA.
        Buscar en el bloque la dirEntry correspondiente a `pepe.txt`.

Para la entrada indirecta doble del inodo (hay 1):
    Leer el bloque de indirectas simples correspondiente a la entrada indirecta doble.
    Para cada indirecta simple en el bloque:
        Leer el bloque de LBAs correspondiente a la indirecta simple.
        Para cada LBA en este bloque:
            Leer el bloque de datos correspondiente a la LBA.
            Buscar en el bloque la dirEntry correspondiente a `pepe.txt`.

Para la entrada indirecta triple del inodo (hay 1):
    Leer el bloque de indirectas dobles correspondiente a la entrada indirecta triple.
    Para cada indirecta doble en el bloque:
        Leer el bloque de indirectas simples correspondiente a la indirecta doble.
        Para cada indirecta simple en este bloque:
            Leer el bloque de LBAs correspondiente a la indirecta simple.
            Para cada LBA en este bloque:
                Leer el bloque de datos correspondiente a la LBA.
                Buscar en el bloque la dirEntry correspondiente a `pepe.txt`.

Una vez encontrada la dirEntry de `pepe.txt`, obtener el inodo correspondiente al enlace simbolico.  
Al ser un hard link, el inodo contendrá la ruta `/home/aprobar.txt`.
Repetir el proceso anterior para encontrar el inodo de `/home/aprobar.txt`.
```

Si fuera un hard link, este ultimo paso no sería necesario, ya que la dirEntry de `pepe.txt` apuntaria al inodo de `aprobar.txt`.

Tambien hay que notar que la busqueda de la dirEntry en FAT es mas sencilla ya que no hay que manejar inodos y las dirEntrys tienen tamaño fijo.