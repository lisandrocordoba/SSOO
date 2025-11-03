## EJ 7: Stack Randomization

## a)
```
Escribir el valor de retorno de una función utilizando un buffer overflow sobre un buffer en
stack dentro de dicha función.
```
Si bien probablemente esa direccion virtual no sea efectivamente la que el atacante queria acceder, sigue siendo una vulnerabilidad ya que permite modificar el flujo de ejecucion del programa.

## b)
```
Utilizar el control del valor de retorno de una función para saltar a código externo introducido en un buffer en stack controlado por el usuario.
```
Por las mismas razones que en el punto anterior, esta vulnerabilidad es mitigada pues no se puede garantizar que la direccion de memoria a la que se salta sea la correcta.

## c)
```
Utilizar el control del valor de retorno de una función para ejecutar una syscall particular (por ejemplo read) que fue usada en otra parte del programa.
```
Es mitigada por la misma razon, no sabemos la direccion virtual en la que estamos.