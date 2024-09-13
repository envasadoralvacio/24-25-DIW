
# Fundamentos de React

## Escribir Marcado con JSX
- JSX combina HTML y JavaScript.
- Requiere etiquetas cerradas,  <img />.
- Las propiedades y métodos en JSX son camelCase.
- Envolver múltiples etiquetas en `<div>` o `<>`.


Las expresiones de JavaScript también se pueden incrustar dentro de JSX usando llaves, incluidas variables, funciones y propiedades.

```
const App = () => {
  return (
    <div>
        <p>Esto es un parrafo</p>
    </div>
  )
}

export default App
```
[Source](https://es.react.dev/learn#writing-markup-with-jsx)

Si tienes mucho HTML que convertir a JSX, puedes utilizar un [convertidor en línea](https://transform.tools/html-to-jsx).

## Añadir Estilos
- Uso de `className` para CSS.
- Escribir CSS en archivos separados.
- Ejemplo:
```
const Parrafo = () => {
  return (
    <>
        <p className="text-center">Esto es un parrafo</p>
    </>
  )
}
```

```
/* In your CSS */  
.text-center {
    text-align: center;
}
```

[Source](https://es.react.dev/learn#adding-styles)


## Interolación
- Incrustar variables en JSX con `{}`.
- Uso en marcado y atributos.
- Ejemplo:
```
const App = () => {

    const title = "Titulo desde React"
    const imagen = "src/assets/images/imagen1.jpg"
    const classTitle = "text-center"

  return (

    <div>
        <h1 className='text-center'>{title}</h1>
        <h1 className='text-center'>{title.toUpperCase()}</h1>
        <h1 className={classTitle}>{title.toUpperCase()}</h1>
        <h2>Componente variables {1+1}</h2>
        {/* <img src="src/assets/images/imagen1.jpg" alt="" /> */}
        <img src={imagen} alt="" />
        <img src={imagen} alt={`imagen - ${title}`} />
    </div>
  )
}

export default App
```

[Source](https://es.react.dev/learn#displaying-data)

## Componentes
- Componentes son bloques de UI.
- Funciones JavaScript que devuelven marcado.
- Ejemplo de declaración y anidación.

```
function MyButton() {  
  return (  
    <button>Soy un botón</button>  
  );  
}

export default function MyApp() {  
  return (  
    <div>  
      <h1>Bienvenido a mi aplicación</h1>  
      <MyButton />  
    </div>  
  );  
}
```
[Source](https://es.react.dev/learn#components)

## Renderizado Condicional
- Uso de `if` o operadores condicionales.
- Ejemplo con `if` y operador `?`:
```
let content;  
if (isLoggedIn) {  
  content = <AdminPanel />;  
} else {  
  content = <LoginForm />;  
}
return (<div>{content}</div>);
```
Si prefieres un código más compacto, puedes utilizar el operador ? condicional. A diferencia de if, funciona dentro de JSX:

```
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

Cuando no necesites la rama else, puedes también usar la sintaxis lógica &&, más breve:
```
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```
[Source](https://es.react.dev/learn#conditional-rendering)

## Renderizado de Listas
- Uso de `map()` para listas.
- Atributo `key` para elementos únicos.
- Ejemplo:
```
const App = () => {
  const frutas = ["🍐", "🍌", "🍎"];
  return (
    <div className="container">
      <ul>
        {frutas.map((fruta, index) => {
          return <li key={index}>{fruta}</li>;
        })}
      </ul>
    </div>
  );
};

export default App;
```
[Source](https://es.react.dev/learn#rendering-lists)

## Compartir Datos entre Componentes - props
- Las props se utilizan para enviar información al componente anidado.
- Ejemplo:
```
const MyButton = (props) => {
  return <button>{props.text}</button>;
};

const App = () => {
  const title = "Mi primero proyecto con React.js";
  return (
    <div className="container">
      <h1 className="text-primary">{title}</h1>
      <MyButton text="botón 1"/>
      <MyButton text="botón 2"/>
      <MyButton text="botón 3"/>
    </div>
  );
};

export default App;
```

[Source](https://es.react.dev/learn#sharing-data-between-components)

## PropTypes
- Proporciona una forma de documentar y validar las props que se esperan en un componente.
- Ayuda a evitar errores y facilita el desarrollo y mantenimiento del código.
- Ejemplo:

```
const MyButton = (props) => {
  return <button>{props.text}-{props.edad}</button>;
};

MyButton.propTypes = {
  text: PropTypes.string.isRequired,
  edad: PropTypes.number.isRequired,
};

const App = () => {
  const title = "Mi primero proyecto con React.js";
  return (
    <div className="container">
      <h1 className="text-primary">{title}</h1>
      <MyButton text="botón 1" edad={18}/>

    </div>
  );
};

export default App;
```

## Eventos
- Los eventos de React se nombran usando camelCase, en vez de minúsculas.
- Con JSX pasas una función como el manejador del evento, en vez de un string.
- Ejemplo:
```
const MyButton = () => {
  const handleClick = () => {
    console.log("me diste click");
  };

  return <button onClick={handleClick}>i'am a button</button>;
};
```
[Source](https://es.react.dev/learn#responding-to-events)

## Modularizar con Componentes

Los componentes permiten separar la interfaz de usuario en piezas independientes, reutilizables y pensar en cada pieza de forma aislada.

*components/MyButton.jsx*

```
const MyButton = () => {
  const handleClick = () => {
    console.log("me diste click");
  };

  return <button onClick={handleClick}>i'am a button</button>;
   };

export default MyButton;
```

*App.jsx*

```
import MyButton from "./components/MyButton";
const App = () => {
  return (
    <div className="container">
      <h1 className="text-primary">{title}</h1>
      <MyButton />
    </div>
  );
};

export default App;
```

## Actualizar la pantalla 

A menudo, querrás que tu componente «recuerde» alguna información y la muestre. Por ejemplo, quizá quieras contar el número de veces que hiciste clic en un botón. 

¿Qué pasa si hago esto? 

*ButtonState.jsx*

```
export default () => {
  let counter = 0;

  const handleClickIncrement = () => {
    counter++;
    console.log(counter);
  };

  return <button onClick={handleClickIncrement}>Counter: {counter}</button>;
};
```

*App.jsx*

```
import MyButton from "./components/MyButton";
const App = () => {
  return (
    <div className="container">
      <h1 className="text-primary">{title}</h1>
      <ButtonState />
    </div>
  );
};

export default App;
```

- Nada le indica a React que tenemos que volver a renderizar para pintar nuevamente button.
- Necesitamos un Hook que modifique el estado.

[Source](https://es.react.dev/learn#responding-to-events)


## Uso de Hooks

- Uso de `useState` para manejar estado.
- Ejemplo:
```
import { useState } from "react";

export default () => {
  const [counter, setCounter] = useState(0);

  const handleClickIncrement = () => {
    setCounter(counter + 1);
    //setCounter((prevCounter) => prevCounter + 1);
  };

  return <button onClick={handleClickIncrement}>Counter: {counter}</button>;
};

```

[Source](https://es.react.dev/learn#updating-the-screen)
