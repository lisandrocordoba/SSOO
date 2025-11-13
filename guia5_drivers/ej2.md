# Ej 2: Cronometro
Registros:
- ``CHRONO_CURRENT_TIME``: permite leer el tiempo medido
- ``CHRONO_CTRL``: la constante ``CHRONO_RESET`` permite ordenar al dispositivo que reinicie el contador


Interfaz:
- Read para leer el valor actual del cronometro
- Write para reiniciar el cronometro


```c
sem mutex;

int init(){
    mutex.init(1);
    return 0;
}

int destroy(){
    mutex.destroy()
}

int read(char* buffer){
    mute.wait()
    int curr_time = IN(CHRONO_CURRENT_TIME);
    copy_to_user(buffer, &curr_time, sizeof(curr_time));
    mutex.signal()
    return sizeof(curr_time);
}

int write(char* buffer){
    // No me importa lo que haya escribido el usuario
    mutex.wait()
    OUT(CHRONO_CTRL, CHRONO_RESET);
    mutex.signal()
    return 0;
}
```