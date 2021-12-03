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

#TODO: code

1. Aquí nos aseguramos que el punto se encuentre dentro de la curva.
2. Los puntos son iguales sí, y sólo sí, están en la misma curva y tienen las mismas coordenadas.

Podemos ahora crear objetos de tipo `Point`, y obtendremos un error si el punto se encuentra afuera de la curva.

#TODO: code

En otras palabras, el método de `__init__` va a levantar a una excepción cuando el punto no se encuentre dentro de la curva.

### Ejercicio 1.

Determine cuál de los siguientes puntos se encuentran dentro de la curva: `y^2 = x^3 + 5x + 7`.

### Ejercicio 2.

Escriba el método de `__ne__` para el objeto tipo `Point`.

## Adición de Puntos

Las curvas elípticas son útiles debido a a una propiedad conocida como la adición de puntos. La adición de puntos es cuando se realiza una operación con dos de los puntos, dentro de la misma curva, y se genera un tercer punto, que también está dentro de la curva. A esto se le conoce como adición porque la operación tiene mucho que ver con la intuición asociada a la operación de adición en las matemáticas. La adición de puntos es conmutativa. Esto quiere decir, que sumar el punto `A` con el punto `B`, es lo mismo que sumar el punto `B` con el punto `A`.





