## EJ 1: Función de hash
Tenemos un sistema que no almacena las contraseñas de los usuarios, sino el resultado de una función hash aplicada a la contraseña.  
Supongamos hash de 64 bits y resultados bien distribuidos.  

### a)
El sistema debe aplicar la función hash a la contraseña ingresada por el usuario y comparar el resultado con el valor almacenado.

### b)
Hay $2^{64}$ posibles resultados de la función hash.  
La probabilidad de acertar a uno de ellos es $\frac{1}{2^{64}}$.

### c)
Para tener un $50$% de probabilidad de acertar al valor de hash, deberiamos probar $\frac{2^{64}}{2} = 2^{63}$ hashes.  
- En un segundo podemos probar: $1.000.000.000 = 10^{9}$ hashes.
- En un minuto: $60*10^{9}$
- En una hora: $60*60*10^{9} = 3.6*10^{12}$
- En un día: $24*3.6*10^{12} = 8.64*10^{13}$
- En un año: $365*8.64*10^{13} = 3.1536*10^{16}$

Necesitariamos entonces:
$$\frac{2^{63}}{3.1536*10^{16}} \approx 292 \text{ años}$$

### d)
Contraseña de:
- A lo sumo $6$ caracteres.
- Letras minusculas o digitos.

Hay $26$ letras minusculas y $10$ digitos, un total de $36$ caracteres posibles.  
Hay $36^6 = 2.18 *10^{9}$ combinaciones posibles.  

Podiamos probar $10^{9}$ contraseñas por segundo, por lo que nos llevaria unicamente $$\frac{2.18 *10^{9}}{10^{9}} = 2.18 \text{ segundos}$$ averiguar la contraseña.