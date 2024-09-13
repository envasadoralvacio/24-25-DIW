---
hide:
  - navigation
---

# Proyecto 4: Maquetación y Desarrollo de una aplicación web

<center>
![](assets/proyecto4.png)
</center>


##

> *“La práctica no es lo que un@ hace cuando es buen@. Es lo que un@ hace para volverse buen@.”*.


## Fundamentación teórica

El uso de las API REST en programación web se ha vuelto muy popular en los últimos años, debido, entre otras cosas,  a que es una tecnología fácil de usar y de entender. 

### ¿Qué es una API?
Las API son mecanismos que permiten a dos componentes de software comunicarse entre sí. Por ejemplo, la aplicación del instituto de meteorología contiene datos meteorológicos, y la aplicación meteorológica de nuestro teléfono “habla” con este sistema a través de las API y nos muestra las actualizaciones meteorológicas diarias en nuestro teléfono.

### ¿Cómo funcionan las API?
La arquitectura de las API suele explicarse en términos de cliente y servidor. La aplicación que envía la solicitud se llama cliente, y la que envía la respuesta se llama servidor. En el ejemplo del tiempo, la base de datos meteorológica del instituto es el servidor y la aplicación móvil es el cliente. 

Las API pueden funcionar de cuatro maneras diferentes, según el momento y las tecnologías usadas:

* API SOAP
* API RPC
* API WebSocket
* API REST

Nosotros nos centraremos en las API REST, ya que son, actualmente, las más populares en la programación web. 

### API REST
REST significa transferencia de estado representacional. REST define un conjunto de funciones como GET, PUT, DELETE, etc. que los clientes pueden utilizar para acceder a los datos del servidor. Los clientes y los servidores intercambian datos mediante HTTP.
La principal característica de la API de REST es que no tiene estado, es decir, los servidores no guardan los datos del cliente entre las solicitudes. 
Las solicitudes de los clientes al servidor son similares a las URL que se escriben en el navegador para visitar un sitio web. La respuesta del servidor son datos simples, sin la típica representación gráfica de una página web.

### ¿Qué beneficios ofrecen las API REST?

Las API REST ofrecen cuatro beneficios principales:
Integración 
No hay que escribir cada funcionalidad desde cero, podemos utilizar las API para aprovechar funcionalidades ya implementadas.

* Innovación 
Sectores enteros pueden cambiar con la llegada de una nueva aplicación. Las empresas deben responder con rapidez y respaldar la rápida implementación de servicios innovadores. Para ello, pueden hacer cambios en la API sin tener que reescribir todo el código.

* Ampliación
Las API presentan una oportunidad única para que las empresas satisfagan las necesidades de sus clientes en diferentes plataformas. Por ejemplo, la API de mapas permite la integración de información de los mapas en sitios web, Android, iOS, etc. Cualquier empresa puede dar un acceso similar a sus bases de datos internas mediante el uso de API gratuitas o de pago.

* Facilidad de mantenimiento
La API actúa como una puerta de enlace entre dos sistemas, cualquier cambio futuro que haga una de las partes en el código no afectará a la otra.

### ¿Qué es un punto de conexión o end-points de la API y por qué es importante?
Los puntos de conexión de las API son los últimos puntos de contacto del sistema de comunicación de las API. Se trata de las URL de servidores y servicios desde las que se envía y recibe información entre sistemas. 

## Objetivos del proyecto

Este proyecto tiene por objetivo evaluar el grado de asimilación de los diferentes mecanismos para la gestión de eventos y validación de formularios a través de JavaScript, además de desarrollar una aplicación web asíncrona que consuma una API Rest y renderice contenido de la misma.


## Descripción del proyecto

Este proyecto forma parte de un proyecto conjunto con el módulo profesional Diseño de Interfaces Web (DIW), en concreto con el desarrollado en la  UT4, Maquetación y desarrollo de la aplicación web (API). 

Para el desarrollo de esta parte del proyecto, se deberá elegir una API Rest pública. En [éste enlace](https://github.com/public-api-lists/public-api-lists) emncontraréis una relación de APIs públicas, aunque no tiene por qué ser necesariamente de esta fuente. 

Anotaciones:

* El alumnado que esté cursando el módulo de DIW ya habrá elegido la API para realizar la maquetación, no sería necesario, por tanto, elegir una nuevamente.
* El alumnado que no curse el módulo de DIW, no tendrá que realizar la parte de diseño y maquetación del proyecto.


### Secciones obligatorias en la aplicación
La aplicación deberá de contar, como mínimo,  con las siguientes secciones:

#### 1. Login/Registro
La aplicación dispondrá de una página de Login y otra de Registro, recogiendo y validando todos los datos necesarios para realizar estas acciones

#### 2. Página de perfil del usuario
La aplicación debe contar con un apartado o sección, perfil de usuario, que almacene información del mismo, por ejemplo, favoritos, foto de perfil, etc…

#### 3. Página de búsqueda
La aplicación contará con una página en la cual se realizará una llamada a una API externa y renderizar el contenido de las respuestas de la misma. Siendo necesario contar con, mínimo, una llamada anidada. Por ejemplo, si nuestra aplicación es de un juego, posiblemente en nuestra primera llamada a la API estamos renderizando los personajes del juego (llamada de primer nivel). Después el usuario podrá elegir un personaje en concreto y realizará una segunda llamada con dicho personaje (llamada anidada). Las respuestas de la API deben mostrarse correctamente paginadas.
Esta página contará con un filtro de búsqueda con opciones múltiples, es decir, permitirá al usuario filtrar por, al menos, 4 o 5 campos.

#### 4. Página de contacto

Nota: Se valorará positivamente cualquier otra sección que se haya desarrollado.


### Otras funcionalidades a implementar:
En el desarrollo de la aplicación se deberán tener en cuenta los siguientes aspectos y/o funcionalidades:

* Se debe de hacer uso del almacenamiento del navegador, por ejemplo para almacenar favoritos, o cualquier otra funcionalidad que estiméis pertinente.
* En los formularios de la aplicación se  deberá hacer uso de variedad de inputs ( 5 tipos diferentes como mínimo), por ejemplo, fecha, texto, número, checkbox, email, radio button, range, etc. Todos los campos deberán ser validados desde el front end.
* Con el objetivo de enriquecer la experiencia del usuario, haremos uso de diversos eventos,  siendo los siguientes los mínimos a usar:
    * Un evento que sea dependiente del tiempo
    * Eventos para la validación de los formularios
    * Un evento que se active en una determinada área de la pantalla
    * Un evento que sea dependiente de los datos que aporte el usuario, por ejemplo, si el usuario selecciona una determinada acción, desencadenará unas funcionalidades, sin embargo seleccionando otra acción, desencadenará otras funcionalidades distintas.
* La aplicación deberá ser desplegada. Será elección del alumnado donde hacerlo, por ejemplo,  GitHub Pages o Netlify, o cualquier otro sitio donde se estime pertinente. Si la opción elegida difiere de las dos indicadas en este enunciado, se deberá justificar la elección hecha.


## Formato y entrega

- La entrega se realizará mediante el aula virtual.
- A través del enlace habilitado, sube un enlace al repositorio de Github del proyecto.
- Asegúrate de que el repositorio sea **público** o de que **me invites como colaborador** para que pueda acceder y evaluar tu trabajo.
- El readme del repo deberá contener un enlace al despliegue de la aplicación.

## Evaluación del proyecto
Mediante este proyecto se evalúan dos bloques de contenido del módulo, los cuales se corresponden con los siguientes RA y ce:

* Se evaluarán los criterios de evaluación del RA5, en la que se centra la gestión de eventos y validación de formularios.
* Se evaluarán los criterios de evaluación del RA6, en la que se centra la comunicación asíncrona para nutrir de contenido la aplicación.

### Indicaciones generales
* Si se detecta plagio o copia entre compañeros, la calificación será de 0 puntos en todas las partes de las que se compone el trabajo **para ambos**.
* Igualmente, si se detecta plagio o copia de internet, la calificación será de 0 puntos en todas las partes de la que se compone el trabajo.
* Si alguna de las partes es entregada fuera de plazo, no se evaluará.
* **Para la calificación del proyecto se usarán las guías de evaluación que se muestran a continuación**. Aparte de los indicadores recogidos en dichas guías, **en cada una de ellas se tendrá en cuenta que el código haya sido escrito haciendo uso de una sintaxis moderna (Javascript ES6 o superior)**. Entre otros indicadores, los siguientes:
  * **Uso adecuado de las variables y constantes**, así como el ámbito de las mismas.
  * Uso de **funciones de flecha**, en las situaciones apropiadas.
  * **Desestructuración de objetos**.
  * Manipulación del DOM,  selectores usados.
  * Uso apropiado de los **métodos actuales para el manejo de los array**.
  * Emplear **buenas prácticas de programación**, por ejemplo, utilizar notación camelCase, indentación correcta, nombres de funciones y variables representativas, etc.


### Rúbricas de evaluación

### Rúbrica para la Gestión de Eventos y Validación de Formularios (RA5-CEa)

| Criterios/Logro              | Excelente (2.5 pt) | Bien (1.9 pt) | Suficiente (1.25 pt) | Insuficiente (0.6 pt) | Muy Deficiente (0 pt) |
|------------------------------|--------------------|---------------|----------------------|-----------------------|-----------------------|
| **Gestión de Eventos**       | Implementación completa de eventos avanzados con interactividad y retroalimentación óptimas. | Buena implementación de eventos que facilitan la interacción efectiva del usuario. | Implementación básica de eventos con funcionalidad limitada. | Eventos implementados pero con interactividad y retroalimentación insuficientes. | Ausencia de gestión de eventos o implementación no funcional. |
| **Validación de Formularios** | Validaciones avanzadas en todos los campos con retroalimentación instantánea y clara. | Validaciones correctas en la mayoría de los campos con retroalimentación adecuada. | Validaciones básicas presentes con retroalimentación limitada. | Validaciones insuficientes o ausentes en varios campos. | No se realizan validaciones o son completamente inadecuadas. |
| **Uso de JavaScript ES6+**   | Uso excepcional de características modernas de JavaScript (ES6+) para eventos y validaciones. | Buen uso de características modernas de JavaScript, aunque con margen de mejora. | Uso adecuado de algunas características modernas de JavaScript. | Uso limitado y poco efectivo de características modernas de JavaScript. | Ausencia o uso inadecuado de JavaScript moderno. |
| **Interacción UX**           | Experiencia de usuario excepcionalmente fluida e intuitiva al interactuar con eventos y formularios. | Experiencia de usuario positiva con pequeñas áreas de mejora. | Experiencia de usuario adecuada, aunque con cierta confusión en la interacción. | La interacción del usuario es a menudo confusa o frustrante. | La experiencia del usuario es extremadamente pobre o no intuitiva. |

### Rúbrica para la Comunicación Asíncrona (RA6-CEa)

| Criterios/Logro               | Excelente (2.5 pt) | Bien (1.9 pt) | Suficiente (1.25 pt) | Insuficiente (0.6 pt) | Muy Deficiente (0 pt) |
|-------------------------------|--------------------|---------------|----------------------|-----------------------|-----------------------|
| **Comunicación Asíncrona**    | Implementación perfecta de llamadas asíncronas con manejo avanzado de promesas/async-await. | Buena implementación de comunicación asíncrona con algunos retrasos o errores manejables. | Comunicación asíncrona básica, funcional pero con margen de mejora. | Problemas frecuentes en la comunicación asíncrona que afectan la funcionalidad. | Fallos críticos o ausencia de comunicación asíncrona. |
| **Uso de API REST**           | Uso excepcional de la API REST, incluyendo múltiples endpoints y manejo avanzado de respuestas. | Buen uso de la API con integración efectiva, aunque con espacio para una mayor exploración. | Uso adecuado de la API con integración funcional de endpoints básicos. | Uso limitado de la API, con integración insuficiente y manejo pobre de respuestas. | Integración fallida o ausente de la API REST. |
| **Manejo de Datos**           | Manejo de datos avanzado, incluyendo transformación, filtrado y presentación óptima. | Manejo de datos competente con algunas áreas de mejora en la presentación. | Manejo de datos básico, cumpliendo con los requisitos mínimos de funcionalidad. | Manejo de datos deficiente, con problemas en la transformación o presentación. | Ausencia o manejo muy deficiente de datos. |
| **Rendimiento de la Aplicación** | Rendimiento óptimo en todas las operaciones asíncronas. | Rendimiento generalmente bueno con algunos momentos de lentitud. | Rendimiento aceptable con notables áreas de mejora. | Rendimiento insuficiente que afecta la experiencia de usuario. | Rendimiento muy deficiente que compromete la funcionalidad de la aplicación. |


## Nota: Propiedad Intelectual

> > A menudo es inevitable, al escribir un documento, hacer uso de recursos creados por terceras personas. Es por tanto comprensible hacerlo en el marco de una práctica de nuestros estudios, siempre que esto se documente claramente y no suponga plagio en la práctica.

> > Por lo tanto, al presentar una práctica que haga uso de recursos ajenos, se deberá referenciar claramente estos recursos, especificando el nombre de cada recurso, su autor, el lugar donde se obtuvo y el su estatus legal: si la obra está protegida por copyright o se acoge a alguna otra licencia de uso (Creative Commons, licencia GNU, GPL ...). El estudiante deberá asegurarse de que la licencia que sea no impide específicamente su uso en el marco de la práctica. En caso de no encontrar la información correspondiente deberá asumir que la obra está protegida por copyright.