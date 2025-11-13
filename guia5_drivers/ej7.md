# Ej 7: Controladora discos opticos

3 registros de escritura:
- ``DOR_IO``: enciende (escribiendo 1) o apaga (escribiendo 0) el motor de la unidad.
- ``ARM``: número de pista a seleccionar.
- ``SEEK_SECTOR``: número de sector a seleccionar dentro de la pista.

3 registros de lectura:
- ``DOR_STATUS``: contiene el valor 0 si el motor está apagado (o en proceso de apagarse), 1 si está encendido. Un valor 1 en este registro no garantiza que la velocidad rotacional del motor sea la suficiente como para realizar exitosamente una operación en el disco.
- ``ARM_STATUS``: contiene el valor 0 si el brazo se está moviendo, 1 si se ubica en la pista indicada en el registro ARM.
- ``DATA_READY``: contiene el valor 1 cuando el dato ya fue enviado.

3 funciones auxiliares:
- ``int cantidad_sectores_por_pista()``: Devuelve la cantidad de sectores por cada pista del disco. El sector 0 es el primer sector de la pista.
- ``void escribir_datos(void *src)``: Escribe los datos apuntados por src en el último sector seleccionado.
- ``void sleep(int ms)``: Espera durante ms milisegundos.

### Pseudocodigo
```
write:
    if(no está prendido el motor):
        prenderlo y esperar 50 ms
    
    hacer lo que deba hacer

    apagar motor, bloquear nuevas operaciones durante 200 ms
```

### a)
```c
enum {APAGAR, PRENDER};
enum {BRAZO_MOVIENDO, BRAZO_LISTO};
enum {ESCRIBIENDO, TERMINADO};
sem mutex;

int init(){
    mutex.init(1);
    return 0;
}

int write(sector, void* data, size_t len){
    char* buffer = kmalloc(len);
    copy_from_user(buffer, data, len);

    // Calculo la pista y el sector dentro de la pista  
    int nro_pista = sector / cantidad_sectores_por_pista(); 
    int nro_sector = sector % cantidad_sectores_por_pista();
    
    mutex.wait();
    // Prender el motor si hace falta
    int prendido = IN(DOR_STATUS);
    if(!prendido) {
        OUT(DOR_IO, PRENDER);
        sleep(50);
    }

    // Hacer que el la controladora apunte a la pista y el sector correcto
    OUT(ARM, nro_pista);
    while(IN(ARM_STATUS) != BRAZO_LISTO){}    // Busy waiting hasta que el brazo se mueva
    OUT(SEEK_SECTOR, nro_sector);

    // Escribir, apagar y esperar 200 ms sin nuevas operaciones 
    escribir_datos(buffer, len);
    while(IN(DATA_READY) != TERMINADO){}    // Busy waiting hasta que termine de escribir
    kfree(buffer);
    OUT(DOR_IO, APAGAR);
    sleep(200);
    mutex.signal()
    
    return len;
}
```

### b)
```c
enum {APAGAR, PRENDER};
enum {BRAZO_MOVIENDO, BRAZO_LISTO};
enum {ESCRIBIENDO, TERMINADO};
int cronometro_prendiendo;
int cronometro_apagando;
bool esperando_prender;
bool esperando_apagar;
sem mutex;
sem prendiendo;
sem apagando;
sem listo;

void handler_50ms(){
    cronometro_prendiendo +=50;
    cronometro_apagando += 50;
    if (esperando_prender && cronometro_prendiendo >= 50){
        esperando_prender = false;
        prendiendo.signal();
    }
    if(esperando_apagar && cronometro_apagando >= 200){
        esperando_apagar = false;
        apagando.signal();
    }
    
    return 0;
} 

void handler_listo(){
    listo.signal();
    return 0;
}

int init(){
    mutex.init(1);
    prendiendo.init(0);
    apagando.init(0);
    listo.init(0);
    request_irq(6, &handler_listo);
    request_irq(7, &handler_50ms);
    esperando_prender = false;
    esperando_apagar = false;
    cronometro_prendiendo = 0;
    cronometro_apagando = 0;
    return 0;
}

int destroy(){
    mutex.destroy();
    prendiendo.destroy();
    apagando.destroy();
    listo.destroy();
    free_irq(6);
    free_irq(7);
    return 0;
}

int write(sector, void* data, size_t len){
    char* buffer = kmalloc(len);
    copy_from_user(buffer, data, len);

    // Calculo la pista y el sector dentro de la pista  
    int nro_pista = sector / cantidad_sectores_por_pista(); 
    int nro_sector = sector % cantidad_sectores_por_pista();
    
    mutex.wait();
    // Prender el motor si hace falta
    int prendido = IN(DOR_STATUS);
    if(!prendido) {
        OUT(DOR_IO, PRENDER);
        cronometro_prendiendo = 0;
        esperando_prender = true;
        prendiendo.wait()
    }

    // Hacer que el la controladora apunte a la pista y el sector correcto
    OUT(ARM, nro_pista);
    listo.wait()
    OUT(SEEK_SECTOR, nro_sector);

    // Escribir, apagar y esperar 200 ms sin nuevas operaciones 
    escribir_datos(buffer, len);
    listo.wait();
    kfree(buffer);
    OUT(DOR_IO, APAGAR);
    cronometro_apagando = 0;
    esperando_apagar = true;
    apagando.wait();
    mutex.signal()
    
    return len;
}

```

