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

Figura 3-1. *secp256k1* sobre el conjunto de números reales.

Resulta que podemos utilizar la ecuación de adición de puntos sobre cualquier campo, incluyendo el campo finito que aprendimos en el Capítulo 1. La única diferencia es que tenemos que usar utilizar las operaciones de adición/sustracción/multiplicación/división definidas en el Capítulo 1, no las operaciones normales que utilizan los números reales.

## Curvas Elípticas sobre Campos Finitos

¿Cómo luce una curva elíptica sobre un campo finito? Veamos la ecuación `y^2 = x^3 + 7` sobre `F103`. Podemos verificar que el punto `(17, 64)` se encuentra dentro de la curva, mediante el cálculo de ambos lados de la ecuación:

```text
y^2 = 64^2 % 103 = 79

x^3 + 7 = (17^3 + 7) % 103 = 79
```

Hemos verificado que este punto se encuentra dentro de la curva utilizando matemáticas de campo finito.
