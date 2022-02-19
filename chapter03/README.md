# Capítulo 03
## Criptografía de Curva Elíptica

Los últimos dos capítulos cubrieron la parte fundamental de matemáticas. Aprendimos cómo trabajan los Campos Finitos, y qué son las Curvas Elipticas. En este capítulo, combinaremos estos dos conceptos para aprender la criptografía de curva elíptica. Específicamente, vamos a programar los fundamentos necesarios para firmar y verificar mensajes, siendo estas dos funciones el corazón de Bitcoin.

## Curvas Elípticas sobre los Números Reales

En el capítulo 2, discutimos cómo lucen las curvas elípticas y las graficamos sobre el conjunto de números reales. Específicamente, no se trata sólo de números enteros, sino que hablamos de todos los números reales. Como, Pi, `sqrt(2)`, `e + la séptima raíz de 19`, y similares. Todos estos forman parte del conjunto de números reales.

Esto funciona porque los números reales son también un campo. A diferencia del campo *finito*, existe un número *infinito* de números reales, pero conservando las mismas propiedades:

1. Sí `a` y `b` se encuentran dentro del conjunto, `a + b` y `a * b` están en el conjunto.
2. 0 existe y tiene la siguiente propiedad `a + 0 = a`.
3. 1 existe y tiene la siguiente propiedad `a * 1 = a`
4. Sí `a` se encuentra dentro del conjunto, `-a` está en el conjunto, por lo que se define como el valor que toma `a + (-a) = 0`.
5. Sí `a` está dentro del conjunto y es diferente a `0`, `a^-1` está en el conjunto, por lo que se define cómo el valor que toma `a * (a^-1) = 1`.

Claramente, es cierto que la suma normal y multiplicación aplican para la primera parte, las identidades de `0` y `1` existen para la adición y multiplicación. `-x` es el aditivo inverso, y `1/x` es el multiplicativo inverso.

Los números reales son fáciles de plasmar en una gráfica. Por ejemplo, `y^2 = x^3 + 7` se grafica como la Figura 3-1.

![figura 3-1](https://github.com/jimmysong/programmingbitcoin/blob/master/images/prbc_0301.png)

*Figura 3-1. secp256k1 sobre el conjunto de números reales.*

Resulta que podemos utilizar la ecuación de adición de puntos sobre cualquier campo, incluyendo el campo finito que aprendimos en el Capítulo 1. La única diferencia es que tenemos que usar utilizar las operaciones de adición/sustracción/multiplicación/división definidas en el Capítulo 1, no las operaciones normales que utilizan los números reales.

## Curvas Elípticas sobre Campos Finitos

¿Cómo luce una curva elíptica sobre un campo finito? Veamos la ecuación `y^2 = x^3 + 7` sobre `F103`. Podemos verificar que el punto `(17, 64)` se encuentra dentro de la curva, mediante el cálculo de ambos lados de la ecuación:

```text
y^2 = 64^2 % 103 = 79

x^3 + 7 = (17^3 + 7) % 103 = 79
```

Hemos verificado que este punto se encuentra dentro de la curva utilizando matemáticas de campo finito.

Debido a que nos encontramos evaluando la ecuación sobre un campo finito, al momento de graficarla, luce completamente diferente (Figura 3-2).

![figura 3-2](https://github.com/jimmysong/programmingbitcoin/blob/master/images/prbc_0302.png)

*Figura 3-2. Curva elíptica sobre un campo finito.*

Cómo se puede observar, la gráfica luce más como un diagrama de dispersión y no existe una curva definida y continúa. Esto no es una sorpresa porque la representación de los puntos es discreta. El único patrón que existe en la curva es simétrica justo en el medio, debido al término de `y^2`. En cambio, la gráfica no es simétrica sobre el eje de las `x` como la curva de números reales, pero sí a mitad de camino en el eje `y` debido a que no existen números negativos en un campo finito.

Lo más impresionante es que podemos utilizar la misma ecuación para la adición de puntos, con la adición, sustracción, multiplicación, división y potenciación cómo hemos definido para el campo finito y todo sigue funcionando correctamente. Esto puede parecer sorprendente, pero en las matemáticas abstractas existen regularidades cómo ésta, a pesar de ser una manera muy distinta al modo tradicional de realizar las operaciones con las que estamos familiarizados.

### Ejercicio 1

Evalúe si los siguientes puntos se encuentran dentro de la curva `y^2 = x^3 + 7` sobre `F223`:

```text
(192, 105), (17, 56), (200, 119), (1, 193), (42, 99)
```

## Codeando Curvas Elípticas sobre Campos Finitos

Dado que hemos definido el punto de la curva elíptica y también las operaciones `+`, `-`, `*` y `/` para los campos finitos, podemos combinar las dos clases para crear los puntos de la curva elíptica sobre un campo finito:

```python
>>> from ecc import FieldElement, Point
>>> a = FieldElement(num=0, prime=223)
>>> b = FieldElement(num=7, prime=223)
>>> x = FieldElement(num=192, prime=223)
>>> y = FieldElement(num=105, prime=223)
>>> p1 = Point(x, y, a, b)
>>> print(p1)
Point(192,105)_0_7 FieldElement(223)
```

Cuándo inicializamos `Point`, vamos a correr la siguiente parte del código:

```python
class Point:

    def __init__(self, x, y, a, b):
        self.a = a
        self.b = b
        self.x = x
        self.y = y
        if self.x is None and self.y is None:
            return
        if self.y**2 != self.x**3 + a * x + b:
            raise ValueError('({}, {}) is not on the curve'.format(x, y))
```

Los operadores de adición `(+)`, multiplicación `(*)`, potenciación `(**)` y el booleano ¨no igual a¨ `(!=)` utilizan los métodos de `__add__`, `__mul__`, `__pow__` y `__ne__` del campo finito, respectivamente, y no las operaciones equivalentes de números enteros. Siendo capaces de realizar las mismas ecuaciones pero con una definición diferente para cada operador básico aritmético, podemos construír una librería para criptografía de curva elíptica.

Ya hemos realizado el código de las dos clases que necesitamos para implementar los puntos de la curva eléctrica sobre un campo finito. Sin embargo, para probar nuestro trabajo, es útil que tener un suite de pruebas. Haremos esto utilizando los resultados del Ejercicio 1:

```python
class ECCTest(TestCase):

    def test_on_curve(self):
        prime = 223
        a = FieldElement(0, prime)
        b = FieldElement(7, prime)
        valid_points = ((192, 105), (17, 56), (1, 193))
        invalid_points = ((200, 119), (42, 99))
        for x_raw, y_raw in valid_points:
            x = FieldElement(x_raw, prime)
            y = FieldElement(y_raw, prime)
            Point(x, y, a, b)  # <1>
        for x_raw, y_raw in invalid_points:
            x = FieldElement(x_raw, prime)
            y = FieldElement(y_raw, prime)
            with self.assertRaises(ValueError):
                Point(x, y, a, b)  # <1>
```
1. Vamos a pasar el objeto `FieldElement` a la clase `Point` para su inicialización. Este procedimiento utilizará todas las operaciones matemáticas que definimos en `FieldElement`.

Podemos correr la prueba de la siguiente manera:

```python
>>> import ecc
>>> from helper import run
>>> run(ecc.ECCTest('test_on_curve'))
.
----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
```

1. `helper` es un módulo con valiosas funciones de utilidad, incluyendo la habilidad de correr pruebas unitarias de manera individual.

## Adición de Puntos sobre un Campo Finito

Podemos utilizar las mismas que ecuaciones sobre un campo finito, incluyendo la ecuación lineal:

```text
y = mx + b
```

Resulta que una "línea" en un campo finito no es precisamente lo que esperaríamos (Figura 3-3).

![figura 3-3](https://github.com/jimmysong/programmingbitcoin/blob/master/images/prbc_0303.png)

*Figura 3-3. Línea sobre un campo finito.*

La ecuación, sin embargo, funciona y podemos calcular el valor de `y` a partir de un valor de `x` dado.

Notablemente, la adición de puntos sobre un campo finito también funciona. ¡Esto se debe a que la adición de puntos de una curva elíptica funciona sobre todos los campos! Las mismas funciones que utilizamos para calcular la adición de puntos para números reales, funciona para campos finitos. Específicamente, cuando `x1 ≠ x2`:

```text
P1 = (x1, y1), P2 = (x2, y2), P3 = (x3, y3)
P1 + P2 = P3
s = (y2 – y1)/(x2 – x1)
x3 = s^2 – x1 – x2
y3 = s(x1 – x3) – y1
```

Y cuando `P1 = P2`:

```text
P1 = (x1, y1), P3 = (x3, y3)
P1 + P1 = P3
s = (3 * (x1^2) + a)/(2 * y1)
x3 = s2 – (2 * x1)
y3 = s(x1 – x3) – y1
```

Todas las ecuaciones de curva elíptica funcionan sobre un campo finito, esto nos permitirá definir algunos primitivos criptográficos.

