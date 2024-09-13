
# Context API

En esta sección aprenderemos a trabajar con [Context API](https://es.react.dev/reference/react/useContext). Según React, Context API “proporciona una forma de pasar datos mediante el árbol de componentes sin pasar props manualmente a todos los niveles”.

## ¿Por qué utilizar Context API?
En React normalmente se pasan los datos de padre a componente hijo, mediante propiedades (props). El problema es que esto, a veces, se vuelve insostenible cuando la aplicación crece. 

Context se diseñó con el objetivo de compartir datos considerados “globales” para un árbol de componentes de React. Algunos ejemplos en los que podría ser interesante su uso, por ejemplo, cuando tengamos en el árbol de componentes datos del usuario autenticado, la plantilla seleccionada, o el idioma preferido.

<center>
![](assets/ud5/context-props.png)
</center>

## Casos de Uso Comunes para Context
Algunos casos de uso en el que podemos aplicar la API de contexto:

* **Manejo de Temas**: Utilizar Context para manejar temas claros y oscuros en toda la aplicación.
* **Autenticación de Usuario**: Almacenar información del usuario autenticado y controlar el acceso a distintas partes de la aplicación.
* **Configuración de Idioma y Localización**: Gestionar las preferencias de idioma y localización del usuario.

## Rendimiento y Optimización con Context
* **Rendimiento**: Context puede causar renderizados innecesarios si no se utiliza cuidadosamente. Es importante comprender cuándo un componente necesita suscribirse a cambios de Context.
* **Optimización**: Dividir Context en contextos más pequeños según la necesidad, y usar React.memo para evitar renderizados innecesarios en componentes consumidores.

## ¿Cómo usar Context?
Context tiene las siguientes características y/o elementos principales:

* React.createContext
* Context.Provider
* Context.Consumer 

### React.createContext
CreateContext crea un objeto Context. Cuando React renderiza un componente que se suscribe a este objeto Context, este leerá el valor de contexto actual del Provider más cercano en el árbol.
Cada objeto Context viene con un componente Provider de React que permite que los componentes que lo consumen se suscriban a los cambios del contexto.

~~~
const MyContext = React.createContext()
~~~

### Provider
Cada objeto Context viene con un componente [Provider](https://es.react.dev/reference/react/useContext) de React que permite que los componentes que lo consumen se suscriban a los cambios del contexto.

El componente Provider acepta una prop value que se pasará a los componentes consumidores que son descendientes de este Provider.

~~~
<MyContext.Provider value={/* algún valor */}>
~~~

### useContext
[useContext](https://es.react.dev/reference/react/useContext) acepta un objeto de contexto (el valor devuelto de React.createContext) y devuelve el valor de contexto actual. El valor actual del contexto es determinado por la propiedad value del <MyContext.Provider> ascendentemente más cercano en el árbol al componente que hace la llamada.

~~~
const contextValue = React.MyContext()
~~~

Cuando el <MyContext.Provider> ascendentemente más cercano en el árbol se actualiza, el Hook activa una renderización con el value más reciente del contexto pasado a ese proveedor MyContext. 

El argumento enviado a useContext debe ser el objeto del contexto en sí mismo:

* Correcto: useContext(MyContext)
* Incorrecto: useContext(MyContext.Consumer)
* Incorrecto: useContext(MyContext.Provider)

Un componente que llama a useContext siempre se volverá a renderizar cuando el valor del contexto cambie. 

## Ejemplo de uso de context API
En la aplicación del ejempo vamos a necesitar crear un contexto user para que todos los elementos y páginas de la aplicación puedan saber si hay un usuario logueado o no, la información a mostrar no será la misma en ambos casos. Vamos a partir del siguiente [código base](assets/codigo-react/template-context-router-6.4.zip).

### CreateContext
Lo primero que tenemos que hacer es crear el contexto, habitualmente cada contexto se almacena en un archivo independiente y se almacenan dentro de un directorio que suele nombrarse como contexts. Es conveniente darle al archivo un nombre descriptivo, en caso le llamaremos UserContext.

context/UserContext.jsx
~~~
import { createContext, useContext } from "react";

// UserContext es el contexto que se crea. Este contexto eventualmente proporcionará y recibirá datos a/de los componentes que lo consuman.

export const UserContext = createContext(false);
~~~

Una vez tenemos creado el contexto UserContext podemos acceder al valor de este desde cualquier parte de la aplicación. Disponemos para ello de un hook de React, useContext.

components/Navbar.js
~~~
import React, { useContext } from 'react'
import { NavLink } from 'react-router-dom'
import { UserContext } from '../context/UserContext'

const Navbar = () => {
  console.log(useContext(UserContext))
  return (
    <nav>
      <NavLink to='/'> Home </NavLink> |
      <NavLink to='/dashboard'> Dashboard </NavLink>
    </nav>
  )
}

export default Navbar
~~~

Como vemos, ya podemos acceder al contexto, pero eso probablemente no es suficiente, necesitaremos que los componentes puedan tambien modificar del contexto. Para ello vamos a almacenar el contexto creado en un estado y pasar a los componentes tanto el estado como su función modificadora. Para hacerlo vamos a usar un componente [Provider](https://es.react.dev/reference/react/useContext#usecontext).

### Provider

context/UserContext.jsx

~~~
import { createContext, useState } from 'react'

export const UserContext = createContext()

const UserProvider = ({ children }) => {
  const [user, setUser] = useState(null)
  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  )
}

export default UserProvider
~~~

Vamos a analizar el código anterior:

* *UserProvider*: UserProvider es un componente que actúa como proveedor del contexto UserContext. Todo componente que necesite acceso al contexto debe estar dentro de este UserProvider.

* *useState para Manejar el Estado*: Aquí se utiliza el Hook useState para crear un estado llamado user. Inicialmente, este estado está establecido en false. setUser es una función que permite actualizar este estado.

* *Propagando el Contexto*: UserContext.Provider es un componente que hace que el valor del contexto ({ user, setUser }) esté disponible para todos los componentes hijos (denotados por {children}). Esto significa que cualquier componente dentro de UserProvider puede acceder al estado user y a la función setUser.

* *Value del Provider*: El value del UserContext.Provider es un objeto que contiene el estado user y la función setUser. Cualquier componente que acceda a UserContext podrá obtener user y llamar a setUser para actualizarlo

main.jsx

~~~
import React from 'react'
import ReactDOM from 'react-dom/client'
import { RouterProvider } from 'react-router-dom'
import UserProvider from './context/UserContext'
import { router } from './router'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <UserProvider>
      <RouterProvider router={router} />
    </UserProvider>
  </React.StrictMode>
)
~~~

Navbar.jsx

~~~
import React, { useContext } from 'react'
import { NavLink } from 'react-router-dom'
import { UserContext } from '../context/UserContext'

const Navbar = () => {
  console.log(useContext(UserContext)) // Podemos ver por consola que recibimos el objeto.
  return (
    <nav>
      <NavLink to='/'> Home </NavLink> |
      <NavLink to='/dashboard'> Dashboard </NavLink>
    </nav>
  )
}

export default Navbar
~~~

Ahora vemos que ya recibimos un objeto con el user y su función modificadora setUser, de esta forma des cualquier componente podremos modificar el valor del mismo.

Vamos a probarlo con el navbar:

Navbar.jsx

~~~
import React, { useContext } from 'react'
import { NavLink } from 'react-router-dom'
import { UserContext } from '../context/UserContext'

const Navbar = () => {
  const { user, setUser } = useContext(UserContext)
  return (
    <nav>
      <NavLink to='/'> Home </NavLink> |
          {user && (
            <>
              <NavLink to='/dashboard'>
                Dashboard
              </NavLink>
              <button onClick={() => setUser(null)}>
               Logout
              </button>
            </>
          )}
    </nav>
  )
}

export default Navbar
~~~


*Home.jsx*
~~~
import { useUserContext } from "../context/UserContext";

const Home = () => {
  const { user, setUser } = useUserContext();

  return (
    <>
      <h1>Home</h1>
      {!user && (
        <button onClick={() => setUser({ name: "Guillermo Ventanas" })}>Login</button>
      )}
    </>
  );
};

export default Home;
~~~

### Navbar
Una vez tenemos el login funcionando, utilizaremos el contexto para mostrar en el navbar solo las opciones pertinentes en función de si hay un usuario logueado. Por ejemplo, la opción dashboard no se mostrará si el usuario no está logueago. 

~~~
import React, { useContext } from 'react'
import { NavLink, useNavigate } from 'react-router-dom'
import { UserContext } from '../context/UserContext'

const Navbar = () => {

  const {user, setUser} = useContext(UserContext)
  const navigate = useNavigate()

  const closeSesion = () => {
    setUser(false)
    navigate("/")
  }

  const handleLogin = () => {
    setUser(true)
    navigate("/dashboard")
  }

  return (
    <nav>
        <NavLink to ="/">Home |</NavLink>
      {
        user?(
          <>
          <NavLink to ="/dashboard">Dashboard</NavLink>
          <button onClick={()=> closeSesion()}>Logout</button>
          </>
        ):(
          <button onClick={()=> handleLogin()}>Login</button>
        )
      }
    </nav>
  )
}

export default Navbar
~~~


### Private Routes
Las rutas protegidas son rutas que solo se pueden acceder si el usuario está autenticado. Si el usuario no está autenticado, se redirige a la página de inicio de sesión. En la aplicación tenemos un LayoutPrivate, esto nos permite proteger mas facilmente todo lo que necesitemos, ya que protegiendo dicho Layout quedaría protegido todo las páginas que mostremos bajo este Layout.

*LayoutPrivate.jsx*
~~~
import React, { useEffect, useContext} from 'react'
import { Outlet, useNavigate } from 'react-router-dom'
import { UserContext } from '../context/UserContext'

const LayoutPrivate = () => {
  const {user} = useContext(UserContext)
  const navigate = useNavigate()

  useEffect(() => {
    if (!user) {
      navigate('/login')
    }
  }, [user])

  return (
    <div>
      LayoutPrivate
      <Outlet />
    </div>
  )
}

export default LayoutPrivate
~~~

Una alternativa sin useEffect:

~~~
import React, { useContext } from 'react'
import { Navigate, Outlet } from 'react-router-dom'
import { UserContext } from '../context/UserContext'

const LayoutPrivate = () => {

  const {user} = useContext(UserContext)

  return ( 
    <>
      {
        user? <Outlet /> : <Navigate to ="/" />
      }
    </>
  )
}

export default LayoutPrivate
~~~

*Dashboard.jsx*
~~~
import React, { useContext } from 'react'
import { UserContext } from '../context/UserContext'


const Dashboard = () => {
  const { user, setUser } = useContext(UserContext)

  return (
    <>
      <h1>Dashboard</h1>
      <p>{user?.name}</p>
    </>
  );
};

export default Dashboard;
~~~


## Integración con APIs Externas y Manejo de Efectos
Ejemplo de cómo Context puede ser utilizado para manejar datos de APIs externas y cómo integrarlo con useEffect para operaciones asíncronas.

~~~
// Ejemplo: Uso de Context para gestionar datos de una API externa
const DataContext = React.createContext();

const DataProvider = ({ children }) => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetchData().then(setData); // fetchData es una función que llama a una API externa
  }, []);

  return <DataContext.Provider value={data}>{children}</DataContext.Provider>;
};
~~~ 

De esta manera nos aseguramos que los componentes consumidores puedan acceder a los datos cargados de la API externa de manera eficiente y centralizada 

## Alternativas a Context API

* **MobX**: Una alternativa que ofrece una gestión de estado más automática y reactiva. MobX se centra en la gestión reactiva del estado a través de observables, lo que puede simplificar el manejo de estados complejos en aplicaciones grandes.

* **Recoil**: Una biblioteca de gestión de estado de React más reciente, que proporciona una forma más atomizada y eficiente de manejar estados en aplicaciones grandes. Recoil permite manejar un estado global con un enfoque más granular, lo que puede ser beneficioso en aplicaciones con muchos estados interdependientes.

* **Zustand**: Una biblioteca de gestión de estado minimalista y eficiente para React. Zustand destaca por su simplicidad y facilidad de uso, ofreciendo una solución liviana para la gestión de estados sin la complejidad adicional de Redux. Es especialmente útil en proyectos donde se desea una solución rápida y sencilla para manejar estados compartidos.

* **Redux**: Redux es un “contenedor de estados” previsible y centralizado para aplicaciones de JavaScript. Nos proporciona un conjunto completo de herramientas para administrar el estado:
    * Viene con un depurador que nos permite "viajar en el tiempo" (historial de acciones y ver cómo el estado de cada uno de esos momentos).
    * Proporciona una API de middleware que le brinda acceso a herramientas como [redux-sagas](https://github.com/redux-saga/redux-saga/blob/main/README_es.md).
    * Sus enlaces de React evitan muchos renderizados innecesarios.
    * Como puede ver, el contexto no reemplaza a Redux. El contexto no le permitirá viajar en el tiempo con depuración, middleware configurable.

* [npmtrends; context vs mobx vs recoil vs redux vs zustand](https://npmtrends.com/context-vs-mobx-vs-recoil-vs-redux-vs-zustand)

## Recursos
* [Contex API](https://es.react.dev/reference/react/useContextl)
* [alternativa a redux](https://www.itdo.com/blog/react-context-api-puede-ser-alternativa-a-redux/)
* [context-api-vs-redux](https://daveceddia.com/context-api-vs-redux/)
* [when context replaces redux](https://frontarm.com/james-k-nelson/when-context-replaces-redux/)