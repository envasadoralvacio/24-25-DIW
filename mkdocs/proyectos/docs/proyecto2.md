---
hide:
  - navigation
---

# Proyecto 2: Implementación de Funcionalidades con JavaScript

<center>
![](assets/proyecto2.png)
</center>

## Fundamentación Teórica

Este proyecto simula un escenario común en la vida de un desarrollador web: la necesidad de implementar funcionalidades desconocidas bajo demanda. En el mundo real, los desarrolladores a menudo enfrentan el desafío de aprender y aplicar nuevas tecnologías rápidamente para satisfacer las necesidades de sus clientes. Este proyecto pone a prueba la capacidad de los estudiantes para investigar, aprender y aplicar de forma efectiva una tecnología no familiar, en este caso, IndexedDB, un componente interesante en el desarrollo de aplicaciones web actuales.


## Objetivos del Proyecto
- Implementar la validación de formularios utilizando eventos JavaScript.
- Facilitar los procedimientos de validación mediante expresiones regulares.
- Utilizar las capacidades de almacenamiento del navegador para guardar y recuperar información.
- Probar y documentar adecuadamente el código desarrollado.

## Descripción del Proyecto
La tarea asignada por tu jefe de proyecto implica implementar diversas funcionalidades en un CRM (Customer Relationship Management), donde se proporciona la [estructura básica (HTML y CSS)](https://github.com/0xmrivas/materiales-complementarios-DWEC/blob/main/Javascript/25-PROYECTO-CRM-CRUD-REST.zip) y se necesita la implementación en JavaScript de las correspondientes funcionalidades.

**Funcionalidades a implementar**

* **Formulario Nuevo Cliente**: Implementar validaciones completas en todos sus campos, utilizando eventos 'onblur' para verificar la información en tiempo real. Además, se debe incluir una característica para resaltar visualmente el campo activo en el que el usuario se encuentra en ese momento.
* **Agregar Cliente**: Implementar la lógica para que el botón de agregar cliente solo se active cuando todos los datos del formulario sean adecuados. Al presionar el botón, el cliente se añadirá al listado de clientes en el CRM.
* **Gestión de Clientes**: Crear un listado dinámico que muestre todos los clientes agregados. Incluir funcionalidades para editar y eliminar clientes del listado, asegurando que los cambios persistan en el navegador gracias al uso de IndexedDB.
* **Persistencia de Datos**: Asegurar que toda la información de clientes se mantenga disponible en el navegador, incluso después de cerrarlo, utilizando IndexedDB para almacenar y recuperar los datos de manera eficiente.
- Se anima a los estudiantes a explorar e implementar funcionalidades adicionales, valorando positivamente la **innovación y creatividad**.

Una vez revisadas las funcionalidades, rapidamente te das cuenta que no controlas indexedDB, por lo que lo ideal sería dividir esto en dos partes:

### Parte 1: Investigación y Aprendizaje de IndexedDB

Esta sección requiere que el alumnado investigue el uso de IndexedDB, una tecnología interesante para la gestión de datos en aplicaciones web. Deberán explorar sus capacidades, sintaxis y mejores prácticas.

Se espera que documenten su proceso de aprendizaje, creando una guía o cheatsheet que resuma los aspectos clave de IndexedDB, desde la configuración inicial hasta operaciones avanzadas como transacciones y consultas. La creación de tu propio material sirve como referencia útil para futuros proyectos.

### Parte 2: Implementación de Funcionalidades en el CRM
Una vez investigado el funcionamiento de la base de datos indexedDB, procedemos a la implementación de las funcionalidades.

## Formato y Entrega
- **Plataforma de Entrega:** Realiza la entrega de tu trabajo mediante el aula virtual de nuestro curso.
- **Repositorio GitHub:** Sube el enlace de tu proyecto alojado en GitHub. Antes de hacerlo, verifica que tu repositorio esté configurado como público. Si prefieres mantenerlo privado, añádeme como colaborador para que pueda acceder y evaluar tu trabajo. Asegurate de que el archivo readme.md explica el contenido del repositorio, como mínimo, un enlace a:
      - Cheatsheet en formato markdown.
      - Código de la aplicación.

**Recomendaciones Adicionales:**

- Organiza tu código de manera lógica y limpia, facilitando su lectura y mantenimiento.
- Recuerda comentar las secciones más complejas o relevantes de tu código para explicar su funcionamiento.
- Asegúrate de que cumples con los criterios de calificación establecidos.


## Evaluación del proyecto

Mediante este proyecto se evalúan los siguientes criterios de evaluación:

### Criterio 3g) Se han utilizado el navegador para almacenar información y recuperar su contenido.

|     **Indicador**     |                   **Excelente**                   |                   **Aceptable**                   |                **Insatisfactorio**                |
| :--------------------: | :----------------------------------------------: | :----------------------------------------------: | :----------------------------------------------: |
| **Uso de IndexedDB (4pt)** | El código demuestra un uso avanzado y eficiente de IndexedDB para almacenar y recuperar información, con implementaciones complejas y bien estructuradas (4pt) | El código utiliza IndexedDB para almacenar y recuperar información, aunque podría ser más eficiente o estructurado (2pt) | El uso de IndexedDB en el código es insuficiente o inexistente (0pt) |
| **Manejo de Datos (3pt)** | El código maneja los datos de manera efectiva, asegurando integridad, seguridad y accesibilidad en las operaciones con IndexedDB (3pt) | El código maneja datos con IndexedDB, pero con oportunidades de mejora en la integridad, seguridad o accesibilidad (1,5pt) | El manejo de datos con IndexedDB en el código es deficiente o poco efectivo (0pt) |
| **Optimización y Rendimiento (3pt)** | El código muestra una optimización significativa en el uso de IndexedDB, garantizando un rendimiento excelente y tiempos de respuesta rápidos (3pt) | El código es funcional en el uso de IndexedDB, pero carece de optimización avanzada, afectando el rendimiento (1,5pt) | El código carece de optimización en el uso de IndexedDB, resultando en un rendimiento pobre (0pt) |

### Criterio 5f) Se han validado formularios Web utilizando eventos.

|     **Indicador**     |                   **Excelente**                   |                   **Aceptable**                   |                **Insatisfactorio**                |
| :--------------------: | :----------------------------------------------: | :----------------------------------------------: | :----------------------------------------------: |
| **Validación de Formularios (4pt)** | El código demuestra la capacidad de validar formularios web de manera efectiva, con validaciones completas y personalizadas (4pt) | El código valida formularios web, aunque podría haberse implementado de manera más efectiva o con validaciones limitadas (2pt) | La validación de formularios web en el código es insatisfactoria o inexistente (0pt) |
| **Manejo de Errores (3pt)** | El código maneja errores de validación de forma efectiva, proporcionando retroalimentación clara al usuario y previniendo el envío de datos inválidos (3pt) | El código maneja errores de validación, pero con oportunidades de mejora en efectividad o retroalimentación al usuario (1,5pt) | El manejo de errores de validación en el código es deficiente o poco efectivo (0pt) |
| **Integración con Eventos (3pt)** | El código integra la validación de formularios con eventos, de modo que la validación se desencadena de manera eficiente y oportuna (3pt) | El código integra la validación de formularios con eventos, aunque con oportunidades de mejora en eficiencia o puntualidad (1,5pt) | La integración de la validación de formularios con eventos en el código es ineficaz o inexistente (0pt) |

### Criterio 5g) Se han utilizado expresiones regulares para facilitar los procedimientos de validación.

|     **Indicador**     |                   **Excelente**                   |                   **Aceptable**                   |                **Insatisfactorio**                |
| :--------------------: | :----------------------------------------------: | :----------------------------------------------: | :----------------------------------------------: |
| **Uso de Expresiones Regulares (4pt)** | El código utiliza expresiones regulares de manera efectiva y apropiada para validar datos en formularios, cubriendo una amplia variedad de casos (4pt) | El código utiliza expresiones regulares, pero con oportunidades de mejora en la cobertura o eficacia (2pt) | El uso de expresiones regulares en el código es inadecuado o inexistente (0pt) |
| **Validación Avanzada (3pt)** | El código aplica expresiones regulares de manera avanzada para validar datos de manera sofisticada y precisa (3pt) | El código utiliza expresiones regulares para validación, aunque no de manera avanzada o sofisticada (1,5pt) | La aplicación de expresiones regulares en el código es básica o inadecuada (0pt) |
| **Documentación de Expresiones Regulares (3pt)** | El código documenta claramente las expresiones regulares utilizadas, proporcionando explicaciones detalladas y ejemplos de su función (3pt) | El código documenta expresiones regulares, aunque con oportunidades de mejora en la claridad o cantidad de detalles proporcionados (1,5pt) | La documentación de expresiones regulares en el código es insatisfactoria o inexistente (0pt) |

## Nota: Propiedad Intelectual

> > A menudo es inevitable, al escribir un documento, hacer uso de recursos creados por terceras personas. Es por tanto comprensible hacerlo en el marco de una práctica de nuestros estudios, siempre que esto se documente claramente y no suponga plagio en la práctica.

> > Por lo tanto, al presentar una práctica que haga uso de recursos ajenos, se deberá referenciar claramente estos recursos, especificando el nombre de cada recurso, su autor, el lugar donde se obtuvo y el su estatus legal: si la obra está protegida por copyright o se acoge a alguna otra licencia de uso (Creative Commons, licencia GNU, GPL ...). El estudiante deberá asegurarse de que la licencia que sea no impide específicamente su uso en el marco de la práctica. En caso de no encontrar la información correspondiente deberá asumir que la obra está protegida por copyright.
