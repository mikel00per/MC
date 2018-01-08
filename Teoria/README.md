# Examen de MC 2017/2018

## Pregunta 1
Contruir el AFD que acepte palabras con 1's y 0's con:

  - a) Un número par de 0's.
  - b) Expresión regular con número para de 0's y 1's.
  - c) AFND para cadenas del apartado b) y poner algún ejemplo de uso.

    Sin hacer aún.

* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

## Pregunta 2 | Lema del bombeo
Demuestra que el siguiente lenguaje no es regular: {0^j 1^i / j>=0}

    Sin hacer aún.

* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

## Pregunta 3
Formula para {a^i b^j / 0 <= i <= j} y mostrar ejemplos de uso para generar
cadenas.

    Genera cadenas con menor números de a's que de b's. Con S -> aSb me quito
    las que son de la forma aa$bb pero como necesito que haya menor número de a's
    añado la producción S -> Sb para así tener más b's. Entonces aaSbb -> aaSbbb.
    Para quitar la S usamos la producción S -> E. Finalmente tendremos aaSbbb ->
    aabbb.

    Lenguaje obtenido:
        S -> aSb,  S -> Sb, S -> E

* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *


## Pregunta 4 | Automata con pila
Obtener un automata con pila a partir de la gramática obtenida en la pregunta 3.

    Transiciones del autómata:

      - (q,e,S) = {(q,aSb),(q,Sb),(q,e)}
      - (q,a,a) = {(q,e)}
      - (q,b,b) = {(q,e)}

    Ahora producimos una cadena para poder hacer maching con el autómata, tenemos
    las producciones:

      - 1) S -> aSb
      - 2) S -> Sb
      - 3) S -> E

    Usamos primeramente la producción **1)**, luego la **2)** y luego la **3)**
    obteniendo la cadena **abb**.

    Ahora hacemos que pasaría con la pila con la cadena anterior, usando las
    transiciones del autómata.

* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

## Pregunta 5 | Problema de la pertenencia
Obtener un autómata con pila para {0^i 1^j / 0 <= j <= i} programando dicho
automata deseado.

                                <1,X>/e
                  --> q1 ----------------------> q2
                     |   |      <e,X>/e         |   |
                     \ _ /                      \ _ /
                    <0,R>/IR                   <1,X>/e
                    <e,R>/R                    <e,X>/e
                    <0,X>/XX                   <e,R>/e

    - <0,R>IR quiere decir que leo un 0, quito del tope de la pila la R y meto en la pila IR
    - <0,X>/XX quiere decir que leo un 0, quito del tope de la pila la X y meto en la pila XX
    - <e,R>/R quiere decir que no leo nada, quito del tope de la pila la R y meto en la pila R
    - <1,X>/e quiere decir que leo un 0, quito del tope de la pila la X y no meto nada.
    - <e,X>/e quiere decir que no leo nada, quito del tope de la pila la X y no meto nada.
    - <e,R>/e quiere decir que no leo nada, quito del tope de la pila la R y no meto nada.






















,,
