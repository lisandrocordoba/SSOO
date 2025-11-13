# Ej 5

## a) open()
Al hacer open(device_id), el adminsitrador de E/S debe chequear que el dispositivo exista y que el usuario tenga permisos para accederlo.
Luego inicializa las estructuras de datos necesarias para la comunicacion y se actualiza la tabla de archivos abiertos del proceso.

## b) write()
Al hacer write(fd, buffer, size), el administrador de E/S debe verificar que el file descriptor (fd) sea válido y que el proceso tenga permisos de escritura en el dispositivo asociado.
Luego, se copian los datos desde el buffer del usuario al buffer del dispositivo mediante ``copy_from_user()`` y se inicia la operación de escritura en el dispositivo mediante `out`. Se retorna la cantidad de bytes escritos o un código de error en caso de fallo.