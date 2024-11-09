# DSL AWK (hawk)
Implementacion de AWK, en haskell. 

## Motivacion 
AWK es un DSL diseniado para procesado de de texto y extraccion de informacion.
Utilizado generalmente con archivos con formato de tabla (columnas separadas por algun caracter, particularmente tabulaciones)
No lo limita a estos casos.

El lenguaje itera sobre las lineas de un archivo, estas se llaman *record*, 
aplicando una sentencia dependiendo de si la linea cumple un patron.

El trabajo utilizara como referencia el libro *The AWK Programming Language* por Kernighan, Aho y Weineberg.

## Descripcion
El DSL podra asignar variables (strings, numeros o arreglos), tener control de flujo, funciones especiales 
(print, length, index, split) para strings, operadores ternarios, funciones en general. 
Debera poder recibir varios archivos, concatenarlos, y trabajar con el script.
Poseera variables predefinadas, algunas se setearan con cada linea definida o archivo tomado
    - FNR : Numero total de records en el archivo (file, number record)
    - NR : Numero total de records iterados.
    - NF : Numero de archivo.
    - FILENAME: nombre del archivo.
Y otras serviran de referencia (se pueden volver a setear).
    - FS : caracter separador, en la entrada.
    - OFS : caracter separador, en la salida.
    - RS : separador de records en entrada. Comunmente es '\\n'.
    - ORS : separador de records en salida. 

Poseera como operador de secuenciacion el punto y coma (;).

### Sintaxis
La sintaxis, al igual que las implementaciones de awk, tendran el siguiente
patron.

pattern { action }

Action se ejecutara en aquellas lineas que cumplan el patron durante
la ejecucion del programa.

Un patron puede tener las siguientes formas :
    - BEGIN { statement }, se ejecuta antes de leer el input
    - END { statement }, se ejecuta luego de leer el input
    - expression { statement }
    donde expression puede ser:
        - /regular expression/
        - compound expressions. Estos es, clausulas con &&(AND), || (OR), not (NOT)
        - pattern1, pattern2, ..., patternN

Una accion puede tener las siguientes formas :
    - expression (constantes, variables, asignaciones, llamadas a funciones)
    - print *expression list*
    - if (expression) statement
    - if (expression) statement else statement
    - while (expression) statement
    - for (expression; expression; expression) statement
    - next (skip)
    - exit
    - { statement }

## Ejemplos del lenguaje

### Ejemplo sencillo

Empleado        Paga/Hr        Horas Trabajadas

Beth            4                   0
Dan             3.00                0
Mark            5.0                 20
Kathy           4.50                10
Mary            5.0                 22
Susie           4.0                 18

Queremos saber quien trabajo mas de cero horas y ver que paga le corresponde a cada uno.

\$3 > 0 \{ print \$1, \$2 * \$3 \}

Mark    100.0
Kathy   45.0      
Mary    110.0
Susie   72.0

### Condicionales
Promedio de empleados que cobran mas de 6 dolares la hora

$2 > 6 \{ n = n + 1; pay = pay + $2 * $3 \}
END \{ 
    if(n > 0) \{
        print n, "Paga total:", pay, "Paga promedio:", pay / n
    \} else \{ 
        print "No existen empleados que ganen mas de U$D6/h"
    \}
\}

### Funciones
Para el lenguaje, una string siempre "es mayor" que un numero.
\{ print max($1, max($2, $3) \}

function max(m, n)\{
    return m > n ? m : n
\}
