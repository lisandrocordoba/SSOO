## EJ 8: Code Injection

```c
#define BUF_SIZE 1024
void wrapper_ls(const char * dir) {
    char cmd[BUF_SIZE];
    snprintf(cmd, BUF_SIZE-1, "ls %s", dir);
    system(cmd);
}
```

### a)
Si el input es `"; cat /etc/passwd"`, se ejecutara el siguiente comando:
```bash
ls; cat /etc/passwd
```
 
### b)
Este cambio no soluciona nada.
Si el input es `"; cat /etc/passwd"`, el comando ejecutado sera:
```bash
ls \"; cat /etc/passwd\"
```
El shell interpretara las comillas escapadas como parte del nombre del directorio a listar, por lo que intentara listar un directorio llamado " y luego ejecutara `cat /etc/passwd`.

### c)
Evitando el $;$ igualmente quedan varios problemas de seguridad:
- Al usar paths relativos, podemos modificar la env PATH para que ls ejecute otro binario malicioso.
- Podemos usar caracteres especiales como `&` o `|` para concatenar comandos.

### d)
Para evitar problemas de seguridad es necesario:
- Sanitizar completamente el input, evitando $;$. $|$, $&$ o $&&$ y usando paths absolutos.
