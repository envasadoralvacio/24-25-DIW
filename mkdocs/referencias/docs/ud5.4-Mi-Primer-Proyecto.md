# Mi primer proyecto

En este apartado se introducen algunos recursos útiles para iniciar tu primer proyecto en React.

## ¿Que voy a necesitar para trabajar con React?
Es necesario contar con:

* [Node js](https://nodejs.org/es/), un entorno en tiempo de ejecución multiplataforma, de código abierto, para la capa del servidor (pero no limitándose a ello) basado en el lenguaje de programación JavaScript.

* [npm](https://www.npmjs.com/) (Node Package Manager), es un gestor de paquetes desarrollado en su totalidad bajo el lenguaje JavaScript por Isaac Schlueter, a través del cual podemos obtener cualquier librería con tan solo una sencilla línea de código.


## Extensiones Visual Code Studio
A continuación se listan algunas extensiones que pueden resultar muy útiles para trabajar con React en éste editor.

* [ES7 React/Redux/React-Native/JS snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)

## ¿Cómo iniciar un proyecto React?
Tenemos dos opciones:

* Create React App (CRA)
* Vite

## Create React App

El paquete Create React App fue desarrollado por facebook, es un entorno que viene preconfigurado con todo lo necesario para crear una aplicación React.

Herramientas como webpack o Babel están ya preconfigurados y nos podemos despreocupar de ellas.

[https://create-react-app.dev/](https://create-react-app.dev/)

### ¿Cómo creamos una app con CRA?
~~~
npx create-react-app my-app
cd my-app
code .
npm start
~~~
Wualá ya tendrás tu aplicación funcionando y ejecutada en Visual Studio Code.


## Vite
[Vite](https://vitejs.dev/) es una herramienta de que te ayudará a crear proyectos (sin atarte a ningún framework concreto) y que su desarrollo y construcción final sea lo más sencilla posible.

Está desarrollada por Evan You, el creador de Vue.

Actualmente, Vite soporta tanto proyectos vanilla (sin utilizar frameworks), como proyectos utilizando Vue, React, Preact o Lit-element (tanto en versión Javascript, como Typescript). Vite trabaja con [Rollup](https://rollupjs.org/guide/en/) como empaquetador.

Las [templates](https://github.com/vitejs/awesome-vite#templates)
de Vite, sirven para proporcionar puntos de partida preconfigurados para diferentes tipos de proyectos web. Estas templates incluyen una configuración inicial que es específica para un marco o una pila de tecnología en particular.


### ¿Cómo creamos una app con Vite?

~~~
npm create vite@latest
code .
npm start
~~~
Wualá ya tendrás tu aplicación funcionando y ejecutada en Visual Studio Code.

## CRA vs Vite
[Comparativa entre CRA y Vite](https://blog.logrocket.com/vite-3-vs-create-react-app-comparison-migration-guide/)