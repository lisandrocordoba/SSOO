# Ej 3: Tecla con Busy waiting

Registro:
- ``BTN_STATUS``
    - ``BTN_STATUS_0``: el bit menos significativo indica si la tecla fue pulsada (0 = no, 1 = sí)
    - ``BTN_STATUS_1``: escribir 0 en el segundo bit para limpiar la memoria de la tecla.

Driver con busy waiting. Debe retornar la cte BTN_PRESSED cuando se presiona la tecla.


```c
sem mutex;
int driver_init(){
    mutex.init(1);
    return 0;
}


int read(char* buffer){
    // Busy waiting mientras la tecla no sea presionada
    mutex.wait();
    while((IN(BTN_STATUS) && 0b1) == 0){}

    // Se presionó, la reiniciamos
    int reinicio = 1;
    OUT(BTN_STATUS_1, &reinicio)
    mutex.signal();

    // Retornamos la cte
    int res = BTN_PRESSED;
    copy_to_user(buffer, &res, sizeof(res));
    return 1;
}

```