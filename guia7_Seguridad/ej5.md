## EJ 5: Buffer overflow

```c
bool check_pass(const char* user){
    char pass[128], realpass[128], salt[2];

    //Carga la contraseña(encriptada) desde /etc/shadow
    load_pass_from(realpass, sizeof(realpass), salt, user, "/etc/shadow");

    //Pregunta al usuario por la contraseña.
    printf("Password:");
    gets(pass);

    //Demora de un segundo para evitar abuso de fuerza bruta
    sleep(1);

    //Encripta la contraseña y compara con la almacenada
    return strcmp(crypt(pass, salt), realpass) == 0;
}
```

### a)
`/etc/shadow` esta protegido para los usuarios sin privilegio, pues almacena la contraseña encriptada de los usuarios junto al `salt` usado para encriptarla.  
Para que un usuario mormal pueda acceder, el programa debe hacer uso de setuid para correr con privilegios de root.  
Esta funcion no modifica el uid del proceso, pero si modifica el euid (efective user id) otorgandole permisos de root para leer `/etc/shadow`.  
Luego de que termine el login, el proceso vuelve a tener los permisos del usuario normal (uid).


### b) 
Este codigo es vulnerable por buffer overflow en la funcion `gets()`. No verifica el tamaño de la entrada del usuario, por lo que si el usuario ingresa mas de 128 caracteres, se sobrescribiran realpass y salt, pudiendo saltearse la autenticacion.


### c)
Sigue siendo un problema de seguridad, pues por mas que el usuario tenga una contraseña valida para autenticarse, puede sobreescribir todo lo que desee del stack.