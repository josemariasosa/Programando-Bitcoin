# Capítulo 02
## Curvas Elípticas

En este capítulo, aprenderemos acerca de las curvas elípticas. En el capítulo 3 combinaremos los conceptos de curvas elípticas, con los campos finitos, para trabajar con Criptografía de Curva Elíptica.

Al igual que los campos finitos, las curvas elípticas pueden lucir intimidantes, si es la primera vez que te topas con ellas. Pero les recuerdo, que las matemáticas que se utilizan no son tan difíciles. La mayor parte de lo que necesitas, para utilizar las curvas elípticas, se pudo haber aprendido antes que el álgebra. En este capítulo, exploraremos lo que son estas curvas, y qué podemos hacer con ellas.

## Definición

Las curvas elípticas son como muchas de las ecuaciones que pudiste haber visto pre-algebra. Tienen `y` de un lado, y a `x` del otro. De alguna manera, las curvas elípticas, tienen la siguiente forma:

#TODO: formula

Seguramente ya has trabajado con otras ecuaciones que lucen de manera similar. Por ejemplo, es probable que recuerdes la ecuación lineal, de alguna clase de matemáticas.

#TODO: formula

Hasta incluso, podrás recordar, que a la `m` se le conoce como *pendiente* y `b` es la intersección. Podemos incluso graficar la ecuación lineal, como se muestra en la Figura 2-1.

#TODO: image

*Figura 2-1. Ecuación lineal.*

Similarmente, es probable que estés familiarizado con la ecuación cuadrática, la gráfica en Figura 2-2. Y seguramente, mientras llevabas la clase de álgebra, pudiste haber visto ecuaciones de más alto nivel, de magnitud de `x`. Algo como la ecuación cúbica, gráfica en Figura 2-3.

#TODO: image

*Figura 2-2. Ecuación cuadrática.*

#TODO: image

*Figura 2-3. Ecuación cúbica.*

Las curvas elípticas no son tan diferentes. La única real diferencia entre la curva elíptica y la curva cúbica de la Figura 2-3, es el término `y^2` que se encuentra del lado izquierdo de la ecuación. Esto hace que la función tenga el efecto de simetría, al rededor del eje de las `x`, como se muestra en la Figura 2-4.

#TODO: image

*Figura 2-4. Curva elíptica contínua.*

La curva elíptica tiene una menor inclinación que la curva cúbica. Esto se debe al término `y^2` que se encuentra del lado izquierdo de la ecuación. En ocasiones, la curva puede estar separada en dos partes como en la Figura 2-5. Se dice que está disasociada. 

#TODO: image

*Figura 2-5. Curva elíptica disasociada.*

Para imaginarnos una curva elíptica, ayudaría el pensarla cómo si comenzáramos realizando la gráfica de la ecuación cúbica, Figura 2-6. Aplanamos la parte que está arriba del eje de las `x`, como en la Figura 2-7. Y por último, le damos un efecto de espejos con la parte que está por debajo del eje de las `x`, Figura 2-8.

#TODO: image

*Figura 2-6. Paso 1. Ecuación cúbica.*

#TODO: image

*Figura 2-7. Paso 2. Aplanar la ecuación cúbica.*

#TODO: image

*Figura 2-8. Efecto de espejo sobre el eje de las `x`.*

Específicamente, la curva elíptica utilizada en Bitcoin se llama `secp256k1` y utiliza esta ecuación en particular:

#TODO: formula

La forma canónica es `y^2 = x^3 + ax + b`, así que la curva está definida por las constantes `a = 0`, `b = 7`. Luce como la Figura 2-9.

#TODO: image

*Figura 2-9. Curva elíptica secp256k1.*

## Codeando Curvas Elípticas en Python

Por una variedad de razones, que serán claras más adelante, no estamos interesados en la curva como tal, sino más bien, en los puntos específicos de esa curva. Por ejemplo, en la curva `y^2 = x^3 + 5x + 7`, estamos interesados en la coordenada `(-1, 1)`. Vamos entonces a definir la clase `Point`, para hacer referencia a un punto específico de la curva. La curva tiene que ser de la siguiente manera `y^2 = x^3 + ax + b`, para que podamos definirla, únicamente, con los valores de `a` y `b`.

```python
class Point:

    def __init__(self, x, y, a, b):
        self.a = a
        self.b = b
        self.x = x
        self.y = y
        if self.y**2 != self.x**3 + a * x + b:  # <1>
            raise ValueError('({}, {}) is not on the curve'.format(x, y))

    def __eq__(self, other):  # <2>
        return self.x == other.x and self.y == other.y \
            and self.a == other.a and self.b == other.b
```

1. Aquí nos aseguramos que el punto se encuentre dentro de la curva.
2. Los puntos son iguales sí, y sólo sí, están en la misma curva y tienen las mismas coordenadas.

Podemos ahora crear objetos de tipo `Point`, y obtendremos un error si el punto se encuentra afuera de la curva:

```python
>>> from ecc import Point
>>> p1 = Point(-1, -1, 5, 7)
>>> p2 = Point(-1, -2, 5, 7)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "ecc.py", line 143, in __init__
    raise ValueError('({}, {}) is not on the curve'.format(self.x, self.y))
ValueError: (-1, -2) is not on the curve
```

En otras palabras, el método de `__init__` va a levantar a una excepción cuando el punto no se encuentre dentro de la curva.

### Ejercicio 1.

Determine cuál de los siguientes puntos se encuentran dentro de la curva: `y^2 = x^3 + 5x + 7`.

### Ejercicio 2.

Escriba el método de `__ne__` para el objeto tipo `Point`.

## Adición de Puntos

Las curvas elípticas son útiles debido a a una propiedad conocida como la adición de puntos. La adición de puntos es cuando se realiza una operación con dos de los puntos, dentro de la misma curva, y se genera un tercer punto, que también está dentro de la curva. A esto se le conoce como adición porque la operación tiene mucho que ver con la intuición asociada a la operación de adición en las matemáticas. La adición de puntos es conmutativa. Esto quiere decir, que sumar el punto `A` con el punto `B`, es lo mismo que sumar el punto `B` con el punto `A`.

#TODO: image

*Figura 2-10. La línea intersecta en un solo punto.*

#TODO: image

*Figura 2-11. La línea intersecta en tres puntos.*

Las únicas dos excepciones son, cuando la línea cruza verticalmente, Figura 2-12. Y cuando la línea es tangente a la curva, Figura 2-13.

#TODO: image

*Figura 2-12. La línea intersecta en dos puntos, porque es vertical.*

#TODO: image

*Figura 2-13. La línea intersecta en dos puntos, porque toca tangencialmente a la curva.*

Volveremos a revisar estos dos casos en particular más adelante.

Podemos definir la adición de puntos, utilizando el hecho de que la línea intersecta en 1 o 3 puntos en la curva elíptica. Dos puntos definen una línea, así que, como esta línea debe interceptar la curva más de una vez, ese tercer punto, reflejado sobre el eje de las `x`, es el resultado de la adición de los puntos.

Así que, para cualquier par de puntos `P1 = (x1, y1)` y `P2 = (x2, y2)`, podemos calcular `P1 + P2` de la siguiente forma:

- Encontrar el punto que intersecta la curva elíptica en una tercera vez, dibujando una línea que pase por `P1` y `P2`.
- Proyectar el punto sobre el eje de las `x`, como en la Figura 2-14.

#TODO: image

*Figura 2-14. Adición de puntos.*

Primero, dibujamos una línea que cruce los dos puntos que estamos sumando, `A` y `B`. La tercera intersección es el punto `C`. Reflejamos el valor de ese punto sobre el eje de las `x`, el cual nos pone en el punto `A + B`, como en la Figura 2-14.

Una de las propiedades qué vamos a utilizar para la suma de los puntos, no es necesariamente intuitiva. Podemos calcular la adición de puntos fácilmente, utilizando una fórmula. La solución es poco intuitiva, porque el resultado de la suma de puntos, puede estar en cualquier lugar dados dos puntos en la curva. Regresando a la Figura 2-14, el resultado de `A + B` está a la derecha de los dos puntos. Para el punto `A + C` el resultado está en algún lugar en medio de los puntos. Y para `A + C`, el resultado estaría a la izquierda de los dos puntos. Utilizando terminología de matemáticas, se puede decir que la adición de puntos es *no-lineal*.

## La Matemática de Sumar Puntos

La adición de puntos satisface algunas propiedades que asociamos con la suma, como:

- Identidad
- Conmutatividad
- Asociatividad
- Invertibilidad

La propiedad de *identidad* significa que es posible hacer una operación con cero. Existe un punto `I` que, cuándo se suma al punto `A`, el resultado es `A`:

```math
I + A = A
```

Llamaremos a este punto: *punto al infinito*, la razón quedará clara en un momento.

Esto está relacionado a la propiedad de *invertibilidad*. Para un punto `A`, existe otro punto `-A` qué el sumarse resulta el punto de *identidad*. Esto es:

```math
A + (-A) = I
```

Visualmente, estos puntos son opuestos, el uno del otro, en el eje de las `x` dentro de la curva, ver Figura 2-15.

#TODO: image

*Figura 2-15. Intersección de línea vertical.*

Esta es la razón por la que llamamos a este punto: *punto al infinito*. Tenemos un punto extra en la curva elíptica que hace que la línea vertical intersecte la curva en una tercera ocasión.

La *conmutatividad* significa que `A + B = B + A`. Esto resulta obvio porque la línea que va del punto `A` al punto `B`, intersecta la curva una tercera vez en el mismo lugar, sin importar el orden.

La *asociatividad* significa que `(A + B) + C = A + (B + C)`. Esta propiedad resulta no ser tan obvia y es la razón por la cual hacemos el salto sobre el eje de las `x`. Esto se muestra en la Figuras 2-16 y Figura 2-17.

Se puede observar en las dos Figuras 2-16 y 2-17, que el punto final es el mismo. En otras palabras, tenemos una buena razón para creer en `(A + B) + C = A + (B + C)`. Esta no se trata de una prueba formal para la asociatividad en la suma de puntos, sin embargo, esta gráfica debería de darnos una intuición de que la propiedad es verdad.

#TODO: image

*Figura 2-16. (A + B) + C.*

#TODO: image

*Figura 2-15. A + (B + C).*

Para codear la adición puntos, separaremos el proceso en tres pasos:

1. Cuándo los puntos se encuentran en una línea vertical, o se está utilizando el punto identidad.
2. Cuándo los puntos no se encuentran en una línea vertidad, y son diferentes.
3. Cuando los dos puntos son iguales.

# Haciendo el Código para la Adición de Puntos

Primero nos encargaremos del punto identidad, o del punto al infinito. Debido a que no podemos utilizar el valor de `infinito` en Python, utilizaremos el valor de `None` en su lugar. Queremos que lo siguiente funcione:

```python
>>> from ecc import Point
>>> p1 = Point(-1, -1, 5, 7)
>>> p2 = Point(-1, 1, 5, 7)
>>> inf = Point(None, None, 5, 7)
>>> print(p1 + inf)
Point(-1,-1)_5_7
>>> print(inf + p2)
Point(-1,1)_5_7
>>> print(p1 + p2)
Point(infinity)
```

Para hacer que esto funcione, necesitamos hacer dos cosas. Primero, tenemos que ajustar el método de `__init__` para evitar que evalúe, si la ecuación de la curva está satisfecha, cuando tenemos un punto al infinito. Posteriormente, tenemos que cargar el operador de adición, o el método de `__add__`, de la misma manera que lo hicimos con la clase `FieldElement`.

```python
class Point:

    def __init__(self, x, y, a, b):
        self.a = a
        self.b = b
        self.x = x
        self.y = y
        if self.x is None and self.y is None:  # <1>
            return
        if self.y**2 != self.x**3 + a * x + b:
            raise ValueError('({}, {}) is not on the curve'.format(x, y))

    def __add__(self, other):  # <2>
        if self.a != other.a or self.b != other.b:
            raise TypeError('Points {}, {} are not on the same curve'.format
            (self, other))

        if self.x is None:  # <3>
            return other
        if other.x is None:  # <4>
            return self
```

1. Si las coordenadas de `x` y `y` son `None`, identificamos el punto al infinito. Note que el siguiente condicional de `if` va a fallar si no retornamos aquí.
2. Sobrescribimos el operador de adición aquí.
3. Si `self.x` es `None` significa qué `self` es el punto al infinito, o la identidad aditiva. Si es así, entonces regresamos `other`.
4. Si `other.x` es `None` significa qué `other` es el punto al infinito, o la identidad aditiva. Si es así, entonces regresamos `self`.

### Ejercicio 3

Asegúrese de que el código sea capáz de procesar el caso cuando los dos puntos son la inversa aditiva, esto es, que tienen la misma `x` pero diferente `y`, resultando en una línea vertical. Esto deberá de retornar el punto al infinito.

## Adición de Puntos cuando X1 es diferente a X2

Ahora que hemos cubierto la línea vertical, examinaremos el caso cuando los puntos son diferentes. Cuando tenemos dos puntos dónde la `x` difiere, podemos sumar utilizando una simple fórmula. Para llevar a cabo la operación, primero calcularemos la pendiente creada por los dos puntos. Podemos hacer esto utilizando la fórmula de pre-álgebra:

```math
P1 = (x1, y1); P2 = (x2, y2); P3 = (x3, y3);

P1 + P2 = P3

s = (y2 - y1)/(x2 - x1)
```

La variable `s` representa la *pendiente*, y podemos utilizarla para calcular `x3`. Una vez calculada `x3`, podemos calcular `y3`. `P3` puede ser obtenido mediante la siguiente fórmula:

```math
x3 = s^2 - x1 - x2

y3 = s(x1 - x3) - y1
```

Recuerde que `y3` es el reflejo del punto sobre el eje de las `x`.

## BOX: Derivando la Fórmula de Adición de Puntos

---

Supongamos lo siguiente:

#TODO: formula

Queremos conocer lo que vale `P3`.

Comencemos por el hecho de que la línea pasa a través de los puntos `P1` y `P2`, y tiene la siguiente fórmula:

#TODO: formula

La segunda fórmula es la ecuación de la línea que intersecta los puntos `P1` y `P2`. Utilizando esta fórmula, y combinándola con la ecuación de la curva elíptica, obtenemos lo siguiente:

#TODO: formula

Recolectando los términos, tenemos esta ecuación polinomial:

#TODO: formula

También, sabemos que `x1`, `x2` y `x3` son soluciones de esta ecuación, entonces:

#TODO: formula

De antes, conocíamos que:

#TODO: formula

Existe un resultado, proveniente de la fórmula de Vieta, que específica que los coeficientes deben ser iguales si la raíz es la misma. El primer coeficiente que intersecta es el coeficiente frente `x^2`:

#TODO: formula

Podemos utilizar esto para derivar la fórmula de `x3`:

#TODO: formula

Posteriormente, insertamos este resultado en la fórmula de arriba:

#TODO: formula

Pero tenemos que reflejar sobre el eje de las `x`, por lo que el lado derecho de la ecuación es negado:

#TODO: formula

Quedando así comprobado.

---

### Ejercicio 4

Para la curva `y^2 = x^3 + 5x + 7`, calcule la suma: `(2, 5) + (-1, -1)`.

## Codeando la Adición de Puntos cuando X1 es diferente a X2

Ahora, incluiremos el código en nuestra librería. Tenemos que ajustar el método de `__add__` para que sea capáz de manejar el caso en el que `x1` es diferente a `x2`. Tenemos las fórmulas:

#TODO: formula

Al final del método, regresaremos una instancia de la clase `Point`. Utilizando el método de `self.__class__` para heredar la clase con mayor facilidad.

### Ejercicio 5

Escriba el método de `__add__` dónde `x1` sea diferente de `x2`.

## Adición de Puntos cuando P1 = P2

Cuando las coordenadas de `x` son la mismas, y la coordenada de `y` diferente, nos encontramos con dos puntos opuestos sobre el eje de las `x`. Sabemos que esto significa:

Esto se había quedado resuelto previamente en el Ejercicio 3.

¿Qué pasaría cuando `P1 = P2`? Visualmente, tendríamos que calcular la línea tangente a la curva en `P1`, y encontrar el punto donde vuelve a intersectar la curva elíptica. La gráfica luce como en la Figura 2-18, que habíamos visto previamente.

*Figura 2-18. Línea tangente a la curva.*

Una vez más, encontramos la pendiente del punto:

#TODO: formula

El resto de la fórmula continúa como ya se habíamos hecho antes, excepto cuando `x1 = x2`, por lo que las combinamos:

#TODO: formula

## BIRD: Derivando la Pendiente Tangente a la Curva

Es posible derivar la pendiente de la línea tangente utilizando matemáticas un poco más avanzadas, Cálculo. Sabemos que la pendiente en cualquier punto es:

#TODO: formula

Para obtener el resultado, necesitamos derivar ambos lados de la ecuación de curva elíptica:

#TODO: formula

Realizando la derivada en ambos lados, obtenemos:

#TODO: formula

Resolviendo para `dy/dx`:

#TODO: formula

Y así es como llegamos a la fórmula de la pendiente. Esto nos ayudará para realizar la adición de puntos.

### Ejercicio 6

Para la curva `y^2 = x^3 + 5x + 7`, calcule: `(-1, -1) + (-1, -1)`.

## Codeando la Adición de Puntos cuando P1 = P2

Ajustamos el método de `__add__` para cubrir este caso en particular. Tenemos las fórmulas y ahora las implementaremos:

#TODO: formula

### Ejercicio 7

Escriba el método de `__add__` cuando `P1 = P2`.

## Codeando una Excepción Más

Existe una excepción adicional, y esto involucra el caso cuando la línea tangente es vertical, Figura 2-19.

#TODO: image

*Figura 2-19. Línea vertical y tangencial a la curva.*

Esto solo puede pasar si `P1 = P2` y la coordenada de `y` es `0`, por lo que el cálculo de la pendiente terminar involucrando al `0` en el denominador.

Para manejar este caso especial:

```python
class Point:
...
    def __add__(self, other):
        ...
        if self == other and self.y == 0 * self.x:   # <1>
            return self.__class__(None, None, self.a, self.b)
```

1. Si los dos puntos son iguales, y la coordenada de `y` es 0, regresamos el punto al infinito.

## Conclusión

Hemos cubierto lo que son las curvas elípticas, cómo funcionan y cómo realizar la adición de puntos. Ahora combinaremos estos conceptos para aprender la criptografía de la curva elíptica en el capítulo 3.

