# Drag & Drop

En esta sección realizaremos un Drag&Drop, para ello usaremos una librería externa. React hello-pangea/dnd es una librería que nos permite utilizar drag and drop en React. Esta diseñada para ser liviana, rápida y fácil de usar.

## Recursos

En la sección usaremos los siguientes recursos:

* [hello-pangea/dnd](https://github.com/hello-pangea/dnd)
* [documentación api](https://github.com/hello-pangea/dnd/tree/main/docs/api)
* [freecodecamp tutorial](https://www.freecodecamp.org/news/how-to-add-drag-and-drop-in-react-with-react-beautiful-dnd/)
* [forwarding-refs](https://es.reactjs.org/docs/forwarding-refs.html)
* [Tutorial: Yoelvis Mulen { code }](https://www.youtube.com/watch?v=bZsMWorjtFI)

## Instalación
Instalamos la librería hello-pangea/dnd.
~~~
# npm
npm install @hello-pangea/dnd --save
~~~

## Proyecto inicial
Para este ejemplo vamos a usar un proyecto de simple, ordenar un array con varias tareas.

Código inicial del App.jsx
~~~
import { useState } from "react";
import { DragDropContext, Droppable, Draggable } from "@hello-pangea/dnd";

function App() {
  const initialTodos = [
    { id: 1, text: "Aprender React" },
    { id: 2, text: "Aprender Js" },
    { id: 3, text: "Aprender Vue" },
  ];
  const [todos, setTodos] = useState(initialTodos);

  return (
    <div className="container">
      <h1>Drag & Drop</h1>
      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>{todo.text}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
~~~

index.css

~~~
li {
  border: 2px solid rgb(114, 168, 250);
  padding: 0.5rem;
}

ul {
  list-style: none;
  padding: 1rem;
  border: 2px solid rgb(30, 72, 210);
}
~~~

## Importamos los componentes de la librería
Lo primero que tenemos que hacer es importar los componentes que vamos a usar, en este caso:
~~~
import { DragDropContext, Droppable, Draggable } from "@hello-pangea/dnd";
~~~

Estos componentes van a servir para implementar el Drag&Drop:

* DragDropContext le dará a nuestra aplicación la capacidad de usar la librería.

* Droppable es un componente que le permite a la librería saber dónde se pueden soltar los elementos. Es como un contenedor que puede contener elementos que se pueden arrastrar. Podemos tener una o varias areas hello-pangea/dnd.

* Draggable es un componente que le permite a la librería saber qué elementos se pueden arrastrar.


App.jsx después de la importación:

~~~
import { useState } from "react";
import { DragDropContext, Droppable, Draggable } from "@hello-pangea/dnd";

function App() {
  const initialTodos = [
    { id: 1, text: "Aprender React" },
    { id: 2, text: "Aprender Js"},
    { id: 3, text: "Aprender Vue"},
  ];
  const [todos, setTodos] = useState(initialTodos);

  return (
    <div>
      <h1>Drag & Drop</h1>
      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>{todo.text}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
~~~

## DragDropContext
Ahora ya podemos empezar a trabajar con la librería, vamos a utilizar el componente <DragDropContext /> para envolver la parte de la aplicación en donde queremos que se habilite arrastrar y soltar (Drag & Drop).
~~~
import { useState } from "react";
import { DragDropContext, Droppable, Draggable } from "@hello-pangea/dnd";

function App() {
  const initialTodos = [
    { id: 1, text: "Aprender React" },
    { id: 2, text: "Aprender Js"},
    { id: 3, text: "Aprender Vue"},
  ];
  const [todos, setTodos] = useState(initialTodos);

  return (
    <DragDropContext>
      <h1>Drag & Drop</h1>
      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>{todo.text}</li>
        ))}
      </ul>
    </DragDropContext>
  );
}

export default App;
~~~

## Droppable
Ahora necesitamos crear un área Droppable, es decir nos permitirá proporcionar un área específica donde nuestros items puedan ser movidos.

~~~
import { useState } from "react";
import { DragDropContext, Droppable, Draggable } from "@hello-pangea/dnd";

function App() {
  const initialTodos = [
    { id: 1, text: "Aprender React" },
    { id: 2, text: "Aprender Js" },
    { id: 3, text: "Aprender Vue" },
  ];
  const [todos, setTodos] = useState(initialTodos);

  return (
    <DragDropContext>
      <h1>Drag & Drop</h1>
      <Droppable droppableId="droppable-1">
        {(droppableProvided) => (
          <ul
            ref={droppableProvided.innerRef}
            {...droppableProvided.droppableProps}
          >
            {todos.map((todo) => (
              <li key={todo.id}>{todo.text}</li>
            ))}
          </ul>
        )}
      </Droppable>
    </DragDropContext>
  );
}

export default App;
~~~

* droppableId: Es un identificador único que se usa para identificar esta instancia específica de Droppable.
* droppableProvider: Nos permite acceder a la información de estado de la librería.
* droppableProvider.innerRef: Esto creará una referencia (provided.innerRef) para que la librería acceda al elemento HTML del elemento de la lista.
* droppableProvider.droppableProps: [referencia API](https://github.com/hello-pangea/dnd/blob/main/docs/api/draggable.md)


## Draggable
Ahora usaremos el componente Draggable, que nuevamente, similar al componente Droppable, incluirá una función en la que pasaremos los accesorios a los componentes de nuestro elemento de lista.

~~~
import { useState } from "react";
import { DragDropContext, Droppable, Draggable } from "@hello-pangea/dnd";

function App() {
  const initialTodos = [
    { id: 1, text: "Aprender React" },
    { id: 2, text: "Aprender Js" },
    { id: 3, text: "Aprender Vue" },
  ];
  const [todos, setTodos] = useState(initialTodos);

  return (
    <DragDropContext>
      <h1>Drag & Drop</h1>
      <Droppable droppableId="droppable-1">
        {(droppableProvided) => (
          <ul
            ref={droppableProvided.innerRef}
            {...droppableProvided.droppableProps}
          >
            {todos.map((todo, index) => (
              <Draggable key={todo.id} draggableId={`${todo.id}`} index={index}>
                {(dragableProvided) => (
                  <li
                    ref={dragableProvided.innerRef}
                    {...dragableProvided.draggableProps}
                    {...dragableProvided.dragHandleProps}
                  >
                    {todo.text}
                  </li>
                )}
              </Draggable>
            ))}
          </ul>
        )}
      </Droppable>
    </DragDropContext>
  );
}

export default App;
~~~

* draggableId={todo.id} : Identificador único que se usa para identificar esta instancia específica de Draggable.
* index={index} : Es el índice de la lista de elementos que se está iterando. Se usa para determinar el orden de los elementos en la lista.
* draggableProvider.innerRef: Esto creará una referencia ( provided.innerRef) para que la librería acceda al elemento HTML del elemento de la lista.
* draggableProvider.draggableProps y draggableProvider.dragHandleProps: [referencia API](https://github.com/hello-pangea/dnd/blob/main/docs/api/draggable.md)



## droppableProvider.placeholder
Se puede utilizar un espacio reservado para mostrar dónde se colocará el elemento cuando se suelta.

~~~
import { useState } from "react";
import { DragDropContext, Droppable, Draggable } from "@hello-pangea/dnd";

function App() {
  const initialTodos = [
    { id: 1, text: "Aprender React" },
    { id: 2, text: "Aprender Js" },
    { id: 3, text: "Aprender Vue" },
  ];
  const [todos, setTodos] = useState(initialTodos);

  return (
    <DragDropContext>
      <h1>Drag & Drop</h1>
      <Droppable droppableId="droppable-1">
        {(droppableProvided) => (
          <ul
            ref={droppableProvided.innerRef}
            {...droppableProvided.droppableProps}
          >
            {todos.map((todo, index) => (
              <Draggable key={todo.id} draggableId={`${todo.id}`} index={index}>
                {(dragableProvided) => (
                  <li
                    ref={dragableProvided.innerRef}
                    {...dragableProvided.draggableProps}
                    {...dragableProvided.dragHandleProps}
                  >
                    {todo.text}
                  </li>
                )}
              </Draggable>
            ))}
            {droppableProvided.placeholder}
          </ul>
        )}
      </Droppable>
    </DragDropContext>
  );
}

export default App;
~~~

## Reordenar los elementos despues del drag & drop
Para resolverlo, podemos ver que DragDropContext toma un props onDragEnd que nos permitirá activar una función después de que se haya completado el arrastre.

~~~
    <DragDropContext onDragEnd={handleonDragEnd}>
~~~

Ahora implementamos la función handleonDragEnd y observamos que nos devuelve, veremos que nos da la posición iniicial y final del elemento movido, con eso podemos ya ordenar el array:

~~~
  const handleonDragEnd = (result) => {
    const { destination, source } = result;
    if (!destination) return;
    //console.log(result);
    const startIndex = source.index;
    const endIndex = destination.index;

    reorder(startIndex, endIndex);
  };

  const reorder = (startIndex, endIndex) => {
    const result = [...todos];
    //const removed = result.splice(startIndex, 1);
    const [removed] = result.splice(startIndex, 1);
    //console.log(removed);
    //console.log(result);

    result.splice(endIndex, 0, removed);
    //console.log(result);

    setTodos(result);
  };
~~~

* [splice js](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Array/splice): El método splice() cambia el contenido de un array eliminando elementos existentes y/o agregando nuevos elementos.
* Sintaxis: *array.splice(start[, deleteCount[, item1[, item2[, ...]]]])*

* start: Indice donde se comenzará a cambiar el array.
* deleteCount: Número de elementos (enteros) a eliminar, comenzando por start.
    * Si es 1, se eliminará un elemento.
    * Si es 0, no se eliminarán elementos. En este caso, se debe especificarse al menos un nuevo elemento.
* item1, item2, ...: Los elementos que se agregarán al array. Si no se especifican, splice() solo eliminará elementos del array.


## Código final

~~~
import { useState } from "react";
import { DragDropContext, Droppable, Draggable } from "@hello-pangea/dnd";

function App() {
  const initialTodos = [
    { id: 1, text: "Aprender React" },
    { id: 2, text: "Aprender Js" },
    { id: 3, text: "Aprender Vue" },
  ];
  const [todos, setTodos] = useState(initialTodos);

  const handleonDragEnd = (result) => {
    const { destination, source } = result;
    if (!destination) return;
    const startIndex = source.index;
    const endIndex = destination.index;
    reorder(startIndex, endIndex);
  };

  const reorder = (startIndex, endIndex) => {
    const result = [...todos];
    const [removed] = result.splice(startIndex, 1);
    result.splice(endIndex, 0, removed);
    setTodos(result);
  };

  return (
    <DragDropContext onDragEnd={handleonDragEnd}>
      <h1>Drag & Drop</h1>
      <Droppable droppableId="droppable-1">
        {(droppableProvided) => (
          <ul
            ref={droppableProvided.innerRef}
            {...droppableProvided.droppableProps}
          >
            {todos.map((todo, index) => (
              <Draggable key={todo.id} draggableId={`${todo.id}`} index={index}>
                {(dragableProvided) => (
                  <li
                    ref={dragableProvided.innerRef}
                    {...dragableProvided.draggableProps}
                    {...dragableProvided.dragHandleProps}
                  >
                    {todo.text}
                  </li>
                )}
              </Draggable>
            ))}
            {droppableProvided.placeholder}
          </ul>
        )}
      </Droppable>
    </DragDropContext>
  );
}

export default App;
~~~