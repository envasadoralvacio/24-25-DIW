# Formularios

Los formularios funcionan de manera un poco diferente a otros elementos DOM en React. En esta sección veremos:

- Formularios no controlados
- Formularios controlados

## Recursos

En la sección usaremos los siguientes recursos:

* [React Developer Tools](https://react.dev/learn/react-developer-tools)


## Formularios no controlados
En la mayoría de los casos se recomienda el uso de [componentes controlados](https://reactjs.org/docs/forms.html#controlled-components) para implementar formularios. En un componente controlado, los datos del formulario son manejados por un componente React.

La alternativa son los [componentes no controlados](https://reactjs.org/docs/uncontrolled-components.html), donde los datos del formulario son manejados por el propio DOM. 

Para escribir un componente no controlado, a la hora de acceder al componente no usaremos los selectores habituales de javascript, ya que estos pueden generar conflictos entre el DOM virtual de React y el DOM real, por ello usaremos una [referencia](https://react.dev/learn/referencing-values-with-refs) para obtener los valores del formulario desde el DOM.

* [refs](https://react.dev/learn/referencing-values-with-refs): Las referencias proporcionan una forma de acceder a los nodos del DOM o a elementos React creados en el método de renderizado.
* [useRef](https://react.dev/reference/react/useRef): useRef devuelve un objeto ref mutable cuya propiedad .current se inicializa con el argumento pasado (initialValue). El objeto devuelto se mantendrá persistente durante la vida completa del componente.

Veamos un ejemplo de formulario no controlado:

~~~
import React, { useRef } from 'react'

const FormNoControlados = () => {

    const formulario = useRef(null)
    //console.log(formulario)

    const handleSubmit = e => {     
        e.preventDefault()
        const datos = new FormData(formulario.current)
        console.log(...datos.entries())
        const objetoDatos = Object.fromEntries([...datos.entries()])
        console.log(objetoDatos);

        const {title, description, state} = objetoDatos
        
        if (!title.trim() || !description.trim() || !state.trim()) {
            console.log("error!!!!!!!!!!");
            return
        }

        console.log("Enviando objeto datos al server!!!!")

    }

  return (
    <div>
        <p>Form No Controlados</p>
        <form ref={formulario} onSubmit={handleSubmit}>
            <input 
                name="title"
                placeholder="Introduce nombre de la tarea"
                type="text" 
                className="form-control mb-2" 
                defaultValue="Tarea 1"
            />
            <textarea
                name="description"
                placeholder="Introduce la descripcion"
                className="form-control mb-2" 
                defaultValue="Descripción de la tarea 1"
            />
            <select 
                name="state" 
                className="form-control mb-2" 
                defaultValue="Pendiente"

            >
                <option value="pendiente">Pendiente</option>
                <option value="completada">Completada</option>
            </select>
            <button
                 type="submit" 
                 className='btn btn-primary'
            >
                Añadir
            </button>
        </form>
    </div>
  )
}

export default FormNoControlados
~~~


## Formularios controlados
Los componentes React que rendericen un formulario también pueden controlar lo que pasa en ese formulario, así podremos detectar que ocurre en los input input en tiempo real.

Veamos un ejemplo:

~~~
import React, { useState } from 'react'

const FormularioControlado = () => {
   
    // 1. Creamos un estado por cada input.
    const [title, setTitle] = useState("")
    const [description, setDescription] = useState("")
    const [state, setState] = useState("pendiente")


    const handleSubmit = (e) => {
        e.preventDefault()
        console.log(`Enviando ${title}, ${description}, y ${state} al servidor ...`)
    }

  return (
    <div>
        <p>Formularios Controlados</p>
        <form onSubmit={handleSubmit}>
            <input 
                name="title"
                placeholder="Introduce nombre de la tarea"
                type="text" 
                className="form-control mb-2" 
                // 2. Para asociar el estado con el valor del campo, usamos el value del input.
                value = {title}
                // 3. Tal y como hemos visto en el error de la consola, la propiedad value tiene que estar pendiente de un onChange.
                // onChange={e => console.log(e.target.value)} 

                onChange={e => setTitle(e.target.value)} 
            />
            <textarea
                name="description"
                placeholder="Introduce la descripcion"
                className="form-control mb-2" 
                value = {description}
                onChange={e => setDescription(e.target.value)} 
            />
            <select 
                name="state" 
                className="form-control mb-2" 
                value = {state}
                onChange={e => setState(e.target.value)} 

            >
                <option value="pendiente">Pendiente</option>
                <option value="completada">Completada</option>
            </select>
            <button
                 type="submit" 
                 className='btn btn-primary'
            >
                Añadir
            </button>
        </form>

    </div>
  )
}

export default FormularioControlado
~~~

Si tuvieramos un formulario con muchos inputs, la solución anterior sería algo tediosa. Vamos a mejorar el código:

~~~

import React, { useState } from 'react'

const FormularioControlado = () => {
   
    const [todo, setTodo] = useState({
        title: "Todo 01",
        description:"Descripción 01",
        state:"pendiente"
    })
 
    const handleSubmit = (e) => {
        e.preventDefault()
        console.log(`Enviando ${todo.title}, ${todo.description}, y ${todo.state} al servidor ...`)
    }

    const handlechange = e => {
        console.log(e.target.value)
        console.log(e.target.name)
        setTodo({
            ...todo, 
           [ e.target.name]:e.target.value
        })
    }

  return (
    <div>
        <p>Formularios Controlados</p>
        <form onSubmit={handleSubmit}>
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
            <button
                 type="submit" 
                 className='btn btn-primary'
            >
                Añadir
            </button>
        </form>

    </div>
  )
}

export default FormularioControlado
~~~

Añadimos ahora un checkbox, el cual tiene un [comportamiento algo diferente](https://react.dev/reference/react-dom/components/input#displaying-inputs-of-different-types):

~~~
import React, { useState } from 'react'

const FormularioCheckBox = () => {

    const estadoInicial = {
        title: "", 
        description: "", 
        state: "pendiente",
        todoCheck:false
    }
    const [todo, setTodo] = useState(estadoInicial)

    const [error, setError] = useState(false)
    

    const handleSubmit = (e) => {
        e.preventDefault()
        const {title, description} = todo

        if (!title.trim() || !description.trim()) {
            setError(true);
            console.log(error)
            return
        }
        console.log("Enviando objeto datos al server!!!!")
        setError(false)
        setTodo(estadoInicial)
    }

    // const handleChange = (e) => {
    //     //console.log(e.target)
    //     setTodo({
    //         ...todo,
    //         [e.target.name]: e.target.type === 'checkbox' ? e.target.checked : e.target.value
    //     })
    // }

    const handleChange = (e) => {
        const {name, value, checked, type} = e.target 
        setTodo({
            ...todo,
            [name]: type === 'checkbox' ? checked : value
        })
    }

  return (
    <div>
        <p>Formularios Controlados con CheckBox</p>

        {
            error ? <div className="alert alert-danger">Campos obligatorios</div> : null
        }

        <form onSubmit={handleSubmit}>
            <input 
                name="title"
                placeholder="Introduce nombre de la tarea"
                type="text" 
                className="form-control mb-2" 
                // onChange={e =>setTodo({...todo, title: e.target.value})}
                onChange={e=>handleChange(e)}
                value={todo.title}

            />
            <textarea
                name="description"
                placeholder="Introduce la descripcion"
                className="form-control mb-2" 
                onChange={e=>handleChange(e)}
                value={todo.description}
            />
            <select 
                name="state" 
                className="form-control mb-2" 
                onChange={e=>handleChange(e)}
                value={todo.state}
            >
                <option value="pendiente">Pendiente</option>
                <option value="completada">Completada</option>
            </select>
            <div className="form-check mb-2">
                <input 
                    className="form-check-input" 
                    type="checkbox" 
                    name="todoCheck"
                    checked={todo.todoCheck}
                    onChange={e => handleChange(e)}
                />
                <label 
                    className="form-check-label" 
                    htmlFor="flexCheckDefault">
                </label>
                Prioridad
            </div>
            <button
                 type="submit" 
                 className='btn btn-primary'
            >
                Añadir
            </button>
        </form>

    </div>
  )
}

export default FormularioCheckBox
~~~