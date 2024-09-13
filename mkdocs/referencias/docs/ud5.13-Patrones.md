# Patrones de Diseño

Los patrones de diseño son soluciones repetibles para problemas comunes en el desarrollo de software, aceleran el proceso de desarrollo y hacen que el código sea más fácil de leer y mantener.

## Patrones de Diseño más Usados en React

* **Componentes Funcionales vs. Contenedores**: Diferencia entre componentes con estado (stateful) y sin estado (stateless). Los componentes funcionales siempre renderizan lo mismo o solo lo que se les pasa a través de props.

* **Patrón Compuesto**: Utilizado cuando dos o más componentes trabajan juntos, donde uno es el padre y los demás son hijos. Este patrón muestra relaciones entre componentes y permite que se comuniquen de manera flexible.

* **Renderizado Condicional**: Basado en el estado, se renderiza un JSX específico. Por ejemplo, mostrar diferentes mensajes al usuario según su estado de inicio de sesión.

* **Componentes Controlados**: Utilizado para manejar el estado de los formularios web. El componente controlado toma su estado a través de props y notifica cualquier cambio mediante callbacks.

* **Hooks de React**: Permiten usar React sin clases. Proporcionan funciones como useEffect y useState para manejar el estado y los efectos secundarios en los componentes funcionales.

* **Higher-Order Component (HOC)**: Un HOC toma un componente como argumento y, al devolverlo, agrega datos y funcionalidad al componente. Por ejemplo, en React con Redux, puedes pasar el componente a través de la función connect para inyectar datos desde el Redux store.


## Introducción a Higher-Order Components (HOC)
Los HOC son funciones que toman un componente y devuelven un nuevo componente con funcionalidades adicionales. Los HOCs son utilizados principalmente para reutilizar la lógica de componentes, manipular props, o abstraer el estado.

## Ejemplo Práctico: Aplicación Todo

Reescribir nuestra aplicación Todo para utilizar Higher-Order Components (HOCs) es posible, aunque al ser una aplicación bastante directa no sacaríamos mucho partido al uso de HOCs. Sin embargo, para propósitos educativos o de exploración, ciertamente podemos hacerlo.

Vamos a crear un HOC que podría ser útil en nuestra aplicación, un HOC que maneja el estado de "edición" de una tarea. Este HOC se encargará de gestionar si una tarea está en modo de edición y proporcionará la lógica y los métodos necesarios para cambiar entre modos.

Vamos a reescribir el componente Todo para usar un HOC que maneje su estado de edición. El HOC proporcionará props adicionales para gestionar este estado.

Primero, creamos el HOC: 

withEditState.jsx
~~~
import React, { useState } from 'react';

const withEditState = WrappedComponent => {
  return props => {
    const [isEditing, setIsEditing] = useState(false);
    const toggleEdit = () => setIsEditing(!isEditing);

    return (
      <WrappedComponent 
        {...props} 
        isEditing={isEditing} 
        toggleEdit={toggleEdit} 
      />
    );
  };
};

export default withEditState;

~~~ 

Ahora, modifiquemos el componente Todo para que utilice este HOC:
~~~
// Todo.jsx
import React from 'react';
import withEditState from './withEditState';

const Todo = ({ todo, deleteTodo, updateTodoState, toggleEdit, isEditing }) => {
  const { id, title, description, priority, state } = todo;

  return (
    <li className='list-group-item'>
      <div className="d-flex justify-content-between align-items-start">
        <div>
          {isEditing ? (
            <div>
              {/* Aquí iría tu formulario de edición */}
              <button onClick={toggleEdit} className='btn btn-sm btn-success mr-2'>Guardar</button>
            </div>
          ) : (
            <div>
              <h5 className={state ? 'completada' : undefined}>{title}</h5>
              <p className={state ? 'completada' : undefined}>{description}</p>
              <div className='d-flex'>
                <button onClick={() => deleteTodo(id)} className='btn btn-sm btn-danger mr-2'>Eliminar</button>
                <button onClick={toggleEdit} className='btn btn-sm btn-warning mr-2'>Editar</button>
                <button onClick={() => updateTodoState(id)} className='btn btn-sm btn-primary'>Actualizar Estado</button>
              </div>
            </div>
          )}
        </div>
        <span className="badge badge-primary">
          {priority && "prioridad"}
        </span>
      </div>
    </li>
  );
};

export default withEditState(Todo);
~~~

En este ejemplo, el HOC withEditState agrega lógica adicional al componente Todo, permitiendo que cada tarea maneje su propio estado de edición. Esto es útil si quieres que cada tarea en tu lista tenga su propia lógica de edición independiente del estado global de la aplicación.

Ten en cuenta que esta es una demostración del concepto de HOC. En la práctica real, hay que evaluar muy bien si el uso de un HOC es la mejor opción para un caso específico o si hay otras formas más eficientes de lograr la misma funcionalidad, como usando Hooks personalizados o Context API.