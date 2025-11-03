## EJ 3: Buffer overflow
```c
void saludo(void) {
    char nombre[80];
    printf("Ingrese su nombre: ");
    gets(nombre);
    printf("Hola %s!\n", nombre);
}
```

### a)
La funcion $gets()$ No verifica el tamaño de la entrada del usuario. Si el usuario ingresa una cadena de caracteres que excede los 80 caracteres asignados al arreglo `nombre`, se producirá un buffer overflow, sobrescribiendo cosas del stack.

### b)
En la pila se encuentran las variables locales, los parámetros de la función y la dirección de retorno.
El usuario puede sobrescribir la dirección de retorno y hacer que el programa salte a una ubicación arbitraria en la memoria, lo que puede llevar a la ejecución de código malicioso.

### c)
Printf() y gets() no reservan memoria local para un buffer, simplemente reciben como argumento un puntero a una dirección de memoria. Por esto, saludo() es la unica que puede tener buffer overflow y modiicar la direccion de retorno.

### d)
El segundo printf no tiene nada que ver con la vulnerabilidad, para ese entonces el buffer overflow ya ocurrió con el gets(). 