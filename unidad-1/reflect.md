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
El uso de las coordenadas al hacer la caminata, pues, no tengo ninguna referencia visual de cómo funcionan las caminatas dentro del lienzo de p5.js. Esto no lo entendía hasta que el docente me explicó cómo se distribuyen, para hacer uso correcto de todo el lienzo cuando haga caminatas con el walker.
###
2. Describe un momento durante el desarrollo de tu obra final (Actividad 08) en el que un “error” o un resultado inesperado te llevó a una idea creativa interesante.
###
Tuve siempre la idea de que mientras hiciese el tono de un trazo, este puediera variar en tono por si solo, como un espectro de rojos por ejemplo. Sin embargo, no lograba implementar esto por más que lo intentase, por lo que se me ocurrió una alternativa. Hice que esto se puediera hacer por desición propia del usuario. Icialmente con lo que planté antes, haciendo que el color cambie tenuemente entre un rango cada que se presionase una tecla, pero luego también agregue que los colores se vuelvan o más claros, o más oscura con otro par de teclas.
###
3. Al combinar diferentes técnicas de aleatoriedad, ¿Cuál fue el mayor desafío? ¿La interacción entre los sistemas, el control de los parámetros, el rendimiento?
###
La interacción entre los sistemas, pues quería que muchas cosas se pudiesen hacer con variables internas de las funciones, obligándome a definir en varias de estas variables globales que cambiasen según ciertas condiciones. Esto lo hice con la dirección, color y forma del trazo.
###
4. Si tuvieras que empezar la Actividad 08 de nuevo, ¿Qué harías de manera diferente basándote en lo que sabes ahora?
###
Tal vez probaría más uso de las diferentes formas que se pueden dibujar, y hacer que el movimiento sea menos predecible cuando realiza el noise().

## Actividad 10
### Coevaluación
1. Encuentra un compañero de trabajo.
###
Esteban Puerta
###
2. Intercambien las URLs de sus bitácoras de aprendizaje.
###
URL de mi compañero: https://github.com/jfUPB/simulacion-2025-20-LS1653
###
3. Dirígete a la entrada de la Actividad 08: creación de obra generativa de tu compañero. Lee su concepto, interactúa con su sketch y analiza su código.
###
4. Basándote en la rúbrica para la actividad 08 evalúa el trabajo del compañero y escribe un comentario de retroalimentación constructiva. Esto lo harás en tu bitácora de aprendizaje.
###
Todo esta bien, diría que el único defecto notable es el rendimiento (el programa se congela algunas veces), y que tal vez el cambio de formas en el trazo es poco notable, pero la aplicación de conceptos, aleatoriedad y la interactividad están muy bien implementadas e interesantes.
###
5. Conversa con tu compañero sobre su obra y tu feedback. Escucha sus reflexiones y comparte tus propias ideas.
-b

## Actividad 11
### Feedback
Responde a las siguientes preguntas con total sinceridad. No hay respuestas correctas o incorrectas.
###
1. *Continuar*: ¿Qué actividad, ejemplo o explicación de esta unidad te resultó más reveladora o útil para tu aprendizaje? ¿Qué deberíamos mantener sí o sí?
###
El uso de condicionales para definir ciertos eventos dentro del trabajo generativo.
###
2. *Dejar de hacer*: ¿Hubo alguna actividad o concepto que te pareció redundante, confuso o menos útil? ¿Hay algo que eliminarías o cambiarías por completo?
###
Distribuciones usando `randomGaussian()`, me pareció poco interesante y práctico a la hora de aplicarlo.
###
3. *Empezar a hacer*: ¿Qué te faltó en esta unidad? ¿Quizás más ejemplos de artistas, más desafíos técnicos, más teoría? ¿Qué idea tienes para hacerla mejor?
###
Más teoría o ejemplos más claros, algunas veces es difícil entender el lenguaje o el funcionamiento de ciertos temas a primera vista.
###
4. *Balance Teoría-Práctica*: ¿Cómo sentiste el equilibrio entre analizar los ejemplos del texto guía y ponerte a programar tus propios sketches? Explica tu respuesta.
###
Extraño, algunos los comprendí rápido, pero la mayoría me dejaban con más dudas que respuestas al intentar aplicarlos conmigo mismo, y luego compararlos con los ejemplos.
###
5. *Comentario Adicional*: ¿Hay algo más que quieras compartir sobre tu experiencia en esta unidad?
###
Me gustó el resultado final que pude alcanzar, a pesar de mis dificultades para comprender el código en ciertas secciones.

