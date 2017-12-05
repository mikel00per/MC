# 1. Practica 1
Determinar si la gramática G = ({S,A,B}, {a,b,c,d}, P, S) donde P es el
conjunto de reglas de producción:

  - S -> AB
  - B -> cB
  - A -> Ab
  - B -> d
  - A -> a

Genera un lenguaje de tipo 3, ¿es regular?

## 1.1. Solución
Lo

Lo primero que deberemos hacer es determinar L, el lenguaje que genera
la gramatica anterior.

* * * * *

# 2. Practica 2
Implemente con jFlash un automata determinista aportandole a la herramienta un
autómatá no determinista, además hacerlo también con un autómata con
transiciones nulas.

## 2.1. Solución

 * * * * *

# 3. Practica 3
Crear un fichero Lex con código mostrado en el tema dos y comprobar que
funciona correctamente.

## 3.1. Solución

* * * * *

# 4. Practica 4 | A.P -> Gramática
Obtener la gramatica libre del contexto del autómata con pila vacia siendo:
    M = ({q1,q2}, {0,1}, {R,X}, δ, q1, R, 0)
    L={0^i 1^i : i≥0}

    δ(q1,0,R) = {(q1,XR)}
    δ(q1,0,X) = {(q1,XX)}
    δ(q1,1,R) = {(q2,XR)}
    δ(q2,1,X) = {(q2,XX)}
    δ(q1,1,X) = {(q1,ε)}
    δ(q2,0,X) = {(q2,ε)}
    δ(q1,ε,R) = {(q1,ε)}
    δ(q2,ε,R) = {(q1,R)}

## 4.1. Solución
Primero ponemos todas las producciones de forma que el lenguaje L se contruya
a partir de variables S de la forma [p,X,q] donde esta variable indica que
se puede ir del estado p a q quitando X de la pila. Por lo que todas
las producciones S → [q0,Z0,q], q pertenece a Q.

  - S → [q1,R,q1]
  - S → [q1,R,q2]

Ahora ponemos todas las producciones necesarias para hacer matching que consiste
en sacar de la pila al leer un simbolo de entrada yno meter nada en la pila
es decir: δ(q1,1,X) = {(q1,ε)}

  - δ(q1,1,X) = {(q1,ε)} : [q1,X,q1] → 1
  - δ(q1,ε,R) = {(q1,ε)} : [q1,R,q1] → ε
  - δ(q2,ε,R) = {(q1,R)} : [q2,X,q2] → 0

Tras los matchings añadimos los estados de transición que simbolizan que hay que
meter uno o varios simbolos en la pila tras leer. Es decir las que son de la
forma: δ(q2,ε,R) = {(q1,R)}

  - δ(q2,ε,R) = {(q1,R)}
    - [q2,R,q1] → [q1,R,q1]
    - [q2,R,q2] → [q2,R,q2]
  - δ(q1,0,R) = {(q1,XR)}
    - [q1,R,q1] → 0[q1,X,q1] [q1,R,q1]
    - [q1,R,q1] → 0[q1,X,q2] [q2,R,q1]
    - [q1,R,q2] → 0[q1,X,q1] [q1,R,q2]
    - [q1,R,q2] → 0[q1,X,q2] [q2,R,q2]
  - δ(q1,1,R) = {(q2,XR)}
    - [q1,R,q2] → 1[q1,X,q1] [q1,R,q2]
    - [q1,R,q2] → 1[q1,X,q2] [q2,R,q2]
    - [q1,R,q1] → 1[q1,X,q1] [q1,R,q1]
    - [q1,R,q1] → 1[q1,X,q2] [q2,R,q1]
  - δ(q1,0,X) = {(q1,XX)}
    - [q1,x,q1] → 0[q1,X,q1] [q1,X,q1]
    - [q1,x,q1] → 0[q1,X,q2] [q2,X,q1]
    - [q1,x,q2] → 0[q1,X,q1] [q1,X,q2]
    - [q1,x,q2] → 0[q1,X,q2] [q2,X,q2]
  - δ(q2,1,X) = {(q2,XX)}
    - [q2,x,q2] → 1[q2,X,q2] [q2,X,q2]
    - [q2,x,q2] → 1[q2,X,q1] [q1,X,q2]
    - [q2,x,q1] → 1[q2,X,q2] [q2,X,q1]
    - [q2,x,q1] → 1[q2,X,q1] [q1,X,q1]

* * * * *
