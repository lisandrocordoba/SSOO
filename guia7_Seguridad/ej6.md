## EJ 6: Floats y NaN

```c
#define NEGATIVO 1
#define CERO 2
#define POSITIVO 3
int signo(float f){
    if(f < 0.0)return NEGATIVO;
    if(f == 0.0)return CERO;
    if (f > 0.0) return POSITIVO;
    assert(false && "Por aca no paso nunca");
    return 0; // Si no pongo esto el compilador se queja =(
}
```

### a)
En el estandar $IEEE-754$ se incluye el valor especial `NaN` (Not a Number) para representar resultados indefinidos o no representables en operaciones de punto flotante.

Si el usuario pasa f = NaN, se ejecutaria el assert.

### b)
Por como es la funcion, sacar el assert unicamente haria que la funcion retorne 0 en el caso de NaN.  
Sin embargo, traeria problemas si se intenta hacer operaciones con f en ese caso.