# Evidencias de la unidad 6
_______________________________________________________________________________________________________________________________________________________________________________
# Set y Seek
## Actividad 1
En esta actividad propondré que te encuentres de nuevo el trabajo de [Tyler Hobbs](https://youtu.be/8tTGJvijoDw?si=7KWuEhMTIjH41JOj) y específicamente que mires [su artículo sobre campos de flujo](https://www.tylerxhobbs.com/words/flow-fields).
- Captura en tu bitácora dos imágenes de Tyler Hobbs que te llamen la atención y explica por qué.
####
...
####
- ¿Qué te inspira de su trabajo?
####
...
####

## Actividad 2
En esta actividad quiero que investigues alrededor de estas dos preguntas:
####
1. ¿Qué es una fuerza de dirección (steering force)?
####
...
####
2. ¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?
####
...
####
3. ¿Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?
####
...

## Actividad 3
Vamos a analizar el primer algoritmo clave: los campos de flujo (Flow Fields), basándonos en el ejemplo del libro “The Nature of Code”. Entenderemos cómo una cuadrícula de vectores dirige el movimiento de los agentes.
- Libro “The Nature of Code” (TNoC) de Daniel Shiffman: [Capítulo 5, sección “Flow Fields”](https://natureofcode.com/autonomous-agents/#flow-fields) (y ejemplos de código asociados).
- El código fuente del ejemplo de Flow Fields de TNoC.
####
Ver los [pasos](https://juanferfranco.github.io/simulacion-2025-20/units/unit6/).
####
1. Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores.
####
...
####
2. Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.
####
...
####
3. Lista los parámetros clave identificados (resolución, maxspeed, maxforce).
####
...
####
4. Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.
####
...

## Actividad 4
Ahora analizaremos el segundo algoritmo: el comportamiento de enjambre (Flocking), famoso por simular el movimiento coordinado de pájaros o peces. Nos basaremos nuevamente en “The Nature of Code” para entender las tres reglas básicas que lo gobiernan.
- Libro “The Nature of Code” (TNoC) de Daniel Shiffman: [capítulo 5, sección “Flocking”](https://natureofcode.com/autonomous-agents/#flocking) (y ejemplos de código asociados).
- El código fuente del ejemplo de Flocking de TNoC.
####
Ver los [pasos](https://juanferfranco.github.io/simulacion-2025-20/units/unit6/).
####
1. Explica con tus palabras el objetivo y la lógica general de cálculo de cada una de las tres reglas de Flocking (Separación, Alineación, Cohesión).
####
...
####
Lista los parámetros clave identificados (radio de percepción, pesos de las reglas, maxspeed, maxforce).
####
...
####
Describe la modificación que realizaste al código y *explica detalladamente* el efecto que tuvo en el comportamiento colectivo del enjambre (¿Se dispersan? ¿Forman grupos compactos? ¿se mueven caóticamente?). Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.
####
...

# Apply
## Actividad 5
Ahora que entiendes los algoritmos, es tu turno de crear. Aplicarás **uno** de ellos (flow fields o flocking) para generar una pieza de arte interactivo que permita visualizar un tema musical de tu elección. La interacción del usuario debe influir en el comportamiento de los agentes.
####
**Condiciones**
- Elige un tema musical que te inspire.
- Diseña una pieza de arte generativo que utilice el algoritmo de flow fields y/o flocking.
- Vas a visualizar el tema musical y además vas a “tocar” las visuales, es decir, tu pieza de arte debe ser interactiva y debe permitir la interpretación en tiempo real de las visuales como si fuera un instrumento más que acompaña de manera coherente el tema musical.
####
1. Documenta todo el proceso de diseño y creación en tu bitácora, incluyendo bocetos y decisiones de diseño.
####
...
####
2. El código fuente completo de tu sketch en p5.js.
``` js

```
3. Un enlace a tu sketch en el editor de p5.js.
####
link
####
Capturas de pantalla mostrando tu pieza en acción.
####

# Autoevaluación
**Nota:** -
####
...
