# React Router DOM

El "Client-Side Routing" (enrutamiento del lado del cliente) es una técnica utilizada en aplicaciones web modernas, donde el enrutamiento y la carga de diferentes vistas o componentes de la página se realizan enteramente en el navegador del usuario, sin necesidad de realizar peticiones adicionales al servidor para cada cambio de página.

[React Router DOM](https://reactrouter.com) es una librería de React que permite implementar enrutamiento del lado del cliente (client-side routing), esto se logra mediante la manipulación del historial del navegador y el uso de componentes de React que responden a cambios en la URL.

Algunos puntos clave sobre el React Router y el enrutamiento del lado del cliente son:

* **Mejora de la Experiencia del Usuario**: Al evitar recargas completas de la página, las transiciones entre diferentes vistas son más rápidas y fluidas, lo que mejora la experiencia del usuario.

* **Mantenimiento del Estado de la Aplicación**: Dado que la página no se recarga, el estado de la aplicación se mantiene. Esto es útil, por ejemplo, para mantener el estado de los formularios o la posición de desplazamiento en la página.

* **Rutas Dinámicas**: React Router permite definir rutas dinámicamente y cargar componentes en función de los parámetros de la URL, lo que facilita la creación de aplicaciones web complejas.

* **Integración con React**: Al ser una biblioteca diseñada específicamente para React, React Router se integra perfectamente con el ecosistema de React, incluyendo el uso de hooks y el contexto.

* **Optimización de Carga**: Con el enrutamiento del lado del cliente, solo se cargan los recursos necesarios para el componente o vista actual, lo que puede mejorar el rendimiento de la aplicación, especialmente en conexiones lentas o dispositivos con recursos limitados.

Manejo de URLs Amigables: React Router ayuda a manejar URLs amigables, lo cual es bueno para la usabilidad y el SEO (optimización en motores de búsqueda).

## Transiciones en una SPA (Single Page Application)

Veamos cómo afecta se manejan las transiciones de página y las solicitudes de datos en una aplicación SPA usando React y React Router.

* **Carga Inicial de la Aplicación**: La primera vez que un usuario accede a tu aplicación, se realiza una llamada al servidor para cargar los archivos necesarios de React (HTML, JavaScript, CSS, etc.). Esta es la única carga completa de la página que ocurre en el modelo de enrutamiento del lado del cliente.

* **Navegación entre Rutas**: Una vez que la aplicación está cargada, la navegación entre diferentes rutas (páginas o vistas) manejadas por React Router no implica llamadas adicionales al servidor para cargar la página. Las transiciones se manejan internamente por React, actualizando solo los componentes necesarios en el DOM (Document Object Model).

* **Llamadas API / Datos Dinámicos**: Si tus componentes React necesitan obtener datos del servidor (como datos de usuario, listas de productos, etc.), realizarán llamadas API al backend cada vez que sea necesario. Estas llamadas suelen ser solicitudes HTTP (como GET, POST, etc.) y se hacen independientemente del enrutamiento del lado del cliente.

* **Carga Perezosa (Lazy Loading)**: Si implementas la carga perezosa de componentes (usando React.lazy y Suspense, por ejemplo), React Router cargará dinámicamente solo los componentes necesarios para la ruta actual. Esto podría implicar solicitudes adicionales para cargar los archivos JavaScript correspondientes a esos componentes, pero solo cuando se accede a la ruta por primera vez.

Servicios Web o Recursos Externos: Cualquier llamada a servicios web, bases de datos, o recursos externos (como imágenes o archivos) desde tus componentes React implicará llamadas al servidor o a los servicios pertinentes.

## Ejemplo de uso
Para esta ejemplo vamos a usar una estructura de archivos y carpetas similar a la de un proyecto real.

Crearemos dentro del src, las siguientes carpetas: 

* **components**. Tal y como hemos hecho hasta ahora, tendremos una carpeta para almacenar nuestros componentes personalizados.
* **layouts**. La usaremos para almacenar los layouts de nuestro proyecto, puede ser uno o mas de uno.
* **router**. Guardaremos en esta carpeta los archivoas necesarios para el enrutamiento de la aplicación.
* **pages**. Finalmente, en pages tendremos las diferents páginas de nuestra aplicación.

Por ejemplo:

<center>
![](assets/ud5/router_folders.png)
</center>

### Instalación de react-router-dom

La versión que vamos a usar es **React Router v.6.4+**, dicha versión introdujo nuevos enrutadores complatibles las nuevas API de datos:

* createBrowserRouter

* createMemoryRouter

* createHashRouter

Para instalar la última versión:
~~~
npm install react-router-dom
~~~

Para instalar una versión específica:
~~~
npm install react-router-dom@6.4
~~~

### Crear enrutador
Para habilitar el enrutamiento del lado del cliente tenemos que crear un router de navegador y configurar nuestras rutas. 

El router que vamos a usar es uno de los que se han incorporado en la nueva versión, [createBrowserRouter](https://reactrouter.com/en/main/routers/create-browser-router). Este es el enrutador recomendado para todos los proyectos web de React Router, utiliza [DOM history API](https://developer.mozilla.org/en-US/docs/Web/API/History) para actualizar la URL y administrar la pila de historial.

También habilita la data API v6.4 como [loaders](https://reactrouter.com/en/main/route/loader), [actions](https://reactrouter.com/en/main/route/action), [fetchers](https://reactrouter.com/en/main/hooks/use-fetcher) y más. 

router/index.jsx

~~~
import { createBrowserRouter } from "react-router-dom";
import Home from "../pages/Home";
import About from "../pages/About";
import Blog from "../pages/Blog";

export const router = createBrowserRouter([
    {
        path: "/",
        element: <Home />,
    },
    {
        path: "/about",
        element: <About />,
    },
    {
        path: "/blog",
        element: <Blog />,
    },
]);
~~~

Incorporamos el RouterProvider al archivo main.jsx:
~~~
import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";

import { router } from "./router";
import { RouterProvider } from "react-router-dom";

ReactDOM.createRoot(document.getElementById("root")).render(
    <React.StrictMode>
        <RouterProvider router={router} />
    </React.StrictMode>
);
~~~

Ya tendríamos nuestra aplicación enrutada. El problema es que si accedemos a una ruta que no existe nos da un error no controlado, es decir no tenemos la típica página de 404. Esto ocurre porque ya no estamos usando al enrutación tradicional, sino una nuestra del lado de cliente. Por tanto, tenemos que controlar los errores en las rutas:

### useRouterError
Para poder controlar los errores en las rutas tenemos el hook [useRouterError](https://reactrouter.com/en/main/hooks/use-route-error), vamos a crearnos un componente nuevo que sea NotFound.

NotFound.jsx
~~~
import { useRouteError } from "react-router-dom";

const NotFound = () => {
    const error = useRouteError();
    console.log(error);

    return (
        <div>
            <h1>404</h1>
            <p>Page not found</p>
            <p>{error.statusText || error.message}</p>
        </div>
    );
};
export default NotFound;
~~~

router/index.jsx
~~~
export const router = createBrowserRouter([
    {
        path: "/",
        element: <Home />,
        errorElement: <NotFound />,
    },
    {
        path: "/about",
        element: <About />,
        errorElement: <NotFound />,
    },
    {
        path: "/blog",
        element: <Blog />,
        errorElement: <NotFound />,
    },
]);
~~~

### Rutas anidadas
layout/LayoutPublic.jsx
~~~
import { Outlet } from "react-router-dom";

const LayoutPublic = () => {
    return (
        <div>
            <nav>Navbar</nav>
            <Outlet />
            <footer>Footer</footer>
        </div>
    );
};
export default LayoutPublic;
~~~

router/index.jsx
~~~
export const router = createBrowserRouter([
    {
        path: "/",
        element: <LayoutPublic />,
        errorElement: <NotFound />,
        children: [
            {
                index: true,
                element: <Home />,
            },
            {
                path: "/about",
                element: <About />,
            },
            {
                path: "/blog",
                element: <Blog />,
            },
        ],
    },
]);
~~~

### Navbar
components/Navbar.jsx

~~~
import { Link } from "react-router-dom";
const Navbar = () => {
    return (
        <nav className="navbar navbar-dark bg-dark">
            <div className="container">
                <Link to="/" className="btn btn-outline-primary">
                    Home
                </Link>
                <Link to="/about" className="btn btn-outline-primary">
                    About
                </Link>
                <Link to="/blog" className="btn btn-outline-primary">
                    Blog
                </Link>
            </div>
        </nav>
    );
};

export default Navbar;
~~~

layout/LayoutPublic.jsx
~~~
import { Outlet } from "react-router-dom";
import Navbar from "../components/Navbar";

const LayoutPublic = () => {
    return (
        <div>
            <Navbar />
            <div className="container">
                <Outlet />
            </div>
            <footer className="container">Footer</footer>
        </div>
    );
};
export default LayoutPublic;
~~~

### NavLink
[NavLink](https://reactrouter.com/en/main/components/nav-link) es un componente especializado para crear enlaces de navegación. Este componente se utiliza para crear enlaces de navegación que se activan cuando la ruta coincide con la URL actual.

De forma predeterminada se utiliza la clase CSS active para marcar el enlace como activo.

components/Navbar.jsx
~~~
<NavLink to="/" className="btn btn-outline-primary">
    Home
</NavLink>
<NavLink to="/about" className="btn btn-outline-primary">
    About
</NavLink>
<NavLink to="/blog" className="btn btn-outline-primary">
    Blog
</NavLink>
~~~

*isActive*
~~~
<NavLink
    to="tasks"
    className={({ isActive }) => (isActive ? activeClassName : undefined)}
>
    Tasks
</NavLink>
~~~


### Loader y useLoaderData
El loader en React Router desempeña un papel importante en la carga y manejo de datos antes de que se renderice una ruta. 

* Carga de Datos Previa al Renderizado: El loader se ejecuta antes de que el componente de la ruta se muestre, cargando los datos necesarios para ese componente. Esto asegura que todos los datos estén disponibles antes de que el usuario vea la página.

* Separación de Responsabilidades: Mantiene el código de carga de datos separado del componente de la interfaz de usuario, lo que facilita la organización y el mantenimiento del código.

* Manejo Eficiente del Estado de Carga: Permite manejar de manera eficiente el estado de carga de los datos, mostrando indicadores de carga mientras los datos están siendo obtenidos, y luego renderizando el componente con los datos ya cargados.

* Mejora del Rendimiento y la Experiencia del Usuario: Al cargar los datos necesarios antes del renderizado del componente, se reduce el tiempo visible de carga y se evitan parpadeos en la interfaz, mejorando así la experiencia del usuario.

Vamos a ver cómo usar las API loader y useLoaderData para cargar y mostrar datos en nuestras rutas.


*Blog.jsx*
~~~
const Blog = () => {
    return "Blog";
};
export default Blog;

export const loaderBlogs = async () => {
    const data = await fetch("https://jsonplaceholder.typicode.com/posts");
    const blogs = await data.json();
    return { blogs };
};
~~~

Este loaderBlogs es un ejemplo de un loader:

* Qué Hace: Carga la lista de blogs desde una API antes de que el componente Blog se muestre.

* Cómo se Usa: Este loader se asocia con una ruta específica. Cuando la ruta /blog es accedida, React Router ejecuta loaderBlogs antes de renderizar el componente Blog.

* Ventajas: Gracias a este loader, el componente Blog tiene inmediatamente disponible los datos de los blogs al momento de su renderizado, evitando así el tener que manejar el estado de carga dentro del propio componente.


*router/index.jsx*
~~~
import Blog, { loaderBlogs } from "../pages/Blog";

{
    path: "/blog",
    element: <Blog />,
    loader: loaderBlogs
},
~~~

Utilizamos 'useLoaderData' para acceder a los datos cargados por 'loaderBlogs':

*Blog.jsx*
~~~
// Este componente muestra una lista de blogs.

import { Link, useLoaderData } from "react-router-dom";

const Blog = () => {
    const { blogs } = useLoaderData(); // Accede a los datos cargados
    return (
        <ul>
            {blogs.length > 0 ? (
                blogs.map((blog) => (
                    <li key={blog.id}>
                        <Link to={`/blog/${blog.id}`}>{blog.title}</Link>
                    </li>
                ))
            ) : (
                <li>No blogs found</li>
            )}
        </ul>
    );
};
export default Blog;

// Función loader: Carga datos desde una API externa
export const loaderBlogs = async () => {
    const response = await fetch("https://jsonplaceholder.typicode.com/posts");
    const blogs = await response.json(); // Convierte la respuesta en JSON
    return { blogs };
};
~~~

### Rutas con parámetros
Veamos cómo manejar rutas con parámetros, por ejemplo, para mostrar un post específico.

~~~
{
    path: "/blog/:id",
    element: <Post />,
},
~~~

*Post.jsx*
~~~
// Este componente muestra los detalles de un post específico.

const Post = () => {
    return "Post";
};
export default Post;

// Carga los datos de un post específico basado en el ID de la ruta
export const loaderPost = async ({ params }) => {
    const data = await fetch(
        `https://jsonplaceholder.typicode.com/posts/${params.id}`
    );
    const post = await data.json();
    return { post };
};
~~~

~~~
{
    path: "/blog/:id",
    element: <Post />,
    loader: loaderPost,
},
~~~

*Post.jsx*
~~~

import { useLoaderData } from "react-router-dom";

const Post = () => {
    const { post } = useLoaderData(); // Accede a los datos del post específico
    return (
        <div>
            <h1>{post.title}</h1>
            <p>{post.body}</p>
        </div>
    );
};
export default Post;

export const loaderPost = async ({ params }) => {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${params.id}`);
    const post = await response.json();
    return { post };
};
~~~

### useNavigation
El hook useNavigation nos proporciona herramientas para manejar y responder a cambios en la navegación dentro de tu aplicación.

* Acceso al Estado de Navegación: Permite a los componentes acceder al estado actual de la navegación. Esto incluye si la navegación está "idle" (inactiva), "loading" (cargando), o "submitting" (enviando datos).

* Control de Indicadores de Carga: Con useNavigation, puedes mostrar indicadores de carga o cualquier otra interfaz de usuario relevante durante las transiciones de navegación. Por ejemplo, puedes mostrar un mensaje de "Cargando..." o una animación de carga cuando el usuario navega de una página a otra, mejorando así la experiencia de usuario.

* Respuesta a Cambios en la Navegación: Permite a los componentes reaccionar a los cambios en el estado de navegación. Esto es útil, por ejemplo, para deshabilitar botones o cambiar la interfaz de usuario mientras se está cargando una nueva página o se están enviando datos a un servidor.

* Mejorar la Experiencia del Usuario: Al informar al usuario sobre el estado de navegación (por ejemplo, que una página está cargando), se mejora la transparencia y se reduce la confusión o la frustración del usuario.


*LayoutPublic.jsx*
~~~
// Este componente actúa como un layout para páginas públicas.
// Utiliza 'useNavigation' para mostrar indicadores de carga durante la navegación.

import { Outlet, useNavigation } from "react-router-dom";
import Navbar from "../components/Navbar";

const LayoutPublic = () => {
    const navigation = useNavigation(); // Accede al estado de navegación

    return (
        <div>
            <Navbar />
            <main className="container">
                {navigation.state === "loading" && (
                    <div className="alert alert-info my-5">Loading...</div> // Indicador de carga
                )}
                <Outlet /> // Renderiza el componente de la ruta actual
            </main>
            <footer className="container">Footer</footer>
        </div>
    );
};
export default LayoutPublic;

~~~

En este código, useNavigation se utiliza para mostrar un mensaje de "Loading..." cada vez que se está cargando una nueva ruta. Esto informa al usuario que la navegación está en proceso y mejora la experiencia de espera.


### Error data
Vamos a manejar los errores en las solicitudes, para darle mas robuztez a la aplicación.

*Post.jsx*

~~~
// Mejora el manejo de errores en la carga de datos del post

export const loaderPost = async ({ params }) => {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${params.id}`);

    if (!response.ok) {
        throw new Error(`Error: ${response.status} - ${response.statusText}`);
    }

    const post = await response.json();
    return { post };
};

~~~

*Podríamos, si queremos, usar el diseño del layout en error.*

~~~
export const router = createBrowserRouter([
    {
        path: "/",
        element: <LayoutPublic />,
        errorElement: <NotFound />,
        children: [
            {
                errorElement: <NotFound />,
                children: [
                    {
                        index: true,
                        element: <Home />,
                    },
                    {
                        path: "/about",
                        element: <About />,
                    },
                    {
                        path: "/blog",
                        element: <Blog />,
                        loader: loaderBlogs,
                    },
                    {
                        path: "/blog/:id",
                        element: <Post />,
                        loader: loaderPost,
                    },
                ],
            },
        ],
    },
]);

~~~

## What are good alternatives to data fetching in Effects? 

Escribir llamadas fetch dentro de los efectos (Effects) es un método común para obtener datos en aplicaciones completamente basadas en el cliente. Sin embargo, este enfoque manual tiene varias desventajas:

* **No Funcionan en el Servidor**: Los efectos no se ejecutan en el servidor. Esto significa que el HTML renderizado inicialmente en el servidor solo incluirá un estado de carga sin datos. El cliente tendrá que descargar todo el JavaScript y renderizar la aplicación solo para descubrir que ahora necesita cargar los datos. Esto no es eficiente.

* **Waterfalls de Red**: Hacer fetch directamente en los efectos puede crear "waterfalls" (cascadas) de red. Renderizas el componente padre, este obtiene algunos datos, luego renderiza los componentes hijos, y ellos comienzan a obtener sus datos. Si la red es lenta, esto es mucho más lento que obtener todos los datos en paralelo.

* **Falta de Precarga y Caché**: Obtener datos directamente en los efectos generalmente significa que no se pre-cargan o almacenan en caché los datos. Por ejemplo, si el componente se desmonta y luego se monta de nuevo, tendría que obtener los datos nuevamente.

* **No es Ergonómico**: Hay bastante código repetitivo involucrado en escribir llamadas fetch de manera que no sufran de problemas como condiciones de carrera.


Estas desventajas no son específicas de React, sino que se aplican a la obtención de datos al montar con cualquier biblioteca. La obtención de datos no es trivial de hacer bien, por lo que se recomiendan los siguientes enfoques:

* Usar Mecanismos Integrados de Frameworks: Si usas un framework, utiliza su mecanismo integrado de obtención de datos. Los [frameworks modernos](https://react.dev/learn/start-a-new-react-project#production-grade-react-frameworks) de React tienen mecanismos de obtención de datos integrados que son eficientes y no sufren de estas desventajas.

* Otra opción es usar o construir un caché del lado del cliente, para ello podmeos usar soluciones, como React Query, useSWR y React Router 6.4+. 

Fuente: [What are good alternatives to data fetching in Effects?](https://react.dev/reference/react/useEffect#what-are-good-alternatives-to-data-fetching-in-effects)


## Recursos
* [{JSON} Placeholder (Free fake API for testing and prototyping)](https://jsonplaceholder.typicode.com/)
* [React Router](https://reactrouter.com/en/main/start/overview)
* [Bootstrap](https://getbootstrap.com/)
