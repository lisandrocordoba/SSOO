# Ej 9
Supongamos que el fd de un archivo en ext2 permite saber el numero de inodo del archivo.

Si el proceso que recibe el fd del directorio `/home` quiere saber el nombre del directorio, no lo podrá encontrar en el inodo, ya que el nombre se almacena en la dirEntry del padre.

El proceso deberá:
1. Cargar el inodo referenciado por el fd, mediante la funcion ``load_inode``.
2. Guardar el numero de inodo, por ejemplo en una variable llamada `inode_num`.
3. Leer la primer dirEntry del inodo, que corresponderá al inodo del padre (en este caso `/`). Guardarlo en una variable llamada `parent_inode_num`.
4. Cargar el inodo del padre mediante la funcion ``load_inode`` con `parent_inode_num`.
5. Recorrer todas las dirEntrys del inodo del padre, leyendo los bloques correspondientes y buscando la dirEntry cuyo numero de inodo sea igual a `inode_num`.
6. Una vez encontrada, obtener el nombre de la dirEntry (será `/home`) y devolverlo. 
