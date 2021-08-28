# Crear una aplicación web cliente

## 1. NPM

Creamos una carpeta e inicializamos el proyecto con -y (Sí a todo)

````
npm init -y
````

Esto creará un fichero package.json en el directorio raiz

Como este fichero guarda todas las dependencias necesarias, si nos clonasemos un proyecto de github, con npm install se nos actualizarían las dependencias en local.

Para instalar una dependencia:
````
npm install nombreDelPaquete -D
````
Las dependencias son de dos tipos:
 - Las que solo necesitamos en desarrollo y no las llevamos a producción: Añadimos -D o --save-dev 
 - Las que tienen que empaquetarse con la aplicación porque son necesarias para su funcionamiento: --save o simplemente no añadir nada

También en el caso de una applicación propia, nos interesará añadir "private": true para evitar publicarla accidentalmente en el registry de npm.

Se pueden consultar las [opciones de configuración del package.json](https://docs.npmjs.com/cli/v7/configuring-npm/package-json).

Si tenemos nuestro proyecto en GitHub podemos mantener nuestras dependencias actualizadas activando Dependabot en la configuración de nuestro repositorio. (settings -> Security & Analysis)

#### Scripts

En el package.json podemos definir scripts o comandos que lancen ordenes concretas. Con npm run podemos ver la lista de scripts disponibles. Si queremos ejecutar uno en concreto 
````
npm run nombreScript
npm start
npm test
```` 
Test y start se pueden ejecutar sin el run, ya que son estándares.

También podríamos hacer npx comandoLargo. Esto nos permitiría ejecutar un comando, sin ni siquiera instalar la dependendencia. Se bajaría en algún sitio temporal lo necesario y ejecutaría el comando.

## 2. Directorio src para javascript

Creamos una carpeta src con un fichero index.js. Después, a medida que necesitemos otros ficheros .js los iremos añadiendo aquí.

Antiguamente, lo que se hacía era cargar los scripts directamente en el fichero html que los iba a usar. En lugar de cargar los scripts en el header, se cargan final del body (para que no se ejecute las inicializaciones del js cuando aun no se ha cargado el dom)

El principal problema de esta aproximación era que habia que cargar manualmente las dependencias, y en el orden adecuado. Además, si hubiera colisión entre los nombres de variables o funciones de los scripts, sería complicado preever la sobreescritura de estas.

#### Módulos

Una primera mejora de este sistema son los módulos. En lugar de cargar todos nuestros scripts, cargamos solo el index, añadiendo type='module'

````
<script type="module" src='./index.js'></script>
````
Y en el propio index.js, importamos las funciones de otros ficheros que vayamos a utilizar:
````
import functionName from './fileName.js'
````

Para que la función se pueda exportar en otros módulos, hay que marcarla como exportable con *export default*
````
export default function functionName() {
}
````

## 3. Webpack

El siguiente paso para administrar las dependencias más automaticamente sería instalar webpack. instalo webpack y su cli como dependencias de desarrollo:
````
npm install webpack webpack-cli -D
````

Podemos delegar en webpack la construcción de nuestros entregables, es decir, que genere todos los ficheros estáticos que necesitamos para ir a producción, el conocido como proceso de build. Creamos los siguientes scripts en el package.json:

````
build: webpack --mode production
dev: webpack --mode development --watch
````
Hay dos modos:
 - **Desarrollo** Para desarrollar, en caso de que haya errores, es fácil ver en que linea se produce
 - **Producción** Optimiza los ficheros para producción

Y al ejecutar npm run build, se generará la build de la aplicación

El parámetro watch hace queen cuanto se detecte un campio, compile y lo sirva inmediatamente.

### Entry point

El punto de entrada de la aplicación es por donde empieza webpack a tirar del hilo. A partir de ahí va buscando dependencias, y todo lo que encuentre lo dejará en la carpeta de salida. El punto de entrada por defecto es './src/index.js', y el punto de salida por defecto './dist/main.js'

### Punto2 webpack
### Punto3 webpack
### Punto4 webpack

## Babel

A través de webpack, Babel es la herramienta que permite traducir javascript moderno a javascript que entienda cualquer navegador antiguo. Así, nosotros podemos utilizar las últimas novedades del lenguaje, dando el máximo soporte posible.

````
npm install @babel/preset-env @babel/core @babel-loader -D
````



Bibliografía:

https://www.youtube.com/watch?v=ansUGkcrhwY

https://pro.codely.tv/library/js-moderno
