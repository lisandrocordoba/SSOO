## EJ 2: POP3
    
Tenemos un sistema remoto de autenticación de usuarios, como POP3.
No transmiten la contraseña en el procesos de autenticacion ya que puede ser interceptada.

### a)
No sería seguro transmitir el hash de la contraseña por el canal, ya que es vulnerable a `replay-attacks`.  
Un atacante que intercepte el hash podría reutilizarlo para autenticarse sin necesidad de conocer la contraseña original.

También hay que tener en cuenta la dificultad de la contraseña, si son pocos caracteres, vimos en el ejercicio anterior que se pueden hacer fuerza bruta en pocos segundos.

### b)
Supongamos que un atacante intercepta:
- La seed aleatoria enviada por el servidor.
- hash(contraseña+seed) enviada por el cliente.

El atacante podría intentar realizar un ataque de fuerza bruta utilizando la seed interceptada para generar hashes de contraseñas comunes y compararlos con el hash interceptado. Si encuentra una coincidencia, podría obtener acceso al sistema.  
Esto nuevamente es posible unicamente si la contraseña es débil.

Sin embargo, si en cada login se utiliza una seed diferente, el atacante no podría reutilizar la información interceptada en futuros intentos de autenticación, lo que mejora la seguridad contra replay attacks.