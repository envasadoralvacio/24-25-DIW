---
hide:
  - navigation
---

# Prueba práctica 1: Fundamentos de Javascript

<center>
![](assets/prueba1.jpeg)
</center>


## Descripción

Las dos últimas unidades didácticas hemos estado viendo los fundamentos de javascript, es ese contenido el que tenemos que aplicar para resolver estos ejercicios:

* Tipos de datos
* Variables y ámbitos
* Operadores
* Arrays
* Funciones
* Estructuras de control
* Iteradores
* Clases y Objetos

No olvides que normalmente tendrás que colaborar con un equipo en tu futuro puesto de trabajo, por lo que comentar el código es fundamental (incluso cuando los ejercicios se resuelvan 5 líneas). 

Por último, ten en cuenta que no está permitido la búsqueda en Google (o cualquier otro motor de búsquedas), si podrás consultar la documentación que hemos utilizado en clase:

* [Free Code Camp](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)
* [https:/es.javascript.info](https://es.javascript.info/)
* [https://developer.mozilla.org](https://developer.mozilla.org/es/docs/Web/JavaScript)
* [https://lenguajejs.com/javascript/](https://lenguajejs.com/javascript/)

### Ejercicio 1

El cálculo de la letra del Documento Nacional de Identidad (DNI) es un proceso matemático sencillo que se basa en obtener el resto de la división entera del número de DNI y el número 23.
A partir del resto de la división, se obtiene la letra seleccionándola dentro de un array de letras:

~~~
['T', 'R', 'W', 'A', 'G', 'M', 'Y', 'F', 'P', 'D', 'X', 'B', 'N', 'J', 'Z', 'S', 'Q', 'V', 'H', 'L', 'C', 'K', 'E']
~~~

Por tanto si el resto de la división es 0, la letra del DNI es la T y si el resto es 3 la letra es la A.

Con estos datos, elaborar un pequeño script que:

* Pida al usuario un número de DNI.

* Almacene en una variable el número indicado por el usuario en tiempo de ejecución.

* Compruebe si es un número de DNI válido:
    * En el caso de no ser válido, mostrará un mensaje al usuario indicando que el número proporcionado no es válido y el script no muestra más mensajes.
    * Si el número es válido, se calcula la letra que le corresponde según el método explicado anteriormente, y una vez calculada la letra, mostrará por pantalla el DNI completo con la letra.

### Ejercicio 2

Cómo todos sabemos los números pares son aquellos que son múltiplos de dos, por ejemplo, 2, 4, 6, 8, 10, 12, …  Implementar una función “findPairs()” con los datos que se proporcionan a continuación: 

* Debe aceptar como argumento dos números entre 1 y 100.
* Debe realizar las siguientes validaciones de la entrada:
  * Si alguno de los argumentos que se pasan no es un número, muestra un mensaje al usuario indicando que el número proporcionado no es válido y el script no muestra más mensajes.
  * Si recibe un número decimal, debe pasarlo a entero. Es decir, ante la entrada (2.4, 10), debe de considerarla (2,10).
  * Debe comprobar si los argumentos recibidos son menor que 1 o mayor que 100. Si ese es el caso, se muestra un mensaje al usuario indicando que el número proporcionado no es válido y se corta la ejecución.
* Si los argumentos que recibe son válidos, se calcularán los números pares que hay entre los dos números pasados a la función y se introducirán en un array.
* Una vez calculados debe mostrar por pantalla todos los números pares separados por coma.

### Ejercicio 3

La empresa Alberti’s Enternainment S.L. está trabajando en un juego de zombies. Necesita generar una estructura de datos para representar a los jugadores. El juego aún está en versión alfa, por lo que no tenemos que preocuparnos por el encapsulamiento de la información, todo lo contrario, cuanto más simple sea esta estructura, mejor.

La estructura de datos en esta fase del juego será la de un objeto, en el que se necesitará de cada jugador las siguientes propiedades:

  * Nombre
  * Apellidos
  * Nivel (comenzará en 1)
  * Puntuación (comenzará en 1)

Además, para hacer pruebas se almacenarán los jugadores en un array. Inicialmente habrá 2 jugadores precargados en el sistema.

Se deberá crear un control de puntuación. Para ello se desarrollarán los métodos correspondientes para:

  * sumarPuntuacion => Cada vez que se elimine a un zombie, se sumará 1 a la puntuación y cada 10 puntos conseguidos subirá de nivel.
  * restarPuntuacion => Cada vez que un zombie alcance al jugador, restará 1 a la puntuación y cada 10 puntos perdidos, bajará de nivel. Si la puntuación llega a 0, reseteará al jugador.
  * resetearPuntuacion => Cada vez que un jugador sea eliminado de la partida, mostrará una alerta indicando qué jugador se ha reseteado.


### Ejercicio 4

Indica, para cada ejercicio, las herramientas que has utilizado para cada caso:

* Escribir el código
* Probar el código
* Depurar el código


## Entrega

En Moodle Centros, abre la tarea llamada “Prueba 1 y entrega sus soluciones con la siguiente estructura y nombres:

* Ejercicio 1
e1-Apellido1Apellido2-Nombre.js
e1-Apellido1Apellido2-Nombre.html

* Ejercicio 2
e2-Apellido1Apellido2-Nombre.js
e2-Apellido1Apellido2-Nombre.html

* Ejercicio 3
e3-Apellido1Apellido2-Nombre.js
e3-Apellido1Apellido2-Nombre.html

* Ejercicio 4
e4-Apellido1Apellido2-Nombre.txt

NOTA: No comprimas las soluciones, la entrega permite adjuntar directamente tus ejercicios.

## Importante

* Si los ejercicios no se ejecutan, no se procederá a la corrección de los mismos.
* Si los ejercicios no siguen la estructura y/o nombres indicados, no se procederá a la corrección de los mismos.
* Si se detecta plagio o copia entre compañeros/-as, la calificación será de 0 puntos en todas las partes de las que se compone el trabajo para ambos/-as.
* Igualmente, si se detecta plagio o copia de internet, la calificación será de 0 puntos en todas las partes de la que se compone el trabajo.
* Si alguna de las partes es entregada fuera de plazo no se procederá a la corrección de la misma.


## Evaluación de la prueba

Mediante esta prueba se evalúan los siguientes criterios de evaluación:

### Criterio 2a) Selección del lenguaje de programación

| **Indicador** | **Excelente** | **Aceptable** | **Insatisfactorio** |
| :------------: | :-----------: | :-----------: | :-----------------: |
| **Selección del Lenguaje (0.5pt)** | Se ha seleccionado un lenguaje de programación de clientes Web en función de sus posibilidades y se ha justificado la elección (0.5pt) | Se ha seleccionado un lenguaje de programación apropiado, pero la justificación es parcial (0.25pt) | No se ha seleccionado un lenguaje de programación adecuado o no se ha proporcionado justificación (0pt) |

### Criterio 2b) Uso de variables y operadores

| **Indicador** | **Excelente** | **Aceptable** | **Insatisfactorio** |
| :------------: | :-----------: | :-----------: | :-----------------: |
| **Uso de Variables y Operadores (1pt)** | Se ha utilizado adecuadamente variables y operadores, demostrando un sólido entendimiento (1pt) | Se ha utilizado variables y operadores de manera correcta, pero podría mejorar su uso (0.5pt) | Se han cometido errores significativos en el uso de variables y operadores (0pt) |

### Criterio 2c) Identificación de ámbitos de variables

| **Indicador** | **Excelente** | **Aceptable** | **Insatisfactorio** |
| :------------: | :-----------: | :-----------: | :-----------------: |
| **Identificación de Ámbitos (2pt)** | Se ha identificado y explicado de manera clara los ámbitos de variables (2pt) | Se ha identificado ámbitos de variables, pero la explicación es parcial o confusa (1pt) | No se ha identificado adecuadamente los ámbitos de las variables (0pt) |

### Criterio 2d) Conversiones entre tipos de datos

| **Indicador** | **Excelente** | **Aceptable** | **Insatisfactorio** |
| :------------: | :-----------: | :-----------: | :-----------------: |
| **Conversiones entre Tipos de Datos (2pt)** | Se ha identificado y explicado las conversiones entre tipos de datos de manera precisa (2pt) | Se ha identificado conversiones entre tipos de datos, pero la explicación es parcial o poco clara (1pt) | No se ha identificado adecuadamente las conversiones entre tipos de datos (0pt) |

### Criterio 2e) Comentarios en el código

| **Indicador** | **Excelente** | **Aceptable** | **Insatisfactorio** |
| :------------: | :-----------: | :-----------: | :-----------------: |
| **Uso de Comentarios (2pt)** | Se han agregado comentarios de manera efectiva y coherente al código (2pt) | Se han utilizado comentarios, pero podrían ser más detallados o coherentes (1pt) | Se han utilizado pocos o ningún comentario en el código (0pt) |

### Criterio 2f) Mecanismos de decisión

| **Indicador** | **Excelente** | **Aceptable** | **Insatisfactorio** |
| :------------: | :-----------: | :-----------: | :-----------------: |
| **Uso de Mecanismos de Decisión (1pt)** | Se ha aplicado mecanismos de decisión de manera efectiva y lógica en el código (1pt) | Se ha utilizado mecanismos de decisión, pero podría mejorar su lógica (0.5pt) | No se ha aplicado adecuadamente mecanismos de decisión (0pt) |

### Criterio 2g) Uso de bucles

| **Indicador** | **Excelente** | **Aceptable** | **Insatisfactorio** |
| :------------: | :-----------: | :-----------: | :-----------------: |
| **Uso de Bucles (2pt)** | Se ha utilizado bucles de manera efectiva y lógica en el código (2pt) | Se ha utilizado bucles, pero podría mejorar su lógica (1pt) | No se ha aplicado adecuadamente bucles (0pt) |

### Criterio 2h) Uso de herramientas y entornos

| **Indicador** | **Excelente** | **Aceptable** | **Insatisfactorio** |
| :------------: | :-----------: | :-----------: | :-----------------: |
| **Uso de Herramientas y Entornos (0.5pt)** | Se ha utilizado herramientas y entornos de programación de manera efectiva y adecuada (0.5pt) | Se ha utilizado herramientas y entornos, pero podría mejorar su uso (0.25pt) | No se ha utilizado adecuadamente herramientas y entornos (0pt) |
