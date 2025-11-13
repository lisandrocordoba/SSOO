# EJ 4: Tecla con Interrupciones

```c
sem tecla;
bool esperando;

// Si no habia nadie esperando en read, ignoramos la interrupción
void handler(){
    if(esperando = true){
        esperando = false;
        tecla.signal();
    }
}

int driver_init(){
    tecla.init(0);
    esperando = false;
    request_irq(7, &handler);
    return 0;
}

int destroy(){
    tecla.destroy();
    free_irq(7);
    return 0;
}


int read(char* buffer){
    esperando = true;
    tecla.wait();
    // llegó la interrupción, presionaron la tecla
    OUT(BTN_STATUS, BTN_INT)    // la reiniciamos

    // Retornamos la cte
    int res = BTN_PRESSED;
    copy_to_user(buffer, &res, sizeof(res));
    return 1;
}

```