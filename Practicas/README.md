# 1. Práctica 1 | Optimizando gramáticas

Determinar si la gramática G = ({S,A,B}, {a,b,c,d}, P, S) donde P es el
conjunto de reglas de producción:

  - S → AB (1)
  - B → cB (2)
  - A → Ab (3)
  - B → d (4)  
  - A → a (5)  

Genera un lenguaje de tipo 3, ¿es regular?

## 1.1. Solución

Lo primero que deberemos hacer es determinar L, el lenguaje que genera
la gramatica anterior usando sus producciones.

S → AB le podemos aplicar la producción 5) ó 3):

  - S → AB → aB
  - S → AB → AbB

Tras esto a ambas producciones si le aplicamos respectivamente las producciones
2) 4) y 2) 3) 4) 5) lo que ocurriría serría:

  - Con el primer caso:
    - S → AB → aB → acB      // La usaremos
    - S → AB → aB → ad       // Final, todos terminales.

  - Con el segundo caso:
    - S → AB → AbB → AbcB    // La usaremos
    - S → AB → AbB → AbbB    // La usaremos
    - S → AB → AbB → Abd     // La usaremos
    - S → AB → AbB → abB     // Misma producción que antes por lo que no
                             // seguiremos por aquí.

La variable A siembre será sustituida por el simbolo terminal A o por Ab, lo
que quiere decir que con A solo producciremos o otra A o una b que irá precedida por una A por lo que sabemos que el lenguaje siempre tendrá como mínimo una "a" al comenzar y seguida por una cantidad de b superior.

Volvemos a aplicar producciones para ver los resultados, como sé lo que va a
pasar con la variable A, no hago producciones con ella, solo con B:

  - Con el primer caso:
    - S → AB → aB → acB → accB
    - S → AB → aB → acB → acd

  - Con el segundo caso:
    - S → AB → AbB → AbcB → AbccB
    - S → AB → AbB → AbcB → Abcd
    - S → AB → AbB → AbbB → AbbcB
    - S → AB → AbB → AbbB → Abbd

Como podemos ver sabemos que, siempre comenzará por A y de la misma forma, siempre ha de acabar por una d, es decir, estos dis simbolos siempre van a aparecer en todas las producciones. En cambio el número de b's y c's es variable puede haber más simbolos de c que de b y viceversa, e incluso no haber uso de ellos como hemos visto en una de las producciones que nos quedaba "ad". Puedo afirmar pues que el lenguaje generado es:

    {ab^ic^jd : 0<=i<=j i,j ∈ N}

Ahora para terminar deberemos de genera una gramática de tipo 3 que pueda generar el lenguaje anterior. La primera producción con S debería dejarnos como mínimo el simbolo terminal "a" a la izquerda y una variable "B" por ejemplo que usaremos para seguir producciendo la sucesión de b's y c's, además de poder poner d. Así con S tendríamos:

  - S → aB

Para poder tener tener b^i veces usaremos:
  - B → bB

Para poder tener c^j veces usaremos dos producciones, una para cambiar la
B de la producción anterior a C y así generar C's o para terminar.
  - B → C
  - C → cC

Para poder poner el simbolo terminal d usaremos una última producción:
  - C → d

Por lo que la gramática de tipo 3 sería la siguiente:
    S → aB
    B → bB
    B → C
    C → cC
    C → d

* * * * *

# 2. Práctica 2 | AD → AND con jFlap
Implemente con jFlap un automata determinista aportandole a la herramienta un
autómatá no determinista, además hacerlo también con un autómata con
transiciones nulas.

## 2.1. Solución
Los autómatas que he elegido de las transparencias de teoría para implementar
en jFlap son:

  - AD sin transiciones nulas, acepta cadena con la subcadena 010010.

  ![AFD](https://github.com/mikel00per/MC/blob/master/Practicas/AFD.png)

Usando jFlap el resultado sería el siguiente:

  ![AFND](https://github.com/mikel00per/MC/blob/master/Practicas/AFND.png)

  - Con transiciones nulas:

  ![AFDTN](https://github.com/mikel00per/MC/blob/master/Practicas/AFDTN.png)

Usando jFlap el resultado sería el siguiente:

  ![AFNDTN](https://github.com/mikel00per/MC/blob/master/Practicas/AFNDTN.png)

 * * * * *

# 3. Práctica 3 | Lex (Expresiones regulares)
Crear un fichero Lex con código mostrado en el tema dos y comprobar que
funciona correctamente.

## 3.1. Solución
El código lex empleado es el siguiente:

``` [lex]
  car     [a-zA-Z]
  digito	[0-9]
  signo	  (\-|\+)
  suc     ({digito}+)
  enter		({signo}?{suc})
  real1		({enter}\.{digito}*)
  real2		({signo}?\.{suc})
  int ent=0, real=0, ident=0, sumaent=0;

  %%
    int i;
    {enter}   {ent++;sscanf(yytext,"%d",&i); sumaent +=i;
    			printf("Numero entero %s\n",yytext);}
    ({real1}|{real2})	{real++; printf("Num. real%s\n",yytext);}
    {car}({car}|{digito})*	{ident++; printf("Var. ident.%s\n", yytext);}
    .|\n			{;}
  %%

  yywrap()
  	{printf("Numero de Enteros%d, reales%d, ident%d, \
  		Suma de Enteros %d",ent,real,ident,sumaent);return 1;}
```

Lo que deberemos hacer con el código es:
  - Crear un fichero "codigo" con el código.
  - Ejecutar lex con el fichero creado: $ lex ejemplo
  - Compilar el programa que crea lex: $ gcc lex.yy.c -o prog -ll
  - Ejecutar el programa: $ ./prog < Entrada > Salida

    **NOTA**: Si usais flex en lugar de lex quizás tendreis que cambiar las
    opciones de compilación: gcc lex.yy.c -o prog -lfl

Mi fichero de ejemplo será una sucesión de productos a las que llamo "algo" y sobre las que expreso como número enteros la cantidad de kilos y en decimal el precio del quilo de ese "algo", así pues, mi fichero es el siguiente:

    algo1 50 kg -> 1.3 €/kg
    algo2 100 kg -> 1.2 €/kg
    algo3 200 kg -> 1.1 €/kg
    algo4 120 kg -> 1.8 €/kg
    algo5 120 kg -> 1.9 €/kg
    algo6 20 kg -> 0.32 €/kg
    algo7 40 kg -> 1.34 €/kg
    algo8 40 kg -> 2.35 €/kg
    algo9 50 kg -> 4.45 €/kg
    algo10 32 kg -> 5.67 €/kg
    algo11 28 kg -> 1.81 €/kg
    algo12 50 kg -> 0.73 €/kg
    algo13 20 kg -> 4.53 €/kg
    algo14 28 kg -> 1.21 €/kg
    algo15 39 kg -> 5.23 €/kg

Con este fichero la salida sería la siguiente:

  ![salida_lex](https://github.com/mikel00per/MC/blob/master/Practicas/salida_lex.png)


* * * * *

# 4. Práctica 4 | A.P → Gramática
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
