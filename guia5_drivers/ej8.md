# Ej 8: Impresora

Para comenzar se debe:
- Ingresar en el registro de 32 bits ``LOC_TEXT_POINTER`` la dirección de memoria dónde empieza el buffer que contiene el string a imprimir.
- Ingresar en el registro de 32 bits ``LOC_TEXT_SIZE`` la cantidad de caracteres que se deben leer del buffer.
- Colocar la constante START en el registro ``LOC_CTRL``.

- Si la impresora no tiene tinta, inmediatamente escribe:  
    - LOW_INK en  ``LOC_CTRL``
    - READY en ``LOC_STATUS``
- Si tiene tinta, escribe:  
    - PRINTING en ``LOC_CTRL``
    - BUSY en ``LOC_STATUS``

- Al terminar, escribe:
    - FINISHED en ``LOC_CTRL``
    - READY en ``LOC_STATUS``

La impresora puede dar falsos negativos al chequear si tiene tinta. Alcanza con probar 5 veces seguidas para confirmar si tiene o no tinta.  

Interrupciones:
- ``HP_LOW_INK_INT`` se lanza cuando la impresora detecta que se está quedando sin tinta.
- ``HP_FINISHED_INT`` se lanza cuando la impresora termina de imprimir.

## Solucion
TO DO