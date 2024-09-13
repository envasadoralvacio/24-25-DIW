# Mi primer proyecto

## Componentes

Los [componentes](https://vuejs.org/guide/essentials/component-basics.html) nos permiten dividir la interfaz de usuario en piezas independientes y reutilizables, y pensar en cada pieza de forma aislada. Es común que una aplicación se organice en un árbol de componentes anidados:

<center>

![](assets/ud8/components.png)

</center>

## Mi primer componente

Normalmente cada componente Vue suele implementarse en un archivo dedicado usando la extensión .vue, esto se le conoce como [componente de un solo archivo](https://vuejs.org/guide/scaling-up/sfc.html) (SFC, por sus siglas en inglés).

Los SFC son un formato de archivo especial que nos permite encapsular la plantilla, la lógica y el estilo de un componente de Vue en un solo archivo. Aquí hay un ejemplo de SFC:

Vamos a definir el primer componente de nuestra aplicación, counter.vue, dentro de la carpeta components:

Counter.vue

```
<template>
  <div>
    <h2>Counter</h2>
    <p>10</p>
  </div>
</template>

<script>
  export default {

  }
</script>

<style scoped>

</style>

```

Los bloques template, script y style encapsulan y colocan la vista, la lógica y el estilo de un componente en el mismo archivo. Podemos ver mas información sobre la [especificación de sintaxis SFC en la documentación de vue](https://vuejs.org/api/sfc-spec.html#overview).

Si cargamos la aplicación vemos que el componente no se muestra, tenemos ahora que relacionarlo con nuesto App.vue para que se muestre en la aplicación:

```
<template>
  <h1>Mi primera App</h1>
  <Counter />
</template>

<script>
import Counter from './components/Counter.vue'

export default {
  components: { Counter },
}
</script>

<style scoped></style>
```

Si refrescamos el navegador, vemos que ahora nuestro nuevo componente si se está mostrando en la aplicación.

## Estado del componente

Vamos a declarar un estado local, counter, [utilizando la función data de la misma manera que lo hicimos anteriormente en el apartado de fundamentos](https://cautious-barnacle-l82lz13.pages.github.io/ud8.2-Fundamentos-Vue/#declaracion-de-estado-reactivo). Esta función va a devolver un objeto y cada una de las propiedades que tenga declaradas serán reactivas, es decir vue estará pendiente de los cambios que sufran y reaccionara a estos.

Counter.vue

```
<template>
  <div>
    <h2>Counter</h2>
    <p>{{counter}}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        // Cualquier propiedad que declaremos aquí será reactiva.
        counter: 5,
      }
    }
  }
</script>

<style scoped>

</style>
```

Vamos a realizar una pequeña operación matemática con el estado que hemos creado.

Counter.vue

```
<template>
  <div>
    <h2>Counter</h2>
    <p>{{ counter }} <sup>2</sup>= {{ counter * counter }}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        // Cualquier propiedad que declaremos aquí será reactiva.
        counter: 5,
      }
    }
  }
</script>

<style scoped>

</style>
```

Las expresiones en el template son muy convenientes, pero están diseñadas para operaciones simples. Poner demasiada lógica en las templates puede hacerlos grandes, complejos y difíciles de mantener. En este punto, el template ya no es simple y declarativo.

Para evitar esto posiblemente lo primero que se nos ocurriría sería crearnos un método que realice la operación.

Counter.vue

```
<template>
  <div>
    <h2>Counter</h2>
    <p>{{ counter }} <sup>2</sup>= {{ getSquare() }}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        // Cualquier propiedad que declaremos aquí será reactiva.
        counter: 5,
      }
    },
    methods: {
      getSquare() {
        console.log('getSquareValue')
        return this.counter * this.counter
      }
    }
  }
</script>

<style scoped>

</style>
```

Así minimizamos la lógica en el template, pero vamos a suponer que necesitamos realizar dicha operación vaias veces:

Counter.vue

```
<template>
  <div>
    <h2>Counter</h2>
    <p>{{ counter }} <sup>2</sup>= {{ getSquare() }}</p>
    <p>{{ counter }} <sup>2</sup>= {{ getSquare() }}</p>
    <p>{{ counter }} <sup>2</sup>= {{ getSquare() }}</p>
    <p>{{ counter }} <sup>2</sup>= {{ getSquare() }}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        // Cualquier propiedad que declaremos aquí será reactiva.
        counter: 5,
      }
    },
    methods: {
      getSquare() {
        console.log('getSquareValue')
        return this.counter * this.counter
      }
    }
  }
</script>

<style scoped>

</style>
```

Si nos fijamos en la consola, como es de esperar, el código se está ejecutando varias veces. En este caso, una operación matemática simple, no supone un gran problema, pero en otras ocasiones si lo será ya que estamos ocupando un espacio de memoria innecesariamente.

A continuación veremos como darle solución a esto.

## Propiedades computadas

Las propiedades computadas o computed properties son una característica muy interesante de Vue. Básicamente una computada es una variable, la diferencia con las variables de Vue es que las computadas normalmente transforman la variable o hacen algún tipo de cálculo antes de devolverla.

Vamos a repetir el código anterior, pero usando una propiedad computada en vez de un método.

Counter.vue

```
<template>
  <div>
    <h2>Counter</h2>
    <p>{{ counter }} <sup>2</sup>= {{ getSquareComputed }}</p>
    <p>{{ counter }} <sup>2</sup>= {{ getSquareComputed }}</p>
    <p>{{ counter }} <sup>2</sup>= {{ getSquareComputed }}</p>
    <p>{{ counter }} <sup>2</sup>= {{ getSquareComputed }}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        // Cualquier propiedad que declaremos aquí será reactiva.
        counter: 5,
      }
    },
    methods: {
      getSquare() {
        console.log('getSquareValue')
        return this.counter * this.counter
      }
    },
    computed: {
      getSquareComputed() {
        console.log('getSquareValueComputed')
        return this.counter * this.counter
      },
  },
  }
</script>

<style scoped>

</style>
```

Si ahora nos fijamos en la consola, vemos que el código se ha ejecutado una sola vez.

La diferencia entre ambos códigos, usar un método y utilizar las propiedades computadas es que sería más óptimo trabajar con éstas, ya que se cachea su valor y al contrario de los métodos, no se llaman cada vez que se renderiza la plantilla a menos que cambie su valor.

## Propiedes computadas vs métodos

La diferencia fundamental es la reactividad. Las computadas son reactivas, como las variables, mientras que los métodos no lo son.

Que las computadas sean reactivas quiere decir que cuando el valor del data cambie, la computada automáticamente se va a ejecutar y va a devolver el nuevo valor.

Las computadas NO pueden recibir parámetros de entrada, mientas que los métodos sí. Las computadas siempre tienen que hace return de un valor, mientras que los métodos puede que no devuelvan nada.

Otra diferencia es que las propiedades computadas tienen caché, es decir, utilizar propiedades computadas es más óptimo porque si Vue detecta que la computada va a devolver el mismo valor, no ejecutará la computada ahorrando cálculos. Los métodos se ejecutan siempre cada vez aunque el resultado sea el mismo.

Por último decir que es normal llamar a las computadas desde dentro de los métodos, mientras que ejecutar métodos dentro de computadas no se recomienda (por lo de la caché), es mejor busca otra alternativa para no tener que hacer eso.

## Incrementar y decrementar

```
<template>
  <div>
    <h2>Counter</h2>
    <p>{{ counter }} <sup>2</sup>= {{ getSquareComputed }}</p>
    <div>
      <button v-on:click="decrease">-1</button>
      <button v-on:click="increase">+1</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Cualquier propiedad que declaremos aquí será reactiva.
      counter: 5,
    }
  },
  methods: {
    getSquare() {
      return this.counter * this.counter
    },
    increase() {
      this.counter++
    },
    decrease() {
      this.counter--
    },
  },
  computed: {
    getSquareComputed() {
      return this.counter * this.counter
    },
  },
}
</script>

<style scoped></style>

```

## Properties - props

Las [properties o props](https://vuejs.org/guide/components/props.html#props-declaration) son atributos HTML personalizados que podemos registrar en nuestros componentes, lesto nos permite pasar datos a nuestros componentes.

App.vue

```
<template>
  <h1>Mi primera App</h1>
  <Counter title="Primer contador" />
  <Counter />
</template>

<script>
import Counter from './components/Counter.vue'

export default {
  components: { Counter },
}
</script>

<style scoped></style>
```

Counter.vue

```
<template>
  <h2>{{ title }}</h2>
  <p>{{ counter }} <sup>2</sup>= {{ getSquareComputed }}</p>
  <div>
    <button v-on:click="decrease">-1</button>
    <button v-on:click="increase">+1</button>
  </div>
</template>

<script>
export default {
  props: ['title'],
  data() {
    return {
      // Cualquier propiedad que declaremos aquí será reactiva.
      counter: 5,
    }
  },
  methods: {
    getSquare() {
      console.log('getSquareValue')
      return this.counter * this.counter
    },
    increase() {
      this.counter++
    },
    decrease() {
      this.counter--
    },
  },
  computed: {
    getSquareComputed() {
      console.log('getSquareValueComputed')
      return this.counter * this.counter
    },
  },
}
</script>

<style scoped></style>

```

## Otras formas de definir las props

Además de declarar accesorios con notación de array, también podemos hacerlo usando sintaxis de objeto:

Counter.vue

```
<template>
  <h2>{{ title }}</h2>
  <p>{{ counter }} <sup>2</sup>= {{ getSquareComputed }}</p>
  <div>
    <button v-on:click="decrease">-1</button>
    <button v-on:click="increase">+1</button>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    }
  data() {
    return {
      // Cualquier propiedad que declaremos aquí será reactiva.
      counter: 5,
    }
  },
  methods: {
    getSquare() {
      console.log('getSquareValue')
      return this.counter * this.counter
    },
    increase() {
      this.counter++
    },
    decrease() {
      this.counter--
    },
  },
  computed: {
    getSquareComputed() {
      console.log('getSquareValueComputed')
      return this.counter * this.counter
    },
  },
}
</script>

<style scoped></style>

```

En la sintaxis de objeto, la clave es el nombre de la prop y valor es el tipo esperado.

Esto no sólo documenta el componente, sino que también advertirá a otros desarrolladores que utilicen el componente (en la consola del navegador) si pasan el tipo incorrecto.

## Validación de las props

En Vue podemos establecer raquisitos para los props que reciben nuestros componentes, es decir podemos aplicar [validaciones](https://vuejs.org/guide/components/props.html#prop-validation). Ya hemos visto en el punto anterior, por ejemplo, como validar el tipo del dato que reciben. Si no se cumple un requisito, Vue avisará en la consola de JavaScript del navegador. Esto es especialmente útil cuando se desarrolla un componente destinado a ser utilizado por otros.

Para especificar validaciones de props, usaremos la notación de objeto en lugar de la de array. Por ejemplo:

Counter.vue

```
<template>
  <h2>{{ customTitle }}</h2>

  <p>{{ counter }} <sup>2</sup>= {{ getSquareComputed }}</p>

  <div>
    <button v-on:click="decrease">-1</button>
    <button v-on:click="increase">+1</button>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    start: {
      type: Number,
      // required: true,
      default: 5,
    },
  },
  data() {
    return {
      // Cualquier propiedad que declaremos aqué será reactiva.
      // counter: 5,
      counter: this.start,
    }
  },
  methods: {
    getSquare() {
      console.log('getSquareValue')
      return this.counter * this.counter
    },
    increase() {
      this.counter++
    },
    decrease() {
      this.counter--
    },
  },
  computed: {
    getSquareComputed() {
      console.log('getSquareValueComputed')
      return this.counter * this.counter
    },
    customTitle() {
      return this.title || 'Counter'
    },
  },
}
</script>

<style scoped></style>

```

## Validaciones pesonalizadas

Podemos agregar validaciones personalizadas en función de nuestra necesidad. Por ejemplo, vamos a imaginar que necesitamos que el valor de start sea mayor o igual que 0.

Counter.vue

```
<template>
  <h2>{{ customTitle }}</h2>

  <p>{{ counter }} <sup>2</sup>= {{ getSquareComputed }}</p>

  <div>
    <button v-on:click="decrease">-1</button>
    <button v-on:click="increase">+1</button>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    start: {
      type: Number,
      // required: true,
      default: 5,
      validator(value) {
        return value >= 0
      },
    },
  },
  data() {
    return {
      // Cualquier propiedad que declaremos aqué será reactiva.
      // counter: 5,
      counter: this.start,
    }
  },
  methods: {
    getSquare() {
      console.log('getSquareValue')
      return this.counter * this.counter
    },
    increase() {
      this.counter++
    },
    decrease() {
      this.counter--
    },
  },
  computed: {
    getSquareComputed() {
      console.log('getSquareValueComputed')
      return this.counter * this.counter
    },
    customTitle() {
      return this.title || 'Counter'
    },
  },
}
</script>

<style scoped></style>

```

Si el valor que recibe como prop es menor que 0, nos dará un mensaje de error en la consola del navegador.

En la guía de Vue podemos encontrar [varias validaciones más](https://vuejs.org/guide/components/props.html#prop-validation). Por ejemplo:

```
export default {
  props: {
    // Basic type check
    //  (`null` and `undefined` values will allow any type)
    propA: Number,
    // Multiple possible types
    propB: [String, Number],
    // Required string
    propC: {
      type: String,
      required: true
    },
    // Number with a default value
    propD: {
      type: Number,
      default: 100
    },
    // Object with a default value
    propE: {
      type: Object,
      // Object or array defaults must be returned from
      // a factory function. The function receives the raw
      // props received by the component as the argument.
      default(rawProps) {
        return { message: 'hello' }
      }
    },
    // Custom validator function
    propF: {
      validator(value) {
        // The value must match one of these strings
        return ['success', 'warning', 'danger'].includes(value)
      }
    },
    // Function with a default value
    propG: {
      type: Function,
      // Unlike object or array default, this is not a factory function - this is a function to serve as a default value
      default() {
        return 'Default function'
      }
    }
  }
}

```
