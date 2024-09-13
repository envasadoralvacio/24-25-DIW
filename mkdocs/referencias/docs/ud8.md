# UD8 - Vue.js

<center>
![](assets/ud8/vuejs.png)
</center>

## ¿Qué es Vue?

Vue (pronunciado /vjuː/) es un framework de JavaScript para construir interfaces de usuario. Se basa en HTML, CSS y JavaScript estándar y proporciona un modelo de programación declarativo y basado en componentes que lo ayuda a desarrollar interfaces de usuario de manera eficiente, ya sean simples o complejas.

Ejemplo de un contador:

```
import { createApp } from 'vue'

createApp({
  data() {
    return {
      count: 0
    }
  }
}).mount('#app')
```

```
<div id="app">
  <button @click="count++">
    Count is: {{ count }}
  </button>
</div>
```

El ejemplo anterior demuestra las dos características principales de Vue:

- **Representación declarativa** : Vue amplía HTML estándar con una sintaxis de plantilla que nos permite describir declarativamente la salida HTML en función del estado de JavaScript.

- **Reactividad** : Vue rastrea automáticamente los cambios de estado de JavaScript y actualiza de manera eficiente el DOM cuando ocurren cambios.

## Progressive Framework

Vue es un marco y un ecosistema que cubre la mayoría de las características comunes necesarias en el desarrollo frontend. Pero la web es extremadamente diversa: las cosas que construimos en la web pueden variar drásticamente en forma y escala. Con eso en mente, Vue está diseñado para ser flexible y adoptarse de forma incremental. Dependiendo de su caso de uso, Vue se puede usar de diferentes maneras:

- Mejora de HTML estático sin un paso de compilación
- Incrustación como componentes web en cualquier página
- Solicitud de una sola página (SPA)
- Fullstack / Representación del lado del servidor (SSR)
- Jamstack / Generación de sitios estáticos (SSG)
- Orientación a escritorio, móvil, WebGL e incluso al terminal

Los términos anteriores se analizan en detalle en el apartado [formas de usar Vue](https://vuejs.org/guide/extras/ways-of-using-vue.html), de la guía del framework.

Según la documentación el nombre de "The Progressive Framework" se debe a que: "es un marco que puede crecer contigo y adaptarse a tus necesidades".

## Single File Components

En la mayoría de los proyectos de Vue habilitados para herramientas de compilación, creamos componentes de Vue utilizando un formato de archivo similar a HTML llamado componente de archivo único (también conocido como archivos \*.vue, abreviado como SFC). Un SFC de Vue, como sugiere el nombre, encapsula la lógica del componente (JavaScript), la plantilla (HTML) y los estilos (CSS) en un solo archivo. Aquí está el ejemplo anterior, escrito en formato SFC:

```
<script>
export default {
  data() {
    return {
      count: 0
    }
  }
}
</script>

<template>
  <button @click="count++">Count is: {{ count }}</button>
</template>

<style scoped>
button {
  font-weight: bold;
}
</style
```

SFC es una característica definitoria de Vue y es la forma recomendada de crear componentes de Vue si su caso de uso justifica una configuración de compilación. Puede obtener más información sobre [cómo y por qué de SFC](https://vuejs.org/guide/scaling-up/sfc.html) en la documentación, pero por ahora, solo sepa que Vue se encargará de la configuración de todas las herramientas de compilación por usted.

## Estilos de API

Los componentes de Vue se pueden crear en dos estilos de API diferentes: API de opciones y API de composición .

### Options API

Con options API, definimos la lógica de un componente utilizando un objeto de opciones como data, methods y mounted. Las propiedades definidas por las opciones se exponen en "this" dentro de las funciones, que apunta a la instancia del componente:

```
<script>
export default {
  // Properties returned from data() become reactive state
  // and will be exposed on `this`.
  data() {
    return {
      count: 0
    }
  },

  // Methods are functions that mutate state and trigger updates.
  // They can be bound as event listeners in templates.
  methods: {
    increment() {
      this.count++
    }
  },

  // Lifecycle hooks are called at different stages
  // of a component's lifecycle.
  // This function will be called when the component is mounted.
  mounted() {
    console.log(`The initial count is ${this.count}.`)
  }
}
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

[Probrar el código del ejemplo](https://sfc.vuejs.org/#eyJBcHAudnVlIjoiPHNjcmlwdD5cbmV4cG9ydCBkZWZhdWx0IHtcbiAgLy8gcmVhY3RpdmUgc3RhdGVcbiAgZGF0YSgpIHtcbiAgICByZXR1cm4ge1xuICAgICAgY291bnQ6IDBcbiAgICB9XG4gIH0sXG5cbiAgLy8gZnVuY3Rpb25zIHRoYXQgbXV0YXRlIHN0YXRlIGFuZCB0cmlnZ2VyIHVwZGF0ZXNcbiAgbWV0aG9kczoge1xuICAgIGluY3JlbWVudCgpIHtcbiAgICAgIHRoaXMuY291bnQrK1xuICAgIH1cbiAgfSxcblxuICAvLyBsaWZlY3ljbGUgaG9va3NcbiAgbW91bnRlZCgpIHtcbiAgICBjb25zb2xlLmxvZyhgVGhlIGluaXRpYWwgY291bnQgaXMgJHt0aGlzLmNvdW50fS5gKVxuICB9XG59XG48L3NjcmlwdD5cblxuPHRlbXBsYXRlPlxuICA8YnV0dG9uIEBjbGljaz1cImluY3JlbWVudFwiPkNvdW50IGlzOiB7eyBjb3VudCB9fTwvYnV0dG9uPlxuPC90ZW1wbGF0ZT4ifQ==)

### Composition API

Con la composition API, definimos la lógica de un componente utilizando funciones API importadas. En SFC, la API de composición se usa normalmente con "script setup". El atributo setup hace que Vue realice transformaciones en tiempo de compilación que nos permiten usar la API de composición con menos repeticiones. Por ejemplo, las importaciones y las variables/funciones de nivel superior declaradas en "script setup" se pueden usar directamente en la plantilla.

Veamos el mismo componente, con exactamente la misma plantilla, pero usando la API de composición y "script setup" en su lugar:

```
<script setup>
import { ref, onMounted } from 'vue'

// reactive state
const count = ref(0)

// functions that mutate state and trigger updates
function increment() {
  count.value++
}

// lifecycle hooks
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

[Probrar el código del ejemplo](https://sfc.vuejs.org/#eNpNkcFuwjAMhl/FiiatCGh3RgVt2nm3HXOgBLcNtEmUOExTlXefQwvaIVLs+Pv9x57Eh3PlLaLYiToorx1BQIruII0enfUEE3hsN2DNl42G8AwJWm9HeGXqVRppqoorGkX6hhCoIZRGWRMIVAZgn/nibbWUttFwKb8D9Q3BGDMxc9CYM5DXXYceojtzKkjzAEAb5XFEQ8UKJmlg1i9vzRBxvZYmLR0G3aL6VQNCb+2VFZ7WCyb3hwdsgh2wHGxXHL97ZHlNuhkW1zrAy/SvQSqP/IPEp67mOfGEOCAc3cBGOQKoT5GInb6rQavrXoqnZSkOn4vuDqZpaZJSXc0I43X11BIbMQ9/OzauvARreD131yx5fwhSsE7O5BxvIsdS9EQu7KoqtCov9RJK67uKb6XnfnrEEsO4PXn7E9CzsBRZggeXRPoDuve6FA==)

### ¿Cuál elegir?

Ambos estilos de API son totalmente capaces de cubrir casos de uso comunes. Los conceptos y conocimientos fundamentales sobre Vue se comparten entre los dos estilos, de hecho, la API de opciones se implementa sobre la API de composición.

La API de opciones se centra en el concepto de una "instancia de componente" ( this como se ve en el ejemplo), que generalmente se alinea mejor con un modelo mental basado en clases para usuarios que provienen de lenguajes de programación orientados a objetos. También es más amigable para principiantes al abstraer los detalles de reactividad y hacer cumplir la organización del código a través de grupos de opciones.

La API de composición se centra en declarar variables de estado reactivas directamente en el ámbito de una función y componer el estado de varias funciones juntas para manejar la complejidad. Tiene una forma más libre y requiere una comprensión de cómo funciona la reactividad en Vue para ser utilizado de manera efectiva. A cambio, su flexibilidad permite patrones más poderosos para organizar y reutilizar la lógica.

## Recursos

- [Vue 3 Documentation](https://vuejs.org/guide/introduction.html)
- [Ways of Using Vue](https://vuejs.org/guide/extras/ways-of-using-vue.html)

Last updated: 22/01/2023

