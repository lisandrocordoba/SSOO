# Ej 8
- Disco rigido de $16$ GB
- Sectores de $1$ KB

Se quiere formatear el disco con un filesystem basado en ``FAT`` llamado HashFS. Consiste en lo siguiente:
- No hay directorios ni archivos. Dado un path, se calcula el ``hash`` y este indica cual es el archivo buscado.
-  Cuenta con 2 tablas:  
    - Una ``FAT`` que guarda el siguiente bloque de cada bloque del disco. Cuando el siguiente es igual al actual, indica el fin del archivo.
    - Una tabla ``HASH`` que para cada ``hash`` posible, guarda el identificador del bloque inicial y el tamaño en bytes del archivo correspondiente a ese ``hash``.
- Permite configurar los siguientes elementos:
    - Tamaño del bloque: 2, 4 u 8 sectores.
    - Tamaño de identificadores de bloque (LBA): 8, 16, 24 o 32 bits.
    - Tamaño del ``hash``: 8, 16, 24 o 32 bits.

## a)
- 2 sectores por bloque implica bloques de $2$ KB.
- LBAs de 24 bits
- Hash de 16 bits

### Tamaño tabla FAT
Tendremos
 $\frac{16 \text{ GB}}{2 \text{ KB}} = 8 \text{ MB}$ bloques en el disco, por lo que habra la misma cantidad de entradas en la tabla ``FAT``.  

Cada entrada de la tabla ``FAT`` tendra $1$ LBA de $24$ bits = $3$ bytes.  

El tamaño total de la tabla ``FAT`` sera:   
$8 \text{ MB} * 3 \text{ B} = 24 \text{ MB}$.

### Tamaño tabla HASH
Hay $2^{16} = 64 \text{ KB}$ hashes posibles, por lo que tendremos esa misma cantidad de entradas en la tabla ``HASH``.  

Cada entrada de la tabla tendrá $1$ LBA de $24$ bits = $3$ bytes y el tamaño en bytes de ese archivo, que puede ser de hasta $16$ GB, por lo que necesitaremos $5$ bytes para representarlo ya que $2^{34} = 16 \text{ GB}$.

El tamaño total de la tabla ``HASH`` sera:
$64 \text{ KB} * (3 \text{ B} + 5 \text{ B}) = 64 \text{ KB} * 8 \text{ B} = 512 \text{ KB}$.

### Espacio disponible para archivos
El espacio total del disco es de $16$ GB.  
El espacio ocupado por las tablas es de $24.5$ MB.

Por lo tanto, el espacio disponible para archivos es de:  
$16 \text{ GB} - 24.5 \text{ MB} = 15 \text{ GB} + 15.9755 \text{ MB}$.


## b)
Se quiere maximizar la cantidad de archivos soportados por el sistema y en promedio cada archivo pesa 1 KB.

Como en la tabla de ``HASH`` cada entrada apunta al primer bloque de un archivo, queremos tener la mayor cantidad de entradas posibles en la tabla ``HASH``. Deberiamos tener el hash de mayor tamaño posible, pero sin que la tabla ``HASH`` ocupe demasiado espacio.

En promedio los archivos ocupan 1 KB, por lo que usaremos el tamaño minimo de bloque, es decir ``bloques de 2 KB``. Cada archivo desperdiciara un solo bloque. Si usaramos bloques de 4 KB o 8 KB, se desperdiciaran mas bloques por archivo.

Teniendo en cuenta el tamaño de bloque elegido y que los archivos pesan en promedio $1$ KB, habrán $\frac{16 \text{ GB}}{2 \text{ KB}} = 8 \text{ MB}$ archivos en total si no tuvieramos en cuenta el espacio necesario para las tablas.

Para poder tener los $8 \text{ MB}$ de archivos, deberiamos tener tambien $8 \text{ MB}$ de entradas en la tabla ``HASH``. Por lo que necesitariamos un hash de $23$ bits, ya que $2^{23} = 8 \text{ MB}$.  
Por esto, elegimos un ``hash de 24 bits``.

Tambien necesitamos $8 \text{ MB}$ LBAs, por lo que elegimos ``LBAs de 24 bits``.

Condiciones finales:
- Tamaño del bloque: 2 sectores (2 KB)
- LBAs: 24 bits
- Hash: 24 bits

De esta forma, la tabla ``FAT`` ocupa $24$ MB y la tabla ``HASH`` ocupa $16 \text{ MB} * (3 \text{ B} + 5 \text{ B}) = 128 \text{ MB}$.

Quedando un espacio disponible para archivos de:  
$16 \text{ GB} - 152 \text{ MB} = 15 \text{ GB} + 872 \text{ MB}$.

## c)
Si el tamaño promedio de archivos es de $16 MB$, tendremos muchos menos archivos en el sistema.  
Tendremos $\frac{16 \text{ GB}}{16 \text{ MB}} = 1024$ archivos en total si no tuvieramos en cuenta el espacio necesario para las tablas.

No vale la pena tablas tan grandes para tan pocos archivos, por lo que elegimos:

``Tamaño del bloque: 8 sectores (8 KB)``, teniendo así $$\frac{16 \text{ GB}}{8 \text{ KB}} = 2 \text{ MB}$$ bloques en el disco.  


``LBAs: 24 bits``, teniendo así $2^{24} = 16 \text{ MB}$ LBAs posibles.  

``Hash: 24 bits``, teniendo así $2^{24} = 16 \text{ MB}$ hashes posibles.

Notar que si usaramos LBA y hash de 16 bits, no nos alcanzaria para todos los bloques pues $2^{16} = 64 \text{ KB}$.

Con esta configuracion:
- La tabla ``FAT`` ocupa $2 \text{ MB} * 3 \text{ B} = 6 \text{ MB}$.
- La tabla ``HASH`` ocupa $16 \text{ MB} * (3 \text{ B} + 5 \text{ B}) = 128 \text{ MB}$.

Quedando un espacio disponible para archivos de:  
$16 \text{ GB} - 134 \text{ MB} = 15 \text{ GB} + 890 \text{ MB}$.