# Unidad 1
`🤔 Fase: Reflect`
_________________________________________________________________________________________________________________________________________________________________________________________
## Actividad 09
### Autoevaluación
### Parte 1: recuperación de conocimiento (Retrieval Practice)
1. Describe la diferencia fundamental entre la aleatoriedad generada por `random()` y la apariencia de aleatoriedad del Ruido Perlin (`noise()`). ¿En qué tipo de situación usarías cada una?
###
La aleatoriedad generada con `random()` se utiliza de dos formas: de 0 a un número, o dentro de un rango de dos números, con la característica fundamental que en cualquiera de estos casos todos los números cuentan con la misma probabilidad de ser elegidos; y que, además, dependiendo del rango pueden haber "saltos" de un número a otro muy pequeños (con un `random(4)` por ejemplo) o muy grandes (con un `random(10, 160)` por ejemplo).
###
Por otro lado, el `noise()` siempre eligirá números de entre 0 y 1, osea, los saltos siempre serán pequeños, sin importar cómo se defina la variable dentro del (); y los números como se explica, no pueden personalizarse a rangos diferentes, haciendo que se genere un desplazamiento mucho menos drástico. Siendo la única forma de hacer este movimiento más rápido multiplicando la función.
###
Para el `random()`, lo utilizaría en situaciones que busque resultados variables y mucho menos predecibles. Para el `noise()`, lo usaría en situaciones que requieran bastante predictividad o busque algo más lineal con lo que trabajar.
###
2. Explica con tus palabras qué es una *distribución de probabilidad*. ¿Qué diferencia visual produce una caminata aleatoria con una *distribución uniforme* versus una con una *distribución normal*?
###
Una distribución de probabilidad se puede definir como el reparto de operaciones, números o cantidad de elementos que pueden definir la probabilidad en como se vaya a distribuir una variable. En una caminata aleatoria con distribución uniforme, los datos tienen la misma posibilidad de ser seleccionados cada uno. Es decir, hay la misma probabilidad que por ejemplo, se mueva a la derecha, o a la izquierda. En cambio, en una caminata aleatoria con distribución normal, cada dato tiene su propia probabilidad individual, osea, las probabilidades de cada uno de estos es diferente a al menos, uno de los demás datos. Esto se puede ver al usar el Lévy flight, que necesita una probabilidad menor a las demás de caminata.
###
3. ¿Cuál es el papel de la aleatoriedad en el arte generativo? Menciona al menos dos funciones distintas que cumple.
###
Permitir que el producto sea de cierta forma, impredesible, pues se busca que cada que se genere el programa haya algo que lo haga diferente de una generación anterior, y en consecuencia sea único.
###
Que este de rienda suelta al artista con solo unas variables no totalmente aleatorias, pues, el resto del programa al depender de la aleatoriedad, da al artista diferentes interpretaciones para interactuar con este y modificarlo a su gusto.
###
4. Piensa en tu obra final (Actividad 08). Describe uno de los conceptos de aleatoriedad que usaste y explica por qué fue una elección adecuada para lograr el efecto que buscabas.
###
El cambio de color cada vez que hiciera un salto, siendo este cambio dentro de un rango definido en cada componente del RGB. Esto lo hice para que cada trazo, a parte de empezar de una posición diferente, se sintiera diferente y diera paso algo visualmente más interesante incluso si solo se deja el programa correr por si solo. Además, recuerda a los trazos usados en muchas pinturas con colores diferentes en partes varias del lienzo, y al no saber que tan prolongado será el trazo, se da un efecto agradable dentro de lo aleatorio.
###
###
5. ¿Qué es un “paseo” o “caminata” (walk) en el contexto de la simulación? ¿Qué característica particular tiene una caminata de tipo “Lévy flight”?
###
En este contexto, es cambiar la posición en donde se dibujará el siguiente `walker`, haciendo la ilusión de que este se mueve o camina a cierta dirección. Sin embargo, en el Lévy flight, esta posición se toma como un punto aleatorio desde la última posición en la que se dibujó el walker, haciendo que este se mueva de forma drástica a una posición, generalmente, mucho más alejada. Esto se usa principalmente para ayudar al walker a no recorrer una misma zona repetidamente, y explore más areas del lienzo más rápido.

### Parte 2: reflexión sobre tu proceso (Metacognición)

1. ¿Cuál fue el concepto más abstracto o difícil de “visualizar” para ti en esta unidad? ¿Qué hiciste para finalmente comprenderlo?
###
...
###
2. Describe un momento durante el desarrollo de tu obra final (Actividad 08) en el que un “error” o un resultado inesperado te llevó a una idea creativa interesante.
###
...
###
3. Al combinar diferentes técnicas de aleatoriedad, ¿Cuál fue el mayor desafío? ¿La interacción entre los sistemas, el control de los parámetros, el rendimiento?
###
...
###
4. Si tuvieras que empezar la Actividad 08 de nuevo, ¿Qué harías de manera diferente basándote en lo que sabes ahora?
###
...


