# UseEffect
El Hook de efecto nos permite llevar a cabo efectos secundarios en componentes funcionales.
Al usar este Hook, le estamos indicando a React que el componente tiene que hacer algo después de renderizarse. React recordará la función que le hemos pasado (nos referiremos a ella como nuestro “efecto”), y la llamará cuando se actualice el DOM.

**¿Se ejecuta useEffect después de cada renderizado?** ¡Sí! Por defecto se ejecuta después del primer renderizado y después de cada actualización.

~~~
import { useEffect, useState } from "react";

const App = () => {
    const [count, setCount] = useState(0);

    useEffect(() => {
        console.log("App mounted");
    });

    return (
        <>
            <h1>useEffect {count}</h1>
            <button onClick={() => setCount(count + 1)}>Increment</button>
        </>
    );
};

export default App;
~~~

**¿Por qué veo dos logs?**

Si ves que se repiten los console logs, es porque está activado strict mode en React. En producción no ocurrirá. Quizás esyaría bien desactivarlo en ésta práctica para estudiar bien los renderizados y que no sea confuso, pero solo para probar, realmente no dememos hacer una aplicación en react sin usar el modo estricto.

~~~
ReactDOM.createRoot(document.getElementById("root")).render(
      // <React.StrictMode>
        <App />
      // </React.StrictMode>
);
~~~
[Más info stric mode](https://es.react.dev/reference/react/StrictMode)

**¿Cómo podemos hacer que useEffect se ejecute solo una vez?** Como segundo argumento useEffect acepta un array de dependencias, si le pasamos un array vacío como segundo argumento ya sólo se ejecutará una vez al cargar la página.

~~~
useEffect(() => {
    console.log("App mounted");
}, []);
~~~

## fetch
Una de los usos más habituales de useEffect es la de hacer peticiones a una API.

~~~
import { useEffect, useState } from "react";

const App = () => {
    const [data, setData] = useState([]);

    useEffect(() => {
        fetch("https://jsonplaceholder.typicode.com/users")
            .then((response) => response.json())
            .then((data) => setData(data));
    }, []);

    return (
        <>
            <h1>useEffect</h1>
            <ul>
                {data.map((user) => (
                    <li key={user.id}>{user.name}</li>
                ))}
            </ul>
        </>
    );
};

export default App;
~~~

## async await
También podemos usar async await para hacer peticiones a una API, para ello necesitamos una función async.

~~~
useEffect(async () => {
    const response = await fetch("https://jsonplaceholder.typicode.com/users");

    const data = await response.json();
    setData(data);
}, []);
~~~
**¡¡Pero esto nos dará un error!!**

*Parece que escribiste useEffect(async () => ...) o devolviste una Promesa. En su lugar, escribe la función asíncrona dentro de tu efecto y llámala inmediatamente.*
*useEffect debe devolver una función de limpieza o nada.*

El problema aquí es que se supone que el primer argumento de useEffect es una función que no devuelve nada (undefined) o una función (para limpiar los efectos secundarios). ¡Pero una función asíncrona devuelve una Promesa, que no se puede llamar como una función! Simplemente no es lo que el useEffect espera para su primer argumento.

[Más info](https://devtrium.com/posts/async-functions-useeffect)

Solución:

~~~
import { useEffect, useState } from "react";

const App = () => {
    const [data, setData] = useState([]);

    useEffect(() => {
        const fetchData = async () => {
            try {
                const response = await fetch(
                    "https://jsonplaceholder.typicode.com/users"
                );
                if (!response.ok) {
                    throw "Error al conectar la API";
                }
                const data = await response.json();
                setData(data);
            } catch (error) {
                console.log(error);
                setData([]);
            }
        };
        fetchData();
    }, []);

    return (
        <>
            <h1>useEffect</h1>
            <ul>
                {data.map((user) => (
                    <li key={user.id}>{user.name}</li>
                ))}
            </ul>
        </>
    );
};

export default App;
~~~

## Extraer función
Parece que tiene poco sentido el que llamemos a la función fetchData automáticamente despues de declararla, vamos a sacar de useEffect la declaración de la función useFetch.

~~~
import { useEffect, useState } from "react";

const App = () => {
    const [data, setData] = useState([]);

    const fetchData = async () => {
        console.log("fetchData");
        try {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/users"
            );
            if (!response.ok) {
                throw "Error al conectar la API";
            }
            const data = await response.json();
            setData(data);
        } catch (error) {
            console.log(error);
            setData([]);
        }
    };

    useEffect(() => {
        fetchData();
    }, []);

    return (
        <>
            <h1>useEffect</h1>
            <ul>
                {data.map((user) => (
                    <li key={user.id}>{user.name}</li>
                ))}
            </ul>
        </>
    );
};

export default App;
~~~

**WARNING**. Pero, ahora nos encontramos con un problema. Si os fijáis, ahora cada vez que un componente se renderize se creará la función fetchData. Ojo, no se va a ejecutar porque la llamada está dentro del useEffect, pero si se va a cargar en memoria una y otra vez. Si no queremos que se cree cada vez, podemos usar useCallback.

Poniendo un console.log en el fetchData podemos comprobar el problema que acabamos de mencionar:

~~~
import { useEffect, useState } from "react";

const App = () => {
    console.log("App");

    const [data, setData] = useState([]);
    const [counter, setCounter] = useState(0);

    const fetchData = async () => {
        console.log("fetchData");
        try {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/users"
            );
            if (!response.ok) {
                throw "Error al conectar la API";
            }
            const data = await response.json();
            setData(data);
        } catch (error) {
            console.log(error);
            setData([]);
        }
    };

    useEffect(() => {
        fetchData();
    }, []);

    return (
        <>
            <h1>useEffect</h1>
            <button onClick={() => setCounter(counter + 1)}>
                Increment {counter}
            </button>
            <ul>
                {data.map((user) => (
                    <li key={user.id}>{user.name}</li>
                ))}
            </ul>
        </>
    );
};

export default App;
~~~

Si bien esto no es un problema en nuestra aplicación, es pequeña, si lo será en una aplicación grande.

## useCallBack
useCallback es un hook que nos permite memorizar una función. Esto quiere decir que si la función que le pasamos como argumento no ha cambiado, useCallback no la volverá a crear.

Necesitas pasar dos cosas a useCallback:

* Una definición de función que desea almacenar en caché.
* Una lista de dependencias.

~~~
import { useEffect, useState, useCallback } from "react";

const App = () => {
    const [data, setData] = useState([]);

    const fetchData = useCallback(async () => {
        console.log("fetchData");
        try {
            const response = await fetch(
                "https://jsonplaceholder.typicode.com/users"
            );
            if (!response.ok) {
                throw "Error al conectar la API";
            }
            const data = await response.json();
            setData(data);
        } catch (error) {
            console.log(error);
            setData([]);
        }
    }, []);

    useEffect(() => {
        fetchData();
    }, [fetchData]);

    return (
        <>
            <h1>useEffect</h1>
            <ul>
                {data.map((user) => (
                    <li key={user.id}>{user.name}</li>
                ))}
            </ul>
        </>
    );
};

export default App;
~~~

Otra solución:

~~~
import { useEffect, useState } from "react";

const fetchData = async (setData) => {
    console.log("fetchData");
    try {
        const response = await fetch(
            "https://jsonplaceholder.typicode.com/users"
        );
        if (!response.ok) {
            throw "Error al conectar la API";
        }
        const data = await response.json();
        setData(data);
    } catch (error) {
        console.log(error);
        setData([]);
    }
};

const App = () => {
    console.log("App");

    const [data, setData] = useState([]);
    const [counter, setCounter] = useState(0);

    useEffect(() => {
        fetchData(setData);
    }, []);

    return (
        <>
            <h1>useEffect</h1>
            <button onClick={() => setCounter(counter + 1)}>
                Increment {counter}
            </button>
            <ul>
                {data.map((user) => (
                    <li key={user.id}>{user.name}</li>
                ))}
            </ul>
        </>
    );
};

export default App;
~~~
En mi opinión, esta segunda solución no es muy adecuada, ya que pe perdemos la reusabilidad del componente. Lo cual, entre otras cosas, es uno de los fundamentos de React.

## custom react hook
Con el objetivo de poder reutilizar la llamada a la API cuantas veces sea necesario, lo ideal sería que la lleváramos a un [custom react hook](https://dev.to/techcheck/custom-react-hook-usefetch-eid). 


useFetch.jsx

~~~
import { useCallback, useEffect, useState } from "react";

export const useFetch = (url) => {
    console.log("useFetch");

    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    const getData = useCallback(async () => {
        try {
            const response = await fetch(url);
            if (!response.ok) {
                throw new Error("Error fetching data");
            }
            const data = await response.json();
            setData(data);
        } catch (error) {
            setError(error.message);
        } finally {
            setLoading(false);
        }
    }, [url]);

    useEffect(() => {
        getData();
    }, [getData]);

    return { data, loading, error };
};
~~~

App.jsx

~~~
import { useState } from "react";
import { useFetch } from "./hooks/useFetch";

const App = () => {
    const [counter, setCounter] = useState(0);

    const { data, loading, error } = useFetch(
        "https://jsonplaceholder.typicode.com/users"
    );

    if (loading) {
        return <h1>Loading...</h1>;
    }

    if (error) {
        return <h1>{error}</h1>;
    }

    return (
        <>
            <h1>useEffect</h1>
            <button onClick={() => setCounter(counter + 1)}>
                Increment {counter}
            </button>
            <ul>
                {data.map((user) => (
                    <li key={user.id}>{user.name}</li>
                ))}
            </ul>
        </>
    );
};

export default App;
~~~

## Recursos
* [{JSON} Placeholder (Free fake API for testing and prototyping)](https://jsonplaceholder.typicode.com/)
* [useEffect](https://es.react.dev/reference/react/useEffect)
* [useCallback](https://es.react.dev/reference/react/useCallback)
* [coffeebytes](https://coffeebytes.dev/react-usecallback-usememo-y-memo-diferencias-y-usos/)
* [devtrium](https://devtrium.com/posts/async-functions-useeffect)
* [stackoverflow](https://stackoverflow.com/questions/53332321/react-hook-warnings-for-async-function-in-useeffect-useeffect-function-must-ret)
