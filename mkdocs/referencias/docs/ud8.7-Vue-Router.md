# Vue Router

## Introducción a la sección

En esta sección, vamos a ver como crear aplicaciones de una sola página (SPA) con el enrutador Vue.

Las SPA son aplicaciones o sitios web que interactúan con el usuario reescribiendo dinámicamente la página actual en lugar de cargar páginas nuevas completas desde el servidor.

[Vue Router](https://router.vuejs.org/introduction.html) es el enrutador oficial de Vue.js. Se integra con el núcleo de Vue.js para facilitar la creación de SPA con Vue.js.

Los objetivos de la sección son enfocarnos en:

- Vue Router
- Guards
- Rutas hijas
- Diferentes diseños entre router views
- Argumentos por URL (Query parameters)
- Segmentos por URL
- Lifecycle hooks

## Inicio de proyecto

Comenzamos un nuevo proyecto Vue utilizando vite.

```
npm init vue@latest
```

```
Vue.js - The Progressive JavaScript Framework

✔ Project name: … VueRouter
✔ Package name: … VueRouter
✔ Add TypeScript? … No / Yes
✔ Add JSX Support? … No / Yes
✔ Add Vue Router for Single Page Application development? … No / Yes
✔ Add Pinia for state management? … No / Yes
✔ Add Vitest for Unit Testing? … No / Yes
✔ Add an End-to-End Testing Solution? › No
✔ Add ESLint for code quality? … No / Yes

Scaffolding project in /xxx/xxx/VueRouter...

Done. Now run:

  cd VueRouter
  npm install
  npm run dev

```

Una vez que tenemos creado el proyecto, eliminamos los elementos que genera por defecto y no vamos a utilizar, componentes, archivos css, etc....

## Creación de páginas necesarias

Como ya habamos anteriormente cuando estamos trabajando con un proyecto grande no se recomienda usar la estructura por defecto ya que a medida que el proyecto crece será mas complejo tener localizados y controlados los diferentes elementos de la aplicación.

En ese caso usaremos una estructura de este tipo:

<center>
![](assets/ud8/estructura_recomendada.png)
</center>

En concreto vamos a comenzar creando la siguiente estructura:

<center>
![](assets/ud8/estructura_carpetas_router.png)
</center>

Una vez creadas las carpetas, vamos con los componentes:

AboutPage.vue

```
<template>
  <h1>About Page</h1>
</template>
```

ListPage.vue

```
<template>
  <h1>List Page</h1>
</template>
```

PokemonPage.vue

```
<template>
  <h1>Pokemon Page</h1>
</template>
```

Los importamos desde nuestra App.vue:

App.vue

```
<template>
  <div>Vue Router</div>
  <AboutPage />
  <ListPage />
  <PokemonPage />
</template>

<script>
import AboutPage from './modules/pokemon/pages/AboutPage.vue'
import ListPage from './modules/pokemon/pages/ListPage.vue'
import PokemonPage from './modules/pokemon/pages/PokemonPage.vue'

export default { components: { AboutPage, ListPage, PokemonPage } }
</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
```

## Configuración manual del Vue Router

Crear una SPA con Vue + Vue Router es bastante fácil y natural ya que con Vue.js, ya estamos desarrollando nuestra aplicación usando componentes. Al agregar Vue Router a la mezcla, solo tenemos que asignar nuestros componentes a las rutas y dejar que Vue Router sepa dónde renderizarlos.

Instalamos el Vue Router:

```
npm install vue-router@4
```

Ahora si nos vamos a [la documentación oficial nos muestra los pasos](https://router.vuejs.org/guide/#javascript) para usar Vue Router en nuestra aplicación. Nos indica los siguientes pasos:

**Paso 1**. Definir los componentes
Esto ya lo tenemos hecho.

**Paso 2**. Definir las rutas.
Para poner la configuración del router usualmente se usa una carpeta router en la raiz del proyecto, la creamos y dentro de esta un archivo router.js.

<center>
![](assets/ud8/estructura_carpetas_router_2.png)
</center>

Y dentro del archivo router.js vamos a configurar las rutas:

```
import AboutPage from '../modules/pokemon/pages/AboutPage.vue'
import ListPage from '../modules/pokemon/pages/ListPage.vue'
import PokemonPage from '../modules/pokemon/pages/PokemonPage.vue'

const routes = [
  { path: '/', component: ListPage },
  { path: '/about', component: AboutPage },
  { path: '/id', component: PokemonPage },
]
```

**Paso 3**. Crear la instancia del enrutador y pasar la opción routes

```
import { createRouter, createWebHashHistory } from 'vue-router'

import AboutPage from '../modules/pokemon/pages/AboutPage.vue'
import ListPage from '../modules/pokemon/pages/ListPage.vue'
import PokemonPage from '../modules/pokemon/pages/PokemonPage.vue'

const routes = [
  { path: '/', component: ListPage },
  { path: '/about', component: AboutPage },
  { path: '/id', component: PokemonPage },
]

const router = createRouter({
  routes,
})

export default router
```

**Paso 4**. Proporciona la [implementación del historial](https://router.vuejs.org/guide/essentials/history-mode.html#different-history-modes) a utilizar.

```
import { createRouter, createWebHashHistory } from 'vue-router'

import AboutPage from '../modules/pokemon/pages/AboutPage.vue'
import ListPage from '../modules/pokemon/pages/ListPage.vue'
import PokemonPage from '../modules/pokemon/pages/PokemonPage.vue'

const routes = [
  { path: '/', component: ListPage },
  { path: '/about', component: AboutPage },
  { path: '/id', component: PokemonPage },
]

const router = createRouter({
  routes,
})

export default router
```

**Paso 5**. Crear y montar la instancia raíz.

Vamos a crear y montyar la instancia en el main de nuestra aplicacióon.

```
import { createApp } from 'vue'
import App from './App.vue'
import router from './router/router'

import './assets/main.css'

createApp(App).use(router).mount('#app')
```

Ahora refrescamos la aplicación y nos fijamos en la url (http://localhost:5173/#/) vemos que está usando una # en la ruta, es por el modo history que hemos definido.

Finalmente nos queda decirle a la aplicación donde tiene que mostrar los componentes de las rutas, para eso tenemos el [router-view](https://router.vuejs.org/guide/#router-view). Podemos ponerlo en cualquier lugar para adaptarlo a nuestro diseño diseño.

App.vue

```
<template>
  <router-view />
</template>

<script>
export default { components: {} }
</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
```

## Error 404 Not Found

Ya estamos mostrando cada una de las páginas, ahora sería útil implementar otra para que [cuando la ruta no exista nos muestre información del error](https://router.vuejs.org/guide/essentials/dynamic-matching.html#catch-all-404-not-found-route).
Esta página la vamos a crear dentro de un nuevo módulo, "shared", la idea de este módulo es que tenga componentes que van a ser usados por toda la aplicación.

NoPageFound.vue

```
<template>
  <h1>Error 404 Not Found</h1>
</template>
```

router.js

```
import { createRouter, createWebHashHistory } from 'vue-router'

import AboutPage from '../modules/pokemon/pages/AboutPage.vue'
import ListPage from '../modules/pokemon/pages/ListPage.vue'
import PokemonPage from '../modules/pokemon/pages/PokemonPage.vue'
import NoPageFound from '../modules/shared/NoPageFound.vue'

const routes = [
  { path: '/', component: ListPage },
  { path: '/about', component: AboutPage },
  { path: '/id', component: PokemonPage },
  { path: '/:pathMatch(.*)*', component: NoPageFound },
]

const router = createRouter({
  history: createWebHashHistory(),
  routes,
})

export default router
```

## LazyLoad de páginas

Para aligerar la carga de la aplicación, podemos hacer un [lazyload de los componentes](https://router.vuejs.org/guide/advanced/lazy-loading.html#lazy-loading-routes). De esta manera al comienzo solo se va cargar el componente o página principal y el resto de los componentes se cargarán bajo demanda .

router.js

```
import { createRouter, createWebHashHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    component: () => import('../modules/pokemon/pages/ListPage.vue'),
  },
  {
    path: '/about',
    component: () => import('../modules/pokemon/pages/AboutPage.vue'),
  },
  {
    path: '/id',
    component: () => import('../modules/pokemon/pages/PokemonPage.vue'),
  },
  {
    path: '/:pathMatch(.*)*',
    component: () => import('../modules/shared/NoPageFound.vue'),
  },
]

const router = createRouter({
  history: createWebHashHistory(),
  routes,
})

export default router
```

## Navegar entre páginas - RouterLink

Vamos ahora a implementa una barra de navegación, como es un componente global en la aplicación la vamos a crear dentro del directorio shared.

Navbar.vue

```
<template>
  <div>
    <router-link to="/">Pokemon List</router-link>
    <router-link to="/id">Pokemon por id</router-link>
    <router-link to="/about"> About</router-link>
  </div>
</template>

<style scoped>
div {
  padding: 0 30px;
}

div a {
  font-weight: hold;
  color: #2c3e50;
  margin: 0 10px;
}
</style>
```

App.vue

```
<template>
  <Navbar />
  <router-view />
</template>

<script>
import { defineAsyncComponent } from 'vue'

export default {
  components: {
    Navbar: defineAsyncComponent(() => import('./modules/shared/Navbar.vue')),
  },
}
</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
```

## RouterLink Active

Si inspeccionamos el código del navlink con el inspector de elementos, vemos que Vue le coloca las clases router-link-active y router-link-exact-active al elemento del menú en el que hemos hecho click. Podemos por tanto aplicar css a ese elemento usando esa clase.

Navbar.vue

```
<template>
  <div>
    <router-link to="/">Pokemon List</router-link>
    <router-link to="/id">Pokemon por id</router-link>
    <router-link to="/about"> About</router-link>
  </div>
</template>

<style scoped>
div {
  padding: 0 30px;
}

div a {
  font-weight: hold;
  color: #2c3e50;
  margin: 0 10px;
}

.router-link-exact-active {
  color: red;
}
</style>

```

## Lifecycle Hooks - Ciclo de vida de un componente

Cada instancia de componente de Vue pasa por [una serie de pasos](https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-hooks) de inicialización cuando se crea; por ejemplo, debe configurar la observación de datos, compilar la plantilla, montar la instancia en el DOM y actualizar el DOM cuando cambian los datos. En el camino, también ejecuta funciones llamadas Lifecycle Hooks o enlaces de ciclo de vida, lo que nos da la oportunidad de imlpementar código propio código en cada para que se ejecute en etapas específicas.

### Registro de ganchos de ciclo de vida

Por ejemplo, el enlace se puede usar para ejecutar código después de que el componente haya terminado la representación inicial y haya creado los nodos DOM:mounted

js

```
export default {
  mounted() {
    console.log(`the component is now mounted.`)
  }
}
```

También hay otros ganchos que se llamarán en diferentes etapas del ciclo de vida de la instancia, siendo los más utilizados, mounted, updated, y unmounted

Todos los enlaces de ciclo de vida se llaman con su contexto this que apunta a la instancia activa actual que lo invoca.

### Diagrama de ciclo de vida

A continuación se muestra un diagrama del ciclo de vida de la instancia. No es necesario que comprenda completamente todo lo que sucede en este momento, pero a medida que aprenda y construya más, será una referencia útil.

<center>
![](assets/ud8/lifecycle.16e4c08e.png)
</center>

## Disparar métodos del ciclo de vida

En la referencia de la [API Lifecycle Hooks](https://vuejs.org/api/options-lifecycle.html) podemos ver los detalles sobre todos los lifecycle hooks y sus respectivos casos de uso.

Vamos a implementar cada uno de los Lifecycle Hooks para probarlos:

AboutPage.vue

```
<template>
  <h1>About Page</h1>
  <hr />
  <h2>{{ name }}</h2>
  <!-- Si comentamos el h2 vemos que no saltan los hooks que están pendientes de -->
  <!-- los estados; renderTracked | renderTriggered. -->
  <button @click="changeName">Change name</button>
</template>

<script>
export default {
  data() {
    return {
      name: 'IES Rafael Alberti',
    }
  },
  methods: {
    changeName() {
      this.name = '2º DAW'
      console.log(this.name)
    },
  },
  beforeCreate() {
    console.log('beforeCreate')
  },
  created() {
    console.log('created')
  },
  beforeMount() {
    console.log('beforeMount')
  },
  mounted() {
    console.log('mounted')
  },
  beforeUpdate() {
    console.log('beforeUpdate')
  },
  updated() {
    console.log('updated')
  },
  beforeUnmount() {
    console.log('beforeUnmount')
  },
  unmounted() {
    console.log('unmounted')
  },
  errorCaptured() {
    console.log('errorCaptured')
  },
  renderTracked() {
    console.log('renderTracked')
  },
  renderTriggered() {
    console.log('renderTriggered')
  },
  activated() {
    console.log('activated')
  },
  deactivated() {
    console.log('deactivated')
  },
}
</script>
```

### ¿En que paso del ciclo de vido hago una llamada HTTP?

Una pregunta que nos hacemos en este punto es, si necesito hacer una llamada HTTP lo antes posible para que el contenido se renderice nada mas cargar la aplicación, ¿donde la hago?

En primer lugar vamos a mencionar donde no podemos hacerlo, y es en beforeCreate, ya que en ese momento el data no se ha creado y una vez que se crea va a sobreescribir todo lo que hay. La petición HTTP idealmente podemos hacerla en created().

Veamoslo en un ejemplo:

AboutPage.vue

```
<template>
  <h1>About Page</h1>
  <hr />
  <h2>{{ name }}</h2>
  <!-- Si comentamos el h2 vemos que no saltan los hooks que están pendientes de -->
  <!-- los estados; renderTracked | renderTriggered. -->
  <button @click="changeName">Change name</button>
</template>

<script>
export default {
  data() {
    return {
      name: 'IES Rafael Alberti',
    }
  },
  methods: {
    changeName() {
      this.name = '2º DAW'
      console.log(this.name)
    },
  },
  beforeCreate() {
    console.log('beforeCreate')
    this.name = 'DWEC'
    // Este cambio no se va a reflejar en el DOM
    // En ese momento no existe el data, y al crearse sobrescribe lo que hay.
  },
  created() {
    console.log('created')
    this.name = 'DWEC'
    // Este si se va a reflejar en el DOM
  },
  beforeMount() {
    console.log('beforeMount')
  },
  mounted() {
    console.log('mounted')
  },
  beforeUpdate() {
    console.log('beforeUpdate')
  },
  updated() {
    console.log('updated')
  },
  beforeUnmount() {
    console.log('beforeUnmount')
  },
  unmounted() {
    console.log('unmounted')
  },
  errorCaptured() {
    console.log('errorCaptured')
  },
  renderTracked() {
    console.log('renderTracked')
  },
  renderTriggered() {
    console.log('renderTriggered')
  },
  activated() {
    console.log('activated')
  },
  deactivated() {
    console.log('deactivated')
  },
}
</script>
```

## Segmentos del URL y QueryParameters

Vamos a trabajar con el componente pokemonPage, este va a recibir por la url un id del pokemon que tiene que mostrar.

Si ahora escribimos una url como por ejemplo, http://localhost:5173/#/55, nos devolverá un 404 ya que no tenemos ninguna ruta que haga match con ese path. Tenemos que indicarle al router que el id es un argumento.

router.js

```
import { createRouter, createWebHashHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    component: () => import('../modules/pokemon/pages/ListPage.vue'),
  },
  {
    path: '/about',
    component: () => import('../modules/pokemon/pages/AboutPage.vue'),
  },
  // {
  //   path: '/id',
  //   component: () => import('../modules/pokemon/pages/PokemonPage.vue'),
  // },
  {
    path: '/:id',
    name: 'pokemonId',
    component: () => import('../modules/pokemon/pages/PokemonPage.vue'),
  },
  {
    path: '/:pathMatch(.*)*',
    component: () => import('../modules/shared/NoPageFound.vue'),
  },
]

const router = createRouter({
  history: createWebHashHistory(),
  routes, // short for `routes: routes`
})

export default router
```

Ahora tenemos que recibir ese id en el componente.

PokemonPage.vue

```
<template>
  <h1>
    Pokemon: <span> # {{ id }}</span>
  </h1>
</template>

<script>
export default {
  data() {
    return {
      id: null,
    }
  },
  created() {
    console.log(this.$route)
    const { id } = this.$route.params
    console.log(id)
    this.id = id
  },
}
</script>
```

No está mal, pero mejor aún si le podemos especifiar como va a funcionar el componente. Es decir, sabemos que el id es necesario para que el componente funcione, estaría bien hacer un prop con el y establecer las validaciones, así cualquiera que vea el código sabe que nuestro componente necesita recibir ese id.

## Recibir Props por URL

router.js

```
import { createRouter, createWebHashHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    component: () => import('../modules/pokemon/pages/ListPage.vue'),
  },
  {
    path: '/about',
    component: () => import('../modules/pokemon/pages/AboutPage.vue'),
  },
  {
    path: '/:id',
    name: 'pokemonId',
    component: () => import('../modules/pokemon/pages/PokemonPage.vue'),
    // Definimos un prop, y accedemos a la ruta.
    props: (route) => {
      // console.log(route)
      // Lo que devolvamos será o que reciba el componente por props.

      // const { id } = route.params
      // return {
      //   id,
      // }
      // Con el código anterior nos da un warning, tenemos que convertirlo en number y validarlo.
      const id = Number(route.params.id)
      return isNaN(id) ? { id: 1 } : { id }
    },
  },
  {
    path: '/:pathMatch(.*)*',
    component: () => import('../modules/shared/NoPageFound.vue'),
  },
]

const router = createRouter({
  history: createWebHashHistory(),
  routes,
})

export default router
```

PokemonPage.vue

```
<template>
  <h1>
    Pokemon: <span> # {{ id }}</span>
  </h1>
</template>

<script>
export default {
  props: {
    id: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      // id: null,
    }
  },
  created() {
    // console.log(this.$attrs)
  },
}
</script>
```

## Petición HTTP y redirecciones

Vamos a hacer la llamada Http para traer al pokemon que nos llega como argumento por la url.

PokemonPage.vue

```
<template>
  <h1>
    Pokemon: <span> # {{ id }}</span>
  </h1>

  <div v-if="pokemon">
    <img :src="pokemon.sprites.front_default" alt="pokemon.name" />
  </div>
</template>

<script>
export default {
  props: {
    id: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      pokemon: null,
    }
  },
  created() {
    this.getPokemon()
  },
  methods: {
    async getPokemon() {
      try {
        const pokemon = await fetch(
          `https://pokeapi.co/api/v2/pokemon/${this.id}`
        ).then((res) => res.json())
        console.log(pokemon)
        this.pokemon = pokemon
      } catch (error) {
        // console.log(error)
        this.$router.push('/')
      }
    },
  },
  watch: {
    id() {
      this.getPokemon()
    },
  },
}
</script>
```

## Redirección desde el router

Navbar.vue

```
<template>
  <div>
    <router-link to="/">Pokemon List</router-link>
    <!-- <router-link to="/id">Pokemon por id's</router-link> -->
    <router-link :to="{ name: 'pokemonId', params: { id: 1 } }"
      >Pokemon por id's</router-link
    >
    <router-link to="/about">About</router-link>
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
div {
  padding: 0 40px;
}

div a {
  font-weight: hold;
  color: #2c3e50;
  margin: 0 10px;
}

.router-link-exact-active {
  color: red;
}
</style>
```

router.js

```
import { createRouter, createWebHashHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    component: () => import('../modules/pokemon/pages/ListPage.vue'),
  },
  {
    path: '/about',
    component: () => import('../modules/pokemon/pages/AboutPage.vue'),
  },
  {
    // path: '/:id',
    path: '/pokemon/:id',
    name: 'pokemonId',
    component: () => import('../modules/pokemon/pages/PokemonPage.vue'),
    props: (route) => {
      const id = Number(route.params.id)
      return isNaN(id) ? { id: 1 } : { id }
    },
  },
  {
    path: '/:pathMatch(.*)*',
    // component: () => import('../modules/shared/NoPageFound.vue'),
    redirect: '/',
  },
]

const router = createRouter({
  history: createWebHashHistory(),
  routes,
})

export default router
```

## RouterLink Personalizado

## Multiples Router-View - Rutas Hijas

Vamos a ver las bases para construir una aplicación que tenga diseños estructurales totalmente diferente en diferentes páginas o componentes.
Para ello vamos a crear los layout que no es mas que un simple componente que internamente tiene otro router-view, el router que tenemos en el app.js seguirá siendo el router principal.

PokemonLayout.vue

```
<template>
  <div class="pokemon-layout">
    <h1>Pokemon Layout</h1>
    <router-view />
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
.pokemon-layout {
  background-color: deeppink;
}
</style>
```

router.js

```
import { createRouter, createWebHashHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    // redirect: '/home',
    redirect: '/pokemon',
  },
  {
    path: '/pokemon',
    name: 'pokemon',
    component: () => import('../modules/pokemon/layouts/PokemonLayout.vue'),
    children: [
      {
        path: '',
        name: 'pokemon-home',
        component: () => import('../modules/pokemon/pages/ListPage.vue'),
      },
      {
        path: 'about',
        name: 'pokemon-about',
        component: () => import('../modules/pokemon/pages/AboutPage.vue'),
      },
      {
        path: 'pokemon/:id',
        name: 'pokemon-id',
        component: () => import('../modules/pokemon/pages/PokemonPage.vue'),
        props: (route) => {
          const id = Number(route.params.id)
          return isNaN(id) ? { id: 1 } : { id }
        },
      },
    ],
  },
  {
    path: '/:pathMatch(.*)*',
    component: () => import('../modules/shared/NoPageFound.vue'),
    // redirect: '/',
  },
]

const router = createRouter({
  history: createWebHashHistory(),
  routes,
})

export default router

```

navbar.js

```
<template>
  <div>
    <!-- <router-link to="/">Pokemon List</router-link> -->
    <router-link :to="{ name: 'pokemon-home' }">Pokemon List</router-link>
    <router-link :to="{ name: 'pokemon-id', params: { id: 1 } }"
      >Pokemon por id's</router-link
    >
    <!-- <router-link to="/about">About</router-link> -->
    <router-link :to="{ name: 'pokemon-about' }">About</router-link>
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
div {
  padding: 0 40px;
}

div a {
  font-weight: hold;
  color: #2c3e50;
  margin: 0 10px;
}

.router-link-exact-active {
  color: red;
}
</style>
```

## Segundo Layout

Vamnos a inmplementa el segundo módulo, spiderman, que va a tener un layout totalmente diferente. Cremos las carpetas layouts y pages, las páginas del módulo, y el nuevo layout.

Characters.vue

```
<template>
  <h2>Personajes</h2>
</template>
```

About.vue

```
<template>
  <h2>About Spiderman</h2>
</template>
```

SpidermanLayout.vue

```
<template>
  <div class="spiderman-layout">
    <h1>Spiderman Layout</h1>
    <router-view />
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
.spiderman-layout {
  background-color: deepskyblue;
}
</style>
```

router.js

```
import { createRouter, createWebHashHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    redirect: '/pokemon',
  },
  {
    path: '/pokemon',
    name: 'pokemon',
    component: () => import('../modules/pokemon/layouts/PokemonLayout.vue'),
    children: [
      {
        path: '',
        name: 'pokemon-home',
        component: () => import('../modules/pokemon/pages/ListPage.vue'),
      },
      {
        path: 'about',
        name: 'pokemon-about',
        component: () => import('../modules/pokemon/pages/AboutPage.vue'),
      },
      {
        path: 'pokemon/:id',
        name: 'pokemon-id',
        component: () => import('../modules/pokemon/pages/PokemonPage.vue'),
        props: (route) => {
          const id = Number(route.params.id)
          return isNaN(id) ? { id: 1 } : { id }
        },
      },
    ],
  },
  // Spiderman Layout
  {
    path: '/spiderman',
    name: 'spiderman',
    component: () => import('../modules/spiderman/layouts/SpidermanLayout.vue'),
    children: [
      {
        path: '',
        name: 'spiderman-home',
        component: () => import('../modules/spiderman/pages/Characters.vue'),
      },
      {
        path: 'about',
        name: 'spiderman-about',
        component: () => import('../modules/spiderman/pages/About.vue'),
      },
    ],
  },

  {
    path: '/:pathMatch(.*)*',
    component: () => import('../modules/shared/NoPageFound.vue'),
  },
]

const router = createRouter({
  history: createWebHashHistory(),
  routes,
})

export default router
```

## Añadimos los nuevos enlaces al NavBar

Vamos a incorporar a nuestra barra de navegación los nuevos componentes.

Navbar.vue

```
<template>
  <div>
    <router-link :to="{ name: 'pokemon-home' }">Pokemon List</router-link>
    <router-link :to="{ name: 'pokemon-id', params: { id: 1 } }"
      >Pokemon por id's</router-link
    >
    <router-link :to="{ name: 'pokemon-about' }">About</router-link>
    <router-link :to="{ name: 'spiderman-home' }">Characters</router-link>
    <router-link :to="{ name: 'spiderman-about' }">Spiderman About</router-link>
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
div {
  padding: 0 40px;
}

div a {
  font-weight: hold;
  color: #2c3e50;
  margin: 0 10px;
}

.router-link-exact-active {
  color: red;
}
</style>

```

## Código fuente de la sección

En [este enlace](assets/codigo-vue/VueRouter.zip) encontraréis el código fuente de la aplicación finalizada.
