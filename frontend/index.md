# Crear una aplicación web cliente

### 1. Inicialización

Creamos una carpeta e inicializamos el proyecto con -y (Sí a todo)

````
npm init -y
````

Esto creará un fichero package.json en el directorio raiz

### 2. Directorio src para javascript

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

#### 

Bibliografía:

https://www.youtube.com/watch?v=ansUGkcrhwY
