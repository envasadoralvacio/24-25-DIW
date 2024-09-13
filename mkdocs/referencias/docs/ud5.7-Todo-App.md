# Todo App

En esta sección realizaremos una práctica para ir aterrizando todo lo aprendido.

## Recursos

En la sección usaremos los siguientes recursos:

* [Bootstrap](https://getbootstrap.com/) 
* [SweetAlert2](https://sweetalert2.github.io/) 
* [Nano ID](https://www.npmjs.com/package/nanoid) 
* [Netlify](https://www.netlify.com/) 
 
## Formulario

Vamos a comenzar diseñando la estructura de la aplicación y nuestros primeros componentes, tendremos los siguientes componentes:
- App.jsx => Será el componente principal de la aplicación, donde vamos a llamar a los demás.
- Formulario.jsx => Formulario de añadir tareas.
- TodoList.jsx => Lista de tareas.
- Todo.jsx => Tarea.

La lógica vamos a ponerla en el componente que envuelve a todos los demás, App.jsx, con el objetivo de pasarle los estados y funciones al resto de los componentes mediante propps.


*App.jsx* 

~~~
import React, { useEffect, useState } from 'react'
import Formulario from './components/Formulario'
import TodoList from './components/TodoList'

const todosInitialState = JSON.parse(localStorage.getItem("todos")) || [ ]

const todoInitialState = {
  title: '',
  description: '',
  state: '',
  priority: false
}

const App = () => {

  const [todos, setTodos] = useState(todosInitialState)
  const [editionMode, setEditionMode] = useState(false)
  const [todo, setTodo] = useState(todoInitialState)

  useEffect(()=> {
    localStorage.setItem("todos", JSON.stringify(todos))
  })

  const addTodo = todo => {
    setTodos([...todos,todo])
  }

  const deleteTodo = id => {
    const newArray = todos.filter(todo => todo.id !== id)
    setTodos(newArray)
  }

  const updateTodoState = id => {
    const newArray = todos.map(todo => {
      if (todo.id == id) {
        todo.state = !todo.state
      }
      return todo
    })
    setTodos(newArray)
  }

  const handleEditionMode = todo => {
    setEditionMode(true)
    setTodo(todo)
  }

  const editTodo = id => {
    const newArray = todos.map(item => {
      if (item.id === todo.id) {
        item = todo
      }
      return item
    })
    setTodos(newArray)
    setEditionMode(false)
    setTodo(todoInitialState)
  }
  
  return (
    <div className='container mt-4'>
      <h1 className='text-center'>Todo App</h1>
      <hr />
      <div className='row mt-2'>
        < Formulario 
            todo={todo}
            setTodo={setTodo}
            addTodo = {addTodo} 
            editionMode={editionMode} 
            editTodo = {editTodo} />
        < TodoList 
            todos={todos} 
            deleteTodo={deleteTodo} 
            updateTodoState={updateTodoState}
            handleEditionMode={handleEditionMode}
          />
      </div>
    </div>
  )
}

export default App
~~~


*Formulario.jsx*

~~~
import { useState } from "react"
import Swal from "sweetalert2"

const Formulario = ({addTodo, editionMode, editTodo, todo, setTodo}) => {

const {title, description, priority, state} = todo

  const handleSubmit = e => {     
     e.preventDefault()
     if (title.trim() === "" || description.trim() === "") {
      return Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Algo va mal...!",
      });
     } 

     addTodo({
      ...todo,
      id:Date.now(),
      state: state == "completada"
     })
    console.log(`Enviando ${todo.title}, ${todo.description} y ${todo.state} al servidor...`)
  }

  const handleEdit = e => {
    e.preventDefault()
    editTodo()
  }
  
  const handlechange = e => {
    const {name, type, checked, value } = e.target
    setTodo({
      ...todo,
    [name]:type === "checkbox"? checked:value
    })
  }

  return (
      <div className='col-4'>
        <h3 className='text-center'>{editionMode ? 'Editar Tarea' : 'Agregar tareas'}</h3>
        <form onSubmit={editionMode ? handleEdit : handleSubmit}>
            <input 
                name="title" 
                placeholder="Introduce nombre de la tarea"
                type="text" 
                className="form-control mb-2" 
                value = {todo.title}
                onChange={handlechange}
            />
            <textarea
                name="description"
                placeholder="Introduce la descripcion"
                className="form-control mb-2" 
                value = {todo.description}
                onChange={handlechange}
            />
            <select 
                name="state" 
                className="form-control mb-2" 
                value = {todo.state} 
                onChange={handlechange}
            >
                <option value="pendiente">Pendiente</option>
                <option value="completada">Completada</option>
            </select>

            <div className="form-checked mb-2">
              <input 
                className="form-checked-input"
                type="checkbox"
                name="priority"
                id = "inputchecked"
                checked = {todo.priority}
                onChange={handlechange}
              />
              <label 
                htmlFor="inputchecked"
                className="form-checked-label"
              >
                Prioridad
              </label>
            </div>
            {editionMode ? (
              <button className='btn btn-warning w-100 mt-2'>
                Guardar Cambios
              </button>
            ) : (
              <button className='btn btn-dark w-100 mt-2'>Agregar</button>
            )}
        </form>
    </div>
  )
}

export default Formulario
~~~


*Todolist.jsx*

~~~
import React from 'react'
import  Todo  from './Todo'

const TodoList = ({todos, deleteTodo, updateTodoState, handleEditionMode}) => {
  return (
    <div className='col-8'>
    <h3 className='text-center'>Lista de tareas</h3>
      <ul>
        {
          todos.sort((a,b) => {
            return (b.priority - a.priority) - (b.state - a.state)*2
          })
          .map(todo=> (
            < Todo 
              key={todo.id} 
              todo={todo} 
              deleteTodo={deleteTodo} 
              updateTodoState={updateTodoState} 
              handleEditionMode={handleEditionMode} 
            />
          ))
        }
        {
          todos.length === 0 && (
            <li className='list-group-item text-center'>No hay tareas pendientes</li>
          )
        }
      </ul>
    </div>

  )
}

export default TodoList
~~~


*Todo.jsx*

~~~
import React from 'react'

const Todo = ({todo, deleteTodo, updateTodoState, handleEditionMode}) => {

  const {id, title, description, priority, state} = todo

  return (
      <li className='list-group-item'>
        <div className="d-flex justify-content-between align-items-start">
          <div>
            <h5 className={state ? 'completada' : undefined}> 
              {title}
            </h5>
            <p className={state ? 'completada' : undefined}> 
              {description}
            </p>
            <p className={state ? 'completada' : undefined}> </p>
            <div className='d-flex'>
              <button onClick={() => deleteTodo(id)} className='btn btn-sm btn-danger mr-2'>Eliminar</button>
              <button onClick={() => handleEditionMode(todo)} className='btn btn-sm btn-warning mr-2'>Editar</button>
              <button onClick={() => updateTodoState(id)} className='btn btn-sm btn-primary'>Actualizar Estado</button>
            </div>
          </div>
          <span className="badge badge-primary">
            {priority && "prioridad"}
            </span>
        </div>
      </li>
  )
}

export default Todo
~~~



## Deploy
Para hacer el deploy en primer lugar generamos los archivos estáticos de la aplicación ejecutando en el terminal del visual code:
~~~
npm run build
~~~

Una vez ejecutado se nos habrá generado una carpeta, *dist*, en el directorio, que es la que subimos a nuestro hosting.