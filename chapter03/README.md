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