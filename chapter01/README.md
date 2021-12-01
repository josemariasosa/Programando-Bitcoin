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
F11 = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16}
```

Un campo finito de orden 983 luce así:


```math
F11 = {0, 1, 2, ..., 982}
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

Figura 1-1. División larga ejemplo 1.

Cuando la división no era exacta, existía un residuo, o el valor restante de la división entera. Definimos el módulo de la misma manera. Utilizamos el operador `%` para el módulo.

```math
7 % 3 = 1
```

Figura 1-2 muestra otro ejemplo.

Figura 1-2. División larga ejemplo 2.

Hablando formalmente, la operación de módulo es el restante después de la división de un número y el otro. Veamos otro ejemplo con números más grandes:

```math
1747 % 241 = 60
```

Es posible que ayude pensar a la aritmética de módulo como unas matemáticas de reloj. Imagine el siguiente problema:

```math
Son las 3 en punto. ¿Qué hora serán en 47 horas hacia el futuro?
```

La respuesta es 2 en punto, porque `(3 + 47) % 12 = 2` (observe la Figura 1-3).

Figura 1-3. Reloj yendo en el tiempo 47 horas hacia delante.

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




