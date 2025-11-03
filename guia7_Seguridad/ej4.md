## EJ 4: Buffer overflow

```c
struct credential {
    char name[32];
    char pass[32];
};

bool login(void) {
    char realpass[32];
    struct credential user;
    // Pregunta el usuario
    printf("User: ");
    fgets(user.name, sizeof(user), stdin);
    // Obtiene la contraseña real desde la base de datos y lo guarda en realpass
    db_get_pass_for_user(user.name, realpass, sizeof(realpass));
    // Pregunta la contraseña
    printf("Pass: ");
    fgets(user.pass, sizeof(user), stdin);

    return strncmp(user.pass, realpass, sizeof(realpass)-1) == 0;
    // True si user.pass == realpass
}
```

### a)

Con $fgets()$ se queire que el usuario pueda modificar user.name y user.pass.  
Sin embargo, al limitar el size de entrada con sizeof(user) en vez de sizeof(user.name) y sizeof(user.pass) se corre el riesgo de que se escriban datos fuera de los límites de los buffers.

```
---- DIRECCIONES BAJAS ----
| user.pass[0-7]      --> fgets(user.pass, 64 bytes)
| user.pass[8-15]
| user.pass[16-23]
| user.pass[24-31]
| user.name[0-7]     --> empieza fgets(user.name, 64 bytes)
| user.name[8-15]
| user.name[16-23]
| user.name[24-31]    --> Termina fgets(user.pass)
| realpass[0-7]
| realpass[8-15]
| realpass[16-23]
| realpass[24-31]     --> Termina fgets(user.name)
| rbp_viejo
| ret_addr

---- DIRECCIONES ALTAS ----
```

### b)
Un atacante podria ingresar en fgets(user.name, sizeof(user), stdin) un string de 64 bytes, donde los ultimos 32 sobreescribirian la variable realpass. Luego el atacante ingresa la misma contraseña que sobreescibió y logra autenticarse sin conocer la contraseña real.

!!!!!!!!!!!!! DUDA: realpass se va a buscar a la db LUEGO de que el usuario ingrese user.name. Real pass no sobreescribiria el intento malintencionado? 
