# Comenzar un proyecto Vue

En este apartado se introducen algunos recursos útiles para iniciar tu primer proyecto en Vue.

## Requisitos previos

Es necesario contar con:

- [Node js](https://nodejs.org/es/), un entorno en tiempo de ejecución multiplataforma, de código abierto, para la capa del servidor (pero no limitándose a ello) basado en el lenguaje de programación JavaScript.

- [npm](https://www.npmjs.com/) (Node Package Manager), es un gestor de paquetes desarrollado en su totalidad bajo el lenguaje JavaScript por Isaac Schlueter, a través del cual podemos obtener cualquier librería con tan solo una sencilla línea de código.

- [https://code.visualstudio.com/](https://code.visualstudio.com/)(opcional)

## Extensiones Visual Code Studio

A continuación se listan algunas extensiones que pueden resultar muy útiles para trabajar con Vue en éste editor.

- [Volar](https://github.com/johnsoncodehk/volar) proporciona resaltado de sintaxis e inteligencia para expresiones de plantillas y accesorios de componentes.
- [Vue VSCode Snippets](https://github.com/sdras/vue-vscode-snippets) proporciona snippets para Vue2 y Vue3.

## Herramientas de desarrollo del navegador

La extensión de herramientas de desarrollo del navegador Vue nos permite explorar el árbol de componentes de una aplicación Vue, inspeccionar el estado de los componentes individuales, realizar un seguimiento de los eventos de gestión del estado y el rendimiento del perfil.

- [Documentación](https://devtools.vuejs.org/)
- [Extensión para Chrome](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)
- [Extensión para Firefox](https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/)

## ¿Cómo iniciar un proyecto Vue?

Un proyecto creado con Vue puede tener múltiples formas de crearse, pero existen unas pautas, recomendaciones y buenas prácticas para crear proyectos similares bajo los mismos criterios.

Para ello, existen herramientas de línea de comandos que actúan de asistentes y permiten crear una estructura de carpetas con un proyecto Vue inicial, con todas las herramientas que necesitemos instaladas y listo para comenzar a trabajar con él.

Tenemos dos opciones:

- Create Vue
- Vue-CLI

### Create Vue (Vite)

[Vite](https://vitejs.dev/) es una herramienta de compilación liviana y rápida con soporte Vue SFC de primera clase. Está desarrollada por Evan You, el creador de Vue.

Para comenzar con Vite + Vue, simplemente ejecute:

```
$ npm init vue@latest
```

Este comando instalará y ejecutará [create-vue](https://github.com/vuejs/create-vue), la herramienta oficial de andamiaje (scaffolding) del proyecto Vue.

### Ejemplo de creación de proyecto con vite

```
npm init vue@latest
```

```
✔ Project name: … <your-project-name>
✔ Add TypeScript? … No / Yes
✔ Add JSX Support? … No / Yes
✔ Add Vue Router for Single Page Application development? … No / Yes
✔ Add Pinia for state management? … No / Yes
✔ Add Vitest for Unit testing? … No / Yes
✔ Add Cypress for both Unit and End-to-End testing? … No / Yes
✔ Add ESLint for code quality? … No / Yes
✔ Add Prettier for code formatting? … No / Yes

Scaffolding project in ./<your-project-name>...
Done.
```

```
> cd <your-project-name>
> npm install
> npm run dev
```

### CLI de Vue

Vue CLI ha sido durante mucho tiempo la cadena de herramientas oficial basada en paquetes web para Vue. Según la documentación oficial del framework está herramienta **está en modo de mantenimiento** y no recomiendan su uso. Actualmente recomiendan comenzar nuevos proyectos con Vite, la cual proporciona una experiencia de desarrollador superior en la mayoría de los casos.

## Referencias

- [ Guía de herramientas de Vue 3](https://vuejs.org/guide/scaling-up/tooling.html#project-scaffolding)
