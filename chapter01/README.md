# Capítulo 01
## Campos Finitos

Una de las cosas más difíciles en el camino de aprender a programar en Bitcoin, es saber dónde comenzar. Existen demasiados componentes que dependen dentre sí, por tanto, muchas veces para aprender algo específico, se requeriere conocer primero de algo más.

Este capítulo llevará al lector a través de comienzo que sea fácil de manejar. Podrá parecer extraño, pero comenzaremos con las matemáticas básicas que se necesitan para comprender la criptografía de las curvas elípticas. La criptografía de curva elíptica aporta un algorimo para firmar y verificar información. Este es el corazón de la manera en que funcionan las tracciones, siendo éstas, la unidad atómica de transferencia de valor en Bitcoin. Al aprender de campos finitos y curvas elípticas, serás capáz de comprender los conceptos que se requieren para progresar lógicamente.

Debo advertir que los siguientes dos capítulos pueden resultar un poco incómodos, especialmente si no has hecho trabajo formal en matemáticas desde hace algún tiempo atrás. Insistiré al lector a estudiarlos debido a que algunos conceptos y el código que serán presentados allí, serán utilizados a lo largo de todo el libro.


## Aprendiendo Matemáticas de Alto Nivel

Aprender sobre estructuras matemáticas nuevas puede resultar un poco intimidante, pero en este capítulo pretendo disipar el mito sobre la dificultad de las matemáticas de alto nivel. Los **Campos Finitos**, en particular, no requieren demasiado, en términos de conocimiento previo en matemáticas, como por ejemplo, álgebra.

Piensa en los campos finitos como algo que pudiste haber aprendido en lugar de trigonometría, excepto que los sistemas de educación, de los que somos partícipes, deciden que la trigonometría es más importante para que tú aprendas. Esta es mi manera de decirte que los campos finitos no son tan difíciles de aprender y que no se requieren conocimientos previos de álgebra.

Este capítulo se requiere si el lector quiere comprender cómo funciona la criptografía de curva elíptica. Y a su vez, conocer la criptografía de curva elíptica permite comprender el proceso de firma y verificación de información, que son el corazón de Bitcoin. Dicho esto, este capítulo y el siguiente pueden parecer sin relación entre ellos, pero invito al lector a continuar y a no detenerse. Los fundamentos aquí mostrados harán que comprender Bitcoin sea mucho más sencillo, y además, facilitará el aprendizaje de otras tecnologías de punta relacionadas con Bitcoin, como: las firmas de Schnorr y las transacciones confidenciales.

## Definición de un Campo Finito

Matemáticamente, un *campo finito* está definido como un set finito de números y dos operaciones `+` (adición) y `.` (multiplicación) que satisfagan lo siguiente:

1. Si `a` y `b` son partes de un set, los resultantes de `a + b` y `a . b` están dentro del set. Llamamos a esta propiedad *cerrada*.
2. `0` existe y tiene la propiedad `a + 0 = a`. Llamamos a esto la *identidad aditiva*.
3. `1` existe y tiene la propiedad `a * 1 = a`. Llamamos a esto la *identidad multiplicativa*.
5. Si `a` está dentro del set, `-a` está en el set, el cual está definido como el valor tal que `a + (-a) = 0`. Llamamos a esto el *inverso aditivo*.
5. Si `a` está dentro del set y no es `0`, `a^-1` está en el set, el cual está definido como el valor tal que `a . (a^-1) = 1`. Llamamos a esto el *inverso multiplicativo*.

Desempaquemos poco a poco esta criteria.

Tenemos un set de números que es finita. Debido a que el set es finito, es posible designar un número `p`, que definita que tan grande es el set. Esto es a lo que llamamos, el *orden* del set.

El punto 1 nos habla de un sistema cerrado de adición y multiplicación. Esto significa que tenemos que definir la operación de suma y multiplicación de manera tal que el resultado se encuentre dentro del mismo set. Por ejemplo, un set que contiene `{0,1,2}` no es cerrado dentro de la adición, puesto que `1 + 2 = 3` y `3` no se encuentra dentro del set, así como tampoco lo estaría `2 + 2 = 4`. Por supuesto que podemos definir a la suma de manera un poco diferente para poderlo hacer funcionar, pero utilizando la adición "normal", este set no es *cerrado*. Por el otro lado, el set `{-1,0,1}` es cerrado para multiplicaciones normales. Dos números cualquiera pueden ser multiplicados (existen nueve combinaciones), y el resultado se encuentra siempre dentro del set.

La otra opción que tenemos, en matemáticas, es definir la multiplicación de una manera particular que convierta al set cerrado. Conoceremos más adelante en este capítulo, cómo definir exactamente a la adición y multiplicación. Sin embargo, el concepto clave es que es posible *definir a la adición y multiplicación de manera distinta a la adición y multiplicación simple con la que estamos familizarizados.*

Los puntos 2 y 3 hablan de la identidad aditiva y multiplicativa. Eso significa que 0 y 1 están en el set.

El punto 4 habla del inverso aditivo. Esto es, si `a` está en el set, `-a` también lo está. Utilizando el inverso aditivo es posible definir la resta, o substracción.

El punto 5 habla de que la multiplicación tiene también esa propiedad. Si `a` está en el set, `a^-1` también, dado que `a . (a^-1) = 1`. Utilizando el invero multiplicativo, podemos definir la división, aunque esta será la más complicada de definir, para un campo finito.

## Definiendo un Set Finito

Si el orden, o tamaño, del set es `p`, podemos llamar al elemento del set: `0, 1, 2, ..., p - 1`. Estos números son lo que llamaremos *elementos* del set, no necesariamente los números tradicionales 0, 1, 2, 3, etc. Se compartan de manera similar a los números tradicionales, pero también son diferentes en cómo se suman, resta, multiplican y dividen.

In notación matemática, el campo finito luce así:

```math
Fp = {0, 1, 2, ..., p - 1}
```

Lo que se encuentra dentro del set del campo finito son sus elementos. `Fp` es un campo finito específico llamado "campo de *p*", o "campo de 29", o de cualquier tamaño que sea (va de nuevo, el tamaño es lo que los matemáticos conocen como *orden*). Los números dentro de los corchetes `{}` representa los elementos que están dentro del campo. Nombramos a los elementos: 0, 1, 2 etc; porque estos nombres son convenientes para nuestro propósito.

Un campo finito de orden 11 luce así:


```math
F11 = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
```

Un campo finito de orden 17 luce así:


```math
F17 = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16}
```

Un campo finito de orden 983 luce así:


```math
F983 = {0, 1, 2, ..., 982}
```

Observe que el orden del campo es siempre 1 mayor que el elemento más grande del set. Habrás notado que los campos tienen un número primo en el orden, en los tres ejemplos anteriores. Por una variedad de razones eso se volvera más claro posteriormente, resulta que los campos *deben* tener un orden equivalente al de un número primo elevado a la `n` potencia. Y de los campos finitos que tienen un orden equivalente a un número primo son de los que más estamos interesados.

## Construyendo un Campo Finito con Python

Queremos representar cada elemento del campo finito. En Python, creamos una clase que represente un simple elemento del campo finito. Naturalmente, nombraremos a la clase `FieldElement`.

Esta clase representa un elemento en un campo Fprimo. La estructura de la clase luce así:

```python
class FieldElement:

    def __init__(self, num, prime):
        if num >= prime or num < 0:  # <1>
            error = 'Num {} not in field range 0 to {}'.format(
                num, prime - 1)
            raise ValueError(error)
        self.num = num  # <2>
        self.prime = prime

    def __repr__(self):
        return 'FieldElement_{}({})'.format(self.prime, self.num)

    def __eq__(self, other):
        if other is None:
            return False
        return self.num == other.num and self.prime == other.prime  # <3>
```

1. Revisamos que el número se encuentre dentro de `0` y `prime - 1`, de manera inclusive. Si no es el caso, obtenemos un elemento inválido de `FieldElement` y se genera una excepción `ValueError` debido al valor inapropiado.
2. El resto del método `__init__` asigna los valores iniciales del objeto.
3. El método `__eq__` revisa si dos objetos de la clase `FieldElement` son iguales. Esto sólo es cierto si ambas propiedades, `num` y `prime`, son iguales.

Lo que acabamos de definir, nos permite hacer lo siguiente:

```py
>>> from ecc import FieldElement
>>> a = FieldElement(7, 13)
>>> b = FieldElement(6, 13)
>>> print(a == b)
False
>>> print(a == a)
True
```

Python nos permite sobreescribir el operador de `==` de la clase `FieldElement` con el método `__eq__`. Estaremos tomando ventaja de esta propiedad de ahora en adelante.

El lector puede observar toda la acción en el código que acompaña este libro. O en el [repositorio de Github del libro en inglés](https://github.com/jimmysong/programmingbitcoin). Una vez listo el Jupyter Notebook, es posible navegar al destino `code-ch01/Chapter1.ipynb` y correr el código para mirar los resultados. Para el siguiente ejercicio, podrás abrir el destino `ecc.py` haciendo click en la liga del Ejercicio 1. Si te atoras, recuerda que las soluciones a todos los ejercicios está dentro de Apéndice A.


### Ejercicio 1

Escribe el método `__ne__` correspondiente, el cual revisa si dos objetos `FieldElement` son distintos uno del otro.


## Aritmética de Módulos

Una de las herramientas que podemos utilizar para hacer campos finitos cerrados bajo suma, resta, multiplicación y división es algo llamado *aritmética de módulo*.

Podemos definir la adición dentro del set finito utilizando la aritmética de módulo, la cual probablemente aprendiste cuando estudiaste las divisiones por primera vez. ¿Recuerdas los problemas como el que se muestra en la Figura 1-1?

![figura 1-1](https://github.com/jimmysong/programmingbitcoin/blob/master/images/prbc_0101.png)

*Figura 1-1. División larga ejemplo 1.*

Cuando la división no era exacta, existía un residuo, o el valor restante de la división entera. Definimos el módulo de la misma manera. Utilizamos el operador `%` para el módulo.

```math
7 % 3 = 1
```

La Figura 1-2 muestra otro ejemplo.

![figura 1-2](https://github.com/jimmysong/programmingbitcoin/blob/master/images/prbc_0102.png)

*Figura 1-2. División larga del ejemplo 2.*

Hablando formalmente, la operación de módulo es el restante después de la división de un número y el otro. Veamos otro ejemplo con números más grandes:

```math
1747 % 241 = 60
```

Es posible que ayude pensar a la aritmética de módulo como unas matemáticas de reloj. Imagine el siguiente problema:

```math
Son las 3 en punto. ¿Qué hora serán en 47 horas hacia el futuro?
```

La respuesta es 2 en punto, porque `(3 + 47) % 12 = 2` (observe la Figura 1-3).

![figura 1-3](https://github.com/jimmysong/programmingbitcoin/blob/master/images/prbc_0103.png)

*Figura 1-3. Reloj yendo en el tiempo 47 horas hacia delante.*

También podríamos verlo como si se tratara de envolver, o "dar vueltas alrededor", en el sentido de que se regresa a 0 cada vez que se mueve al rededor del reloj 12 horas.

Es posible realizar la operación de módulo en números negativos. Por ejemplo, se podría preguntar:

```math
Son las 3 en punto. ¿Qué hora fue hace 16 horas hacia el pasado?
```

La respuesta es 11 en punto.

```math
(3 - 16) % 12 = 11
```

El lado de los minutos es una operación de módulo también. Por ejemplo, puedes preguntar:

```math
Son las 12 minutos después de la hora. ¿Qué minuto serán después de 843 minutos en el futuro?
```

Serán 15 minutos después de la hora.

```math
(12 + 843) % 60 = 15
```

Además, es posible preguntar:

```math
Son las 23 minutos después de la hora. ¿Qué minuto serán después de 97 minutos en el futuro?
```

En este caso, la respuesta es `0`:

```math
(23 + 97) % 60 = 0
```

El módulo 0 indica que no hay residuo.

El resultado de la operación de módulo (%), en minutos, está siempre dentro de 0 y 59, inclusive. Esta resulta ser una propiedad muy útil del módulo, porque incluse números muy grandes pueden ser reducidos a un rango más pequeño:

```math
14738495684013 % 60  = 33
```

Estaremos utilizando el módulo para definir la aritmética del campo. La mayor parte de las operaciones en un campo finito utiliza el operador de módulo de alguna u otra manera.

## Aritmética de Módulos en Python

Python utiliza el operador aritmético de módulo `%`. Aquí un ejemplo de cómo se utiliza:

```python
>>> print(7 % 3)
1
```

Podemos utilizar también el operador de módulo con números negativos, así:

```python
>>> print(-27 % 13)
12
```

## Suma y Resta en Campos Finitos

Recuerde que tenemos que definir la suma del campo finito de tal manera que se asegure que el resultado se encuentre dentro del set. Lo que queremos conseguir es que la suma dentro del campo finito sea cerrada.

Podemos utilizar lo que aprendimos, aritmética de módulo, para volver la suma cerrada. Digamos que tenemos un campo finito de 19:

```math
F19 = {0, 1, 2, ..., 18}
```

Donde, `a, b ϵ F19`. Note que el símbolo `ϵ` significa "es un elemento de". En este caso, `a` y `b` son elementos de `F19`.

La adición cerrada significa que:

```math
a +f b ϵ F19
```

Denotamos la adición del campo finito utilizando el símbolo de `+f` para evitar confusión con la adición tradicional de enteros, `+`.

Si utilizamos la aritmética de módulo, garantizamos que éste sea el caso. Podemos definir `a +f b` de la siguiente manera:

```math
a +f b = (a + b) % 19
```

Por ejemplo:

```math
7 +f 8 = (7 + 8) % 19 = 15

11 +f 17 = (11 + 17) % 19 = 9
```

Así sucesivamente, podemos apreciar cómo es el operador `+f`.

Podemos tomar cualquier par de números en el set, sumárlos y darle la vuelta al final, para obtener el resultado de la suma. Estamos creando nuestro operador y resulta poco intuitivo. Después de todo, `11 +f 17 = 9` no aparenta estar correcto porque no estamos acostumbrados a la suma de campos finitos.

De manera general, definitimos la suma de campos de esta manera:

```math
a +f b = (a + b) % p
```

Donde, `a, b ϵ Fp`.

También definimos el inverso aditivo de la siguiente manera. `a ϵ Fp` implica que `-f a ϵ Fp`.

```math
-f a = (-a) % p
```

De nuevo, por claridad, utilizamos el operador `-f` para indicar que no se trata de una resta tradicional de enteros sino una operación de resta del campo finito.

En `F19`:

```math
-f 9 = (-9) % 19 = 10
```

Lo que significa que:

```math
9 +f 10 = 0
```

Y eso resulta ser verdadero, `True` en Python. Similarmente, es posible hacer una resta en el campo:

```math
a -f b = (a - b) % p
```

Donde, `a, b ϵ Fp`.

En `F19`:

```math
11 -f 9 = (11 - 9) % 19 = 2

6 -f 13 = (6 - 13) % 19 = 12
```

Así sucesivamente.

### Ejercicio 2

Resuelve estos problemas para `F57`. (Asuma que todos los `+`, `-` son en realidad `+f`, `-f` respectivamente):

```math
44 + 33

9 - 29

17 + 42 + 49

52 - 30 - 38
```

## Codeando Sumas y Restas en Python

En la clase `FieldElement` podemos ahora definir los métodos `__add__` y `__sub__`. La idea de estos métodos es que queremos que algo así funcione:

```python
>>> from ecc import FieldElement
>>> a = FieldElement(7, 13)
>>> b = FieldElement(12, 13)
>>> c = FieldElement(6, 13)
>>> print(a+b==c)
True
```

En Python podemos definir lo que significa una suma (el operador `+`) dentro del contexto de nuestra clase mediante el método de `__add__`. ¿Cómo hacemos ésto? Combinaremos lo que ya hemos aprendido de la aritmética de módulo y crearemos un nuevo método para nuestra clase `FieldElement` así:

```python
    def __add__(self, other):
        if self.prime != other.prime:  # <1>
            raise TypeError('Cannot add two numbers in different Fields')
        num = (self.num + other.num) % self.prime  # <2>
        return self.__class__(num, self.prime)  # <3>
```

1. Tenemos que asegurarnos que los elementos pertenezcan al mismo campo, de lo contrario, los cálculos no tendrían mucho sentido.
2. La adición, en un campo finito, está definida con el operador de módulo, tal cual se explicó anteriormente.
3. Debemos retornar una instancia de la clase. La cual tenemos, convenientemente accesible, mediante `self.__class__`. Pasas los dos argumentos principales, `num` y `self.prime`, para el método de `__init__` ya mencionado.


####TODO: make this logic clearer
Note que es posible utilizar `FieldElement`, en lugar de `self.__class__`, pero esto volvería a nuestro método menos fácil al momento de ser heredado. Estaremos utilizando una subclase de `FieldElement` más adelante, así que hacer el código heredable es importante aquí.

### Ejercicio 3

Escriba el correspondiente método de `__sub__`, que defina la resta de dos objetos de tipo `FieldElement`.

## Multiplicación y Potencias con Campos Finitos

Tal cual definimos nuestra suma `(+f)`, dentro de un campo finito cerrado, también podemos definir un nuevo método para llevar a cabo la multiplicación dentro de un campo finto cerrado. Así mismo, multiplicar el mismo número muchas veces, es una manera de definir elevar a la potencia. En esta sección, aprenderemos exactamente cómo definirlo utilizando la aritmética de módulo.

La multiplicación se puede definir como sumar el mismo número en múltiples ocasiones:

```math
5 * 3 = 5 + 5 + 5 = 15

8 * 17 = 8 + 8 + 8 + ... (un total de 17 veces 8) + 8 = 136
```

Podemos definir la multiplicación dentro de un campo finito de la misma forma. Operando en el campo `F19`, podemos observar:


```math
5 *f 3 = 5 +f 5 +f 5

8 *f 17 = 8 +f 8 +f 8 +f ... (un total de 17 veces 8) +f 8
```

Ya aprendimos cómo calcular el lado derecho de la ecuación, y dicho resultado estará dentro del set de `F19`:

```math
5 *f 3 = 5 +f 5 +f 5 = 15 % 19 = 15

8 *f 17 = 8 +f 8 +f ... (17 veces 8) +f 8 = (8 * 17) % 19 = 136 % 19 = 3
```

Note que cómo, el segundo resultado, no se intuye con tanta facilidad. No pensamos normalmente en la operación de `8 *f 17 = 3`, pero es una parte necesaria para definir a la multiplicación cerrada. Esto significa, que el resultado de una multiplicación dentro de un campo deberá estar siempre dentro del set `{0, 1, ..., p - 1}`.

Elevar a la potencia, o un exponente, es simplemente multiplicar el número una y otra vez:

```math
7^3 = 7 *f 7 *f 7 = 343
```

Dentro de un campo finito, podemos elevar al exponente mediante la aritmética de módulo:

```math
7^3 = 343 % 19 = 1

9^12 = 7
```

De nuevo, elevar al exponente nos arroja resultados muy poco intuitivos. Comúnmente no pensamos en términos de `7^3 = 1` ni de `9^12 = 7`. De nuevo, los campos finitos deben estar definidos para que el resultado de cualquier operación *siempre* esté dentro del campo.

### Ejercicio 4

Resuelva las siguientes ecuaciones en un campo `F97` (recuerde que `*` , `^`, son la versiones utilizadas dentro del campo):

```math
95 * 45 * 31

17 * 13 * 19 * 44

(12^7) * (77^49)
```

### Ejercicio 5

Para valores de `k = 1, 3, 7, 13, 18`, ¿cuál es el set en `F19`?

```math
{k * 0, k * 1, k * 2, k * 3, ..., k * 18}
```

Acaso, ¿no ha notado algo particular en los elementos de los sets?

## BIRD: ¿Por qué los campos primos son útiles?

    > La respuesta al Ejercicio 5 es la razón por la que decidimos utilizar campos finitos con un número de elementos igual a un número primo. Sin importar el valor de se elija de *k*, mientras sea mayor que 0, multiplicar el set entero por *k* resultará en el mismo set con el que se comenzó.
    > Intuitivamente, el hecho de tener un orden primo (tamaño del set), permite que cada elemento del campo finito sea equivalente.Si el orden del set fuera un número compuesto, multiplicar el set por uno de los divisores, resultaría en un set más pequeño.

## Codeando una Multiplicación con Python

Ahora que comprendemos cómo debe de ser una multiplicación de objetos `FieldElement`, queremos definir el método de `__mul__` que sobreescribe al operador de `*`. Queremos que ésto funcione:

```python
>>> from ecc import FieldElement
>>> a = FieldElement(3, 13)
>>> b = FieldElement(12, 13)
>>> c = FieldElement(10, 13)
>>> print(a*b==c)
True
```

De la misma manera que lo hicimos para la suma y la resta, el siguiente ejercicio es hacer funcionar la multiplicación dentro de nuestra clase. Definiendo el método de `__mul__`.


### Ejercicio 6

Escriba el método correspondiente a `__mul__`, que defina la multiplicación de dos elementos pertenecientes al mismo campo finito.

## Codeando la Potencia con Python

Debemos de definir, la potencia un objeto de tipo `FieldElement`, en Python se utilizas el método de `__pow__`, sobreescribiendo el operador `**`. La diferencia aquí, radica en que el exponente no es un objeto de tipo `FieldElement`. Por lo tanto, tiene que ser tratado de manera un poco distinta. Queremos que lo siguiente funcione:

```python
>>> from ecc import FieldElement
>>> a = FieldElement(3, 13)
>>> b = FieldElement(1, 13)
>>> print(a**3==b)
True
```

Note que debido a que el exponente es un número entero, en lugar de otra instancia de `FieldElement`, el método recibe la variable `exponente` como entero, o `int`. Podemos codearlo de esta manera:

```python
class FieldElement:
...
    def __pow__(self, exponent):
        n = exponent % (self.prime - 1)  # <1>
        num = pow(self.num, n, self.prime)
        return self.__class__(num, self.prime)  # <2>
```

1. Este es un modo perfecto de hacerlo, pero la función `pow(self.num, exponent, self.prime)` es mucho más eficiente.

2. Debemos retornar una instancia de la clase, como se hizo anteriormente.

¿Por qué no forzamos al exponente, a ser un objeto de tipo `FieldElement`? Resulta, que el exponente no tiene que ser parte del campo finito para que las matemáticas funcionen. De hecho, si lo fuera, el exponente no representaría el comportamiento intuitivo que esperaríamos, como la capacidad de sumar los exponentes cuando se multiplica con la misma base.

Parte de lo que estamos haciendo, con esta explicación, parecería muy lenta para números muy grandes. Sin embargo, estaremos utilizando algunos trucos para mejorar el desempeño de los algoritmos.

### Ejercicio 7

Para cada valor de `p = 7, 11, 17, 31`. ¿Cuál es el set en `Fp`?

```math
{1^(p - 1), 2^(p - 1), 3^(p - 1), 4^(p - 1), (p - 1)^(p - 1)}
```

## División en un Campo Finito

La intuición que nos ha ayudado con la adición, substracción, multiplicación, y quizá el exponente; desafortunádamente, no nos ayudará mucho con la división. Porque la división es la operación más difícil para hacer sentido, comenzaremos entonces, con algo que sí lo haga.

En matemáticas normales, la división es el inverso a la multiplicación:

```math
7 * 8 = 56; implica que 56/8 = 7

12 * 2 = 24; implica que 24/12 = 2
```

Y así sucesivamente, podemos utilizar esta definición de ahora en adelante. Note cómo es imposible, al igual que en las matemáticas regulares, dividir entre cero.

En el campo `F19`, sabemos que:

```math
3 *f 7 = 21 % 19 = 2; implica que (2 /f 7) = 3

9 *f 5 = 45 % 19 = 7; implica que (7 /f 5) = 9
```

Esta es una manera muy poco intuitiva de lo que pensamos de las fracciones: `(2 /f 7)` o `(7 /f 5)`; al menos, hasta antes de conocer los elementos de un campo finito. Sin embargo, esa es una de las propiedades importantes de los campos finitos: los campos finitos están cerrados ante la división. Esto significa que, dividir cualquier par de números, donde el denominador no sea 0, resultará en otro elemento dentro del campo finito.

La pregunta entonces es, ¿cómo calculo `(2 /f 7)` si no conozco de antemano que `3 *f 7 = 2`? Ésta es, sin dudarlo, una muy buena pregunta; y para responderla, necesitamos el resultado del Ejercicio 7.

Si no atrapaste la idea, la respuesta es que `n^(p - 1)` es siempre 1 para cada `p` que sea prima, y `n > 0`. Este es un bonito resultado, provinente de la teoría de los números, conocida como el Pequeño Teorema de Fermat.

```math
n^(p - 1) % p = 1
```

Donde `p` es prima.

Y dado que estamos operando en campos primos, esto será siempre verdad.


## BOX: Pequeño Teorema de Fermat

---

Existen muchas pruebas de este teorema, pero quizá la simple es utilizando lo que vimos en el Ejercicio 5, o sea, que estos sets son equivalentes:

```math
{1, 2, 3, ..., p - 2, p - 1} = {n % p, 2n % p, 3n % p, ..., (p-2)n % p, (p-1)n % p}
```

Los números resultantes podrán no estár en el orden correcto, pero los mismos números están presentes dentro de los dos sets. Podemos entonces multiplicar cada elemento en ambos sets, y obtener esta equivalencia:

```math
1 * 2 * 3 * ... * (p-2) * (p-1) % p = n * 2n * 3n * ... * (p-2)n * (p-1)n % p
```

El lado izquierdo de la ecuación, es lo mismo que `(p-1)! % p`, donde `!` es el número factorial (e.g., `5! = 5 * 4 * 3 * 2 * 1`). En el lado derecho de la ecuación, podemos juntar todas las `n` y obtener:

```math
(p-1)! * n^(p-1) % p
```

Por lo tanto:

```math
(p-1)! % p = (p-1)! * n^(p-1) % p
```

El `(p-1)!` se elimina en ambos lados de la ecuación, resultando en:

```math
1 = n^(p-1) % p
```

Esta es una prueba del Pequeño Teorema de Fermat.

---

Porque la división es el inverso de la multiplicación, sabemos que:

```math
a/b = a *f (1/b) = a *f (b^-1)
```

Podemos reducir el problema de la división, a un simple problema de multiplicación, al menos mientras sepamos el valor de `b^-1`. Aquí es cuando el Pequeño Teorema de Fermat entra en juego. Sabemos que:

```math
b^(p-1) = 1
```

Porque `p` es primo. Entonces:

```math
(b^-1) = (b^-1) *f 1 = (b^-1) *f b^(p-1) = b^(p-2)
```

O, lo que lo mismo:

```math
(b^-1) = b^(p-2)
```

En `F19`, esto indica que prácticamente `b^18 = 1`, lo que significa que `b^-1 = b^17`, para todos los valores de `b > 0`. En otras palabras, podemos calcular la inversa utilizando el operador de potencia. En el campo `F19`:

```math
2/7 = 2 * 7^(19 - 2) = 2 * 7^17 = 465261027974414 % 19 = 3

7/5 = 7 * 5^(19 - 2) = 7 * 5^17 = 5340576171875 % 19 = 9
```

Esta es una operación relativamente costosa, porque los cálculos exponenciales crecen muy rápido. La división es la operación más costosa, por esta misma razón. Para aligerar la carga, podemos utilizar la función de libería básica: `pow`. En Python, `pow(7, 17)` es equivalente a `7**17`. La función de `pow`, sin embargo, permite un tercer argumento que permiten hacer los cálculos de manera más eficiente. Específicamente, la función `pow` extraerá el módulo, tomándolo del tercer argumento. Entonces, `pow(7, 17, 19)` tiene el mismo resultado que `7**17 % 19`, pero lo hará mucho más rápido, porque la función de módulo corre cada que se hace una ronda de multiplicación.

### Ejercicio 8

Resuelve las siguientes ecuaciones dentro del campo `F31`:

```math
3/24

17^-3

(4^-4) * 11
```

### Ejercicio 9

Escriba el correspondiente método de `__truediv__`, el cual define la división de dos elementos.

Note que en Python 3, la división se separa en los siguiente métodos: `__truediv__` y `__floordiv__`. La primera hace la división regular, y la segunda hace la división de enteros.

## Redefiniendo Elevar a la Potencia

El último punto que tenemos que revisar, antes de terminar este capítulo, es el método de `__pow__`, el cual necesita manejar exponentes negativos. Por ejemplo, `a^-3` necesita ser parte de los elementos del campo finito, pero el código actual no cumple con estos requisitos. Necesitamos que funcione el siguiente código:

```python
>>> from ecc import FieldElement
>>> a = FieldElement(7, 13)
>>> b = FieldElement(8, 13)
>>> print(a**-3==b)
True
```

Desafortunadamente, de la manera en la que definimos `__pow__`, no es capaz de manejar exponentes con negativos, porque el segundo argumento de la función básica de Python `pow` debe de ser necesariamente positiva.

Sin embargo, podemos utilizar algunas matemáticas, de las que ya conocemos, para resolverlo. Conocemos por el Pequeño Teorema de Fermat que:

Esto significa que podemos multiplicar por `a^(p-1)` las veces querramos. Entonces, para `a^-3`, podemos hacer lo siguiente:

#TODO: formula

Esta es una manera en la que podemos hacer exponentes negativos. Una implementación sencilla, sería de la siguiente manera:

```python
class FieldElement:
...
    def __pow__(self, exponent):
        n = exponent
        while n < 0:
            n += self.prime - 1  # <1>
        num = pow(self.num, n, self.prime)  # <2>
        return self.__class__(num, self.prime)
```

1. Sumamos hasta encontrar un exponente positivo.
2. Usamos la función básica de Python `pow` para hacer el cómputo más eficiente.

Por fortuna, podríamos hacerlo incluso mejor. Ya aprendimos cómo forzar que un número no sea negativo, a utilizando nuestro amigo familiar `%`! Como bonus, podríamos incluso reducir los tiempos de cálculo de exponentes muy grandes, a su equivalente `a^(p-1) = 1`. Esto hará que la función `pow` no tenga que trabajar tan duro.

```python
    def __pow__(self, exponent):
        n = exponent % (self.prime - 1)  # <1>
        num = pow(self.num, n, self.prime)
        return self.__class__(num, self.prime)
```

1. Convierte al exponente, a un número entero dentro del rango de `0` a `p-2`, inclusive.

## Conclusión

En este capítulo hemos aprendido acerca de los campos finitos y su implementación en Python. Estaremos utilizando los campos finitos en el Capítulo 3 para la introducir la Criptografía de Curva Elíptica. Posteriormente, aprenderemos de otro concepto matemático, que nos ayudará a comprender lo qué será la Criptografía de Curva Elíptica: las curvas elípticas.
