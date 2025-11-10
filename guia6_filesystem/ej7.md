# EJ 7
### 1) Posibilidad de crear enlaces simbolicos
La compañia deberia utilizar un enfoque con ``inodos``, ya que permite tener dirEntrys que apunten a inodos de tipo link simbolico.  

``FAT`` no soporta enlaces simbolicos.

### 2) La cantidad de sectores utilizados para guardar estructuras auxiliares debe ser acotada, sin importar el tamaño del disco.

Deben utilizarse ``inodos``, ya que tienen un tamaño fijo y no dependen del tamaño del disco.

La tabla ``FAT`` puede ser extremadamente grande a medida que crece el tamaño del disco, ya que debe tener una entrada por cada bloque del disco.  


### 3) El tamaño maximo de archivo solo debe estar limitado por el tamaño del disco.

En `FAT` el tamaño maximo de un archivo depende del tamaño de la tabla FAT, es decir del tamaño del disco.

En cambio, en `inodos` esto no ocurre, ya que el tamaño de un archivo está limitado la cantidad de bloques direccionables con 1 ``inodo``. Es decir la suma de la cantidad direccionable con las entradas directas, indirectas, doblemente indirectas, etc.

### 4) La cantidad de memoria principal ocupada por estructuras del filesystem en un instante dado debe ser a lo sumo lineal en la cantidad de archivos abiertos en ese momento.

Debe utilzarse ``inodos``, ya que solo se cargan en memoria los inodos de los archivos abiertos.    

En cambio, en ``FAT`` es necesario mantener en memoria toda la tabla FAT para poder seguir los bloques de un archivo.