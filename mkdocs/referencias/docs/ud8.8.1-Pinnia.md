# Pinia

## Introducción a la sección

Esta sección está dedicada a proporcionar una introducción completa a  [Pinia](https://pinia.vuejs.org/), el almacén de estado oficial para aplicaciones Vue.js. Diseñado para reemplazar y mejorar sobre Vuex, Pinia ofrece una experiencia más simplificada y potente para el manejo del estado global en aplicaciones Vue, especialmente aquellas que utilizan Vue 3 y la Composition API.


Los objetivos de la sección son enfocarnos en:

- **Comprender qué es Pinia** y por qué se ha convertido en el gestor de estado recomendado para Vue 3.

- **Identificar los problemas que resuelve Pinia**, especialmente en comparación con Vuex y cómo simplifica la gestión del estado global.

- Explorar las **características principales de Pinia**, incluyendo su estructura de almacén, la forma de definir estados, acciones, getters y más.

- **Aprender a configurar Pinia** en un proyecto Vue y cómo integrarlo con Vue 3 y la Composition API.

- **Prácticas recomendadas** para organizar y estructurar tus almacenes de Pinia para mantener tu código limpio y mantenible.

- **Ejemplos prácticos** de cómo utilizar Pinia para manejar estados complejos y reactividad en aplicaciones Vue de mediana a gran escala.

Esta sección es muy importante para comprender Pinia, el cual es un gestor de estado muy adecuado para aplicaciones de mediana a gran escala.

### ¿Cuándo debo usarlo?

Si nuestra aplicación es simple, probablemente un almacenamiento de estados no aporte una complejidad innecesaria. Un simple patrón reactiva puede ser todo lo que necesitas. Pero si construyes una SPA de mediana a gran escala, es probable que te encuentres con situaciones que te hagan pensar en cómo manejar mejor el estado fuera de tus componentes Vue, y Pinia será el siguiente paso natural.

## Inicio de proyecto

Comenzamos un nuevo proyecto Vue utilizando Vite.

```
npm init vue@latest
```

```
Vue.js - The Progressive JavaScript Framework

✔ Project name: … Vuex
✔ Package name: … Vuex
✔ Add TypeScript? … No / Yes
✔ Add JSX Support? … No / Yes
✔ Add Vue Router for Single Page Application development? … No / Yes
✔ Add Pinia for state management? … No / Yes
✔ Add Vitest for Unit Testing? … No / Yes
✔ Add an End-to-End Testing Solution? › No
✔ Add ESLint for code quality? … No / Yes

Scaffolding project in /xxx/xxx/Vuex...

Done. Now run:

  cd Vuex
  npm install
  npm run dev

```

Una vez que tenemos creado el proyecto, eliminamos los elementos que genera por defecto y no vamos a utilizar, componentes, archivos css, etc....


## Instalación manual de Pinia

En la página oficial de Vue tenemos las [diferentes formas de instalar Pinia](https://pinia.vuejs.org/installation.html) en nuestra aplicación. Nosotros vamos a usar npm:


```
npm install pinia
```

## Configuramos nuestro primer Store

Preparamos la estructura de carpetas y archivos para usar Vuex, para ello creamos una carpeta store y un archivo para almacenar nuestro store.

~~~
src/
└── stores/
    └── counterStore.js
~~~

`Archivo: src/stores/counterStore.js`

```
import { defineStore } from 'pinia';

export const useCounterStore = defineStore('counterStore', {
  // Estado inicial del almacén
  state: () => ({
    counter: 0, // Inicializamos el contador en 0
  }),
});
```

main.js

```
import { createApp } from 'vue'
import App from './App.vue'
import { createPinia } from 'pinia'

import './assets/main.css'

const pinia = createPinia()
createApp(App).use(pinia).mount('#app')
```

Ya hemos configurado nuestro primer store utilizando Pinia, lo cual nos permitirá manejar el estado global de nuestra aplicación de manera más eficiente y con una sintaxis más sencilla

## Leer el state reactivo

Una vez que tienes tu store definido en Pinia, el siguiente paso es leer el estado de este store en un componente Vue. Aquí está cómo hacerlo de manera simple y directa.

`Archivo: src/components/CounterComponent.vue`

~~~
<template>
  <div>Contador: {{ counter }}</div>
</template>

<script>
import { useCounterStore } from '../stores/counterStore';
import { mapState } from 'pinia';

export default {
  name: 'CounterComponent',
  computed: {
    // Mapeamos el estado del counterStore a propiedades computadas locales
    ...mapState(useCounterStore, ['counter']),
  },
};
</script>
~~~

Añadimos el componente al App.vue.

`Archivo: src/App.vue`

~~~
<script>
import CounterComponent from './components/counterComponent.vue';

export default {
  components: { CounterComponent },
}

</script>

<template>
    <CounterComponent /> <!-- Añade el componente counterComponent aquí -->
</template>

<style scoped>
/* Estilos */
</style>
~~~


## Getters

Los getters en Pinia funcionan de manera similar a las propiedades computadas en Vue, permitiendo calcular derivados del estado.


`Archivo: src/stores/counterStore.js`

~~~
import { defineStore } from 'pinia';

export const useCounterStore = defineStore('counterStore', {
  // Estado inicial del almacén
  state: () => ({
    counter: 0, // Inicializamos el contador en 0
  }),
  // Getters para el almacén
  getters: {
    // Definimos el getter squareCount
    squareCount: (state) => state.counter * state.counter,
  },
});
~~~

Para utilizar este getter en un componente Vue, simplemente accedemos al store de Pinia y luego a los getters definidos en él. A continuación, te muestro cómo hacerlo:

`Archivo: src/components/CounterComponent.vue`

```
<template>
  <div>
    <p>Contador: {{ counter }}</p>
    <p>Cuadrado del Contador: {{ squareCount }}</p>
  </div>
</template>

<script>
import { useCounterStore } from '../stores/counterStore';
import { mapState } from 'pinia';

export default {/*  */
  name: 'CounterComponent',
  computed: {
    // Utilizamos mapState para mapear tanto el estado como los getters del counterStore
    // ...mapState(useCounterStore, ['counter', 'squareCount']),
    ...mapState(useCounterStore, {
      counter: (state) => state.counter,
      squareCount: (state) => state.squareCount,
    }),
  },
};
</script>
```

## Actions

Las acciones en Pinia son métodos que puedes definir dentro de tu store que, a diferencia de las mutaciones en Vuex, pueden ser asíncronas. Esto las hace ideales para operaciones que necesitan esperar a que algo suceda (como la recuperación de datos de una API) antes de modificar el estado.


### Action síncrona

Vamos a empezar definiendo un store de Pinia que incluye una acción simple para incrementar el contador de manera síncrona.

`Archivo: src/stores/counterStore.js`

~~~
import { defineStore } from 'pinia';

export const useCounterStore = defineStore('counterStore', {
  // Estado inicial del almacén
  state: () => ({
    counter: 0, // Inicializamos el contador en 0
  }),
  // Getters para el almacén
  getters: {
    squareCount: (state) => state.counter * state.counter,
  },
  // Acciones para mutar el estado
  actions: {
    increment() {
      this.counter++;
    }
  },
});
~~~

Ahora, mostraremos cómo utilizar esta acción síncrona increment desde un componente Vue que utiliza el Options API.

`Archivo: src/components/CounterComponent.vue`
~~~
<template>
  <div>
    <p>Contador: {{ counter }}</p>
    <p>Cuadrado del Contador: {{ squareCount }}</p>
    <button @click="increment">Incrementar</button>
  </div>
</template>

<script>
import { useCounterStore } from '../stores/counterStore';
import { mapState } from 'pinia';

export default {
  name: 'CounterComponent',
  computed: {
    // ...mapState(useCounterStore, ['counter', 'squareCount']),
    ...mapState(useCounterStore, {
      counter: (state) => state.counter,
      squareCount: (state) => state.squareCount,
    }),
  },
  methods: {
    increment() {
      const store = useCounterStore();
      store.increment();
    }
  },
};
</script>
~~~


### Acción asíncrona

Ahora, vamos a expandir nuestro store de Pinia para incluir una acción asíncrona que simula la obtención de un número aleatorio de manera asíncrona y lo suma al contador.

Primero, necesitaremos un helper para generar un número entero aleatori.

`Archivo: src/helpers/getRandomInt.js`

~~~
export const getRandomInt = () => {
  return new Promise((resolve) => {
    const rndInt = Math.floor(Math.random() * 20 + 1)
    setTimeout(() => {
      resolve(rndInt)
    }, 1000) // Simula una operación asíncrona, como una petición a una API
  })
}
~~~

Expandimos nuestro store para incluir la nueva acción:

`Archivo: src/stores/counterStore.js`

~~~
import { defineStore } from 'pinia';
import { getRandomInt } from '../helpers/getRandomInt';

export const useCounterStore = defineStore('counterStore', {
  // Estado inicial del almacén
  state: () => ({
    counter: 0, // Inicializamos el contador en 0
  }),
  // Getters para el almacén
  getters: {
    squareCount: (state) => state.counter * state.counter,
  },
  // Acciones para mutar el estado
  actions: {
    increment() {
      this.counter++;
    },
    async incrementRandom() {
      const randomInt = await getRandomInt();
      this.counter += randomInt; // Asegúrate también de que estás modificando `counter`, no `count`
    },
  },
});
~~~

Finalmente, actualizamos nuestro componente para incluir un botón que al hacer clic invoque la nueva acción asíncrona:

`Archivo: src/components/CounterComponent.vue`

~~~
<template>
  <div>
    <p>Contador: {{ counter }}</p>
    <p>Cuadrado del Contador: {{ squareCount }}</p>
    <button @click="increment">Incrementar</button>
    <button @click="incrementRandom">Incrementar Aleatoriamente</button>
  </div>
</template>

<script>
import { useCounterStore } from '../stores/counterStore';
import { mapState, mapActions } from 'pinia';

export default {
  name: 'CounterComponent',
  computed: {
    // ...mapState(useCounterStore, ['counter', 'squareCount']),
    ...mapState(useCounterStore, {
      counter: (state) => state.counter,
      squareCount: (state) => state.squareCount,
    }),
  },
  methods: {
    // Mapea las acciones
    ...mapActions(useCounterStore, ['increment', 'incrementRandom']),
  },
};
</script>
~~~


## Bloquear botón mientras una acción trabaja

Cuando hacemos click en el botón solicitamos un número aleatorio, si durante el tiempo que tarda en responder le damos mas veces, se van a encadenar las llamadas. Vamos a deshabilitar el botón hasta que no tengamos respuesta.

Vamos a agregar un nuevo estado isLoading a tu almacén counterStore, que se utilizará para rastrear si la acción está en progreso o no.

`Archivo: src/stores/counterStore.js`
~~~
import { defineStore } from 'pinia';
import { getRandomInt } from '../helpers/getRandomInt';

export const useCounterStore = defineStore('counterStore', {
  // Estado inicial del almacén
  state: () => ({
    counter: 0, // Inicializamos el contador en 0
    isLoading: false, // Nuevo estado para rastrear si estamos cargando un número aleatorio
  }),
  // Getters para el almacén
  getters: {
    squareCount: (state) => state.counter * state.counter,
  },
  // Acciones para mutar el estado
  actions: {
    increment() {
      this.counter++;
    },
    async incrementRandom() {
      this.isLoading = true; // Comenzamos a cargar
      const randomInt = await getRandomInt();
      this.counter += randomInt;
      this.isLoading = false; // Terminamos de cargar
    },
  },
});
~~~

Modificamos el componente para deshabilitar el botón basado en el estado isLoading. Esto se hace utilizando el atributo disabled del botón, que se establece condicionalmente según el valor de isLoading.

`Archivo: src/components/CounterComponent.vue`
~~~
<template>
  <div>
    <p>Contador: {{ counter }}</p>
    <p>Cuadrado del contador: {{ squareCount }}</p>
    <button @click="increment" :disabled="isLoading">Incrementar</button>
    <button @click="incrementRandom" :disabled="isLoading">
      Incrementar Aleatoriamente
    </button>
    <p v-if="isLoading">Cargando...</p>
  </div>
</template>

<script>
import { useCounterStore } from '../stores/counterStore';
import { mapState } from 'pinia';

export default {
  name: 'CounterComponent',
  computed: {
    ...mapState(useCounterStore, ['counter', 'squareCount', 'isLoading']),
  },
  methods: {
    increment() {
      const store = useCounterStore();
      store.increment();
    },
    incrementRandom() {
      const store = useCounterStore();
      store.incrementRandom();
    },
  },
};
</script>
~~~


## Organización Modular en Pinia

En lugar de utilizar un único store con múltiples módulos como en Vuex, en Pinia cada store actúa efectivamente como un "módulo" individual. Esto facilita la separación de diferentes partes del estado y la lógica de tu aplicación de manera más intuitiva y mantenible.

### Ejemplo: Creando Stores Separados en Pinia

Digamos que, además de un contador, tienes otra parte de tu aplicación que maneja la autenticación del usuario. En lugar de combinar todo en un único archivo o utilizar un sistema de módulos como en Vuex, simplemente crearías dos stores separados en Pinia:

`Archivo: src/stores/authStore.js`
~~~
import { defineStore } from 'pinia'

export const useAuthStore = defineStore('auth', {
  state: () => ({
    user: null,
  }),
  actions: {
    logIn(user) {
      this.user = user
    },
    logOut() {
      this.user = null
    }
  },
})
~~~

Cada uno de estos archivos define un store separado que maneja su propia parte del estado global de la aplicación. La lógica relacionada está contenida dentro de su respectivo store, haciendo que el código sea más fácil de entender y mantener.

### Acceso y Uso de Múltiples Stores en Componentes

En tus componentes Vue, puedes importar y usar estos stores según sea necesario, sin preocuparte por la configuración de espacios de nombres o la interacción entre módulos:

`Archivo: src/components/AuthComponent.vue`
~~~
<template>
  <div>
    <div v-if="user">
      <p>Bienvenido, {{ user.name }}!</p>
      <button @click="logOut">Cerrar Sesión</button>
    </div>
    <div v-else>
      <p>Por favor, inicie sesión.</p>
      <button @click="logIn">Iniciar Sesión</button>
    </div>
  </div>
</template>

<script>
import { useAuthStore } from '../stores/authStore'; // Asegúrate de que la ruta sea correcta

export default {
  name: 'AuthComponent',
  computed: {
    user() {
      return useAuthStore().user;
    },
  },
  methods: {
    logIn() {
      // Simula un usuario que inicia sesión
      const user = { id: '1', name: 'Usuario Ejemplo' };
      useAuthStore().logIn(user);
    },
    logOut() {
      useAuthStore().logOut();
    },
  },
};
</script>
~~~

Añadimos el componente al App.vue.

`Archivo: src/App.vue`

~~~
<script>
import UserAuth from './components/UserAuth.vue';
import CounterComponent from './components/counterComponent.vue';

export default {
  components: { CounterComponent, UserAuth },
}

</script>

<template>
    <CounterComponent /> <!-- Añade el componente counterComponent aquí -->
    <hr />
    <UserAuth /> <!-- Añade el componente counterComponent aquí -->
</template>

<style scoped>
/* Estilos */
</style>
~~~

## Persistencia del Estado con Pinia

La persistencia del estado es importante para para una experiencia de usuario fluida. Con Pinia y [pinia-plugin-persist](https://seb-l.github.io/pinia-plugin-persist/), logramos que el estado de la aplicación se mantenga entre sesiones con facilidad, mejorando la experiencia del usuario sin complicaciones de configuración. 

Vamos a explorar cómo utilizar este plugin para asegurar una transición suave de datos a través de sesiones de navegador.

### Instalar pinia-plugin-persist

~~~
npm install pinia-plugin-persist
~~~

### Configurar el Plugin en Pinia

Luego, necesitas configurar Pinia para usar este plugin. Esto se hace típicamente en tu archivo principal donde creas la instancia de Pinia y la aplicación Vue.

`Archivo: src/main.js`
~~~
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import piniaPluginPersist from 'pinia-plugin-persist'

import App from './App.vue'

const pinia = createPinia()
pinia.use(piniaPluginPersist)

createApp(App).use(pinia).mount('#app')
~~~

### Configurar la Persistencia en tu Store

Por último, configura tu store para que utilice la persistencia añadiendo la opción persist a la definición del store.

`Archivo: src/stores/counterStore.js`

~~~
import { defineStore } from 'pinia';
import { getRandomInt } from '../helpers/getRandomInt';

export const useCounterStore = defineStore('counterStore', {
  // Estado inicial del almacén
  state: () => ({
    counter: 0, // Inicializamos el contador en 0
    isLoading: false, // Nuevo estado para rastrear si estamos cargando un número aleatorio
  }),
  // Getters para el almacén
  getters: {
    squareCount: (state) => state.counter * state.counter,
  },
  // Acciones para mutar el estado
  actions: {
    increment() {
      this.counter++;
    },
    async incrementRandom() {
      this.isLoading = true; // Comenzamos a cargar
      const randomInt = await getRandomInt();
      this.counter += randomInt;
      this.isLoading = false; // Terminamos de cargar
    },
  },
  persist: {
    enabled: true, // Habilita la persistencia
    strategies: [
      {
        key: 'my-counter-store',
        storage: localStorage, // Define el storage a utilizar
        // storage: sessionStorage, // Usa sessionStorage para la persistencia
      },
    ],
  },
});
~~~

## Persistencia de Estado de Autenticación en Pinia

Esta sección se centrará en la gestión del estado de autenticación de usuarios en aplicaciones Vue utilizando Pinia. Mostraremos cómo el plugin pinia-plugin-persist facilita la persistencia de objetos, como el estado del usuario, directamente en sessionStorage o localStorage, sin la necesidad de serializar o deserializar manualmente los datos. 

### Ampliación del authStore para Incluir el Objeto de Usuario
Vamos a modificar el authStore para manejar el estado de autenticación del usuario mediante un objeto que incluya propiedades como id, name, email, y token.

`Archivo: src/stores/authStore.js`

~~~
import {defineStore} from "pinia"

export const useAuthStore = defineStore("authStore", {
state: () => ({
  user:{
    id:"",
    name:"",
    email:"",
    token:""
  },
}),
actions: {
  logIn(user) {
    this.user = user
  },
  logOut() {
    this.user =  {
      id:"",
      name:"",
      email:"",
      token:""
    }
  }
},
persist: {
  enabled:true,
  strategies: [
    {
      key:"auth",
      storage: localStorage,
      // storage: sessionStorage
    }
  ]
}
})
~~~

### Componente Vue para Autenticación

Para utilizar el authStore en un componente Vue, crearemos un componente que permita a los usuarios iniciar y cerrar sesión, mostrando la información relevante del usuario cuando esté autenticado. Este componente interactuará con el authStore para actualizar y reflejar el estado de autenticación del usuario.

`Archivo: src/components/UserAuth.vue`

~~~
<template>
  <div>
    <h2>User Authentication</h2>
    <!-- Mostrar información del usuario si está autenticado -->
    <div v-if="user.id">
      <p><strong>ID:</strong> {{ user.id }}</p>
      <p><strong>Name:</strong> {{ user.name }}</p>
      <p><strong>Email:</strong> {{ user.email }}</p>
      <button @click="logOut">Log Out</button>
    </div>
    <!-- Formulario simplificado de inicio de sesión para el ejemplo -->
    <div v-else>
      <button @click="logInAsExampleUser">Log In as Example User</button>
    </div>
  </div>
</template>

<script>
import { useAuthStore } from '../stores/authStore'

export default {
  computed: {
    // Accede al estado del usuario desde el authStore
    user() {
      return useAuthStore().user
    }
  },
  methods: {
    logInAsExampleUser() {
      // Datos de ejemplo para el usuario autenticado
      const exampleUserData = {
        id: '123',
        name: 'Jane Doe',
        email: 'jane.doe@example.com',
        token: 'exampleToken'
      }
      const authStore = useAuthStore()
      authStore.logIn(exampleUserData)
    },
    logOut() {
      const authStore = useAuthStore()
      authStore.logOut()
    }
  }
}
</script>
~~~

## Código fuente de la sección

En [este enlace](assets/codigo-vue/pinia.zip) puedes descargarte el código fuente de la aplicación finalizada.
